# Refactoring Handoff Report (`REFACTOR_HANDOFF.md`)

Use this template after completing approved refactoring tranches so any future AI or developer can diagnose edge issues with tiny, targeted fixes instead of blind patching.

## 1. Safety & Rollback Anchor

- **Original Backup Branch / Tag**: `<branch-or-tag>`
- **One-Command Full Rollback**: `git reset --hard <tag-or-commit>`
- **Commits Applied (One Per Tranche)**: `<list of commit hashes and module names>`

## 2. Module-by-Module Before vs. After

| Module / UI Category | Action Taken | Before (Draft / Patched State) | After (Clean State) | Locked External Contract |
| :--- | :--- | :--- | :--- | :--- |
| `<name>` | `Rewrite` / `Consolidate` / `Prune` | `<what was messy>` | `<new clean structure>` | `<inputs, outputs, invariants>` |

## 3. Verification Results

- **Characterization / Snapshot Checks**: `<passed / total>`
- **Performance Benchmark Delta**: `<before vs after on hotspots>`
- **Build & Smoke Tests**: `<command and result>`

## 4. Guide for Follow-Up AI (If Anything Breaks in Manual Testing)

If a regression appears during manual testing, pass this file to a follow-up AI with the failing symptom and affected module name from Section 2. Fix only against the locked contract without re-introducing global patches.
