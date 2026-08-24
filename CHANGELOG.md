# Changelog

All notable changes to `telco-voice-manager` are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
repository adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Every release heading below names a tag that exists in this repository, and its date is the date of
the tag. A `git describe --tags` string such as `v2.0.5-1-gb1c4d13` denotes a number of commits past
a tag rather than a release, so it never appears here as a release heading. Work that is not yet
tagged is listed under `Unreleased`.

Two version numbers travel with this repository, and they are not the same kind of thing:

- **Release tag** - the point in this repository's history that a consumer checks out, recorded as
  the release headings in this file. The current tag is **v2.0.5**.
- **HAL schema version** - the version of the JSON HAL contract itself, carried by the
  `schemaVersion` definition in `hal_schema/telcovoice_hal_schema_v1.json` and
  `hal_schema/telcovoice_hal_schema_v2.json` as the constant **0.0.1**. The schema states that this
  value must not be modified, because HAL operation depends on the client and the vendor server
  agreeing on it. It is advanced independently of the release tag, and it is distinct from the `v1`
  and `v2` in the schema filenames, which distinguish the two shipped data-model variants rather
  than naming a schema version.

## Unreleased

Changes that are merged but not yet part of a tagged release
([compare with v2.0.5](https://github.com/rdkcentral/telco-voice-manager/compare/v2.0.5...HEAD)).

### Added

- `docs/pages/CHANGELOG.md` - a symlink to this file, so the repository's release history renders
  alongside the HAL documentation in the generated Doxygen site.
- This `CHANGELOG.md`, the release-history record for the repository and the source for the
  `Version History` topic of the HAL specification. The repository had no changelog before this
  entry.

## Releases before this file

This changelog was introduced during the HAL documentation work recorded under `Unreleased`, after
the releases below were already tagged. Their contents are not restated here, because doing so would
mean asserting release notes this repository never wrote; the authoritative record for each is the
commit range the tag closes.

| Tag | Date |
|---|---|
| `v2.0.5` | 2026-05-18 |
| `v2.0.4` | 2026-03-18 |
| `v2.0.3` | 2026-03-10 |
| `v2.0.2` | 2026-01-20 |
| `v2.0.1` | 2025-09-04 |
| `v2.0.0` | 2025-07-11 |
