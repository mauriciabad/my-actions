# My custom GitHub actions

This repository (`mauriciabad/my-actions`) contains reusable GitHub Actions for my projects.

## Sample workflows
Sample workflows you can copy paste to yout repo.

- [auto-merge-dependabot](https://github.com/mauriciabad/my-actions/blob/main/.github/workflows/auto-merge-dependabot.yml)

## Reusable workflows

### CI Vite
Complete set of jobs for a CI of a project using Vite, Vue.js 3, Cypress, and npm.

It has jobs for: `lint`, `type-check`, `unit-test`, and `e2e-test`.

After linting, it commits the autofixed changes to the same branch.

After the E2E tests, it can send the records to <https://cypress.io>.

#### Usage
1. Define the secrets `CYPRESS_PROJECT_ID` and `CYPRESS_RECORD_KEY` in your repository configuration.
2. Create this gh action workflow file:

```yml
# .github/workflows/ci.yml

name: CI
on: push
jobs:
  ci:
    uses: mauriciabad/my-actions/.github/workflows/ci-vite.yml@main
    with:
      record-e2e: false
    secrets:
      CYPRESS_PROJECT_ID: ${{ secrets.CYPRESS_PROJECT_ID }}
      CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
```

##### Options
| Name | Type | Default | Description|
|:---|:---:|:----:|:---|
| `autofix` | boolean | true | Commit automatic fixes after lint |
| `record-e2e` | boolean | false | Record e2e tests. Must pass `CYPRESS_PROJECT_ID` and `CYPRESS_RECORD_KEY` secrets |
| `lint` | boolean | true | Run lint job |
| `type-check` | boolean | true | Run type-check job |
| `unit-test` | boolean | true | Run unit-test job |
| `e2e-test` | boolean | true | Run e2e-test job |
##### Secrets
| Name | Description|
|:---|:---|
| `CYPRESS_PROJECT_ID` | Cypress project id. [More detais here](https://docs.cypress.io/guides/cloud/projects#Project-ID). Typically this value is saved in the `cypress.json` file.  |
| `CYPRESS_RECORD_KEY` | Cypress record key. [More detais here](https://docs.cypress.io/guides/cloud/projects#Record-key) |

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
