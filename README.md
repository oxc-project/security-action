# security-action

Run security checks against the current repository. The current check set includes [`zizmor`](https://docs.zizmor.sh/) for GitHub Actions workflows and fails the job when medium-severity or higher findings are present.

## Usage

This action checks out the repository internally and runs the configured security checks. Today, that includes `zizmor --config "${{ github.action_path }}/zizmor.yml" --strict-collection --show-audit-urls=always --min-severity=medium .`, so the job fails on collection errors and on medium-severity or higher findings.

```yaml
name: GitHub Actions Security Analysis

permissions: {}

on:
  workflow_dispatch:
  pull_request:
    types: [opened, synchronize]
    paths:
      - ".github/workflows/**"
  push:
    branches:
      - main
    paths:
      - ".github/workflows/**"

jobs:
  security:
    runs-on: ubuntu-slim
    steps:
      - uses: oxc-project/security-action@v1
```

## Configuration

The zizmor check passes its bundled config file at `${{ github.action_path }}/zizmor.yml` to `zizmor`. The current shared config includes:

```yaml
rules:
  secrets-outside-env:
    config:
      allow:
        - APP_ID
        - APP_PRIVATE_KEY
```
