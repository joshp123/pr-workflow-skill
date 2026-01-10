# pr-workflow-skill

A Claude Code skill for high-signal PR and commit workflows in agentic AI coding.

## What it does

- Enforces **human-written intent** at the top of every PR (never AI-generated)
- Captures **full prompt history** with timestamps for auditability
- Records **environment metadata** (harness, model, thinking level, terminal, system)
- Guides **surgical commits** (one logical change per commit, no bulk adds)
- Defaults PRs to **draft until tests + review pass**

## Installation

### Claude Code (global skill)

```bash
# Clone to your skills directory
git clone https://github.com/joshp123/pr-workflow-skill.git ~/.claude/skills/pr-workflow-skill
```

Then reference it in your `~/.claude/CLAUDE.md` or project's `AGENTS.md`.

### Manual

Copy the contents to your project's `skills/` directory.

## Usage

The skill activates when creating commits or PRs. It will:

1. **Prompt you for intent** — your own words explaining WHY this change exists
2. **Build the PR body** using the template in `references/pr-human-template.md`
3. **Collect environment metadata** using `scripts/build_pr_body.sh`
4. **Include full prompt history** in table format

## Template structure

```markdown
# Human written summary:
The intent of this change is, as written by a human:
> [your verbatim text here]

_The rest of this PR was written by <MODEL>-<THINKING_LEVEL>, running in the <HARNESS> harness._

# Changes
- [what changed]

# Tests
- [command + result]

# Risks
- [bullet or "None: <reason>"]

# Follow-ups
- [bullet or "None"]

# Prompt History
## Environment
...
## Prompts
| ISO-8601 | Prompt |
| --- | --- |
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
