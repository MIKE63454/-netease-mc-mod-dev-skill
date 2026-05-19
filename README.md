# netease-mc-mod-dev

Claude Code skill for developing Minecraft NetEase (网易我的世界) mods with Mod SDK. Includes curated API references and complete developer documentation.

## Repository Contents

### Skill Files (3 files, ~950 lines)

| File | Lines | Description |
|------|-------|-------------|
| `SKILL.md` | 439 | Core skill — mod structure, code templates, conventions, patterns, troubleshooting |
| `api-events.md` | 129 | 50+ engine events with trigger conditions and parameters |
| `api-components.md` | 379 | 15 factory components, all method signatures, enums, system APIs |

**Skill covers:**
- Mod structure (behavior pack, resource pack, directory conventions)
- `modMain.py` entry point with `@Mod.Binding`, lifecycle decorators
- `ServerSystem` / `ClientSystem` templates with event-driven patterns
- 8 common patterns: custom events (Server↔Client), tick loops, data persistence, custom items/blocks
- Component API: Item, Msg, Player, Game, Command, EntityEvent, ExtraData, Scoreboard
- Critical conventions: Python 2, server/client isolation, `RegisterSystem` string path
- 16 troubleshooting entries with symptoms, causes, and fixes

### Developer Documentation (1,448 pages)

| Directory | Content | Pages |
|-----------|---------|-------|
| `我的世界开发者文档/mcdocs/` | ModAPI / Apollo / PresetAPI interface docs | 269 |
| `我的世界开发者文档/mcguide/` | Development guides (tutorials, features) | 565 |
| `我的世界开发者文档/mconline/` | Online courses (Addon, network server) | 614 |

Source: [NetEase Minecraft Developer Portal](https://mc.163.com/dev/apidocs.html)

**Cold API fallback:** When the skill's built-in references don't cover an API, the skill instructs AI to fetch specific `.md` files from this repo via `raw.githubusercontent.com`.

## Installation

```bash
# Clone the repo (includes both skill and full dev docs)
git clone https://github.com/MIKE63454/-netease-mc-mod-dev-skill.git

# Copy skill to Claude Code skills directory
mkdir -p ~/.claude/skills/netease-mc-mod-dev
cp SKILL.md api-events.md api-components.md ~/.claude/skills/netease-mc-mod-dev/
```

## Usage

The skill activates automatically when you work with NetEase Minecraft mod development — creating behavior packs, writing Mod SDK Python scripts, or asking about mod structure and APIs. Type requests like "帮我写一个XXX功能的模组" directly.

## How It Was Built

Created via TDD (test-driven skill development):
1. **RED** — Ran subagents without the skill, documented every failure pattern
2. **GREEN** — Wrote skill addressing every observed failure
3. **REFACTOR** — Tested with skill present, verified compliance, closed loopholes
4. **Iterated 4 rounds** of doc-auditing to fill API gaps found in the 1,448-page source docs

## License

MIT
