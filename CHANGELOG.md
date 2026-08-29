# Changelog

All notable changes to this project will be documented in this file.

## 0.3.1 - 2026-08-29

### Added

- Added OpenAI skill metadata for standalone UI presentation, explicit invocation, and automatic discovery.

### Changed

- Added CI validation that the OpenAI skill metadata remains valid and aligned with the plugin manifest.

## 0.3.0 - 2026-08-29

### Changed

- Added a target-to-action gate for version-sensitive install, update, and migration commands.
- Required downstream commands to resolve and match verified versions, dist-tags, Git refs, and channels before mutation.
- Added regression coverage for stable targets behind canary default branches, explicit canary requests, and continuously updated sources without a release lifecycle.
- Pinned public install and update examples to the verified stable release tag.

## 0.2.0 - 2026-08-24

### Changed

- Added proportional `EXACT_TARGET`, `CURRENT`, and `MIGRATION` verification modes.
- Added per-atomic-claim verdicts and target tuples for material runtime, platform, channel, and API-surface differences.
- Distinguished repository constraints from exact lockfile-resolved versions.
- Replaced the universal evidence hierarchy with claim-type authority rules.
- Made tagged source and tests an escalation for missing, ambiguous, conflicting, behavioral, or security-sensitive evidence.
- Added compact default output while preserving full evidence matrices for higher-risk decisions.
- Refined trigger metadata, regression cases, public positioning, and skills CLI guidance.
- Added CI validation that skill and plugin release versions match.

## 0.1.0 - 2026-08-23

### Added

- Initial public release of the `upstream-docs` skill.
- Skills-only Codex plugin manifest.
- Activation cases, validation workflow, security policy, and public documentation.
