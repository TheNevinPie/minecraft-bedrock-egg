# Pterodactyl Egg - Minecraft Vanilla Bedrock Dedicated Server

An updated Pterodactyl egg for Minecraft Bedrock Dedicated Server with automatic version fetching, preview builds, custom versions, and expanded server configuration.

## Features

- **Reliable downloads** — uses the official Minecraft API (`net-secondary.web.minecraft-services.net`) to fetch download URLs instead of scraping HTML pages that break
- **Preview channel** — select `preview` from the dropdown to get the latest preview build
- **Custom versions** — select `custom` and enter any exact version (e.g. `1.26.33.2`)
- **Expanded variables** — more server.properties options exposed in the panel
- **NetherNet support** — `transport` and `server-udp-ports` variables for the WebRTC-based transport (BDS default since 1.26.5x), with NAT/Docker port-mapping forms documented
- **Config backup/restore** — preserves `server.properties`, `permissions.json`, `allowlist.json`, and the `keys/` server identity key across updates

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
| `TRANSPORT` | `nethernet` | Network transport (`nethernet` WebRTC, or `raknet` for old clients) |
| `SERVER_UDP_PORTS` | *(empty)* | NetherNet P2P UDP ports (see below) |
| `SERVER_PORT_V6` | `19133` | IPv6 port (RakNet mode only) |
| `LAN_VISIBILITY` | `false` | Respond to LAN discovery (keep off on shared hosts) |

## NetherNet notes

Bedrock Dedicated Server defaults to the WebRTC-based NetherNet transport. Each server needs a UDP port **range** for client peer-to-peer connections, not just its primary port:

1. Create extra panel allocations covering the range (one per port, e.g. `32100-32149` for a 10-player server) and assign them all to the server.
2. Set `SERVER_UDP_PORTS`:
   - `32100-32149` — pin local ports (same ports must be open in the host firewall/security group)
   - `203.0.113.10:32100-32149:32100-32149` — Docker/NAT mapping form: bind container ports, advertise them on the public IP (required behind Docker bridge networking, where the container's own addresses are unreachable)
3. Ranges must not overlap between servers on the same node.
4. Leaving `SERVER_UDP_PORTS` empty uses OS ephemeral ports — works behind stateful firewalls (AWS SGs, ufw), but pin a range for strict firewalls.
5. `server-portv6` is ignored under NetherNet (single dual-stack socket on the main port).
6. Keep `TRANSPORT=raknet` as a fallback for players on old clients that cannot speak WebRTC.
7. The `keys/` folder holds the server identity key — it is backed up across reinstalls so players don't get re-accept-trust prompts. Don't delete it.

## Installation

1. Download [`egg-vanilla-bedrock.json`](./egg-vanilla-bedrock.json)
2. In your Pterodactyl panel, go to **Admin → Nests → Import Egg**
3. Upload the JSON file and select the target nest
4. Create a new server using the imported egg

## Credits

Based on [parkervcp/eggs](https://github.com/parkervcp/eggs) with fixes for the broken version scraping and additional variables.
