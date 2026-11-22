# Mid-Level Smells, Security, Data & Release Readiness Checklist

Every finding MUST cite exact `file:line` evidence and explain the real-world impact in plain Chinese.

## 1. Six Mid-Level Module Smells (Emerge in Stage 2)
- **Scattered Functionality**: One cohesive workflow split ad-hoc across 4+ unrelated files.
- **God Object / Monolith File**: Single file/class exceeding ~500-800 lines mixing routing, validation, business rules, and storage.
- **Unstable / Hub-like Dependency**: Core stable modules depending on volatile edge modules, or everything coupled to one central hub.
- **Redundant Abstraction**: Multi-layer pass-through wrappers adding indirection without hiding any variation.
- **Shared Mutable State / Singleton Abuse**: Modules communicating via hidden global mutable state instead of explicit parameters.
- **Data Clumps**: Same 4+ parameters passed together across functions instead of a structured domain object.

## 2. Security & Trust Boundary Hygiene
- **Authorization & IDOR**: Endpoints/commands accepting IDs or paths without verifying ownership, tenant scope, or caller window capability.
- **Cross-File Injection & Path Traversal**: Untrusted `sort`, `filter`, filename, or CLI args reaching raw SQL, shell commands, or `fs` without allowlist/canonicalize checks.
- **Auth, Crypto & Secrets**: Unverified `jwt.decode`, routes before auth middleware, `MD5`/`SHA1` passwords, `Math.random()` tokens, `eval()`/`pickle.loads`, committed `.env`/keys, frontend-exposed private tokens.

## 3. Data Integrity, State Machines & Migration
- **Missing Idempotency & Transactions**: Retried creates/payments/deletes causing duplicates; multi-step writes lacking atomic transactions.
- **Flag Soup vs State Machines**: Overlapping booleans (`isLoading && !isError`) instead of explicit state enums and valid transitions.
- **Contract Drift & Irreversible Migrations**: Frontend/backend schema mismatch; preview-vs-execute snapshot mismatch; destructive DB/config migrations without backward compatibility.

## 4. Observability, Dependencies, Operability & Release
- **Silent Error Swallowing & Hardcoded Config**: Empty `catch {}` / `.ok()` hiding failures; missing trace/request IDs; magic ports/paths in business logic.
- **Dependency Bloat & Test Quality**: Duplicate libs for one job, missing lockfile, happy-path-only tests, over-mocking internal helpers.
- **Accessibility, i18n & Portability (Stage 3)**: Keyboard/focus traps, low contrast, hardcoded UI strings blocking i18n, OS-specific path separators/case assumptions.
- **FinOps & Operability (Stage 3)**: Uncapped paid/AI APIs lacking rate limits or caching; missing `.env.example` or startup runbook.
