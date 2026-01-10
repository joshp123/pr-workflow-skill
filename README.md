# pr-workflow-skill

A skill for high-signal PR and commit workflows in agentic AI coding. Works with your coding agent of choice.

## What it does

- Enforces **human-written intent** at the top of every PR (never AI-generated)
- Captures **full prompt history** with timestamps for auditability
- Records **environment metadata** (harness, model, thinking level, terminal, system)
- Guides **surgical commits** (one logical change per commit, no bulk adds)
- Defaults PRs to **draft until tests + review pass**

## Installation

Clone to your agent's skills directory or your project:

```bash
# Global install (adjust path for your agent)
git clone https://github.com/joshp123/pr-workflow-skill.git ~/skills/pr-workflow-skill

# Or add to a project
git clone https://github.com/joshp123/pr-workflow-skill.git ./skills/pr-workflow-skill
```

Then reference it in your project's `AGENTS.md` or your agent's global config.

## Usage

The skill activates when creating commits or PRs. It will:

1. **Prompt you for intent** — your own words explaining WHY this change exists
2. **Build the PR body** using the template in `references/pr-human-template.md`
3. **Collect environment metadata** using `scripts/build_pr_body.sh`
4. **Include full prompt history** in table format

## Example PR

```markdown
# Human written summary:
The intent of this change is, as written by a human:
> I want to add rate limiting to the API so we don't get hammered by bots.
> Should be configurable per-endpoint.

_The rest of this PR was written by gpt-5.2-codex-high, running in the codex harness. Full environment + prompt history appear at the end._

# Changes
- Add `RateLimiter` middleware with per-endpoint config
- Add `rate_limits.yaml` for endpoint-specific limits
- Wire up middleware in `server.go`
- Add tests for rate limiting behavior

# Tests
- `go test ./...` — ok (47 passed)
- `curl -X POST localhost:8080/api/submit` x100 — 429 after 10 requests ✓

# Risks
- None: additive change, existing endpoints unaffected until configured

# Follow-ups
- Add Redis backend for distributed rate limiting
- Dashboard for monitoring rate limit hits

# Prompt History

## Environment
Harness: codex
Model: gpt-5.2-codex
Thinking level: high
Terminal: ghostty 1.0.0
System: macOS 15.2

## Prompts
| ISO-8601 | Prompt |
| --- | --- |
| 2025-01-10T14:23:11Z | ``add rate limiting to the api, configurable per endpoint`` |
| 2025-01-10T14:31:45Z | ``use a yaml config file for the limits, not hardcoded`` |
| 2025-01-10T14:35:02Z | ``add tests`` |
```

## Files

- `SKILL.md` — skill entry point
- `references/workflow-commit.md` — commit workflow steps
- `references/workflow-pr.md` — PR workflow steps
- `references/pr-human-template.md` — the PR template
- `references/commit-format.md` — commit message format
- `scripts/build_pr_body.sh` — environment metadata collector

## License

MIT
