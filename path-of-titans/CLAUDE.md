# Claude Code Instructions — NexLink Core Game Config Data

> **Repository:** Community-managed game server configuration files
> **Used by:** [NexLink Core Panel Control](https://nexlinkcore.com) (Next.js app)
> **Format:** YAML + JSON files defining game settings, mod mappings, and error patterns

---

## Overview

This repo contains configuration data that the NexLink Core panel pulls at runtime via `raw.githubusercontent.com`. The panel caches files in Redis (1-hour TTL). Changes merged here go live automatically — no deployment needed.

---

## File Reference

### `path-of-titans/game-ini-schema.yaml`
- **Root key:** `settings:`
- **Purpose:** Defines every setting available in the Game.ini visual editor
- **Each entry has:** `property`, `label`, `type` (select/number/text/checkbox), `default`, `helpText`, `section` (INI section), `accordion` (UI group), `accordionOrder`, `order`
- **Optional fields:** `min`, `max`, `step`, `options` (array for selects), `repeatable`, `fullWidth`, `dependsOn`

### `path-of-titans/server-variables-schema.yaml`
- **Root key:** `serverVariables:`
- **Purpose:** Defines server startup variables (name, slots, password, etc.)
- **Same field structure** as game-ini-schema but for Pterodactyl environment variables

### `path-of-titans/map-mods.yaml`
- **Root key:** `mapMods:`
- **Purpose:** List of mod SKU strings that are map replacement mods
- **Format:** Simple YAML array of strings like `UGC_M_Y250G15EVZ_SK`
- Map mods appear in the ServerMap dropdown instead of the regular Mods tab

### `path-of-titans/mod-creatures.json`
- **Format:** JSON object — keys are creature names, values are `{ "author": "...", "sku": "..." }`
- **Purpose:** Maps mod creatures to their creator and mod SKU for the Growth Calculator
- Creature names are **case-sensitive** and must match exactly as they appear in-game
- `sku` links the creature to its parent mod

### `path-of-titans/startup-errors.json`
- **Format:** JSON array of error pattern objects
- **Each entry has:** `id` (unique, use `pot-` prefix), `patternSource` (regex string), `message`, `severity` (error/warning/info), `helpUrl` (optional), `games` (array, use `["path-of-titans"]`)
- **Purpose:** Matched against server console output during startup to detect issues

---

## Rules

1. **YAML files use 2-space indentation** — never tabs
2. **JSON files must be valid** — always validate before committing
3. **Never remove existing entries** without explicit approval — other servers depend on them
4. **Creature names are case-sensitive** — match exactly as they appear in the game
5. **SKU format:** `UGC_M_<ALPHANUMERIC>_SK` — find in GameUserSettings.ini or the Alderon mod browser
6. **Regex patterns in startup-errors.json** — escape special characters with `\\` (e.g., `\\.` for a literal dot)
7. **One logical change per PR** — don't mix adding creatures with changing error patterns
8. **Keep entries sorted** — mod-creatures.json alphabetically by key, settings by accordion/order

---

## Adding a New Game

To add configuration for a new game:
1. Create a new folder (e.g., `ark-survival/`)
2. Follow the same file naming patterns
3. The panel will need code changes to support the new game — coordinate with the panel repo

---

## Testing Changes

- **YAML:** Use any YAML validator (e.g., `yamllint` or an online validator)
- **JSON:** Use `jq . filename.json` or any JSON linter
- **Regex patterns:** Test at https://regex101.com with the `i` flag (case-insensitive)
- Changes are picked up by the panel within 1 hour of merge, or instantly via the admin "Refresh from GitHub" button
