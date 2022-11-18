# My custom GitHub actions

This repository (`mauriciabad/my-actions`) contains reusable GitHub Actions for my projects.

## Sample workflows
Sample workflows you can copy paste to yout repo.

- [ci-vite](https://github.com/mauriciabad/my-actions/blob/main/.github/workflows/ci-vite.yml)
- [auto-merge-dependabot](https://github.com/mauriciabad/my-actions/blob/main/.github/workflows/auto-merge-dependabot.yml)

## Actions
List of avilable actions.

### `setup-node`
This action installs node and the npm dependencies.

#### What does it do specifically?
1. Grant access to the repository code.
1. Sets-up npm cache.
1. Installs Node.js based on the version specified in the file `.nvmrc`.
1. Runs `npm ci`

#### Example usage
```yaml
# .github/workflows/ci.yaml

name: CI
on: push
jobs:
  unit-tests:
    name: Unit tests
    runs-on: ubuntu-latest
    steps:
      - name: ⚙️ Setup
        uses: mauriciabad/my-actions/setup-npm@main
      - name: 🧪 Test
        run: npm run test:unit
```
