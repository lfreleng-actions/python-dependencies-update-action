<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🛠️ Update Python Dependencies

Updates the dependencies of a Python project and raises pull requests in the
repository containing the required changes/updates. Works for projects using
UV, Poetry, PDM project tooling, or those described using a "Pipfile".

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
| update_method   | False    | Tool/method used to update dependencies          |
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
| update_method   | 'auto' (detect and use all available tools) |
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

## update_method Options

| Value   | Description                                         |
| ------- | --------------------------------------------------- |
| auto    | Auto-detect and run all available tools (default)  |
| uv      | Use UV package manager (requires uv.lock)          |
| poetry  | Use Poetry (requires poetry.lock or tool.poetry)   |
| pdm     | Use PDM (requires pdm.lock or tool.pdm)            |
| pip     | Use Pipenv (requires Pipfile)                      |

## Implementation Details

The action supports Python dependency management tools:

**UV**: Modern, fast Python package installer and resolver

- Triggered by: `uv.lock` file exists
- Command: `uv lock --upgrade`

**Poetry**: Popular Python dependency management tool

- Triggered by: `poetry.lock` file exists OR `tool.poetry` in pyproject.toml
- Command: `poetry update`

**PDM**: Modern Python package manager supporting PEP 582

- Triggered by: `pdm.lock` file exists OR `tool.pdm` in pyproject.toml
- Command: `pdm update`

**Pipenv**: Traditional Python package manager using Pipfile

- Triggered by: `Pipfile` exists
- Command: `pipenv lock`

When `update_method: auto` (default), all detected tools run in priority
order. When you specify a tool, that tool executes.

The action consolidates all dependency updates into a single pull request using:

[peter-evans/create-pull-request](https://github.com/peter-evans/create-pull-request)
