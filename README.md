# Git Tag Generator

A GitHub Action to automatically push new tag to master, on push or merge with the latest SemVer formatted version if the commit message contains specified keywords.

> ***Note:*** This action creates a [lightweight tag](https://developer.github.com/v3/git/refs/#create-a-reference).

[![Build Status](https://github.com/ChloePlanet/github-tag-action/workflows/Generate%20tag/badge.svg)](https://github.com/ChloePlanet/github-tag-action/workflows/Generate%20tag/badge.svg)
[![Stable Version](https://img.shields.io/github/v/tag/ChloePlanet/github-tag-action)](https://img.shields.io/github/v/tag/ChloePlanet/github-tag-action)
[![Latest Release](https://img.shields.io/github/v/release/ChloePlanet/github-tag-action?color=%233D9970)](https://img.shields.io/github/v/release/ChloePlanet/github-tag-action?color=%233D9970)
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Git%20Tag%20Generator-yellow.svg?colorA=24292e&colorB=ffd33d&style=flat&longCache=true&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4O3EEtbPwhJbr6Te28CmdSKeqzeqr0YbfVIrTBKakvtOl5dtTkK+v4HfA9PEyBFCY9AGVgCBLaBp1jPAyfAJ/AAdIEG0dNAiyP7+K1qIfMdonZic6+WJoBJvQlvuwDqcXadUuqPA1NKAlexbRTAIMvMOCjTbMwl1LtI/6KWJ5Q6rT6Ht1MA58AX8Apcqqt5r2qhrgAXQC3CZ6i1+KMd9TRu3MvA3aH/fFPnBodb6oe6HM8+lYHrGdRXW8M9bMZtPXUji69lmf5Cmamq7quNLFZXD9Rq7v0Bpc1o/tp0fisAAAAASUVORK5CYII=)](https://github.com/marketplace/actions/git-tag-generator)


## Usage

```YAML
name: Generate tag

on:
  push:
    branches:
      - master

jobs:
  build:
    name: Generate tag
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@master
        with:
          fetch-depth: '0'

      - name: Bump version and push tag
        uses: ChloePlanet/github-tag-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          WITH_V: true

```

> ***NOTE***: set the fetch-depth for `actions/checkout@master` to be sure you retrieve all commits to look for the semver commit message.

Use [GitHub Personal Access Token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) `${{ secrets.REPO_ACCESS_TOKEN }}` in `GITHUB_TOKEN` if you want to trigger another workflow by this workflow.

```YAML
name: Another workflow on tag generated

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  myJob:
    name: On tag generated
    runs-on: ubuntu-latest
    steps:
      # Whatever you want to do on tag generated

```

> An action in a workflow run can't trigger a new workflow run. When you use GITHUB_TOKEN in your actions, all of the interactions with the repository are on behalf of the Github-actions bot. The operations act by Github-actions bot cannot trigger a new workflow run. More details: https://help.github.com/en/actions/reference/events-that-trigger-workflows#about-workflow-events



## Workflow

* Add this action to your repo
* Commit some changes with message contains `#major` or `#minor` or `#patch`
* Either push to master or open a PR
* On push (or merge) to `master`, the action will:
  * Get latest tag
  * Generate tag if commit message contains `#major` or `#patch` or `#patch`
    > ***Note:*** This action **will not** push the tag if the `HEAD` commit has already been tagged. If two or more keywords are present, the highest-ranking one will take precedence.
  * Pushes tag to GitHub



## Environment Variables

* **GITHUB_TOKEN** ***(required)*** - Required for permission to tag the repo.
* **WITH_V** *(optional)* - Tag version with `v` character.
* **RELEASE_BRANCHES** *(optional)* - Comma separated list of branches (bash reg exp accepted) that will generate the release tags. Other branches and pull-requests generate versions postfixed with the commit hash and do not generate any tag. Examples: `master` or `.*` or `release.*,hotfix.*,master` ...
* **CUSTOM_TAG** *(optional)* - Set a custom tag, useful when generating tag based on f.ex FROM image in a docker image. **Setting this tag will invalidate any other settings set!**
* **SOURCE** *(optional)* - Operate on a relative path under $GITHUB_WORKSPACE.



## Outputs

* **last_tag** - The value of the latest tag before running this action.
* **new_tag** - The value of the newly created tag.
* **tag_generated** - 1 if new tag is generated successfully. 0 if no tag is generated
* **last_major** - The major part of last tag
* **last_minor** - The minor part of last tag
* **last_patch** - The patch part of last tag
* **major** - The major part of new tag
* **minor** - The minor part of new tag
* **patch** - The patch part of new tag
* **bump_ver** - The bump version string e.g. `major`, `minor` or `patch`



## Credits

[anothrNick/github-tag-action](https://github.com/anothrNick/github-tag-action)
[fsaintjacques/semver-tool](https://github.com/fsaintjacques/semver-tool)



## Projects using Git Tag Generator

A list of projects using Git Tag Generator for reference.



## License

[MIT License](https://github.com/ChloePlanet/github-tag-action/blob/master/LICENSE)

