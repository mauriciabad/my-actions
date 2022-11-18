# My custom GitHub actions

This repository (`mauriciabad/my-actions`) contains reusable GitHub Actions for my projects.

## Sample workflows
Sample workflows you can copy paste to your repo.

### Auto-merge dependabot PRs
1. Copy this code into a new workflow in your repository: [auto-merge-dependabot](https://github.com/mauriciabad/my-actions/blob/main/.github/workflows/auto-merge-dependabot.yml).
1. [Create a Personal Access Token with `push` permission](https://github.com/ahmadnassri/action-dependabot-auto-merge#token-scope) and save it as a secret `PUSH_TOKEN` in the repository.


## Reusable workflows

### CI Vite
Complete set of jobs for a CI of a project using Vite, Vue.js 3, Cypress, and npm.

It has jobs for: `lint`, `type-check`, `unit-test`, and `e2e-test`.

After linting, it commits the autofixed changes to the same branch.

After the E2E tests, it can send the records to <https://cypress.io>.

Runs Lighthouse CI when all tests pass.

#### Usage
2. Create this gh action workflow file:
    ```yml
    # .github/workflows/ci.yml

    name: CI
    on: push
    jobs:
      ci:
        name: CI
        uses: mauriciabad/my-actions/.github/workflows/ci-vite.yml@main
        with:
          record-e2e: false
        secrets:
          CYPRESS_PROJECT_ID: ${{ secrets.CYPRESS_PROJECT_ID }}
          CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
    ```
1. Configure Cypress: 
    1. Define the secrets `CYPRESS_PROJECT_ID` and `CYPRESS_RECORD_KEY` in your repository configuration. `CYPRESS_PROJECT_ID` Can just be inlined without a secret, if you prefer so.
    1. Set the option `record-e2e` to true if you want to send data to <https://cypress.io>.
1. Configure Lighthouse:
    1. Install the [Lighthouse CI GitHub App](https://github.com/apps/lighthouse-ci)
    1. Copy the token provided on the authorization confirmation page and [add it to your build environment](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables) as `LHCI_GITHUB_APP_TOKEN`.

##### Options
| Name | Type | Default | Description|
|:---|:---:|:----:|:---|
| `autofix` | boolean | true | Commit automatic fixes after lint |
| `record-e2e` | boolean | false | Record e2e tests. Must pass `CYPRESS_PROJECT_ID` and `CYPRESS_RECORD_KEY` secrets |
| `lint` | boolean | true | Run lint job |
| `type-check` | boolean | true | Run type-check job |
| `unit-test` | boolean | true | Run unit-test job |
| `e2e-test` | boolean | true | Run e2e-test job |
| `lighthouse` | boolean | true | Run lighthouse job |
##### Secrets
| Name | Description|
|:---|:---|
| `CYPRESS_PROJECT_ID` | Cypress project id. [More detais here](https://docs.cypress.io/guides/cloud/projects#Project-ID). Typically this value is saved in the `cypress.json` file.  |
| `CYPRESS_RECORD_KEY` | Cypress record key. [More detais here](https://docs.cypress.io/guides/cloud/projects#Record-key). |
| `LHCI_GITHUB_APP_TOKEN` | Lighthouse token provided on the authorization confirmation page after the installation of the [Lighthouse CI GitHub App](https://github.com/apps/lighthouse-ci). [More detais here](https://github.com/GoogleChrome/lighthouse-ci/blob/main/docs/getting-started.md#github-app-method-recommended). |

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
