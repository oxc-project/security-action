# security-action

Run [`zizmor`](https://docs.zizmor.sh/) against the current repository's GitHub Actions workflows and upload the results as SARIF to GitHub code scanning.

## Usage

This action checks out the repository internally. The job needs `security-events: write` so GitHub can accept the uploaded SARIF results.

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
    permissions:
      security-events: write
    steps:
      - uses: oxc-project/security-action@v1
```

## Configuration

`zizmor` will automatically discover a repository-local `zizmor.yml` file. For example:

```yaml
rules:
  secrets-outside-env:
    config:
      allow:
        - APP_ID
        - APP_PRIVATE_KEY
```
