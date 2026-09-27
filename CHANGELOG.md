# Changelog

All notable changes to this egg are documented here, newest first.
Versioning was ad-hoc before 2026-09-27; strict SemVer (`MAJOR.MINOR.PATCH`)
applies to all releases cut after that date. See [RELEASING.md](./RELEASING.md).

## [Unreleased]

## [v1.3] - 2026-09-27

Added (guided NetherNet setup):
- Startup wrapper composes `server-udp-ports` from `NET_PUBLIC_IP`,
  `NET_EXT_PORTS`, `NET_INT_PORTS`, with precedence override → composed →
  ephemeral and console preflight warnings (bad values, RakNet conflicts,
  ephemeral mode, missing `server.properties`, unexpected transport).
- New variables: `NET_PUBLIC_IP`, `NET_EXT_PORTS`, `NET_INT_PORTS`;
  `SERVER_UDP_PORTS` repurposed as advanced override.
- Install generates a P-384 server identity key on first install, so players
  stop getting re-accept-trust prompts after reinstalls.
- Install warns when a custom version is below 1.26.51 (1.26.50.x cannot open
  gameplay sockets behind Docker/NAT).

Fixed:
- Install script had mixed `\n\r` line endings that break Bash; normalized
  to LF-only.

## [v1.2] - 2026-09-27

Added (NetherNet transport support):
- New variables: `TRANSPORT` (default `nethernet`), `SERVER_UDP_PORTS`,
  `SERVER_PORT_V6`, `LAN_VISIBILITY` (default `false`), all wired into the
  `server.properties` auto-configuration.
- Install backs up and restores the `keys/` server identity key.

## [v1.1] - 2026-09-10

No code changes (same commit as v1.0).

## [v1.0] - 2026-09-10

- Refreshed egg metadata, README description and feature list.
- Expanded server configuration variables, API-based version fetching,
  preview channel, custom versions.

## [v1.0.0] - 2026-07-13

- Initial Vanilla Bedrock egg (nullable world name field).
