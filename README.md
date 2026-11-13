# claude

Claude configuration template: personal `CLAUDE.md`, skills, sub-agents and a `settings.json` example.

## Layout

```
.
├── skills/                          # custom skills (one folder per skill, with SKILL.md)
├── agents/                          # custom sub-agents (one .md file per agent)
├── templates/                       # reusable text templates (e.g. AI_DISCLOSURE.md)
└── settings/
    ├── README.md                    # role and good practices for CLAUDE.md and settings.json
    ├── CLAUDE.md                    # personal instructions loaded in every session
    └── settings.example.json        # example settings: privacy env vars and permission rules
```

## Where this configuration applies

The files installed in `~/.claude/` (CLAUDE.md, settings.json, skills, agents) are read by **Claude Code only**.

| Interface | Reads `~/.claude/` | Works on local files |
|-----------|:---:|:---:|
| Claude Code CLI (`claude` in a terminal) | yes | yes |
| Claude Code extension for VS Code / JetBrains | yes | yes |
| Claude Desktop, **Code** tab | yes | yes |
| Claude Desktop **Chat** tab and projects, claude.ai | no | no |
| Claude Code on the web (claude.ai/code) | no | no (cloud, GitHub repository) |

- In the Chat tab and on claude.ai, `settings.json` rules do not apply and only the skills of your claude.ai account are available.
  To use a skill from this repo there, zip its folder and upload it in the app settings (Capabilities / Skills).
- Claude Code on the web runs in the cloud: do not use it for confidential manuscripts or data.
- Start the CLI from an authorized folder (`cd ~/claude/projects && claude`), call a skill with `/<skill-name>`
  or just describe the task, check the setup with `/doctor`, update with `claude update`.

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
