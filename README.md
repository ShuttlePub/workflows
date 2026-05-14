# Workflow Collections

## Worflows

### [check.yml](.github/workflows/check.yml)

Runs lint and test

`ShuttlePub/workflows/.github/workflows/check.yml@main`

Inputs
| name              | required | example                  | description                                              |
| ----------------- | -------- | ------------------------ | -------------------------------------------------------- |
| workspace         | yes      | '[ "kernel", "driver" ]' | Workspaces to run check on                               |
| target-repository | no       | ShuttlePub/Stellar       | Repository to checkout (default: caller repository)      |
| target-ref        | no       | main                     | Ref of target-repository (default: caller ref)           |

### [test-psql.yml](.github/workflows/test-psql.yml)

Runs lint and test with PostgreSQL

`ShuttlePub/workflows/.github/workflows/test-psql.yml@main`

Inputs
| name              | required | example  | description                                              |
| ----------------- | -------- | -------- | -------------------------------------------------------- |
| workspace         | yes      | driver   | Workspace to run check on                                |
| target-repository | no       | ShuttlePub/Emumet | Repository to checkout (default: caller repository) |
| target-ref        | no       | main     | Ref of target-repository (default: caller ref)           |

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

Inputs
| name              | required | example            | description                                              |
| ----------------- | -------- | ------------------ | -------------------------------------------------------- |
| target-repository | no       | ShuttlePub/Stellar | Repository to checkout (default: caller repository)      |
| target-ref        | no       | main               | Ref of target-repository (default: caller ref)           |

outputs

| name   | example              |
| ------ | -------------------- |
| matrix | ["kernel", "driver"] |



### [read-toolchain.yml](.github/workflows/read-toolchain.yml)

Read toolchain channel from rust-toolchain.toml

`ShuttlePub/workflows/.github/workflows/read-toolchain.yml@main`

Inputs
| name              | required | example            | description                                              |
| ----------------- | -------- | ------------------ | -------------------------------------------------------- |
| target-repository | no       | ShuttlePub/Stellar | Repository to checkout (default: caller repository)      |
| target-ref        | no       | main               | Ref of target-repository (default: caller ref)           |

outputs

| name    | example |
| ------- | ------- |
| channel | stable  |

## Consumer integration tests

[`test-consumers.yml`](.github/workflows/test-consumers.yml) runs on push/PR to `main` that touches `.github/workflows/**`. It invokes `check.yml` and `test-psql.yml` (at the same revision as the caller workflow — for `pull_request` events, that is the PR's merge commit) against real downstream repositories (Stellar, Emumet) using `target-repository` / `target-ref` inputs, so breaking changes are caught before merging.

Notes:

- **Fork PRs are skipped.** Consumer jobs only run for pushes to `main` and PRs from branches within the same repository (`github.event.pull_request.head.repo.full_name == github.repository`). This is intentional: cross-repo checkout of an external fork's modified workflow files would let untrusted code run with this repository's token. Maintainers can validate fork PRs by pushing the branch to this repository.
- **Consumer repositories must be public.** Cross-repo `actions/checkout` uses the default `GITHUB_TOKEN`, which is scoped to this repository. Private/internal downstream repositories would require a PAT or GitHub App token, which is not configured.
- **No repository token is exposed to cargo steps.** All checkouts use `persist-credentials: false`, and `cargo build/test/clippy` steps do not receive any `GITHUB_TOKEN` in their environment, so downstream Rust code has no access to this repository's token.

