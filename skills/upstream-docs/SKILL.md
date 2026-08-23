---
name: upstream-docs
description: Verify version- or channel-sensitive claims about fast-moving dependencies and platform APIs, including OpenAI, Codex, and ChatGPT, before architecture, research, migration, security, compatibility, issue resolution, or implementation decisions. Use for current/latest/stable/experimental/deprecated/supported claims or when repository pins, intended targets, documentation, and upstream source may differ; use alongside product-specific official documentation skills when available, and skip ordinary local work with no material upstream claim.
license: MIT
metadata:
  author: KashapovK
  version: "0.1.0"
---

# Upstream evidence gate

Treat a material version- or channel-sensitive claim as unproven until fresh, exact-target evidence supports a verdict. Product-specific documentation skills, documentation search, and general web search are discovery layers; official releases, versioned documentation, tagged source, and tests establish stable-version claims.

## 1. Pin the claim and target

Restate the assertion in falsifiable form. Keep these targets distinct:

- the version pinned in the repository;
- the intended or approved migration target;
- the latest stable upstream release;
- a canary or prerelease channel explicitly requested by the user.

Read the repository pin from its authoritative manifest, lockfile, generated metadata, or maintained configuration. Derive the intended target from the user's request or an approved decision. Never silently substitute one target for another.

Completion criterion: write one exact claim and one exact target version or channel before researching the claim.

## 2. Build the evidence matrix

Collect every applicable row:

| Evidence | Required result |
|---|---|
| Repository pin | Exact version or range and its authoritative repository source |
| Intended target | Exact version or channel and the source of that intent |
| Latest stable | Exact version and release date from an official release source |
| Documentation coverage | Exact matching library/version, or `skipped`/`unavailable` with a reason |
| Official target docs | Version-pinned official documentation when available |
| Official source/tests | Tagged source or tests when capability or status affects the decision |
| Conflicts | Every contradiction among pin, target, docs, source, tests, and discovery layers |
| Verdict | `VERIFIED`, `CONTRADICTED`, or `INCONCLUSIVE`, with direct source links |

Mark a row not applicable only with a concrete reason.

Completion criterion: every row has a result or an explicit, justified not-applicable entry.

## 3. Route to authoritative evidence

- OpenAI, Codex, ChatGPT, and OpenAI SDK/API claims: use an available official OpenAI documentation integration first; use official OpenAI web pages as fallback.
- JavaScript, HTML, CSS, and browser API claims: use MDN for documentation and compatibility, and consult primary standards when status or conformance is material.
- Third-party libraries and frameworks: use an available documentation index or web search to discover relevant material, then establish the claim with official release notes, versioned docs, and tagged source/tests.
- Repository state: read the actual manifest, lockfile, generated metadata, or maintained configuration.

Search snippets, model memory, unversioned pages, cached prose, and issue comments are leads rather than sufficient stable-version evidence. Absence from one page does not prove a feature is unavailable.

Completion criterion: each material fact is tied to the source that owns it, at the target version or channel where available.

## 4. Budget documentation discovery

- Resolve a library once unless its exact documentation identifier is already known.
- Ask one focused question about one concept.
- Make a second documentation query only to resolve a named conflict.
- Stop after three total documentation-index calls for one user question.
- When exact target-version coverage is absent, record that fact and move to official version-pinned sources.
- Send only public library identifiers and generic technical questions. Keep credentials, private code, private endpoints, personal data, and proprietary configuration out of external discovery tools.

Completion criterion: exact-version discovery evidence is cross-checked against authoritative sources, or the matrix states why the discovery layer was skipped or rejected.

## 5. Resolve conflicts by version and channel

For a stable-release claim, prefer evidence in this order:

1. the official release or tag for the target stable version;
2. official documentation pinned to that version or tag;
3. tagged source and tests;
4. current unversioned official docs, with their channel and version made explicit;
5. documentation indexes and search results as discovery or cache layers.

Tagged source and tests outrank stale prose about implemented behavior. Canary-only changes do not describe stable automatically. If authoritative evidence remains split or the target cannot be established, use `INCONCLUSIVE`.

Completion criterion: every conflict is resolved with target-version evidence or remains explicitly `INCONCLUSIVE`; no conclusion depends on guessing.

## 6. Publish the verdict

Before publishing an architecture, migration, security, compatibility, issue-resolution, or implementation conclusion:

- state the repository pin, intended target, latest stable version and release date, and any mismatch;
- place direct official links next to the claims they support;
- label inference separately from sourced fact;
- issue exactly one verdict: `VERIFIED`, `CONTRADICTED`, or `INCONCLUSIVE`;
- re-check time-sensitive release facts if upstream changes during the research window.

In shared artifacts, publish the relevant engineering evidence and verdict without private configuration, credentials, tool mechanics, or personal workflow details.

Completion criterion: a reviewer can reproduce every material conclusion from public links and repository sources without relying on model memory or private configuration.

## Gate

The gate passes only when the target is explicit, the evidence matrix is complete, conflicts are resolved or disclosed, and the verdict is reproducible. Otherwise return `INCONCLUSIVE` and keep the version-sensitive assertion out of downstream decisions as an established fact.
