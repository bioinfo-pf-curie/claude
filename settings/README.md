# settings

This folder holds the two files that shape how Claude Code behaves for one user:

| File | Role | Nature |
|------|------|--------|
| `CLAUDE.md` | Instructions: who I am, how I work, what Claude must not do | Guidance, read by the model |
| `settings.example.json` | Permissions, environment variables, retention | Enforced by the Claude Code harness |

The key difference: `CLAUDE.md` is advice the model follows, `settings.json` is a rule the tool applies.
Anything that must never happen (reading keys, pushing code) belongs in `settings.json`, not only in `CLAUDE.md`.

## CLAUDE.md

### Role

`CLAUDE.md` is loaded into the context at the start of every session. It tells Claude:
- who the user is and the working language;
- the expected style (concise, recommendation-first, sources cited);
- the actions that require explicit approval;
- confidentiality and memory rules.

### Where it is read from

| Location | Scope |
|----------|-------|
| `~/.claude/CLAUDE.md` | User: every project on this machine |
| `<project>/CLAUDE.md` | Project: shared with the team through git |
| `<project>/CLAUDE.local.md` | Project, personal: never committed (see `.gitignore`) |

Files are combined; the more specific one wins on conflicts.

### Good practices

- Keep it short (one or two screens). Every line costs context in every session.
- Write rules, not wishes: "Ask before `git push`" rather than "be careful with git".
- One rule per bullet. A broken or merged bullet changes its meaning.
- Avoid contradictions (e.g. "work only in X" next to "ask before leaving X").
- Never put secrets, tokens, patient data or evaluations in it.
- Keep personal details out of the shared copy (use `XXX` placeholders, as here).
- Review it when Claude repeatedly does something you correct: add the rule once.
- Do not rely on it for security: duplicate critical rules as `deny` or `ask` permissions.

## settings.json

### Where it is read from

| Location | Scope |
|----------|-------|
| `~/.claude/settings.json` | User: every project |
| `<project>/.claude/settings.json` | Project: shared through git |
| `<project>/.claude/settings.local.json` | Project, personal: not committed |

Project settings override user settings. For permissions, `deny` always wins over `ask`, which wins over `allow`.

### Options used in the example

| Key | Effect |
|-----|--------|
| `cleanupPeriodDays` | Local conversation transcripts older than N days are deleted at startup (default 30). |
| `env.CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | Disables non-essential network traffic: telemetry, error reporting, `/bug`, auto-updates. Update Claude Code manually. |
| `env.DISABLE_TELEMETRY` | Disables anonymous usage statistics (redundant with the line above, kept for clarity). |
| `env.DISABLE_ERROR_REPORTING` | Disables crash reports (redundant with the line above, kept for clarity). |
| `permissions.additionalDirectories` | Folders Claude may work in besides the launch folder. Access elsewhere requires approval. |
| `permissions.deny` | Tool calls that are always refused (e.g. reading `~/.ssh`, `.env`, keys). |
| `permissions.ask` | Tool calls that always require confirmation (e.g. `git push`, `rm`, network transfers). |

None of these settings stop prompts and the files Claude reads from being sent to the model: that is how Claude works.
Keep sensitive data out of the folders Claude can access.

### Good practices

- Deny reads of credentials and sensitive folders explicitly; do not rely on `CLAUDE.md` for that.
- Put irreversible or outward-facing actions in `ask` (push, delete, transfer, job submission).
- Allow frequent read-only commands (`git status`, `git diff`, `ls`) to avoid needless prompts.
- Never copy your real `~/.claude/settings.json` into a shared repository without reviewing it.
- Validate JSON after editing: `python -m json.tool settings.example.json`.
