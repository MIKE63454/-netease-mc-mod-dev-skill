# netease-mc-mod-dev

Claude Code skill for developing Minecraft NetEase (网易我的世界) mods with Mod SDK.

## What This Skill Covers

- **Mod structure** — behavior pack, resource pack, directory conventions
- **modMain.py** — entry point with `@Mod.Binding`, lifecycle decorators
- **ServerSystem / ClientSystem** — event-driven system classes
- **Component APIs** — factory pattern `GetEngineCompFactory()` and legacy `CreateComponent()`
- **Event reference** — 8 server-side + 3 client-side commonly used events
- **Critical conventions** — Python 2, server/client isolation, `RegisterSystem` string path
- **Common mistakes** — 8 frequent errors with fixes

## Installation

Copy the skill to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/netease-mc-mod-dev
cp SKILL.md ~/.claude/skills/netease-mc-mod-dev/
```

## Usage

The skill activates automatically when Claude Code detects you're working with NetEase Minecraft mod development — creating behavior packs, writing Mod SDK Python scripts, or asking about mod structure and APIs.

## Example Mod

See the companion [WelcomeMod](https://github.com/hcw217/netease-mc-mod-dev-skill) for a complete, importable `.mcaddon` example mod built with this skill's patterns.

## License

MIT
