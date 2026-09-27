# Pterodactyl Egg - Minecraft Vanilla Bedrock Dedicated Server

An updated Pterodactyl egg for Minecraft Bedrock Dedicated Server with automatic version fetching, preview builds, custom versions, and expanded server configuration.

## Features

- **Reliable downloads** — uses the official Minecraft API (`net-secondary.web.minecraft-services.net`) to fetch download URLs instead of scraping HTML pages that break
- **Preview channel** — select `preview` from the dropdown to get the latest preview build
- **Custom versions** — select `custom` and enter any exact version (e.g. `1.26.33.2`)
- **Expanded variables** — more server.properties options exposed in the panel
- **NetherNet support** — `transport` and port-range variables for the WebRTC-based transport (BDS default since 1.26.5x), with NAT/Docker port-mapping forms documented
- **Startup wrapper** — composes `server-udp-ports` from guided variables and prints preflight warnings for bad values, RakNet conflicts, and ephemeral mode
- **Config backup/restore** — preserves `server.properties`, `permissions.json`, `allowlist.json`, and the `keys/` server identity key across updates (a fresh P-384 identity key is generated on first install so players stop getting re-accept-trust prompts)
- **Version floor guard** — warns at install time if a custom version is below 1.26.51 (1.26.50.x cannot open gameplay sockets behind Docker/NAT)

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
| `SERVER_UDP_PORTS` | *(empty)* | Advanced override: complete `server-udp-ports` value (see precedence below) |
| `SERVER_PORT_V6` | `19133` | IPv6 port (RakNet mode only) |
| `LAN_VISIBILITY` | `false` | Respond to LAN discovery (keep off on shared hosts) |
| `NET_PUBLIC_IP` | *(empty)* | Public address clients use (the `ip:` prefix; required behind Docker/NAT) |
| `NET_EXT_PORTS` | *(empty)* | Client-facing UDP port/range, e.g. `32100-32149` |
| `NET_INT_PORTS` | *(empty)* | Container-local UDP port/range BDS binds, e.g. `32100-32149` |

## NetherNet runbook

Bedrock Dedicated Server defaults to the WebRTC-based NetherNet transport. Each server needs a UDP port **range** for client peer-to-peer connections, not just its primary port. Wings rewrites `server.properties` from panel variables on every start, so all NetherNet configuration must live in variables — never hand-edit the file.

### Setup

1. In the panel, go to **Admin → Nodes → your node → Allocation** and bulk-create the range (one allocation per port, e.g. `32100-32149` for a 10-player server). Assign them all to the Bedrock server. Ranges must not overlap between servers on the same node.
2. Set the three split variables (preferred): `NET_PUBLIC_IP` to the node's public IP, `NET_EXT_PORTS` and `NET_INT_PORTS` to the range (same range for both with 1:1 panel allocations). The startup wrapper composes `PUBLIC_IP:EXT-START-EXT-END:INT-START-INT-END` for you.
   - The `ip:` prefix is **required** behind Docker bridge networking — without it, clients are handed unreachable container IPs and every join dies at `InitialConnection`.
   - Plain `32100-32149` (no mapping) only works on directly-connected hosts.
3. Open the range as UDP in the host firewall/security group.
4. Restart the server from the panel.

Precedence used by the startup wrapper: explicit `SERVER_UDP_PORTS` override → composed split variables → ephemeral (everything empty). Validation warnings are printed to the console for bad characters, RakNet conflicts, ephemeral mode, and a missing `server.properties` — the server always starts regardless.

### Version floor

NetherNet behind Docker/NAT needs **BDS 1.26.51+** (protocol 2193, matches 26.51 clients). The 1.26.50.x builds never open gameplay sockets in containers and silently wipe `server-udp-ports`. The installer warns if you pick a custom version below 1.26.51.

### Trust prompts

Without a saved identity key, BDS generates a throwaway one per boot and players must re-accept server trust after every reinstall. This egg backs up `keys/` across reinstalls and generates a fresh P-384 key on first install — don't delete the folder.

### When joins fail with Door / InitialConnection

Check in order: (1) client and server on the same protocol (server list must show the server, not just the entry), (2) gameplay sockets actually bound, (3) pinned range + public-IP mapping present in `server.properties`, (4) firewall/security-group UDP range open, (5) client-side NAT. If all else fails, `TRANSPORT=raknet` needs only the single game port and works with current clients as a fallback.

## Installation

1. Download [`egg-vanilla-bedrock.json`](./egg-vanilla-bedrock.json)
2. In your Pterodactyl panel, go to **Admin → Nests → Import Egg**
3. Upload the JSON file and select the target nest
4. Create a new server using the imported egg

## Credits

Based on [parkervcp/eggs](https://github.com/parkervcp/eggs) with fixes for the broken version scraping and additional variables.
