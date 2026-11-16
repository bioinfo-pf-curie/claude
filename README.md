# Claude good practices

Guidelines, templates and reusable skills for using Claude well, on your own and as a team: which tool to pick,
how to ask, how to configure it safely, how to share work with colleagues, and how to stay in control of what
it produces.

**One rule before anything else: sharing sensitive data with Claude is strictly forbidden.**

## Where to start

| You… | Read |
|---|---|
| are new to Claude, or use it on your own | [guidelines/individual-use.md](guidelines/individual-use.md) |
| work with colleagues on shared projects | [guidelines/team-use.md](guidelines/team-use.md) (after the individual guide) |
| use Claude Desktop or claude.ai (Chat mode) | [settings/desktop-preferences.md](settings/desktop-preferences.md) |
| use Claude Code (terminal, IDE or the Code tab of Desktop) | [settings/README.md](settings/README.md) |
| repeat the same kind of task and want Claude to follow your method | [skills/](skills/) |
| publish code or documents produced with the help of AI | [templates/AI_DISCLOSURE.md](templates/AI_DISCLOSURE.md) |

## Layout

```
.
├── guidelines/   # good practices: individual use, team use
├── settings/     # configuration templates for Claude Desktop and Claude Code
├── skills/       # reusable skills, one folder per skill
├── agents/       # specialised sub-agents
└── templates/    # reusable text templates (e.g. AI disclosure)
```

## Contributing

Propose a new skill, template or guideline change through a merge request. Keep everything generic:
no personal details, no internal paths, no credentials.

## AI Disclosure

This repository was written with the help of AI (Claude).

- **Process:** most guidelines, templates and skills were drafted by Claude from detailed instructions,
  then reviewed, corrected and restructured by the maintainers over several iterations.
- **Human role:** scope, structure, rules and final wording were decided by the maintainers; every file was reviewed
  before being committed.
- **Responsibility:** the maintainers take full responsibility for the content.
