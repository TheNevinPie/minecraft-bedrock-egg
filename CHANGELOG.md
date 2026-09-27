# Changelog

All notable changes to this egg are documented here, newest first.
Releases follow strict SemVer (`MAJOR.MINOR.PATCH`); see
[RELEASING.md](./RELEASING.md). Tags `v1.0`, `v1.0.0`, `v1.1`, `v1.2`, and
`v1.3` predate the policy (two-part numbers, and an empty `v1.1`); only
`v1.0` and `v1.1` remain published, the rest were removed before any
adoption. `v1.2.0` is the first strict-SemVer release.

## [Unreleased]

## [1.2.0] - 2026-09-27

Added (NetherNet support, verified against live BDS 1.26.51.1):
- New variables: `TRANSPORT` (default `nethernet`), `SERVER_UDP_PORTS`
  (Docker/NAT mapping form supported), `SERVER_PORT_V6`, `LAN_VISIBILITY`
  (default `false`), all wired into the `server.properties`
  auto-configuration.
- Install backs up and restores the `keys/` server identity key, and
  generates a fresh P-384 key on first install so players stop getting
  re-accept-trust prompts.
- Install warns when a custom version is below 1.26.51 (1.26.50.x builds
  never open gameplay sockets in containers and silently wipe
  `server-udp-ports`).
- README NetherNet runbook: allocations, mapping forms, version floor,
  trust prompts, Door checklist, RakNet fallback.

Fixed:
- Install script had mixed `\n\r` line endings that break Bash; normalized
  to LF-only.

## [1.1] - 2026-09-10

No code changes (same commit as v1.0).

## [1.0] - 2026-09-10

- Refreshed egg metadata, README description and feature list.
- Expanded server configuration variables, API-based version fetching,
  preview channel, custom versions.
