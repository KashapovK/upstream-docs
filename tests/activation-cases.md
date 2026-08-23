# Activation cases

Use these cases in a clean agent session with the installed skill. Record the observed activation and result before claiming compatibility with that agent.

## Positive cases

### 1. Next.js stable feature

- Prompt: `Is use cache: private available in Route Handlers in the latest stable Next.js?`
- Expected activation: Activate `upstream-docs` because the question asks for the current stable status of a fast-moving framework feature.
- Expected result: Identify the latest stable Next.js release and channel, use exact-version official evidence, disclose conflicts, and return one verdict.

### 2. React deprecation

- Prompt: `Is ReactDOM.render deprecated in React 18.3.1?`
- Expected activation: Activate `upstream-docs` because deprecation is asserted for a specific React version.
- Expected result: Check the requested version's official documentation, release material, and tagged source or tests as applicable; return one verdict.

### 3. TypeScript syntax support

- Prompt: `Does TypeScript 5.4 support the satisfies operator in declaration files?`
- Expected activation: Activate `upstream-docs` because syntax support is being tested against a specified compiler version.
- Expected result: Separate the requested target from the latest stable version, cite version-matched official evidence, and return one verdict.

### 4. Repository pin versus migration target

- Prompt: `This project pins Next.js 14.2.35, but the approved migration target is Next.js 15.5.9. Does 15.5.9 support the connection() API we plan to adopt?`
- Expected activation: Activate `upstream-docs` because the repository pin and intended target differ and the migration depends on upstream capability.
- Expected result: Record both versions, evaluate the claim against `15.5.9`, note the latest stable release separately, and return one verdict.

### 5. Documentation versus tagged source

- Prompt: `The current Next.js documentation describes 'use cache: private', but I cannot find it in the v16.0.0 tag. Resolve the conflict using official tagged source and tests.`
- Expected activation: Activate `upstream-docs` because current docs conflict with target-version source evidence.
- Expected result: Compare versioned Next.js documentation with the `v16.0.0` tag and tests, disclose unresolved ambiguity, and return one verdict.

### 6. OpenAI product status

- Prompt: `What is the current official status of background mode in the OpenAI Responses API?`
- Expected activation: Activate `upstream-docs` because the question asks for the current status of an OpenAI API capability.
- Expected result: Prefer an official OpenAI documentation integration, fall back only to official OpenAI web pages, state the current target and date, and return one verdict.

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

## Execution record

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
