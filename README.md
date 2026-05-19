# netease-mc-mod-dev

Claude Code skill for developing Minecraft NetEase (网易我的世界) mods with Mod SDK.

## Repository Contents

### Skill Files
| File | Description |
|------|-------------|
| `SKILL.md` | Core skill — mod structure, code templates, conventions, common mistakes |
| `api-events.md` | Complete engine event reference — 50+ events with parameters |
| `api-components.md` | Component API reference — factory methods, signatures, enums |

### Developer Documentation
| Directory | Content | Pages |
|-----------|---------|-------|
| `我的世界开发者文档/mcdocs/` | ModAPI / Apollo / PresetAPI interface docs | 269 |
| `我的世界开发者文档/mcguide/` | Development guides (tutorials, features) | 565 |
| `我的世界开发者文档/mconline/` | Online courses (Addon, network server) | 614 |

Source: [NetEase Minecraft Developer Portal](https://mc.163.com/dev/apidocs.html)

## Installation

```bash
# Clone the repo
git clone https://github.com/MIKE63454/-netease-mc-mod-dev-skill.git

# Copy skill to Claude Code skills directory
mkdir -p ~/.claude/skills/netease-mc-mod-dev
cp SKILL.md api-events.md api-components.md ~/.claude/skills/netease-mc-mod-dev/
```

## Usage

The skill activates automatically when Claude Code detects you're working with NetEase Minecraft mod development — creating behavior packs, writing Mod SDK Python scripts, or asking about mod structure and APIs.

## How It Was Built

Created via TDD (test-driven skill development):
1. **RED** — Ran subagents without the skill to create NetEase mods, documented failures
2. **GREEN** — Wrote skill addressing every observed failure pattern
3. **REFACTOR** — Tested with skill present, verified compliance, closed loopholes

## License

MIT
