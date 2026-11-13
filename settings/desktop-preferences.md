# Claude Desktop preferences

Claude Desktop (Chat mode) and claude.ai do not read `CLAUDE.md` or `settings.json`.
Their configuration lives in your claude.ai account, set through the app: it follows you on the web and on mobile,
but it is neither versioned nor enforced.

This file keeps the text to paste, so that it can be versioned and shared like the rest of this repository.

## Where to set what

| Setting | Where in the app | Equivalent in Claude Code |
|---|---|---|
| Personal preferences | Settings > Profile | `~/.claude/CLAUDE.md` |
| Memory, code execution, skills upload | Settings > Capabilities | `memory/` folders, `~/.claude/skills/` |
| Connectors (mail, Drive, calendar…) | Settings > Connectors | MCP servers in `settings.json` |
| Topic-specific instructions and files | Inside each Project | `<project>/CLAUDE.md` |
| Permissions (`deny`, `ask`) | None | `settings.json` |

Some settings (allowed connectors, enabled features) are set by the organisation's administrator.

The only local file, `claude_desktop_config.json` (on Linux usually in `~/.config/Claude/`), declares local
MCP servers. It does not hold your preferences.

## Text to paste in Settings > Profile

Keep it short: it is sent with every conversation. Replace the `XXX` placeholders.

```text
I am XXX, XXX at XXX, working in XXX.
Reply in French. Write documents in English unless I ask otherwise or the recipient is French-speaking.
Get straight to the point: no rephrasing of my question, no closing recap.
If a request is ambiguous and the stakes are high, ask one targeted question; otherwise pick the most reasonable option and say so.
Give a recommendation, not a catalogue of options.
Say explicitly when you are unsure of a fact. Never invent a bibliographic reference; cite your sources.
Professional and scientific tone, short sentences, no marketing hype.
If you see data that looks identifying, medical, individual-level genomic or personal, stop and tell me instead of processing it.
Never send, publish or share anything without my explicit approval.
```

## Project instructions template

Paste in a Project's instructions and adapt:

```text
This Project is about XXX (goal, deadline, audience).
Reference files: XXX. Treat them as the source of truth.
Expected outputs: XXX (format, length, language).
Out of scope / do not: XXX.
```

## Checklist

- [ ] Profile preferences pasted and adapted.
- [ ] Memory: enabled or disabled deliberately; reviewed monthly.
- [ ] Skills from `skills/` zipped and uploaded if needed in Chat.
- [ ] Only the connectors you actually use are enabled.
- [ ] One Project per recurring topic, with instructions from the template above.
