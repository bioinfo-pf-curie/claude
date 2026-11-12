# claude

Claude configuration template: personal `CLAUDE.md`, skills, sub-agents and a `settings.json` example.

## Layout

```
.
├── skills/                          # custom skills (one folder per skill, with SKILL.md)
├── agents/                          # custom sub-agents (one .md file per agent)
└── settings/
    ├── README.md                    # role and good practices for CLAUDE.md and settings.json
    ├── CLAUDE.md                    # personal instructions loaded in every session
    └── settings.example.json        # example settings: privacy env vars and permission rules
```

## Install

- Symlink or copy `settings/CLAUDE.md` to `~/.claude/CLAUDE.md`.
- Symlink or copy `skills/*` to `~/.claude/skills/`.
- Symlink or copy `agents/*` to `~/.claude/agents/`.
- Merge `settings/settings.example.json` into `~/.claude/settings.json`.
- Start Claude Code from `~/claude/projects` or from a repository under `~/Gitlab`; `additionalDirectories` grants access to `~/claude/projects` and `~/Gitlab`, anything else requires approval.

Example with symlinks:

```bash
ln -s "$PWD/settings/CLAUDE.md" ~/.claude/CLAUDE.md
```

## Notes

- Never commit credentials, tokens or local overrides (`CLAUDE.local.md`, `.env`, keys); see `.gitignore`.
