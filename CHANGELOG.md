# Changelog

All notable changes to `telco-voice-manager` are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
repository adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Every release recorded below names a tag that exists in this repository, and its date is the date of
the tag. All six tags are lightweight tags, which carry no tagger date of their own, so the date is
the tagged commit's date. A `git describe --tags` string of the form
`<tag>-<commits-since-tag>-g<abbreviated-hash>` denotes a position in history rather than a release,
so no such string is recorded here as a release and no literal value of one is reproduced anywhere
in this file. Work that is not yet tagged is listed under `Unreleased`. This file was introduced
after every release below was tagged, so each release entry is compiled from the non-merge commits
in the range its tag closes.

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
moment the next commit landed. The tags themselves are recorded, with their dates, as the release
headings below.

### Added

The repository had no documentation directory and no changelog before these entries. The whole set
below is the HAL documentation milestone for this repository.

- [docs/pages/halSpec.md](https://github.com/rdkcentral/telco-voice-manager/blob/b37587112cedffed65a56d3296fdb6b0e337441d/docs/pages/halSpec.md) - the voice `JSON` HAL interface specification: architecture, the
  deployment contract (configuration file, schema path, server port), initialization and startup,
  threading, process and memory models, blocking behaviour, asynchronous notification, error
  handling, persistence, the non-functional requirements, and the complete action and object surface.
  This is the generated site's main page.
- [docs/pages/halSpecDetailed.md](https://github.com/rdkcentral/telco-voice-manager/blob/b37587112cedffed65a56d3296fdb6b0e337441d/docs/pages/halSpecDetailed.md) - the per-parameter reference for both shipped schemas: every
  parameter definition with its type, constraint, access and description, plus the object index, the
  enumeration appendix, worked message examples per protocol workflow, and the known contract
  defects.
- `docs/generate_docs.sh` - the documentation build entry point, pinning the `hal-doxygen` generator
  at tag `1.2.0` and passing `PROJECT_NAME="RDK-B Telco Voice HAL"`.
- `docs/.gitignore` - excludes the generator checkout, the generated `output` tree and
  `node_modules`, none of which is committed.
- Five symlinks under `docs/pages/` - `CHANGELOG`, `CONTRIBUTING`, `COPYING`, `LICENSE` and `NOTICE`,
  each carrying a `.md` suffix - pointing at the repository-root files, so the release history and the
  legal texts render alongside the HAL documentation in the generated site.
- This changelog, the release-history record for the repository and the source for the
  `Version History` topic of the HAL specification.

### Changed

- The repository README rewritten as a manager overview: component purpose and boundary,
  architecture, the build-time data-model variant selection, runtime behaviour, repository layout
  and the known contract defects, with the per-parameter and per-message contract detail moved to
  the two documents above rather than duplicated. Corrections in the same pass include the shipped
  data-model sizes, the request-path inventory and the variant differences between the two schemas.

## 2.0.5 - 2026-05-18

Release tagged [`v2.0.5`](https://github.com/rdkcentral/telco-voice-manager/releases/tag/v2.0.5).
Its changes since `v2.0.4` are the range
[`v2.0.4...v2.0.5`](https://github.com/rdkcentral/telco-voice-manager/compare/v2.0.4...v2.0.5).

### Changed

- Voice uses the IPv6 address that WAN Manager assigns to the current WAN interface (RDKB-58910,
  RDKB-60007). `telcovoicemgr_nw_monitor.c` reads the interface name from the
  `current_wan_ifname` sysevent and the address from that interface's
  `tr_<ifname>_dhcpv6_client_v6addr` sysevent, instead of deriving it from `ipv6_prefix`, which
  gave the address on the LAN bridge `brlan0`
  ([`6d3af7b`](https://github.com/rdkcentral/telco-voice-manager/commit/6d3af7b1990f3534fd35f96513bc75890ef5ccfc),
  [#5](https://github.com/rdkcentral/telco-voice-manager/pull/5)).

### Fixed

- String parameters accepted values longer than their declared maximum and reported success
  (RDKBDEV-3387). The 230 string type declarations in `config/RdkTelcoVoiceManager_v2.xml` are now
  written `string(N)` rather than `String(N)`, so that an over-length value is rejected with
  `CCSP_ERR_INVALID_PARAMETER_VALUE`
  ([`0f6ce0e`](https://github.com/rdkcentral/telco-voice-manager/commit/0f6ce0e79573951ef7dad42f5711fc752d0936c2),
  [#9](https://github.com/rdkcentral/telco-voice-manager/pull/9)).

## 2.0.4 - 2026-03-18

Release tagged [`v2.0.4`](https://github.com/rdkcentral/telco-voice-manager/releases/tag/v2.0.4).
Its changes since `v2.0.3` are the range
[`v2.0.3...v2.0.4`](https://github.com/rdkcentral/telco-voice-manager/compare/v2.0.3...v2.0.4).

### Fixed

- The voice report carried the device MAC address `00:00:00:00:00:00` on some hubs (RDKB-64017):
  `getDeviceMac` compared the `sysevent_get` result with `CCSP_SUCCESS` instead of 0. It now checks
  for 0, treats a negative `sysevent_open` descriptor as a failure, and logs a failed lookup of
  `eth_wan_mac`
  ([`3a0a10b`](https://github.com/rdkcentral/telco-voice-manager/commit/3a0a10b6f089dcde4c40e4becfb2ed838251dd63),
  [#11](https://github.com/rdkcentral/telco-voice-manager/pull/11)).

## 2.0.3 - 2026-03-10

Release tagged [`v2.0.3`](https://github.com/rdkcentral/telco-voice-manager/releases/tag/v2.0.3).
Its changes since `v2.0.2` are the range
[`v2.0.2...v2.0.3`](https://github.com/rdkcentral/telco-voice-manager/compare/v2.0.2...v2.0.3).

### Fixed

- QoS settings configured for RTP traffic did not take effect (XER10-2679). The `iptables` mangle
  rules that set the RTP packet mark and DSCP value now format their unsigned values with the
  `printf` conversion for `unsigned int` rather than the one for `unsigned long`, which crashed on
  64-bit platforms
  ([`f1e6052`](https://github.com/rdkcentral/telco-voice-manager/commit/f1e6052c4bc6f02a076cb97d7539f265b7814cb6),
  [#10](https://github.com/rdkcentral/telco-voice-manager/pull/10)).

## 2.0.2 - 2026-01-20

Release tagged [`v2.0.2`](https://github.com/rdkcentral/telco-voice-manager/releases/tag/v2.0.2).
Its changes since `v2.0.1` are the range
[`v2.0.1...v2.0.2`](https://github.com/rdkcentral/telco-voice-manager/compare/v2.0.1...v2.0.2).

### Added

- Component documentation in the repository README (RDKBDEV-3323): design, the TR-181 data models,
  the internal modules, the component interactions and the implementation details
  ([`7a0c0ee`](https://github.com/rdkcentral/telco-voice-manager/commit/7a0c0eecb51095e65f6ae9d2be66c3c405776735),
  [#7](https://github.com/rdkcentral/telco-voice-manager/pull/7)).

### Changed

- `.github/workflows/cla.yml` - the Contributor License Agreement workflow now declares its token
  permissions explicitly and calls the shared `rdkcentral/cmf-actions` CLA workflow at `v1` instead
  of `main`
  ([`87f5d78`](https://github.com/rdkcentral/telco-voice-manager/commit/87f5d78ddb66bc99e101c3bdfe58d5136f23d59e),
  [#6](https://github.com/rdkcentral/telco-voice-manager/pull/6)).

### Fixed

- SIP registration failed with `408 Request Timeout` (SHARMAN-3878). The firewall status is now
  protected by a mutex, and the voice firewall rules - the SIP proxy list, the RTP pinholes and the
  DSCP and Ethernet-priority marks - are published only after `check_and_wait_for_firewall()`
  confirms that the firewall has started, with an error logged when it does not start in time.
  IPv6 RTP rules are updated by deleting only the removed entries and adding only the new ones
  ([`7ae1e57`](https://github.com/rdkcentral/telco-voice-manager/commit/7ae1e57528d88285174d2d85ee7ecbdbb41cd5ed),
  [#8](https://github.com/rdkcentral/telco-voice-manager/pull/8)).

## 2.0.1 - 2025-09-04

Release tagged [`v2.0.1`](https://github.com/rdkcentral/telco-voice-manager/releases/tag/v2.0.1).
Its changes since `v2.0.0` are the range
[`v2.0.0...v2.0.1`](https://github.com/rdkcentral/telco-voice-manager/compare/v2.0.0...v2.0.1).
Its commits bring over changes made on the RDK Gerrit `stable2` and sprint branches that `v2.0.0`
did not include.

### Added

- TR-104 v2 SIP network parameters under `Device.Services.VoiceService.{i}.SIP.Network.{i}.`
  (RDKB-58644): `X_RDK_ConferencingOption`, `X_RDK_SipSubscribeEnable`,
  `X_RDK_SessionRefreshForceSelectedMethod` and `X_RDK_SessionRefreshRequestUseUpdate`, in
  `config/RdkTelcoVoiceManager_v2.xml`, `hal_schema/telcovoice_hal_schema_v2.json`,
  `config/telcovoice_config_default_v2.json` and the SIP data-model handlers
  ([`79840aa`](https://github.com/rdkcentral/telco-voice-manager/commit/79840aa4cac001553c9886e6bad364c11817e0d6)).
- The session-expiry parameters `X_RDK_MinSessionExpires` and `X_RDK_SessionExpires` in the same
  object (RDKB-59395), with defaults of 600 and 1800
  ([`1d11b72`](https://github.com/rdkcentral/telco-voice-manager/commit/1d11b72f204f4957f25421dbb4757785473b08a2)).
  Both additions had been made and reverted before `v2.0.0`, so this is the first release that
  carries them.

### Changed

- `AnscEqualString()` is replaced with `strcmp()` in the TR-104 v2 data-model handlers
  (RDKBDEV-3141), and `syscfg` is included as `syscfg/syscfg.h` (RDKBDEV-3140)
  ([`5e604f3`](https://github.com/rdkcentral/telco-voice-manager/commit/5e604f3c0f2a9681b6fc2507b6c14b034e09acb5)).

### Fixed

- High CPU use by `syseventd` (RDKB-58557): `getDeviceMac` in the voice report queries
  `eth_wan_mac` once per call instead of looping until the value is set
  ([`cd09374`](https://github.com/rdkcentral/telco-voice-manager/commit/cd093749543a45f0d43bc905a3be0f6b42ef7f09)).

## 2.0.0 - 2025-07-11

First tagged release, at tag
[`v2.0.0`](https://github.com/rdkcentral/telco-voice-manager/releases/tag/v2.0.0). It brings the
history `RdkTelcoVoiceManager` accumulated on the RDK `stable2` branch from 2021 into this GitHub
repository through [#2](https://github.com/rdkcentral/telco-voice-manager/pull/2), on top of the
repository's initial README
([`5eb4ab4`](https://github.com/rdkcentral/telco-voice-manager/commit/5eb4ab489ffe4ee0e0b8ce376626d1985ca94559))
and the Contributor License Agreement check in `.github/workflows/cla.yml`
([`470f600`](https://github.com/rdkcentral/telco-voice-manager/commit/470f6004ee5e3f860d858a6e6dfba0af68a1555a),
[#3](https://github.com/rdkcentral/telco-voice-manager/pull/3)).

### Added

- The `RdkTelcoVoiceManager` component (RDKB-34410): the voice manager process, the TR-104 v1 and v2
  data models in `config/RdkTelcoVoiceManager_v1.xml` and `config/RdkTelcoVoiceManager_v2.xml`, the
  HAL contracts `hal_schema/telcovoice_hal_schema_v1.json` and
  `hal_schema/telcovoice_hal_schema_v2.json`, the default voice configuration
  `config/telcovoice_config_default.json` and the JSON HAL configuration
  `config/telcovoice_manager_conf.json`
  ([`910d855`](https://github.com/rdkcentral/telco-voice-manager/commit/910d855f6aec171de51ea2678260f2c73cdfd409)).
- WebConfig support through `Device.X_RDK_TelcoVoice.Data` (RDKB-31687), extended to the TR-104 v2
  data model (RDKB-36794)
  ([`7485384`](https://github.com/rdkcentral/telco-voice-manager/commit/7485384ec79d077206c79a7daffca1d3b220f0ef),
  [`ce8505c`](https://github.com/rdkcentral/telco-voice-manager/commit/ce8505ced40a3207c8a95664fc7fed0e16c5eac3)).
- The voice service report with its Avro schema (RDKB-36669), the TR-104 v2 diagnostics data models
  (RDKB-40904), call-log statistics with MOS data models (SHARMAN-550), and the `MOSCQScore`,
  `MOSLQScore` and `RFactor` data models (SHARMAN-1133)
  ([`c8e47af`](https://github.com/rdkcentral/telco-voice-manager/commit/c8e47af17d515c66428a5bd24b017404a6887921),
  [`bf7d583`](https://github.com/rdkcentral/telco-voice-manager/commit/bf7d583a3a5f85bcbcaacc17c1c968fae982256b),
  [`88a2043`](https://github.com/rdkcentral/telco-voice-manager/commit/88a204325b807f012f365b95ff8339d311fbf5ae),
  [`97ad013`](https://github.com/rdkcentral/telco-voice-manager/commit/97ad013aa6a2fc44ab77c4244c2f78ce8b64f5b9)).
- Persistent storage for the TR-104 v2 model in `telcovoice_config_default.json` and
  `telcovoice_config_current.json` (RDKBDEV-944), with `X_RDK_Enable` persistent across reboots
  (RDKBDEV-2323)
  ([`eb791ff`](https://github.com/rdkcentral/telco-voice-manager/commit/eb791ff01ece0c673f15c60ac2d6ba4afb6e2b0f),
  [`95f5176`](https://github.com/rdkcentral/telco-voice-manager/commit/95f517622b840f8a9cef2d9c4b1dda14190ddcaf)).
- Custom TR-104 parameters and features: `X_RDK_DnsServers`, which passes the DNS servers to the
  vendor voice stack (RDKB-37422), `X_RDK_OngoingEmergencyCall` (RDKBDEV-1440), ring-phone support
  (RDKBDEV-2229), `X_RDK_LocalTimeZone` (RDKBDEV-2322) and `SIP.Client.{i}.X_RDK_LastChange`
  (RDKBDEV-2350)
  ([`ebf4063`](https://github.com/rdkcentral/telco-voice-manager/commit/ebf406326b08ae59a093555786daa0eeca740c12),
  [`8ea2c43`](https://github.com/rdkcentral/telco-voice-manager/commit/8ea2c43884265ffa074ade4ddb97c83f08831a2b),
  [`f804a35`](https://github.com/rdkcentral/telco-voice-manager/commit/f804a3558352e14fcbaec729b142245c72064a36),
  [`596d21c`](https://github.com/rdkcentral/telco-voice-manager/commit/596d21c9ab0995a67da976c70e52b3762d0bdb42),
  [`6d89c07`](https://github.com/rdkcentral/telco-voice-manager/commit/6d89c07caa016f27e800db188de67ddf80af434e)).
- Telemetry markers (RDKB-40127), and enabling or disabling the voice stack on WAN Manager's
  `InterfaceActiveStatus` Rbus event (RDKB-42014)
  ([`b5c6b22`](https://github.com/rdkcentral/telco-voice-manager/commit/b5c6b2248db252538c592ccecb61bebe8e2c3d1e),
  [`1f62458`](https://github.com/rdkcentral/telco-voice-manager/commit/1f62458418eaadf248c9c54586bafd60336aee76),
  [`bcced89`](https://github.com/rdkcentral/telco-voice-manager/commit/bcced89f93cef2644b99ad9896a1ca7b17be5af1)).

### Changed

- The JSON HAL server port in `config/telcovoice_manager_conf.json` moved from 40104, which lies in
  the gateway's ephemeral port range, to 20104 (RDKCOM-4453)
  ([`ef0d889`](https://github.com/rdkcentral/telco-voice-manager/commit/ef0d889a495d9c285279bcd0c8bb39afdc9f8c32)).
- The default voice configuration is sent to the HAL in a single JSON HAL set request
  (RDKB-37421), and the firewall rule data uses the
  `Device.Services.VoiceService.{i}.X_RDK_Firewall_Rule_Data` format, which can append or remove a
  single rule sent by the voice application (RDKB-37423, RDKB-38214, RDKB-38780)
  ([`bcae27f`](https://github.com/rdkcentral/telco-voice-manager/commit/bcae27fa28f8b80e23f6aa8fbdb54f9f4a26a055),
  [`46ffcae`](https://github.com/rdkcentral/telco-voice-manager/commit/46ffcae87529a3c39012ec507d0a686b6a40a24a),
  [`74861b2`](https://github.com/rdkcentral/telco-voice-manager/commit/74861b2994468c1df22d15d453a142faa05e27e1),
  [`efc3922`](https://github.com/rdkcentral/telco-voice-manager/commit/efc3922aad12f0dd5133f5c62c6ec4004102612b),
  [`50509a5`](https://github.com/rdkcentral/telco-voice-manager/commit/50509a52df2bde2a8a04d1a5eab647568ca01f02)).
- `TELCOVOICE_DATA_MAX` was raised from 3 to 100 and then to 128, so the data model holds more
  table instances (RDKBDEV-1374, RDKBDEV-2283)
  ([`63ca3fa`](https://github.com/rdkcentral/telco-voice-manager/commit/63ca3fa7ffef6132c3c8a06c4ef5d65a826ace53),
  [`1066d49`](https://github.com/rdkcentral/telco-voice-manager/commit/1066d493c49a67b6596471e549c878c57f3d03f6)).
- Platform-specific code and the platform layer's `sysevent` dependency were removed (RDKB-55482,
  RDKB-59079), as was the D-Bus build dependency (RDKBDEV-2970), and the manager follows the
  converged WAN Manager 1.5 data model (RDKB-48263)
  ([`c67c25c`](https://github.com/rdkcentral/telco-voice-manager/commit/c67c25cdc6d07891c445ec22cb6ae64eaca2307e),
  [`862b218`](https://github.com/rdkcentral/telco-voice-manager/commit/862b218a37ae73345a70a3778293b31be397c1e3),
  [`c57b2ab`](https://github.com/rdkcentral/telco-voice-manager/commit/c57b2abb8a857976d6773770079023d4b104d453),
  [`3f4c410`](https://github.com/rdkcentral/telco-voice-manager/commit/3f4c410b1324955051a11a564e70078dcaab113e)).
- The contribution guide directs contributors to fork the repository on GitHub, send a pull request
  and sign the RDK Contributor License Agreement
  ([`792bfa1`](https://github.com/rdkcentral/telco-voice-manager/commit/792bfa186bb1a342fdda2e6875c52737e4aaabec)).
- Build and code clean-up: autotools tidied, obsolete Windows support dropped, `AnscEqualString()`
  replaced with `strcmp()`, `syscfg` calls moved to the current API without explicit
  `syscfg_init()` calls, a local helper replaced with the `ccsp-common-library` API, and source
  files converted to Unix line endings
  ([`bfda4a1`](https://github.com/rdkcentral/telco-voice-manager/commit/bfda4a1dedec7405f51e4ff54f3ff8866c041b0c),
  [`8d904a3`](https://github.com/rdkcentral/telco-voice-manager/commit/8d904a3617a8058ea68ff2e7839d449cfd8eb19f),
  [`1c69549`](https://github.com/rdkcentral/telco-voice-manager/commit/1c6954901a5b65d68ceb10ce02a6025bcf750866),
  [`5537bd1`](https://github.com/rdkcentral/telco-voice-manager/commit/5537bd111d50d0d532a85cbb4e7572408411e4b5),
  [`f00e6b2`](https://github.com/rdkcentral/telco-voice-manager/commit/f00e6b26313b920c3dacc80d1c687a66955d1f38),
  [`2fb8818`](https://github.com/rdkcentral/telco-voice-manager/commit/2fb88182ecb7154a994efc20434b57414ccf40f2),
  [`ea429d7`](https://github.com/rdkcentral/telco-voice-manager/commit/ea429d70e04ec4fc21e64997eb1978f2eafdf252),
  [`3e4a3e1`](https://github.com/rdkcentral/telco-voice-manager/commit/3e4a3e1b8de359eb338164ed296ce8c3a00f7934),
  [`cd44a6c`](https://github.com/rdkcentral/telco-voice-manager/commit/cd44a6cc9cf1cb644076f10b8156761c962ebc4c)).

### Fixed

- Security: `SIP.AuthPassword` no longer returns the password (ADAHUB-210), the voice password is
  no longer written to the log (SHARMAN-3026), string parameters reject the characters that enabled
  stored cross-site scripting (RDKB-38269, RDKB-38263, SKYH4-5665), and the port and IP address in
  firewall rule data are validated against a schema before a rule is created (RDKB-41674)
  ([`4963391`](https://github.com/rdkcentral/telco-voice-manager/commit/4963391c11de6cace9bc39815711156da75b8a8f),
  [`c668db9`](https://github.com/rdkcentral/telco-voice-manager/commit/c668db9d2955f5dc9397b1bc16f9907fbcf48d6c),
  [`1d13376`](https://github.com/rdkcentral/telco-voice-manager/commit/1d13376e547bc07002d8210b3e03b391cede1e9e),
  [`2ef5e43`](https://github.com/rdkcentral/telco-voice-manager/commit/2ef5e439f3a164cf5d2ad8f716511bd5ac3d3d62),
  [`e025273`](https://github.com/rdkcentral/telco-voice-manager/commit/e02527361a255ac92ec27182c0fe6d5eb08f152a),
  [`1e7dd2a`](https://github.com/rdkcentral/telco-voice-manager/commit/1e7dd2a78b67b8626ba2ea345def2f39f9a02be0)).
- Crashes: a digit map longer than 256 characters (RDKB-36166), more than two `CodecProfile`
  objects (RDKBDEV-1904), a set of some boolean parameters (RDKBDEV-1509), a cloud POST during
  data-model operations (SHARMAN-386), a WAN Manager crash (SHARMAN-338), and memory corruption on
  64-bit builds (SHARMAN-3207)
  ([`3c234a3`](https://github.com/rdkcentral/telco-voice-manager/commit/3c234a3a9000dce752c7aa3f1fabdd675586f2bb),
  [`fd629db`](https://github.com/rdkcentral/telco-voice-manager/commit/fd629db4fe26170a901971bc55aa816a77b633a8),
  [`3da4f16`](https://github.com/rdkcentral/telco-voice-manager/commit/3da4f1629726fedcab0b8179a757c76466441fd7),
  [`dd10451`](https://github.com/rdkcentral/telco-voice-manager/commit/dd104517dffcc089a547b9d5723c68fb5928b28e),
  [`fbcb549`](https://github.com/rdkcentral/telco-voice-manager/commit/fbcb54988e9e8b8e0b7fb1d1c62534ae4b362305),
  [`4c2e403`](https://github.com/rdkcentral/telco-voice-manager/commit/4c2e403ea7cd7e6dc296af8e9c8988d689823551),
  [`aeb1ac0`](https://github.com/rdkcentral/telco-voice-manager/commit/aeb1ac0d87aa338a50c3dc0edbfc7068e3d8ba8a)).
- Configuration integrity: the CRC32 checksum of the configuration file is inverted as the
  algorithm requires (RDKBDEV-1258), the checksum no longer fails after an upgrade (RDKB-59841,
  SKYH4-6529), a factory reset removes the configuration and checksum files (SKYH4-6132), and voice
  configurations are retained (SKYH4-6706)
  ([`77a7230`](https://github.com/rdkcentral/telco-voice-manager/commit/77a7230b49de754f954926fe7214158b68a5adb8),
  [`b64cb85`](https://github.com/rdkcentral/telco-voice-manager/commit/b64cb85c2370d798175ebda45de974e97decd73a),
  [`7a1460f`](https://github.com/rdkcentral/telco-voice-manager/commit/7a1460f50496f5b6d7c800f8bf1ffea6aa2591bb),
  [`43bbee2`](https://github.com/rdkcentral/telco-voice-manager/commit/43bbee2a59760a99671fe8610251937ee6c953af),
  [`2e98eeb`](https://github.com/rdkcentral/telco-voice-manager/commit/2e98eebb1958b925008f85a880988081adce987a),
  [`678e18e`](https://github.com/rdkcentral/telco-voice-manager/commit/678e18e783b5977e7f720d81c10eb6fabbc0e612)).
- Packet marking and firewall rules: SIP and RTP DSCP and Ethernet-priority marks are applied after
  a change, after a reboot and on the first packets of a call (SKYH4-6089, SKYH4-6173, SKYH4-6215,
  SKYH4-6592, RDKB-46014, RDKB-50292), RTP rules are removed promptly when a call ends, including
  when the voice process terminates during a call (RDKB-38860, RDKB-43755), and voice registers only
  after the firewall restart completes (RDKB-43386)
  ([`ebbb37a`](https://github.com/rdkcentral/telco-voice-manager/commit/ebbb37ad004109c9f669b71ad780bf7f85bcbddd),
  [`d83f269`](https://github.com/rdkcentral/telco-voice-manager/commit/d83f269e2e65e3a6f9eafb923bcf53dd5c618105),
  [`027e635`](https://github.com/rdkcentral/telco-voice-manager/commit/027e635ebdb5b938ff707d6353b068386c1efabc),
  [`7077928`](https://github.com/rdkcentral/telco-voice-manager/commit/7077928a025e409d6be6ab110e3255f4a706b833),
  [`619a41d`](https://github.com/rdkcentral/telco-voice-manager/commit/619a41df17dd59b87cf8145fa16c630b29692a29),
  [`1eac146`](https://github.com/rdkcentral/telco-voice-manager/commit/1eac146eefb26e121363bf2da7f8e16129afaa58),
  [`0dc1e11`](https://github.com/rdkcentral/telco-voice-manager/commit/0dc1e1132b54e106510699b2dc2709ddf9abec51),
  [`04450c8`](https://github.com/rdkcentral/telco-voice-manager/commit/04450c89f19d59b12a108c684097dd926e0126ed),
  [`a46fa51`](https://github.com/rdkcentral/telco-voice-manager/commit/a46fa51272db585ea9305358aa5318f4c7f194b3),
  [`43ab7cf`](https://github.com/rdkcentral/telco-voice-manager/commit/43ab7cf46ffbb8b41a9ce4fa45cd6fd3fbeeeb1d)).
- Resource use: a memory leak in `voice_process_get_info()` (RDKBDEV-2179) and high CPU use while
  the harvester report is enabled (RDKB-57682)
  ([`d25acd5`](https://github.com/rdkcentral/telco-voice-manager/commit/d25acd56fce8c11b0d7a15a3afb41b9540dee1cb),
  [`e966df6`](https://github.com/rdkcentral/telco-voice-manager/commit/e966df68c16bd74b16c5f20496086ec385115a14)).
