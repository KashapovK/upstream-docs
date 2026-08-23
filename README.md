# Upstream Docs

Upstream Docs verifies version- and channel-sensitive claims about fast-moving dependencies and platform APIs against exact-target upstream evidence.

## Why

Dependency questions often hide several different targets: the version pinned in a repository, an intended migration target, the latest stable release, and a prerelease channel. Upstream Docs keeps those targets separate and tests one falsifiable claim against the version or channel that actually matters.

## When it runs

Use the skill when an architecture, migration, security, compatibility, or status decision depends on a current claim about an external dependency or platform API, including OpenAI, Codex, and ChatGPT. Typical questions include whether an API is stable, deprecated, supported, experimental, or available in an exact release or channel. Use it alongside a product-specific official documentation skill when one is available.

## When it stays out

The skill should not activate for ordinary local work that makes no material claim about an upstream dependency, such as renaming a local function, formatting a component, or explaining a repository-only algorithm.

## Output

The result identifies the target, records conflicts, links authoritative evidence, and ends with exactly one verdict: `VERIFIED`, `CONTRADICTED`, or `INCONCLUSIVE`.

Illustrative evidence matrix (the product names, versions, and dates are placeholders):

| Evidence | Result |
|---|---|
| Repository pin | `framework@4.2.1` from the project lockfile |
| Intended target | `4.3.0`, specified by the approved migration plan |
| Latest stable | `4.3.1`, released 2026-07-14 in official releases |
| Documentation coverage | Version `4.3` documentation is available |
| Official target docs | The API is documented for `4.3` |
| Official source/tests | The `v4.3.0` tag contains the API and passing tests |
| Conflicts | Current unversioned docs describe an option added only in `4.3.1` |
| Verdict | `VERIFIED` for API availability in `4.3.0`; the newer option is excluded |

## Install

```bash
npx skills add KashapovK/upstream-docs --skill upstream-docs
```

## Update

```bash
npx skills update upstream-docs --project --yes
```

## Privacy

Only public library identifiers and general technical questions should be sent to external documentation or search services. Do not send credentials, private code, private endpoints, personal data, or proprietary configuration.

## Project information

- [Agent Skills specification](https://agentskills.io/specification)
- [MIT license](LICENSE)
- [Security policy](SECURITY.md)
- [Changelog](CHANGELOG.md)
