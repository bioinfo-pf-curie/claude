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
| `env.DISABLE_TELEMETRY` | Disables anonymous usage statistics. |
| `env.DISABLE_ERROR_REPORTING` | Disables crash reports. |
| `permissions.additionalDirectories` | Folders Claude may work in besides the launch folder: `~/claude/projects` and `~/Gitlab`. |
| `permissions.blockReadsOutsideWorkingDirectories` | Blocks file reads outside the launch folder and `additionalDirectories`. |
| `permissions.disableBypassPermissionsMode` | `"disable"` prevents starting Claude in "bypass permissions" mode. |
| `permissions.allow` | Frequent read-only commands run without a prompt (`git status`, `git diff`, `git log`, `nextflow config`). |
| `permissions.deny` | Always refused: reading credentials (`~/.ssh`, `~/.gnupg`, `~/.aws`, `.env*`, `*.pem`, `*.key`), `~/donnees-sensibles/`, and `sudo`. |
| `permissions.ask` | Always confirmed: `git push`, `git reset --hard`, `git clean`, `rm`, transfers (`scp`, `rsync`, `ssh`, `curl`, `wget`), `docker push`, cluster jobs (`sbatch`, `srun`, `qsub`). |

About `blockReadsOutsideWorkingDirectories`: reads outside `~/claude/projects` and `~/Gitlab` are refused outright,
Claude does not ask for approval. If you occasionally need another folder, type `/add-dir <path>` in the session.

Rule behaviour:
- Precedence is `deny` > `ask` > `allow`.
- A `Read` deny rule also blocks edits of the same path.
- Bash rules use the space form `Bash(cmd *)`; the older `Bash(cmd:*)` form is equivalent.
- `WebFetch` is left unrestricted on purpose, to keep documentation searches fluid.

None of these settings stop prompts and the files Claude reads from being sent to the model: that is how Claude works.
Keep sensitive data out of the folders Claude can access.

### Not enabled on purpose: `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`

This variable disables all nonessential traffic: auto-updates, telemetry, error reporting, `/feedback` and feature-flag fetching.
It therefore makes Remote Control unavailable (driving a local session from claude.ai or the mobile app).
We keep Remote Control, so only `DISABLE_TELEMETRY` and `DISABLE_ERROR_REPORTING` are set.
Enable it if you never use Remote Control, and then update Claude Code manually.

### Optional: sandbox

```json
"sandbox": { "enabled": true }
```

Permission `deny` rules cover Claude's file tools and the commands it recognises (`cat`, `head`, `sed`, redirections),
but not a script that opens files by itself. The sandbox enforces filesystem and network limits at OS level for Bash commands.
On Linux it requires bubblewrap; the `/sandbox` command enables it interactively.

### Good practices

- Deny reads of credentials and sensitive folders explicitly; do not rely on `CLAUDE.md` for that.
- Put irreversible or outward-facing actions in `ask` (push, delete, transfer, job submission).
- Allow frequent read-only commands (`git status`, `git diff`, `ls`) to avoid needless prompts.
- Never copy your real `~/.claude/settings.json` into a shared repository without reviewing it.
- Validate JSON after editing: `python -m json.tool settings.example.json`.
