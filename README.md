# NexLink Core - Game Config Data

Community-managed game server configuration data for the [NexLink Core Panel](https://nexlinkcore.com). This repository powers the configuration options, mod mappings, and error detection in the server management panel.

**Anyone can contribute!** Submit a pull request to update configurations, add new mod creatures, flag new map mods, or add startup error patterns. An admin will review and merge your changes.

---

## File Structure

```
game-config-data/
  path-of-titans/
    game-ini-schema.yaml          # Game.ini configuration schema
    server-variables-schema.yaml  # Server startup variable schema
    map-mods.yaml                 # List of mods that are map replacements
    mod-creatures.json            # Mod creature name-to-author mapping
    startup-errors.json           # Console error detection patterns
```

---

## File Formats

### `game-ini-schema.yaml`

Defines available settings for the Game.ini configuration editor in the panel. Each setting maps to an INI property on the game server.

```yaml
settings:
  - property: ServerMap          # INI property name
    label: Server Map            # Display name in the UI
    type: select                 # Input type: select, number, text, checkbox
    default: Island              # Default value
    helpText: The map to use     # Help text shown to users
    section: "[/Script/PathOfTitans.IGameSession]"  # INI section
    accordion: General Server Settings  # UI grouping
    accordionOrder: 0            # Group display order
    order: 0                     # Setting display order within group
    options:                     # For select type only
      - Island
      - Panjura
```

**Supported types:** `select`, `number`, `text`, `checkbox`

**Optional fields:** `min`, `max`, `step` (for numbers), `options` (for selects), `repeatable`, `fullWidth`

---

### `server-variables-schema.yaml`

Defines server startup variables (like server name, slots, password).

```yaml
serverVariables:
  - property: SERVER_NAME        # Variable name
    label: Server Name           # Display name
    type: text                   # text or number
    default: My Server           # Default value
    helptext: Your server name   # Help text
    accordion: Server Settings   # UI grouping
    accordionOrder: 0
    order: 0
```

Same field types as `game-ini-schema.yaml` but uses `serverVariables:` as the root key.

---

### `map-mods.yaml`

Simple list of mod SKUs that are **map replacement mods**. These mods appear in the ServerMap dropdown instead of the regular Mods tab.

```yaml
# Mods listed here appear in the ServerMap dropdown
# instead of the regular Mods tab
mapMods:
  - UGC_M_Y250G15EVZ_SK
  - UGC_M_DYV7XD5EGX_SK
```

**To add a new map mod:** Add the mod's SKU (found in-game or in `GameUserSettings.ini`) to the list.

---

### `mod-creatures.json`

Maps mod creature names to their author/mod pack and SKU. Used by the Growth Calculator to filter creatures by enabled mods.

```json
{
  "AbsentiaAcro": {
    "author": "Absentia",
    "sku": "UGC_M_NV57RV2EJD_SK"
  },
  "Camarasaurus": {
    "author": "Ancient Gods",
    "sku": "UGC_M_DYV7XQ60GX_SK"
  }
}
```

**Fields:**
- Key = creature name (exact match, case-sensitive)
- `author` = mod creator/brand name
- `sku` = mod SKU identifier (links creature to its mod)

**To add a new creature:** Add an entry with the creature name as the key. The `sku` field links it to the correct mod in the Growth Calculator.

---

### `startup-errors.json`

Patterns matched against server console output during startup. When a match is found, users see the configured warning/error message.

```json
[
  {
    "id": "pot-restricted-mod",
    "patternSource": "Mod .* is restricted",
    "message": "A restricted mod was detected. Remove it from your mod list.",
    "severity": "error",
    "helpUrl": "https://docs.example.com/restricted-mods",
    "games": ["path-of-titans"]
  }
]
```

**Fields:**
- `id` - Unique identifier (use `pot-` prefix for Path of Titans)
- `patternSource` - Regex pattern to match in console output (escape special characters)
- `message` - User-friendly message shown when pattern matches
- `severity` - `"error"`, `"warning"`, or `"info"`
- `helpUrl` - (optional) Link to documentation or help page
- `games` - Array of game IDs this pattern applies to

---

## Contributing

1. **Fork** this repository
2. **Edit** the relevant file(s)
3. **Submit a pull request** with a description of your changes
4. An admin will review and merge

### Guidelines

- Keep YAML files properly indented (2 spaces)
- Keep JSON files valid (use a linter if unsure)
- For mod creatures, use the exact creature name as it appears in-game
- For startup errors, test your regex pattern against real console output
- One change per pull request when possible (easier to review)

### Finding SKUs

Mod SKUs follow the pattern `UGC_M_<CODE>_SK`. You can find them:
- In your `GameUserSettings.ini` under `[ServerGameMode]`
- In the mod details on the Alderon Games mod browser
- In the server's Mods tab in the panel

---

## How It Works

The [NexLink Core Panel](https://nexlinkcore.com) fetches these files directly from this repository (via `raw.githubusercontent.com`). Changes are cached for up to 1 hour, and admins can manually refresh the cache from the panel.

This means your contributions are live within an hour of being merged, with no code deployment needed.
