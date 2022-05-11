# NPM Lockfile Changes

[<sub><img src="https://git.io/J38HP" height="16" /></sub>](#) [<sub><img src="https://git.io/J38dY" height="16" /></sub>](#) [<sub><img src="https://git.io/J38ds" height="16" /></sub>](#) [<sub><img src="https://git.io/J38dt" height="16" /></sub>](#)

Creates a comment inside Pull Request with the human-readable summary of the changes to the `package-lock.json` file. Works in public and private repositories and offers a few customization options.

## Usage

### ⚡️ Workflow Example

The example below shows the minimal workflow setup and all the optional inputs for the action (set to their default values). If you are happy with the output generated by the action, it's safe to remove all optional inputs.

```yml
name: NPM Lockfile Changes
on: [pull_request]

jobs:
  lockfile_changes:
    runs-on: ubuntu-latest
    # Permission overwrite is required for Dependabot PRs, see "Common issues" below.
    permissions:
      pull-requests: write
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: NPM Lockfile Changes
        uses: codepunkt/npm-lockfile-changes@main
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # Optional inputs, can be deleted safely if you are happy with default values.
          collapsibleThreshold: 25
          failOnDowngrade: false
          path: package-lock.json
          updateComment: true
```

### 🔌 Inputs

| Input                  |      Required      |       Default       | Description                                                                                                      |
| ---------------------- | :----------------: | :-----------------: | ---------------------------------------------------------------------------------------------------------------- |
| `collapsibleThreshold` |         No         |        `25`         | Number of lock changes, which will result in collapsed comment content and an addition of changes summary table. |
| `failOnDowngrade`      |         No         |       `false`       | When a dependency downgrade is detected, fail the action. **Comment will still be posted.**                      |
| `path`                 |         No         | `package-lock.json` | Path to the `package-lock.json` file in the repository. Default value points to the file at project root.        |
| `token`                | <ins>**Yes**</ins> |          –          | Repository `GITHUB_TOKEN` which allows action to make calls to the GitHub API (Octokit).                         |
| `updateComment`        |         No         |       `true`        | Update the comment on each new commit. If value is set to `false`, bot will post a new comment on each change.   |

## 📸 Preview

### Basic comment appearance

<img alt="basic" src="https://user-images.githubusercontent.com/719641/116818857-c5029d80-ab6d-11eb-8b48-122b851c1d9e.png">

### Comment appearance when `collapsibleThreshold` has been reached

<img alt="summary" src="https://user-images.githubusercontent.com/719641/116819012-7efa0980-ab6e-11eb-99f1-15996b6f12b4.png">

## 📋 Common issues

### The action fails on the Dependabot pull requests

Due to the security reasons from March 1st, 2021 workflow runs that are triggered by Dependabot have permissions reduced by default:

- [GitHub Actions: Workflows triggered by Dependabot PRs will run with read-only permissions](https://github.blog/changelog/2021-02-19-github-actions-workflows-triggered-by-dependabot-prs-will-run-with-read-only-permissions/)

To ensure that sufficient permissions for this action are always granted, you will need to add `permissions` entry to the job which runs `npm-lockfile-changes`:

```yml
jobs:
  ...:
    runs-on: ...
    #####
    permissions:
      pull-requests: write
    #####
    steps: ...
```

## 🔍️ Debugging

To run action in the debug mode you need to add the `ACTIONS_STEP_DEBUG` repository secret and set it to `true`, as stated in the [GitHub documentation](https://docs.github.com/en/actions/managing-workflow-runs/enabling-debug-logging#enabling-step-debug-logging).

Then additional information which might be useful for the users when debugging the issues will be available in the action output, prefixed by `##[debug]`.
