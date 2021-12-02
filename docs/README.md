# Github Workflows

[Github Actions](https://docs.github.com/en/actions) reusable workflows

## Content

- [Setup](#setup)
	- [Inputs](#inputs)
- [Workflows](#workflows)
	- [Lock closed issues and PRs](#lock-closed-issues-and-prs)

## Setup

Setup any workflow with few simple steps:

- Create `.github/workflows` directory
- Create file `workflow-name.yaml` in the directory
- Copy one of following workflows into the file
- Set required inputs (if workflow has any)
- Set `on` run condition (or use our recommended defaults)
- Good to go!

### Inputs

Certain workflows can be configured through inputs. Inputs are defined via `with` key.

```yaml
- name: "example"
  uses: "example/action@v1.0"
  with:
    input-name: "Value"
```

## Workflows

### Lock closed issues and PRs

Lock inactive closed issues and PRs after period of time.

`lock-closed-issues-and-prs.yaml`:

```yaml
name: "Lock inactive closed issues and PRs"

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  lock:
    uses: "orisai/github-workflows/.github/workflows/lock-closed-issues-and-prs.yaml@v1.x"
```

Inputs:

`issue-lock-inactive-days`

- type: `string`
- default: `"30"`

`pr-lock-inactive-days`

- type: `string`
- default: `"60"`
