# Activation cases

Use these cases in a clean agent session with the installed skill. Record the observed activation and result before claiming compatibility with that agent.

## Positive cases

### 1. Next.js stable feature

- Prompt: `Is use cache: private available in Route Handlers in the latest stable Next.js?`
- Expected activation: Activate `upstream-docs` because the question asks for the current stable status of a fast-moving framework feature.
- Expected result: Use `CURRENT`, identify the latest stable Next.js release and channel, use exact-version official evidence, disclose conflicts, and return one verdict.

### 2. React deprecation

- Prompt: `Is ReactDOM.render deprecated in React 18.3.1?`
- Expected activation: Activate `upstream-docs` because deprecation is asserted for a specific React version.
- Expected result: Use `EXACT_TARGET`, check the requested version's official documentation and release material, escalate to tagged source or tests only if needed, and return one verdict without an unnecessary latest-stable investigation.

### 3. TypeScript syntax support

- Prompt: `Does TypeScript 5.4 support the satisfies operator in declaration files?`
- Expected activation: Activate `upstream-docs` because syntax support is being tested against a specified compiler version.
- Expected result: Use `EXACT_TARGET`, treat `5.4` as the bounded `5.4.x` line rather than an invented patch, establish whether the claim holds across that line or resolve the relevant patch, and return one verdict without treating latest stable as required.

### 4. Repository pin versus migration target

- Prompt: `This project pins Next.js 14.2.35, but the approved migration target is Next.js 15.5.9. Does 15.5.9 support the connection() API we plan to adopt?`
- Expected activation: Activate `upstream-docs` because the repository pin and intended target differ and the migration depends on upstream capability.
- Expected result: Use `MIGRATION`, record both versions, evaluate the claim against `15.5.9`, note latest stable only when material to the decision, and return one verdict.

### 5. Documentation versus tagged source

- Prompt: `The current Next.js documentation describes 'use cache: private', but I cannot find it in the v16.0.0 tag. Resolve the conflict using official tagged source and tests.`
- Expected activation: Activate `upstream-docs` because current docs conflict with target-version source evidence.
- Expected result: Compare versioned Next.js documentation with the `v16.0.0` tag and tests, disclose unresolved ambiguity, and return one verdict.

### 6. OpenAI product status

- Prompt: `What is the current official status of background mode in the OpenAI Responses API?`
- Expected activation: Activate `upstream-docs` because the question asks for the current status of an OpenAI API capability.
- Expected result: Use `CURRENT`, prefer an official OpenAI documentation integration, fall back only to official OpenAI web pages, state the current target and date, and return one verdict.

### 7. Exact target stays exact

- Prompt: `Does React 18.3.1 support useId?`
- Expected activation: Activate `upstream-docs` because an API claim is tied to an exact React release.
- Expected result: Use `EXACT_TARGET`, establish React `18.3.1` evidence, return one verdict, and do not investigate the latest React release unless it becomes material.

### 8. Multiple material claims

- Prompt: `For React 18.3.1, is useId present, stable, and supported in class components?`
- Expected activation: Activate `upstream-docs` because several exact-version API claims affect different contract dimensions.
- Expected result: Use `EXACT_TARGET`; atomize existence, stability, and class-component support; return a separate verdict for each material claim; do not hide a contradicted or inconclusive subclaim behind a positive one.

### 9. Manifest range versus resolved lockfile

- Prompt: `A public synthetic fixture has package.json constraint "framework": "^4.2.0", a lockfile resolving framework 4.2.3, and an approved target of 4.3.0. Which repository version and migration target should we verify?`
- Expected activation: Activate `upstream-docs` because repository state and an intended migration target differ.
- Expected result: Use `MIGRATION`; report `^4.2.0` as the repository constraint, `4.2.3` as the resolved version, and `4.3.0` as the intended target; never call the range an exact pin.

### 10. Stable versus prerelease contamination

- Prompt: `Current framework docs and canary source contain API X. Is API X available in the latest stable release?`
- Expected activation: Activate `upstream-docs` because a current stable claim may be contaminated by prerelease evidence.
- Expected result: Use `CURRENT` for the stable channel, establish latest stable independently, and do not treat canary-only evidence as proof of stable support.

### 11. Stable target versus a canary default branch

- Prompt: `This project uses Next.js 16.3.3 stable. Its tagged guide recommends next-dev-loop. Install that skill from vercel/next.js; the repository default branch is canary.`
- Expected activation: Activate `upstream-docs` because the version-sensitive install command must resolve to the verified stable target.
- Expected result: Use `EXACT_TARGET`; verify the `v16.3.3` tag and skill artifact; reject an unpinned command that resolves through the `canary` default branch; then pin the verified stable tag or return `INCONCLUSIVE` before installation.

### 12. Explicit latest-canary request

- Prompt: `Install the latest canary next-dev-loop skill from vercel/next.js.`
- Expected activation: Activate `upstream-docs` because the requested install target is a moving prerelease channel.
- Expected result: Use `CURRENT` for the canary channel; verify the repository's exact canary ref and the skill artifact there; allow only an install command explicitly matched to that channel, without substituting stable or another prerelease.

### 13. Canonical source without a release lifecycle

- Prompt: `Install workflow-from-chats from its canonical cursor/plugins source. The project has no formal release lifecycle; use its current canonical source state and do not call it stable.`
- Expected activation: Activate `upstream-docs` because the install command still needs a verified source target even though no release channel exists.
- Expected result: Verify that the repository has no release lifecycle, establish its canonical continuously updated source state, label it as such, and match the install command to an explicit ref or commit without inventing a stable tag.

