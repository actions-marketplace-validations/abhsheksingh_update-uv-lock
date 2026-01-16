
# Update uv.lock Action

Regenerates `uv.lock` and pushes the change to the current branch if it differs.

## Inputs
- `python-version` (default `3.10.7`)
- `repository` (required) – e.g., `your-org/your-repo`
- `ref` (required) – e.g., `${{ github.event.pull_request.head.ref }}`
- `workdir` (default `.`)

## Example (on PR)
```yaml
- uses: abhsheksingh/update-uv-lock@v1
  with:
    python-version: '3.10.7'
    repository: ${{ github.event.pull_request.head.repo.full_name }}
    ref: ${{ github.event.pull_request.head.ref }}
    workdir: '.'
```
## Use it in your target repo (one‑liner in a PR workflow)
Create or update a workflow in the repository where you want to auto‑refresh uv.lock on PRs:
.github/workflows/refresh-uv-lock.yml
```yaml

name: Refresh uv.lock on PR

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: write
  pull-requests: write

jobs:
  refresh-uv-lock:
    runs-on: ubuntu-latest
    if: ${{ github.event.pull_request.head.repo.full_name == github.repository }}
    steps:
      - uses: abhsheksingh/update-uv-lock@v1
        with:
          python-version: '3.10.7'
          repository: ${{ github.event.pull_request.head.repo.full_name }}
          ref: ${{ github.event.pull_request.head.ref }}
          workdir: '.'

```
## Test

Open a PR that changes `pyproject.toml` or dependencies; the workflow will run and, if `uv.lock` changes, you’ll see an extra commit pushed by `github-actions`.

