# Releasing

Releases follow strict [Semantic Versioning](https://semver.org/): `vMAJOR.MINOR.PATCH`
(three numeric parts, always). The old two-part tags (`v1.0`, `v1.1`, plus the
removed `v1.0.0`/`v1.2`/`v1.3`) predate this policy; `v1.2.0` is the first
strict release. Published tags and releases are never rewritten, moved, or
deleted — the old ones were removed before any adoption, which is the only
case where removal is acceptable.

## Bump rules (egg-specific)

- **MAJOR**: breaks existing servers — removed/renamed variables, changed
  startup command semantics, new minimum panel version.
- **MINOR**: backward-compatible additions — new variables (with safe
  defaults), new config mappings, new install-time features.
- **PATCH**: fixes with no behavior change for correctly-configured servers —
  script bug fixes, docs, validation tightening.

## Cut a release

1. Add an entry under a new version heading in [CHANGELOG.md](./CHANGELOG.md);
   move it out of `[Unreleased]`.
2. Commit: `git commit -am "Release vX.Y.Z"`.
3. Tag and push: `git tag vX.Y.Z && git push origin main vX.Y.Z`.
4. Publish notes from the changelog entry (keep title = tag):
   `gh release create vX.Y.Z --title vX.Y.Z --notes-file - <<EOF`
   (paste the entry), or use the GitHub web UI.
5. Pre-release flag: only for `-rc`/`-beta` suffixed tags
   (e.g. `v1.4.0-rc1`). Never mark a stable `vX.Y.Z` as pre-release, and
   never publish a two-part tag again.

## Checklist before tagging

- `egg-vanilla-bedrock.json` parses (`python3 -c "import json; json.load(...)"`)
  and the install script passes `bash -n`.
- New/changed variables have descriptions, sane defaults, and validation
  rules; README variable table matches.
- CHANGELOG entry exists and README docs match behavior.
