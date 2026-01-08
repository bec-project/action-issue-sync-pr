# Sync PR to Project Action

A GitHub Action that synchronizes pull requests to GitHub Projects.

## Usage

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `token` | GitHub token with permissions to manage projects | Yes | - |
| `org` | Organization name | Yes | - |
| `repo` | Repository name | Yes | - |
| `pr-number` | Pull request number | Yes | - |
| `project-number` | Project number to sync to | No | `3` |
| `python-version` | Python version to use | No | `3.11` |
| `script-path` | Path to the pr_issue_sync.py script (relative to action) | No | `scripts/pr_issue_sync/pr_issue_sync.py` |
| `requirements-path` | Path to requirements.txt (relative to action) | No | `scripts/pr_issue_sync/requirements.txt` |

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
      - name: Sync PR to Project
        uses: bec-project/action-issue-sync-pr@v1
        with:
          token: ${{ secrets.ADD_ISSUE_TO_PROJECT }}
          org: 'bec-project'
          repo: 'ophyd_devices'
          pr-number: ${{ github.event.pull_request.number }}
```

### Versioning

This action supports both major version tags and specific version tags:

- **Major version tag (recommended)**: `@v1` - Automatically gets the latest v1.x.x release
- **Specific version tag**: `@v1.0.0` - Pins to an exact version

Using the major version tag is recommended for most use cases as it automatically receives bug fixes and new features while maintaining backward compatibility within the same major version.

### Requirements

This action:
- Bundles the Python scripts needed for syncing PRs to GitHub Projects
- Requires a GitHub token with appropriate permissions stored in repository secrets
- Requires the repository to be checked out (see example workflow above)

## License

BSD 3-Clause License - See [LICENSE](LICENSE) for details.
