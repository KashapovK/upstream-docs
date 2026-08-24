---
name: upstream-docs
description: Verify dependency, framework, and platform API claims against exact-version upstream evidence. Use for current/latest/stable/deprecated/experimental/supported claims, migrations where repository constraints and target versions differ, or conflicts between official docs, releases, tagged source, and tests. Return VERIFIED, CONTRADICTED, or INCONCLUSIVE. Skip purely local code work.
license: MIT
metadata:
  author: KashapovK
  version: "0.2.0"
---

# Upstream evidence gate

Treat a material version- or channel-sensitive upstream claim as unproven until evidence for the relevant target supports it. Keep the skill standalone: product-specific documentation integrations, indexes, and search may help discovery when available, but they are not dependencies and do not establish exact-target claims by themselves.

## 1. Classify the request

Choose one mode before gathering evidence:

| Mode | Use when | Required target state |
|---|---|---|
| `EXACT_TARGET` | The request names a specific version, bounded version line, or exact prerelease | Establish that target and channel with direct target-matched authority. Do not investigate latest stable unless the comparison is requested or material. |
| `CURRENT` | The request asks what is current, latest, stable, deprecated, or supported now | For release-backed products, establish the latest relevant release/channel and official release date. For continuously deployed APIs without a release lifecycle, establish the current official contract/channel and an authoritative as-of date. Keep stable separate from prerelease/canary. |
| `MIGRATION` | A decision depends on moving from repository state to an intended target | Record the repository constraint and resolved version when present, the intended target, and latest stable only when it affects whether the target is current, stale, ahead, or prerelease. |

A named canary, beta, or release candidate can be `EXACT_TARGET`; “latest canary” is `CURRENT` for that prerelease channel.

Treat a bounded line such as `5.4` as `5.4.x`, not as an invented exact patch. Establish whether the claim holds across that line or resolve the relevant patch before applying patch-specific evidence to the whole line.

## 2. Atomize claims and define targets

Split independently falsifiable material assertions before verification. Existence, stability, supported execution surface, and target applicability may require separate claims. Never hide a contradicted or inconclusive subclaim behind an overall positive conclusion.

For each atomic claim, define only the target dimensions that can change the answer:

```text
package or product
version
channel
runtime or platform
API surface
```

When repository state is material, distinguish:

- **Repository constraint** — the manifest range or version expression.
- **Repository resolved version** — the exact version in a lockfile or authoritative resolved metadata.
- **Intended target** — the version or channel the decision concerns.

Do not call a range an exact pin or substitute the repository state, intended target, latest stable, live docs, and prerelease source for one another. If only one repository form exists, report only that supported form.

## 3. Match authority to the claim

Use sources that own the claim instead of one universal hierarchy:

| Claim type | Preferred authoritative evidence |
|---|---|
| Release or version existence | Official package registry metadata when authoritative for the ecosystem; official release record; official tag or release source |
| Public support, stability, deprecation, or documented contract | Version-matched official documentation; target release notes or changelog; official support or deprecation policy |
| Implemented behavior | Tagged target-version source; tagged tests or conformance tests; version-matched implementation documentation |
| Standard or conformance | Primary specification or standard; authoritative compatibility or conformance data; exact-target implementation evidence |
| Security-sensitive behavior | The official security documentation, advisory, or policy that owns the contract; exact-version implementation evidence when runtime behavior is material |

Tagged source may prove implementation but must not silently redefine a documented public support contract. For implemented behavior, tagged source and tests can outrank stale prose. If authoritative sources for the same claim type still conflict after exact-target analysis, return `INCONCLUSIVE`.

Route discovery appropriately:

- OpenAI, Codex, ChatGPT, and OpenAI SDK/API claims: use an available official OpenAI documentation integration first; use official OpenAI web pages as fallback.
- JavaScript, HTML, CSS, and browser API claims: use MDN for documentation and compatibility, and consult primary standards when status or conformance is material.
- Third-party libraries and frameworks: use official versioned docs, release material, registries, tagged source, and tests as appropriate.
- Repository state: read the actual manifest, lockfile, generated metadata, or maintained configuration.

Documentation indexes, general search, snippets, cached prose, model memory, and issue comments are discovery layers, not automatically authoritative exact-version evidence. Absence from one page or search result does not prove a feature is unavailable.

For `EXACT_TARGET`, current unversioned docs or a main-branch changelog cannot establish `VERIFIED` by themselves. Cite at least one direct version-matched document, release, registry record, tag, or tagged source/test that owns the claim. If none is available, escalate or return `INCONCLUSIVE`.

## 4. Escalate research proportionally

Do not inspect tagged source or tests for every ordinary public API question. Escalate when at least one condition applies:

- exact-version official docs are unavailable;
- official sources conflict or are ambiguous;
- the claim concerns implemented runtime behavior;
- stability or support wording does not establish implementation;
- actual behavior is material to a security-sensitive decision;
- the user explicitly requests source or test verification.

It is acceptable to stop when version-matched official documentation and release material unambiguously establish an ordinary public contract.

Keep discovery focused. Resolve an identifier once, ask one focused question per concept, and make additional discovery calls only to resolve named gaps or conflicts. Send only public package or product identifiers and generic technical questions to external services. Never send credentials, private code, private endpoints, proprietary configuration, or personal data.

## 5. Build only the evidence needed

For one ordinary claim, a valid compact result must contain all six fields:

```text
Mode:
Claim:
Target:
Verdict:
Key evidence:
Conflicts: none | ...
```

Use a full evidence matrix when an architecture, migration, security, issue, ADR, or resolution depends on the answer; when authoritative sources conflict; when several targets must be compared; or when the user asks for reproducible research.

Include only applicable rows. Mark `not applicable` or `unavailable` with a concrete reason. For `EXACT_TARGET`, omit latest stable when irrelevant. For `MIGRATION`, include the repository constraint and resolved version when each exists, plus the intended target.

Place direct official links next to the claims they support, label inference separately from sourced fact, and re-check time-sensitive facts if upstream changes during the research window.

## 6. Publish per-claim verdicts

Every atomic material claim receives exactly one canonical verdict:

- `VERIFIED` — required exact-target evidence supports the claim.
- `CONTRADICTED` — required exact-target evidence disproves the claim.
- `INCONCLUSIVE` — the target cannot be established, required evidence is unavailable, or authoritative conflict remains unresolved.

A short downstream decision summary may explain that a contradicted or inconclusive claim blocks architecture or implementation, but it must not replace or merge the canonical verdicts.

## Gate

Evaluate the gate per atomic claim. A claim passes only when it has an explicit target, the required authority for its claim type exists, and conflicts are resolved against that target. An unresolved required-authority conflict makes that claim `INCONCLUSIVE`; preserve the independent verdicts of all other claims.

A downstream decision passes only when every claim it requires is `VERIFIED`. Any required `CONTRADICTED` or `INCONCLUSIVE` claim blocks treating the decision as established.

Before sending the result, verify that every compact-output field is present and every `VERIFIED` `EXACT_TARGET` claim links direct exact-target authority rather than only current docs or a main branch. Rewrite the result or return `INCONCLUSIVE` if this publication check fails.
