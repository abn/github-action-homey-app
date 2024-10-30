# GitHub Actions For Homey App Development

Composite GitHub Actions for Homey App Development

## Introduction

This repository contains composite GitHub Actions designed to streamline the
development workflow for Homey applications. These actions encapsulate common
tasks and simplify the process of building, testing, and deploying Homey apps.

### Why not use the official ones from Athom BV?

This comes down to personal choice, while the steps maintain parity to what the Athom version does,
these actions are written as composite actions instead of containers. Personally, it is just a bit
easier to work with.

## Example Workflows

> The examples here are modified versions of what is generated by Homey CLI.The examples here are modified versions of 
> what is generated by Homey CLI.

<details>
<summary><b>validate.yaml</b></summary>
<p>

```yaml
name: Validate Homey App
on:
  workflow_dispatch:
    inputs:
      level:
        type: choice
        description: Validation Level
        required: true
        default: debug
        options:
          - debug
          - publish
          - verified
  push:
  pull_request:

jobs:
  main:
    name: Validate Homey App
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: abn/github-action-homey-app/setup@main

      - uses: abn/github-action-homey-app/validate@main
        with:
          level: "verified"
```
</p>
</details>

<details>
<summary><b>version-update.yaml</b></summary>
<p>

```yaml
name: Update Homey App Version
on:
  workflow_dispatch:
    inputs:
      version:
        type: choice
        description: Version
        required: true
        default: patch
        options:
          - major
          - minor
          - patch
      changelog:
        type: string
        description: Changelog
        required: true

# Needed in order to push the commit and create a release
permissions:
  contents: write

jobs:
  main:
    name: Update Homey App Version
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: abn/github-action-homey-app/setup@main

      - uses: abn/github-action-homey-app/version@main
        with:
          changelog: "${{ inputs.changelog }}"
          next: "${{ inputs.version }}"

      - name: Commit & Push
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          file_pattern: ".homeychangelog.json .homeycompose/app.json app.json"
          commit_message: "Update Homey App Version to v${{ steps.version-update.outputs.new-version }}"

      - name: Create Release
        uses: ncipollo/release-action@v1
        with:
          tag: "v${{ steps.version-update.outputs.new-version }}"
          generateReleaseNotes: true
          makeLatest: true
```
</p>
</details>

<details>
<summary><b>publish.yaml</b></summary>
<p>

```yaml
name: Publish Homey App
on:
  workflow_dispatch:
  push:
    tags:
      - v**

jobs:
  main:
    name: Publish Homey App
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: abn/github-action-homey-app/setup@main

      - uses: abn/github-action-homey-app/publish@main
        id: publish
        with:
          personal_access_token: ${{ secrets.HOMEY_PAT }}
```
</p>
</details>



## Contributing

Contributions are welcome\! Please open an issue or submit a pull request if you
have any suggestions, bug reports, or feature requests.

## License

This project is licensed under the MIT License - see the LICENSE file for
details.
