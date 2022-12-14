# Workflows

Collection of reusable workflows

## Use example

### CI
Ideally would run on every push to open Pull Request

```yaml
name: CI

on:
  workflow_dispatch:
  pull_request:

jobs:
  lint:
    uses: parloa/workflows/.github/workflows/lint.yaml@main

  test:
    uses: parloa/workflows/.github/workflows/test.yaml@main
    needs: lint
```

### CI/CD
Ideally would run on every push to main branch

```yaml
name: CI/CD to environment


on:
  workflow_dispatch:
  push:
    branches: [ "main" ]

jobs:
  lint:
    uses: parloa/workflows/.github/workflows/lint.yaml@main

  test:
    uses: parloa/workflows/.github/workflows/test.yaml@main
    needs: lint

  build:
    needs: test
    uses: parloa/workflows/.github/workflows/build.yaml@main
    with:
      IMAGE: eu.gcr.io/<namespace>/<component>
    secrets:
      GCR_ACCESS_TOKEN: ${{ secrets.GCR_ACCESS_TOKEN }}

  deploy:
    needs: build
    uses: parloa/workflows/.github/workflows/deployment.yaml@main
    with:
      COMPONENT: <component>
      IMAGE: eu.gcr.io/<namespace>/<component>
      ENVIRONMENT: <environment>
    secrets:
      ARGOCD_USER_TOKEN: ${{ secrets.ARGOCD_USER_TOKEN }}
```
