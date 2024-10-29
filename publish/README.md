<!-- action-docs-header source="action.yaml" -->

<!-- action-docs-header source="action.yaml" -->

<!-- action-docs-description source="action.yaml" -->
## Description

Use homey cli to publish an app.
<!-- action-docs-description source="action.yaml" -->

<!-- action-docs-inputs source="action.yaml" -->
## Inputs

| name | description | required | default |
| --- | --- | --- | --- |
| `personal_access_token` | <p>Homey Personal Access Token (PAT) used for publication.</p> | `true` | `""` |
| `working-directory` | <p>Working directory to setup environment in.</p> | `false` | `${{ github.workspace }}` |
<!-- action-docs-inputs source="action.yaml" -->

<!-- action-docs-outputs source="action.yaml" -->
## Outputs

| name | description |
| --- | --- |
| `url` | <p>App url including FQDN</p> |
<!-- action-docs-outputs source="action.yaml" -->

<!-- action-docs-runs source="action.yaml" -->
## Runs

This action is a `composite` action.
<!-- action-docs-runs source="action.yaml" -->

## Setup

Before using this actions, ensure you have the following:

- A Homey Developer Account
- A Homey Personal Access Token (PAT) with the necessary permissions to
  publish apps.
