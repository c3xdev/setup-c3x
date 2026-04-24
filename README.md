# setup-c3x

GitHub Action to install the [C3X](https://c3x.dev) CLI for cloud cost estimation.

## Usage

```yaml
name: Cost Estimation
on: [pull_request]
permissions:
  pull-requests: write
jobs:
  c3x:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: c3xdev/setup-c3x@v1
        id: c3x
      - run: c3x comment github --path . --github-token ${{ steps.c3x.outputs.token }}
```

That's it. No secrets to configure.

### Branded PR comments

Install the [C3X Cloud](https://github.com/apps/c3x-cloud) app on your repository. Once installed, PR comments automatically appear as **C3X Cloud** with the C3X logo instead of the generic github-actions bot.

No additional configuration needed. The action detects the app installation and generates a branded token automatically.

### Pin a specific version

```yaml
- uses: c3xdev/setup-c3x@v1
  with:
    version: "1.0.0"
```

## Inputs

| Input | Description | Default |
|---|---|---|
| `version` | C3X version to install (e.g. `1.0.0`) | `latest` |

## Outputs

| Output | Description |
|---|---|
| `token` | GitHub token for `c3x comment`. Branded C3X Cloud token if the app is installed, falls back to `GITHUB_TOKEN`. |

## How it works

1. Installs the C3X binary (cached across runs)
2. Checks if the C3X Cloud GitHub App is installed on the repo
3. If installed, requests a branded token from the C3X Cloud token service
4. If not installed, falls back to the default `GITHUB_TOKEN`

## License

Apache-2.0
