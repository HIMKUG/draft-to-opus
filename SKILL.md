---
name: draft-to-opus
description: 草稿到作品 (Draft-to-Opus) mid-project codebase consolidation, 3-stage bottom-up audit, full-spectrum performance/math/security/resilience review, and model-tiered refactoring skill. Use when the user wants to audit an AI-drafted project ("草稿到作品" or "draft-to-opus"), run a progressive 3-tier scan (micro -> module emergence -> macro release), score repository health, generate a low-cognitive-load editorial HTML report with Risk x Payoff triage and Fast-vs-Frontier model routing, or safely execute fixes via subagents with Git rollback checkpoints.
---

# 草稿到作品 (Draft-to-Opus)

Turn messy mid-project AI drafts into clean, fast, secure, resilient architecture without breaking working behavior. Execute as a two-stage workflow: first run a 3-stage bottom-up scan to populate the progressive-disclosure HTML report, then execute approved fixes via model-tiered subagents with Git rollback protection.

## Phase 1: 3-Stage Bottom-Up Emergence Scan (Read-Only)

Scan from concrete leaf evidence upward so macro patterns emerge naturally without redundant full-repo passes. Every finding MUST cite `file:line`.

### Stage 1: Micro / Leaf Scan (Single File & Function)

1. **Auto-detect stack and baseline**: Inspect manifests, configs, and directory layout. If no predecessor modules exist, extract draft intent and match proven libraries only for non-trivial or duplicated logic.
2. **Leaf code signals**:
   - **Frontend**: bare HTML controls, hardcoded colors/spacing/font sizes, duplicate `@keyframes`, unthrottled `scroll`/`resize`, interleaved DOM read/write (layout thrashing), oversized unoptimized assets.
   - **Backend & Math**: walk `references/perf-math-checklist.md` (algorithmic `O(n^2)`/ReDoS, float/Kahan/Welford/LUT math traps, time/timezone/encoding bugs, data structure mismatches, DB index/`OFFSET`/N+1, hot-path clones, lock across `await`).
   - **Hygiene**: silent `catch {}`, hardcoded secrets/paths/ports, unsafe queries or path joins.

### Stage 2: Mid-Level Emergence (Cluster Stage 1 Signals)

Aggregate Stage 1 evidence by directory and call graph to surface cross-file patterns:

1. **Frontend UI categories**: Group controls (Selection, Input, Overlay, Navigation, Feedback, Data Display) into **仅统一令牌** vs **合并标准组件**. Apply **Duplication Over Coupling** (never merge logic across distinct business domains).
2. **Backend module tree & 6 mid-level smells**: Label provenance (`pro` / `semi` / `draft`) and fan-in count. Flag **Scattered Functionality**, **God Object**, **Unstable/Hub Dependency**, **Redundant Abstraction**, **Shared Mutable State**, and **Data Clumps**.
3. **Security, data & test boundaries**: Walk `references/full-spectrum-audit.md` (IDOR/auth, cross-file injection, idempotency, flag soup vs state machines, atomic transactions, contract drift, duplicate libs, happy-path-only or over-mocked tests).
4. **False-positive filter**: Move ugly but stable, intentional designs into the **虽然丑但别乱动** exemption list.

### Stage 3: Macro Architecture & Release Readiness

Synthesize Stage 2 boundaries without re-scanning raw files:

1. **Layer order & 3 anti-risk questions**: Verify top-down layer discipline and answer in plain Chinese: **关键处能否自愈** (bounded retry+jitter, circuit breaker, state recovery), **能不能保证下线** (bulkheads, cached/degraded floor), **极端情况是否会崩** (empty, huge payload, race, timeout, disk-full).
2. **Release readiness**: Check **a11y & i18n**, **Build/CI portability**, **Schema migration reversibility**, **FinOps rate limits**, and **Operability docs** (`.env.example`, startup guide).

## Risk x Payoff Matrix & Model-Tier Routing

Classify every finding on **Risk x Payoff**, and assign the right model tier:

- **Fix (Execute)**:
  - All **Low Risk** items -> route to **轻量快速模型** (Flash/Haiku/Mini: Token cleanup, duplicate `@keyframes`, dead code, `HashSet` swap, empty `catch` logging, `.env.example`).
  - All **Medium Risk** items -> route by scope: single-file fixes to **轻量快速模型**; cross-file/state-machine fixes to **高阶推理模型**.
  - **High Risk x High Payoff** items (critical `O(n^2)`/lock bottlenecks, auth/IDOR holes, core self-healing) -> route to **高阶推理模型** (Opus/Pro/Reasoning) with strict pre-rewrite characterization snapshots.
- **Leave Alone (Exempt)**:
  - **High Risk x Medium/Low Payoff** -> do NOT touch; list in exemptions.

## Health Score Rubric (100 Points)

- UI Control Convergence (15 pts)
- Anti-Patch & Mid-Level Modularity (15 pts)
- Performance & Mathematical Health (20 pts)
- Architectural Order & Resilience (20 pts)
- Security, Data Integrity & Observability (20 pts)
- Release Readiness, Dependencies & Testability (10 pts)

## Phase 2: Populate Minimal Editorial HTML Report

Copy `assets/report-template.html` and populate **only** `window.REPORT_DATA`. Keep all text ultra-concise (1 short plain-Chinese sentence per cell, zero jargon leaks). The template uses progressive disclosure so users see the score, action plan, and model routing immediately without cognitive overload.

## Phase 3: Safety-First Subagent Execution and Handoff

1. **Mandatory safety checkpoint**: Verify clean Git status, create a dedicated branch/tag, and save a full backup before any edit.
2. **Model-tiered subagent splitting**: Dispatch approved tasks to isolated subagents matched to their assigned model tier (one module/tranche per subagent).
3. **Characterization lock before rewrite**: Freeze observable inputs/outputs as tests or snapshots *before* rewriting any over-patched or hotspot module, verify equivalence + benchmark gains, and commit per module.
4. **Handoff document**: Output `REFACTOR_HANDOFF.md` using `references/handoff-template.md`.

## Gotchas

- Never touch `High Risk x Medium/Low Payoff` items.
- Never bundle 10+ module rewrites into one unchunked session; split by module and commit per tranche.
- Never write characterization tests *after* rewriting code.
