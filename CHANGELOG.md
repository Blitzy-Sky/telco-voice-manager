# Changelog

All notable changes to `telco-voice-manager` are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
repository adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Every release heading below names a tag that exists in this repository, and its date is the date of
the tag. A `git describe --tags` string of the form `<tag>-<commits-since-tag>-g<abbreviated-hash>`
denotes a position in history rather than a release, so no such string appears here as a release
heading and no literal value of one is reproduced anywhere in this file. Work that is not yet tagged
is listed under `Unreleased`.

Two version numbers travel with this repository, and they are not the same kind of thing:

- **Release tag** - the point in this repository's history that a consumer checks out, recorded as
  the release headings in this file. The current tag is **v2.0.5**.
- **HAL schema version** - the version of the JSON HAL contract itself, carried by the
  `schemaVersion` definition in `hal_schema/telcovoice_hal_schema_v1.json` and
  `hal_schema/telcovoice_hal_schema_v2.json` as the constant **0.0.1**. The schema states that this
  value must not be modified, because HAL operation depends on the client and the vendor server
  agreeing on it. It is not derived from the release tag and has not changed across the tags listed
  below, and it is distinct from the `v1` and `v2` in the schema filenames, which distinguish the two
  shipped data-model variants rather than naming a schema version.

## Unreleased

Changes that are merged but not yet part of a tagged release. No comparison link is given here: a
range ending at `HEAD` names a different pair of commits on every read, so it would be inaccurate the
moment the next commit landed. The tags themselves are listed with their dates under
`Releases before this file`.

### Added

The repository had no documentation directory and no changelog before these entries. The whole set
below is the HAL documentation milestone for this repository.

- `docs/pages/halSpec.md` - the voice `JSON` HAL interface specification: architecture, the
  deployment contract (configuration file, schema path, server port), initialization and startup,
  threading, process and memory models, blocking behaviour, asynchronous notification, error
  handling, persistence, the non-functional requirements, and the complete action and object surface.
  This is the generated site's main page.
- `docs/pages/halSpecDetailed.md` - the per-parameter reference for both shipped schemas: every
  parameter definition with its type, constraint, access and description, plus the object index, the
  enumeration appendix, worked message examples per protocol workflow, and the known contract
  defects.
- `docs/generate_docs.sh` - the documentation build entry point, pinning the `hal-doxygen` generator
  at tag `1.2.0` and passing `PROJECT_NAME="RDK-B Telco Voice HAL"`.
- `docs/.gitignore` - excludes the generator checkout, the generated `output` tree and
  `node_modules`, none of which is committed.
- Five symlinks under `docs/pages/` - `CHANGELOG.md`, `CONTRIBUTING.md`, `COPYING.md`, `LICENSE.md`
  and `NOTICE.md` - pointing at the repository-root files, so the release history and the legal texts
  render alongside the HAL documentation in the generated site.
- This `CHANGELOG.md`, the release-history record for the repository and the source for the
  `Version History` topic of the HAL specification.

### Changed

- `README.md` rewritten as a manager overview: component purpose and boundary, architecture, the
  build-time data-model variant selection, runtime behaviour, repository layout and the known
  contract defects, with the per-parameter and per-message contract detail moved to the two documents
  above rather than duplicated. Corrections in the same pass include the shipped data-model sizes,
  the request-path inventory and the variant differences between the two schemas.

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
