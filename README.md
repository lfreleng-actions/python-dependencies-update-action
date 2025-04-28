<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🛠️ Update Python Dependencies

Updates the dependencies of a Python project and raises pull requests in the
repository containing the required changes/updates. Works for projects using
the PDM project tooling, or those described using a "Pipfile".

Compatible with modern Python projects described by: pyproject.toml

## python-dependencies-update-action

## Usage Example

<!-- markdownlint-disable MD046 -->

```yaml
steps:
  - name: Update Python dependencies
    uses: lfreleng-actions/python-dependencies-update-action@main
    with:
      token: ${{ secrets.GITHUB_TOKEN }}
```

<!-- markdownlint-enable MD046 -->

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name   | Required | Description                                      |
| --------------- | -------- | ------------------------------------------------ |
| token           | True     | Github token with the required permissions       |
| path_prefix     | False    | Directory location containing project code       |
| message         | False    | Commit message and pull request title            |
| sign-off-commit | False    | Whether commit message contains signed-off-by    |
| sign-commits    | False    | Sign commits as github-actions[bot]              |
| exit_on_fail    | False    | Exit with error if no Python project code found  |
| no_checkout     | False    | Don't perform a checkout of the local repository |

<!-- markdownlint-enable MD013 -->

## Input Defaults

<!-- markdownlint-disable MD013 -->

| Variable Name   | Default                                     |
| --------------- |-------------------------------------------- |
| path_prefix     | '.' '(current working directory)            |
| message         | 'chore: Update Python dependencies'         |
| sign-off-commit | true                                        |
| sign-commits    | true                                        |

<!-- markdownlint-enable MD013 -->

## Token Permissions

The token passed as input requires:

- id-token: write
- pull-requests: write
- repository-projects: write
- contents: write

## Implementation Details

For PDM based projects, leverages the following upstream action:

[pdm-project/update-deps-action](https://github.com/pdm-project/update-deps-action)

Invoked when either "tool.pdm" declared in the pyproject.toml file, or when a
"pdm.lock" file exists in the repository.

For projects containing a "Pipfile", will invoke the command:

`pipenv lock`

In both cases, the following upstream action raises a pull request:

[peter-evans/create-pull-request](https://github.com/peter-evans/create-pull-request)
