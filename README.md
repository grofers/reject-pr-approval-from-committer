# GitHub Action: do not allow committers on a Pull Request to approve the PR

**Name:** `peckjon/reject-pr-approval-from-committer`

(This is a fork of `handy-common-utils/conditionally-approve-pr-from-trusted-committers`)

Do not allow a committer on a GitHub pull request to approve the PR.

## Usage instructions

Create a workflow file (e.g. `.github/workflows/reject-self-approve.yml`) that contains a step 
that has `uses: peckjon/reject-pr-approval-from-committer`.

The workflow using this action is supposed to be triggered by `pull_request` or `pull_request_target` event.

Here's an example workflow file:

```yaml
name: Prevent committers from approvaing a PR

on:
  pull_request_review:
    types: [submitted]

jobs:
  preventapprove:
    name: reject PR approval by committers to the PR
    runs-on: ubuntu-latest
    if: github.event.review.state == 'approved'
    steps:
      - name: Dismiss code reviews from collaborators
        uses: peckjon/reject-pr-approval-from-committer@master
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Why?

While GitHub prevents self-approval of PRs, it doesn't prevent a user from approving a PR they worked on. In some scenarios, this could allow for PRs to be approved with no outside review.
