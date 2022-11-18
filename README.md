# My custom GitHub actions

This repository (`mauriciabad/my-actions`) contains reusable GitHub Actions for my projects.

This README contains the list of avilable actions

## `setup-node`
This action installs node and the npm dependencies.

### What does it do specifically?
1. Grant access to the repository code.
1. Sets-up npm cache.
1. Installs Node.js based on the version specified in the file `.nvmrc`.
1. Runs `npm ci`

### Example usage
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
