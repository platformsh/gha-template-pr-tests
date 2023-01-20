# Platform.sh Template Pull Request Tests

Performs a series of tests on the pull request environment for each pull request.

Will check for the status of the PR environment. Once available will retrieve the URL for the environment, 
and then begin a visual regression test between the production environment and PR environment. If the visual regression
test fails, will create a comment in the PR with a link to the visual regression report.

## Inputs
* `github-token` - **REQUIRED**. Github [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) with 
access rights to the target repository so we can work with the github api. You can reuse the default `GITHUB_TOKEN` that's created for the runner.
* `baseline-url` - **REQUIRED**. Full URL to the "production" version of the template. This URL is used to create
the reference set of images in the visual regression tests. It *must* include a trailing slash.
## Outputs
* None
## Example Usage
```yaml
name: "Test the Pr Environment"
on:
  workflow_run:
    workflows: [ "platformsh" ]
    types:
      - completed
  pull_request:
    branches:
      - master
      - main
jobs:
  test-pr-env:
    name: TestPrEnvironment
    runs-on: ubuntu-latest
    if: ${{ github.repository_owner == 'platformsh-templates' }}
    steps:
      - name: 'Run Pull Request Tests'
        id: pr-tests
        uses: platformsh/gha-template-pr-tests@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          baseline-url: ${{ vars.BASELINE_URL }}
```
## Uses
* [platformsh/gha-retrieve-psh-prenv-url](https://github.com/platformsh/gha-retrieve-psh-prenv-url/)
* [platformsh/gha-vrt](https://github.com/platformsh/gha-vrt/)
* [actions/github-script](https://github.com/actions/github-script)

## Roadmap
* Expand checks on `baseline-url` to ensure a valid URL, and contains trailing slash, add if missing
* Add behavioral testing
* Add performance/[Blackfire.io](https://blackfire.io/) testing