# Sync PR to Project Action

A GitHub Action that synchronizes pull requests to GitHub Projects.

## Usage

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `token` | GitHub token with permissions to manage projects | Yes | - |
| `project-number` | Project number to sync to | No | `3` |
| `org` | Organization name | Yes | - |
| `repo` | Repository name | Yes | - |
| `pr-number` | Pull request number | Yes | - |
| `python-version` | Python version to use | No | `3.11` |
| `script-path` | Path to the pr_issue_sync.py script | No | `./.github/scripts/pr_issue_sync/pr_issue_sync.py` |
| `requirements-path` | Path to requirements.txt | No | `./.github/scripts/pr_issue_sync/requirements.txt` |

### Example Workflow

Create a workflow file (e.g., `.github/workflows/sync-pr.yml`):

```yaml
name: Sync PR to Project

on:
  pull_request:
    types: [opened, assigned, unassigned, edited, ready_for_review, converted_to_draft, reopened, synchronize, closed]

jobs:
  sync-project:
    runs-on: ubuntu-latest

    permissions:
      issues: write
      pull-requests: read
      contents: read

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4
        with:
          repository: ${{ github.repository }}
          ref: ${{ github.event.pull_request.head.ref }}

      - name: Sync PR to Project
        uses: bec-project/action-issue-sync-pr@v1
        with:
          token: ${{ secrets.ADD_ISSUE_TO_PROJECT }}
          org: 'bec-project'
          repo: 'ophyd_devices'
          pr-number: ${{ github.event.pull_request.number }}
```

### Requirements

This action requires:
- A Python script at the path specified in `script-path` (default: `./.github/scripts/pr_issue_sync/pr_issue_sync.py`)
- A requirements file at the path specified in `requirements-path` (default: `./.github/scripts/pr_issue_sync/requirements.txt`)
- A GitHub token with appropriate permissions stored in repository secrets

## License

BSD 3-Clause License - See [LICENSE](LICENSE) for details.
