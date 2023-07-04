# Workflow Collections

## Worflows

### [check.yml](.github/workflows/check.yml)

Runs lint and test

`ShuttlePub/workflows/.github/workflows/check.yml@main`

Inputs

| name    | default |
| ------- | ------- |
| channel | stable  |

### [coverage.yml](.github/workflows/coverage.yml)

Generage coverage and upload to covecov

`ShuttlePub/workflows/.github/workflows/coverage.yml@main`

Secrets

- CODECOV_TOKEN

# Example

```yaml
name: check
on:
  push:
    branches:
      - main

jobs:
  build:
    uses: ShuttlePub/workflows/.github/workflows/check.yml@main
    with:
      chennel: unstable
```

### [bench.yml](.github/workflows/bench.yml)

Benchmark each workspaces

`ShuttlePub/workflows/.github/workflows/bench.yml@main`

Inputs
| name           | default             |
| -------------- | ------------------- |
| channel        | stable              |
| features       | async,tokio-support |
| compare-branch | main                |
