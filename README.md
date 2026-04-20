# security-action

Run security checks against the current repository. The current check set includes [`zizmor`](https://docs.zizmor.sh/) for GitHub Actions workflows and conditional [`cargo-deny`](https://embarkstudios.github.io/cargo-deny/) checks when `Cargo.lock` exists and has changed.

## Usage

This action checks out the repository internally and runs the configured security checks. Today, that includes `zizmor --config "${{ github.action_path }}/zizmor.yml" --strict-collection --show-audit-urls=always --min-severity=medium .`, so the job fails on collection errors and on medium-severity or higher findings.

If the checked-out repository has `Cargo.lock` and the current pull request or push diff changes it, the action also installs `cargo-deny` and runs `cargo deny check --config "${{ github.action_path }}/deny.toml"`.

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

The cargo-deny check passes its bundled config file at `${{ github.action_path }}/deny.toml` to `cargo-deny`.
