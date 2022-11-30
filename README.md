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

If the E2E tests fail, it uploads screenshots and videos artifacts. These are only avilable for 3 days.

Runs Lighthouse CI when all tests pass.

#### Usage

1. Create this gh action workflow file:

    ```yml
    # .github/workflows/ci.yml

    name: CI
    on: push
    jobs:
      ci:
        name: CI
        uses: mauriciabad/my-actions/.github/workflows/ci-vite.yml@main
        with:
          lighthouse: false
        secrets:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
    ```

1. Configure Lighthouse:
    1. Install the [Lighthouse CI GitHub App](https://github.com/apps/lighthouse-ci)
    1. Copy the token provided on the authorization confirmation page and [add it to your build environment](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables) as `LHCI_GITHUB_APP_TOKEN`.
    1. Create a `lighthouserc.yaml` file on the project root with this contents:

    ```yaml
    ci:
      preset: lighthouse:all
      upload:
        target: temporary-public-storage
    ```

##### Options

| Name | Type | Default | Description|
|:---|:---:|:----:|:---|
| `autofix` | boolean | true | Commit automatic fixes after lint |
| `lint` | boolean | true | Run lint job |
| `type-check` | boolean | true | Run type-check job |
| `unit-test` | boolean | true | Run unit-test job |
| `e2e-test` | boolean | true | Run e2e-test job |
| `lighthouse` | boolean | true | Run lighthouse job. Must pass `LHCI_GITHUB_APP_TOKEN` secret |

##### Secrets

| Name | Description|
|:---|:---|
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
