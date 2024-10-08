# Workflow Collections

## Worflows

### [check.yml](.github/workflows/check.yml)

Runs lint and test

`ShuttlePub/workflows/.github/workflows/check.yml@main`

Inputs
| name      | example                  |
| --------  | ------------------------ |
| workspace | '[ "kernel", "driver" ]' |

### [test-psql.yml](.github/workflows/test-psql.yml)

Runs lint and test with PostgreSQL

`ShuttlePub/workflows/.github/workflows/test-psql.yml@main`

Inputs
| name      | example                  |
| --------  | ------------------------ |
| workspace | '[ "kernel", "driver" ]' |

### [coverage.yml](.github/workflows/coverage.yml)

Generage coverage and upload to covecov

`ShuttlePub/workflows/.github/workflows/coverage.yml@main`

Secrets

- CODECOV_TOKEN

### [bench.yml](.github/workflows/bench.yml)

Benchmark each workspaces

`ShuttlePub/workflows/.github/workflows/bench.yml@main`

Inputs
| name           | default             |
| -------------- | ------------------- |
| features       | async,tokio-support |
| compare-branch | main                |

### [validate-pr.yml](.github/workflows/validate-pr.yml)

Validates pull request title

`ShuttlePub/workflows/.github/workflows/validate-pr.yml@main`

```yaml
name: Validate PR title

on:
  pull_request_target:
    types:
      - opened
      - edited
      - synchronize
    
jobs:
  validate:
    uses: ShuttlePub/workflows/.github/workflows/validate-pr.yml@main
```



# Example

```yaml
name: check
on:
  pull_request:
    branches:
      - dev

jobs:
  build:
    uses: ShuttlePub/workflows/.github/workflows/bench.yml@main
    with:
      compare-branch: dev 
```

## Sub workflows

### [detect-workspaces.yml](.github/workflows/detect-workspaces.yml)

Detect workspaces and outputs with matrix compatible format

`ShuttlePub/workflows/.github/workflows/detect-workspaces.yml@main`

outputs

| name   | example              |
| ------ | -------------------- |
| matrix | ["kernel", "driver"] |



### [read-toolchain.yml](.github/workflows/read-toolchain.yml)

Read toolchain channel from rust-toolchain.toml

`ShuttlePub/workflows/.github/workflows/read-toolchain.yml@main`

outputs

| name    | example |
| ------- | ------- |
| channel | stable  |