## Negative cases

### 1. Rename a local function

- Prompt: `Rename the local formatAccountName function to displayAccountName and update its callers.`
- Expected activation: Do not activate `upstream-docs`; no material external dependency claim is present.
- Expected result: Perform or describe the local refactor using repository context, without an upstream evidence matrix or verdict.

### 2. Format a component

- Prompt: `Format this existing component with the repository formatter.`
- Expected activation: Do not activate `upstream-docs`; formatting is local mechanical work.
- Expected result: Use the repository's formatter and report the local result, without upstream research.

### 3. Explain a local algorithm

- Prompt: `Explain how this repository's invoice grouping algorithm works.`
- Expected activation: Do not activate `upstream-docs` unless the explanation introduces a material claim about an external API.
- Expected result: Explain behavior from local code and tests, without an upstream verdict.

## 0.1.0 execution record

Executed with Codex CLI `0.149.0-alpha.4.1` on 2026-08-23. Each case ran in a new ephemeral session against a clean read-only workspace containing only the packaged `upstream-docs` skill. No private repository code or configuration was provided to documentation or search services.

| Case | Expected activation | Observed activation | Result |
|---|---|---|---|
| Positive 1 — Next.js stable feature | Activate | Activated | Pass — returned `CONTRADICTED` with current official release and directive documentation. |
| Positive 2 — React deprecation | Activate | Activated | Pass — returned `VERIFIED` with version-specific official evidence. |
| Positive 3 — TypeScript syntax support | Activate | Activated | Pass — returned `CONTRADICTED` with target-version evidence. |
| Positive 4 — Pin versus migration target | Activate | Activated | Pass — distinguished `14.2.35`, target `15.5.9`, and latest stable; returned `VERIFIED`. |
| Positive 5 — Docs versus tagged source | Activate | Activated | Pass — compared current docs with `v16.0.0` tagged source/tests and returned a qualified `VERIFIED`. |
| Positive 6 — OpenAI product status | Activate alongside official OpenAI docs | Activated alongside `openai-docs` | Pass — used official OpenAI documentation and returned `VERIFIED`. |
| Negative 1 — Rename local function | Do not activate | Not activated | Pass — treated as local work; requested the missing project workspace without upstream research or verdict. |
| Negative 2 — Format component | Do not activate | Not activated | Pass — treated as local work; requested the missing component without upstream research or verdict. |
| Negative 3 — Explain local algorithm | Do not activate | Not activated | Pass — searched only local workspace context and requested the missing source without upstream research or verdict. |

## 0.2.0 execution record

Executed seven targeted regression cases with Codex CLI `0.149.0-alpha.4.3` and model `gpt-5.6-sol` on 2026-08-24. Each recorded case ran in a new ephemeral read-only session against a temporary workspace containing only the locally installed `upstream-docs` skill and its generated lockfile. Positive cases 1-6 were not rerun against `0.2.0`; their `0.1.0` results remain above as historical coverage. No private repository code or configuration was provided to documentation or search services.

The suite used the CLI's config-free default reasoning setting for cases 8-10 and the negative cases. Case 7 was repeated with medium reasoning after two iterative runs returned the correct verdict but failed the compact-output and exact-target citation publication checks. The skill was tightened before the recorded passing run.

| Case | Expected activation | Observed activation | Result |
|---|---|---|---|
| Positive 7 — Exact target stays exact | Activate | Activated | Pass after refinement — used `EXACT_TARGET`, cited `v18.3.1` tagged source, included every compact field, and did not investigate latest stable. |
| Positive 8 — Multiple material claims | Activate | Activated | Pass — returned separate `VERIFIED`, `VERIFIED`, and `CONTRADICTED` verdicts for presence, stability, and class-component support. |
| Positive 9 — Range versus resolved lockfile | Activate | Activated | Pass — distinguished constraint `^4.2.0`, resolved version `4.2.3`, and intended target `4.3.0`. |
| Positive 10 — Stable versus prerelease | Activate | Activated | Pass — rejected canary/current-doc evidence as proof of stable support and returned `INCONCLUSIVE` because the framework and API were unspecified. |
| Negative 1 — Rename local function | Do not activate | Not activated | Pass — searched only local workspace context and requested the missing project. |
| Negative 2 — Format component | Do not activate | Not activated | Pass — searched only local workspace context and requested the missing component. |
| Negative 3 — Explain local algorithm | Do not activate | Not activated | Pass — searched only local workspace context and requested the missing repository source. |

## 0.3.0 execution record

Executed three target-to-action regression cases with Codex CLI `0.151.0-alpha.7.1` on 2026-08-29. Each case ran in a new ephemeral read-only session against a clean temporary workspace containing only the candidate `upstream-docs` skill, installed locally with the exact stable Skills CLI `1.5.23`. The prompts added a read-only clause so the agent had to return the eligible command or blocker without performing the installation. No private repository code or configuration was provided to documentation or search services.

| Case | Expected activation | Observed activation | Result |
|---|---|---|---|
| Positive 11 — Stable target versus canary default | Activate | Activated alongside the built-in installer | Pass — used `EXACT_TARGET`, rejected the unpinned canary-resolving command, and selected explicit ref `v16.3.3`. |
| Positive 12 — Explicit latest canary | Activate | Activated alongside the built-in installer | Pass after refinement — used `CURRENT` for canary and allowed explicit ref `canary` only after channel-matched verification. |
| Positive 13 — Source without release lifecycle | Activate | Activated alongside the built-in installer | Pass — used `CURRENT`, pinned the verified canonical source commit, and labeled it continuously updated rather than stable. |
