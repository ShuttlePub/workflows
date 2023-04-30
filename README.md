# Workflow Collections

## Worflows

### [check.yml](.github/workflows/check.yml)

Runs lint and test

`ShuttlePub/workflows/.github/workflows/check.yml@main`

Inputs

| name    | default |
| ------- | ------- |
| channel | stable  |

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

