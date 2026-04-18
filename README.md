# security-action

Run [`zizmor`](https://docs.zizmor.sh/) against the current repository's GitHub Actions workflows and fail the job when medium-severity or higher findings are present.

## Usage

This action checks out the repository internally and runs `zizmor --config "${{ github.action_path }}/zizmor.yml" --strict-collection --show-audit-urls=always --min-severity=medium .`, so the job fails on collection errors and on medium-severity or higher findings.

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

This action passes its bundled config file at `${{ github.action_path }}/zizmor.yml` to `zizmor`. The current shared config includes:

```yaml
rules:
  secrets-outside-env:
    config:
      allow:
        - APP_ID
        - APP_PRIVATE_KEY
```
