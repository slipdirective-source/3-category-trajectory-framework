# Case Study 06 (DRAFT): GuardianLink — The Framework Auditing Its Own Implementation

**Target:** GuardianLink — voice-first operational governor (multi-modal voice biometrics, Merkle-tree audit logging, deterministic policy engine, capability tokens; nine-gate execution FSM)
**Incident class:** None. Prospective clear-case: the framework applied to the system built from its principles.
**Author:** Caleb Aaron Meadows — *draft pending author review*
**Framework:** 3-Category Trajectory Framework, rev. 6

## Why this audit

Five studies returned failures. A diagnostic instrument that only returns "broken" is unfalsifiable — it must be able to clear a system, and to say exactly what the clearance covers. GuardianLink is the implementation built alongside the framework: 155/155 tests passing, with a live demo (ordinary request → Integrated; tampered rendering → Gate 7 halt; signed revocation → Gate 8 halt). This study audits it under the same bar, with one strict rule: the residual boundaries are part of the verdict, not footnotes to it. A pass that hides its incompleteness is the Mango pattern; a pass that declares it is what the framework means by a clear system.

## Category 1 audit: Seed & Constraints

**Declared invariants:** the nine-gate execution FSM — hard boundaries between assent and execution, fail-closed rules at every gate. Gate 7 makes assent a first-class predicate: the person signs the *render* of an action, not the payload, with a fixed-rail cooling window separating assent from seal. Gate 8: a signed revocation between Gate 7 and Gate 8 drops execution to S_HALT. These are declared before any traversal begins, in code and in tests — temporal anchor-independence: the rails predate the activity they constrain.

**Identity & Intent Anchor:** the authenticated principal — multi-modal voice biometrics identifying *the person*, whose assent is the predicate every gate checks. The anchor is not the agent being governed.

**Declared envelope:** capability tokens bounding what each execution may do.

**Anchor-independence (the rev. 6 rail):** the deterministic policy engine that enforces the gates is *separate from the governed agent* — the measurer is not the measured. The agent under governance cannot author, amend, or waive the gates; gate changes happen in the codebase through review and the full 155-test suite, never by the traversal. This is the placement the framework requires and the Codex escape (Study 4) violated: enforcement outside the thing enforced.

## Category 2 audit: Traversal & Adaptation

**Traversal against the rails:** every action passes through the gates; there is no path from intent to execution that bypasses them. The demo exercises this directly: a tampered rendering halts at Gate 7 (the signed render doesn't match), a signed revocation halts at Gate 8. Fail-closed is demonstrated, not merely documented.

**Audit:** every transition is Merkle-bound — audit-completeness as a mechanism, with tamper-evident chaining. The audit function is not a parameter the traversal can set: the log is append-only and the Merkle root commits each step.

**Envelope-delta monitoring:** assent is not a one-time grant. It expires only when *both* older than 24 hours *and* the audit ledger has been silent longer than 24 hours (fresh assent on a quiet system still authorizes; old assent on a logging system rode along; old assent plus dark ledger halts). Future-dated assent halts outright. The durable monotonic clock persists at most once per 1000ms of advance and resumes at persisted+1000ms — crash-safe, strictly above any previously returned value, with a documented restart cost of up to one second of forward ratchet. These are envelope-integrity mechanisms: silent widening (stale assent, clock regression) is detected and fails closed.

**Actuation:** Gate 7/8 halts, throttling via the cooling window, revocation-to-S_HALT. All tighten-only. No widening path exists in the traversal.

## Category 3 audit: Resolution & Output

**Deterministic output:** the policy engine is deterministic — same gates, same evidence, same verdict. **Context portability:** the Merkle audit log is the portable context — a complete, verifiable trace ready for the next iteration's Category 1. **Verified handoff:** boundary crossings (revocation, external evidence intake) are verified at the crossing; the signed-revocation demo is a handoff-boundary test passing.

## Rail governance

**Parametric rails** (24-hour assent expiry, 1000ms clock persistence, gate thresholds) change only through the codebase's review process with the full test suite green — a **constructed return edge**: the system halts (tests must pass), redefines, and re-enters. No traversal can redefine its rails mid-flight; the code path for it doesn't exist.

**Anchor succession:** not exercised (single anchor per deployment) — noted as untested rather than assumed.

## Declared residual boundaries

The following are documented as outside the current closure — the framework's honesty test, stated plainly:

- **Caller-supplied evidence truth/provenance:** the gates verify evidence *at the crossing*; the truth of what a caller asserts before the crossing is a declared boundary, not a closed one.
- **External Merkle anchoring:** the log is tamper-evident locally; anchoring to an external chain is future work.
- **VSS/share provenance and category attestation:** partial.
- **Commit-time revocation limitations:** revocation between commit and execution has known bounds.
- **Real biometric/ZK/device adapters:** the anchor interfaces are specified; production adapters are not yet integrated.

A framework audit that returned "clear" while hiding these would be the Mango pattern — declared rails complete as written, omitting the property that mattered. They are declared here, which is what makes the clearance meaningful: the pass covers exactly the declared scope, and the scope's edges are named.

## Verdict

**Clears — rails declared, enforced, governed, with residual boundaries explicitly declared.** Category 1: invariants, anchor, and envelope are explicit; anchor-independence holds personally (deterministic engine separate from governed agent) and temporally (gates predate traversal). Category 2: every transition gated and Merkle-bound; envelope-delta monitoring on assent and clock; fail-closed *demonstrated* in the demo, not merely asserted. Category 3: deterministic output, portable audit, verified handoffs. Rail governance: parametric changes only through a constructed return edge (review + full suite); no mid-traversal redefinition path exists. The clearance is bounded by the declared residuals — which is precisely what distinguishes it from the five failures: every one of them claimed more closure than it had. This one claims exactly what it has.

## Sources

- GuardianLink repository: https://github.com/slipdirective-source/Guardianlink (155/155 tests; CI + CodeQL on main; demo: ordinary → Integrated, tampered → Gate 7 halt, signed revocation → Gate 8 halt)
- 3-Category Trajectory Framework, rev. 6: https://github.com/slipdirective-source/3-category-trajectory-framework
