# NexLink Core - Game Config Data

Community-managed game server configuration data for the [NexLink Core Panel](https://nexlinkcore.com). This repository powers the configuration options, mod mappings, and error detection in the server management panel.

**Anyone can contribute!** Submit a pull request to update configurations, add new mod creatures, flag new map mods, or add startup error patterns. An admin will review and merge your changes.

---

## File Structure

```
game-config-data/
  path-of-titans/
    game-ini/                         # Game.ini settings (split by section)
      01-general-server.yaml
      02-quest-system.yaml
      03-waystone.yaml
      04-chat-communication.yaml
      05-map-navigation.yaml
      06-whitelist-permissions.yaml
      07-growth-survival.yaml
      08-nesting-family.yaml
      09-death-respawn.yaml
      10-critters-burrows.yaml
      11-home-cave.yaml
      12-player-lifecycle-security.yaml
      13-character-management.yaml
      14-weather-system.yaml
      15-water-environmental.yaml
      16-network.yaml
    server-variables-schema.yaml      # Server startup variable schema
    map-mods.txt                      # List of mods that are map replacements
    mod-creatures.csv                 # Mod creature name-to-author mapping
    startup-errors.json               # Console error detection patterns
```

---

## File Formats

### `game-ini/*.yaml` — Game.ini Configuration Schema

Each YAML file defines the settings for one section of the Game.ini configuration editor. Files are numbered to control display order — rename the prefix to reorder sections.

```yaml
# Example: 01-general-server.yaml
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

**Optional fields:** `min`, `max`, `step` (for numbers), `options` (for selects), `repeatable`, `fullWidth`, `dependsOn`

**To add a new setting:** Edit the appropriate section file and add a new entry to the `settings` array.

**To reorder sections:** Rename the number prefix (e.g., rename `05-` to `03-` to move it earlier).

**To add a new section:** Create a new numbered YAML file (e.g., `17-new-section.yaml`) with the same format.

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

Same field types as game-ini files but uses `serverVariables:` as the root key.

---

### `map-mods.txt`

Simple list of mod SKUs that are **map replacement mods** (one per line). These mods appear in the ServerMap dropdown instead of the regular Mods tab.

```
# Mods listed here appear in the ServerMap dropdown
UGC_M_Y250G15EVZ_SK
UGC_M_DYV7XD5EGX_SK
```

**To add a new map mod:** Add the mod's SKU on a new line.

---

### `mod-creatures.csv`

Maps mod creature names to their author/mod pack and SKU. Used by the Growth Calculator to filter creatures by enabled mods.

```csv
creature,author,sku
AbsentiaAcro,Absentia,UGC_M_NV57RV2EJD_SK
Camarasaurus,Ancient Gods,UGC_M_DYV7XQ60GX_SK
```

**To add a new creature:** Add a new row with `CreatureName,AuthorName,SKU`.

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
