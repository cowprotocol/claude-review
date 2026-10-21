# claude-review

Shared Claude PR review for cowprotocol repositories.

A repository calls the reusable workflow from its own
`.github/workflows/claude-review.yml`, which keeps the `pull_request` /
`issue_comment` triggers, and pins it by commit SHA:

```yaml
jobs:
  review:
    permissions:
      contents: read
      pull-requests: write
      issues: write
      id-token: write
      actions: read
      statuses: write
    uses: cowprotocol/claude-review/.github/workflows/claude-review.yml@<sha>
    secrets:
      CLAUDE_CODE_OAUTH_TOKEN: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
```

What the review checks in a repository belongs in that repository's
`.claude/skills/pr-review/SKILL.md`; the shared prompt tells Claude to follow it.

Inputs: `runner`, `model`, `max_turns`, `extra_allowed_tools`, `mcp_config`
(JSON, `${VAR}` placeholders), `post_review_status`. Secrets:
`CLAUDE_CODE_OAUTH_TOKEN` (required), `MCP_CI_BEARER_STAGING`,
`MCP_CI_BEARER_PROD`, `LINEAR_API_KEY`.

It is a reusable workflow rather than an organization required workflow (like
`commit-verifier`) because required workflows only run on `pull_request` and
`merge_group`: the `@claude` comment re-trigger needs `issue_comment`.
