# setup-c3x

GitHub Action to install the [C3X](https://c3x.dev) CLI for cloud cost estimation.

## Usage

```yaml
- uses: c3xdev/setup-c3x@v1
```

### Pin a specific version

```yaml
- uses: c3xdev/setup-c3x@v1
  with:
    version: "1.0.0"
```

### Full example

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
      - run: c3x comment github --path . --github-token ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Description | Default |
|---|---|---|
| `version` | C3X version to install (e.g. `1.0.0`) | `latest` |

## Caching

The binary is cached across workflow runs using `actions/cache`. Subsequent runs skip the download entirely.

## License

Apache-2.0
