# Pterodactyl Egg - Minecraft Vanilla Bedrock Dedicated Server

A fully updated Pterodactyl egg for Minecraft Bedrock Dedicated Server with reliable API-based version fetching, preview channel support, and expanded configuration variables.

## Features

- **Reliable downloads** — uses the official Minecraft API (`net-secondary.web.minecraft-services.net`) to fetch download URLs instead of scraping HTML pages that break
- **Preview channel** — select `preview` from the dropdown to get the latest preview build
- **Custom versions** — select `custom` and enter any exact version (e.g. `1.26.33.2`)
- **Expanded variables** — more server.properties options exposed in the panel
- **Config backup/restore** — preserves `server.properties`, `permissions.json`, and `allowlist.json` across updates

## Variables

| Variable | Default | Description |
|---|---|---|
| `BEDROCK_VERSION_TYPE` | `latest` | Version type: `latest`, `preview`, or `custom` |
| `BEDROCK_VERSION_CUSTOM` | *(empty)* | Exact version when type is `custom` (e.g. `1.26.33.2`) |
| `SERVERNAME` | `Bedrock Dedicated Server` | Server name shown in the server list |
| `GAMEMODE` | `survival` | Default game mode (`survival`, `creative`, `adventure`) |
| `DIFFICULTY` | `easy` | World difficulty (`peaceful`, `easy`, `normal`, `hard`) |
| `CHEATS` | `false` | Allow cheats/commands |
| `MAX_PLAYERS` | `10` | Maximum player count |
| `ONLINE_MODE` | `true` | Require Xbox Live authentication |
| `VIEW_DISTANCE` | `32` | Render distance in chunks |
| `FORCE_GAMEMODE` | `false` | Force players to default gamemode |
| `TICK_DISTANCE` | `10` | Simulation distance in chunks |
| `TEXTUREPACK_REQUIRED` | `false` | Require resource pack acceptance |

## Installation

1. Download [`egg-vanilla-bedrock.json`](./egg-vanilla-bedrock.json)
2. In your Pterodactyl panel, go to **Admin → Nests → Import Egg**
3. Upload the JSON file and select the target nest
4. Create a new server using the imported egg

## Credits

Based on [parkervcp/eggs](https://github.com/parkervcp/eggs) with fixes for the broken version scraping and additional variables.
