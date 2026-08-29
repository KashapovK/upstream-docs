# Upstream Docs

> **Know which version your AI is talking about.**

**Upstream Docs is a version-aware evidence gate for AI coding agents.** It verifies dependency, framework, and platform API claims against the upstream evidence that applies to the target you are actually deciding about.

[![skills.sh](https://skills.sh/b/KashapovK/upstream-docs)](https://skills.sh/KashapovK/upstream-docs) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## The problem

AI coding agents often mix several different realities:

- the version your repository declares;
- the version its lockfile actually resolves;
- the version you intend to migrate to;
- today's latest stable release;
- a prerelease or canary channel;
- live docs that may describe a different release.

Upstream Docs keeps those targets separate before a version-sensitive claim is allowed into an architecture or implementation decision.

## Quick start

Install every skill in the repository:

```bash
npx skills add https://github.com/KashapovK/upstream-docs/tree/v0.3.1
```

Install only `upstream-docs`:

```bash
npx skills add https://github.com/KashapovK/upstream-docs/tree/v0.3.1 --skill upstream-docs
```

Try it without installing:

```bash
npx skills use https://github.com/KashapovK/upstream-docs/tree/v0.3.1 --skill upstream-docs
```

Install or replace a project copy with the verified stable release:

```bash
npx skills add https://github.com/KashapovK/upstream-docs/tree/v0.3.1 --skill upstream-docs -y
```

These examples pin the source tag so the installed target matches this documented release. Use another explicit ref only when that version or channel is the intended target.

## What it returns

Each independently falsifiable material claim receives `VERIFIED`, `CONTRADICTED`, or `INCONCLUSIVE`. Ordinary single-claim answers stay compact.

Illustrative example — names and versions are placeholders:

```text
Mode: EXACT_TARGET
Claim: Framework API X is supported on server routes in 4.3.0.
Target: framework 4.3.0, stable channel, server-route surface
Verdict: VERIFIED
Key evidence: version-matched official docs and the 4.3.0 release notes
Conflicts: live docs also describe an option added after 4.3.0; that option is excluded
```

## Verification modes

| Mode | When it is used | Target state | Latest stable |
|---|---|---|---|
| `EXACT_TARGET` | A specific version or prerelease is named | The exact version, channel, and any material runtime or API surface | Only when requested or material |
| `CURRENT` | The question asks what is current, latest, stable, deprecated, or supported now | The latest official release/channel and date, or the current official contract plus an authoritative as-of date for continuously deployed APIs | Required for release-backed stable-current claims |
| `MIGRATION` | Repository state and the intended target differ | Manifest constraint, resolved lockfile version when present, and intended target | When it determines whether the target is stale, ahead, prerelease, or current |

For multi-claim requests, the skill separates claims such as API existence, stability, surface compatibility, and target applicability so one positive result cannot hide a contradiction.

## Evidence, not archaeology

Authority depends on the claim. Version-matched official docs own public contracts; releases and registries establish version existence; tagged source and tests establish implemented behavior; primary specifications own standards claims.

Tagged source and tests are an escalation when docs are unavailable, ambiguous, conflicting, security-sensitive, or insufficient to establish actual behavior. A straightforward exact-version API question does not automatically become a full research project.

Stable and prerelease evidence remain separate. Canary-only evidence never establishes stable support by itself. If the relevant target or authority cannot be established, the gate fails closed with `INCONCLUSIVE`.

## How it fits with other tools

- Documentation retrieval tools find relevant pages.
- Generic research workflows investigate broader questions.
- Adversarial review and domain-modeling workflows pressure-test a decision.
- `upstream-docs` adjudicates a material version- or channel-sensitive upstream claim against the correct target.

These tools are complementary. None is a runtime dependency of this skill.

## When it stays out

The skill skips purely local work with no material upstream claim, such as renaming a local function, formatting a component, or explaining a repository-only algorithm.

## Privacy and security

Only public package or product identifiers and generic technical questions may be sent to external documentation or search services. Never send credentials, private code, private endpoints, personal data, or proprietary configuration. See the [security policy](SECURITY.md).

## Development

Validate the skill with the repository's pinned Agent Skills reference validator:

```bash
skills-ref validate ./skills/upstream-docs
```

The CI workflow also validates the plugin manifest, checks version consistency and whitespace, and scans for secrets.

## Project information

- [Agent Skills specification](https://agentskills.io/specification)
- [MIT license](LICENSE)
- [Security policy](SECURITY.md)
- [Changelog](CHANGELOG.md)
