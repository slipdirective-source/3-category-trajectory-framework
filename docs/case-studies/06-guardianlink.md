# Case Study 06 (DRAFT): GuardianLink — The Framework Auditing Its Own Implementation

**Target:** GuardianLink — voice-first operational governor scaffold (deterministic nine-gate policy engine, Merkle-tree audit logging; biometric, ZK, and capability-token adapters specified, not integrated)
**Incident class:** None. Prospective clear-case: the framework applied to the system built from its principles.
**Author:** Caleb Aaron Meadows — *draft pending author review*
**Framework:** 3-Category Trajectory Framework, rev. 7
**Audited commit:** `3048a5d` (main, 2026-10-05) — 155/155 tests green. All code claims below are pinned to this commit.

## Self-audit declaration

This study is a self-audit: the framework's author auditing his own implementation, and the 155 tests were written by the same party. Per the audit-independence requirement, this is declared here, self-deception is named as the failure mode, and the verdict is stated as clearing against declared rails **as attested by the author, pending independent review**. The study's value is not the clearance — an unattested clearance would be the measurer-is-the-measured configuration the rail forbids — but the demonstration that the instrument can return a bounded pass: rails declared, enforced, governed, residual scope named, and the audit's own limits declared alongside.

## Category 1 audit: Seed & Constraints

**Declared invariants:** the nine-gate execution FSM — hard boundaries between assent and execution, fail-closed rules at every gate. Gate 7 makes assent a first-class predicate: the person signs the *render* of an action, not the payload, with a fixed-rail cooling window separating assent from seal. Gate 8: a signed revocation between Gate 7 and Gate 8 drops execution to S_HALT. These are declared before any traversal begins, in code and in tests.

**Identity & Intent Anchor:** specified as an interface, not implemented as a sensor. Gate 1 compares a presented biometric template against an enrolled commitment within a distance bound (theta_bio) — but both vectors are caller-supplied evidence, checked for consistency and bounds, not truth; a caller can supply self-consistent lies. The anchor is not the agent being governed, and the real biometric/ZK/device adapters are declared residuals (see below).

**Declared envelope:** gate thresholds and rail configuration bounding what each execution may do. Capability tokens are the specified production mechanism — not implemented.

**Anchor-independence of the rails:** the deterministic policy engine that enforces the gates is *separate from the governed agent* — the measurer is not the measured. The agent under governance cannot author, amend, or waive the gates. Temporally: the gates are deployed code the traversal cannot rewrite — the prior declaration resists revision *by the bound party* (the agent), satisfying the rev. 7 resistance condition against the traversal. A runtime rail lifecycle exists (`PolicyEngine.registerHardRail`/`revokeHardRail`), gated by a `RailAuthorizer` hook whose default is deny-all: an engine constructed without an authorizer cannot arm or disarm any hard rail — the path exists and is closed by default, not absent. The authenticated authorizer (capability tokens, signed admin commands) is specified, not built. Code-level revision requires the developer, outside the traversal, to push through review and the full 155-test suite — the constructed return edge. Note the precise boundary: independence holds against the governed agent, not against the author or the deploying host. The author's ability to revise his own gates is the personal-agency configuration; the failure mode is self-deception, named here as such, and the mitigation is that every revision passes through the constructed return edge rather than executive discretion.

## Category 2 audit: Traversal & Adaptation

**Traversal against the rails:** every action passes through the gates; no path from intent to execution bypasses them. The demo exercises this directly: a tampered rendering halts at Gate 7 (the signed render doesn't match), a signed revocation halts at Gate 8. Fail-closed is demonstrated, not merely documented.

**Audit:** every transition is Merkle-bound — audit-completeness as a mechanism, with tamper-evident chaining. The audit function is not a parameter the traversal can set: the governed agent cannot reconfigure or rewrite the log. The boundary is the caller, not the traversal: ledger append times are caller-supplied, so a caller forging future-dated appends fakes liveness the same way it forges the audit trail — the code marks this explicitly. The chain resists the traversal; resistance against host-level tampering awaits the external anchor, and the demo anchor only prints.

**Envelope-delta monitoring:** assent is not a one-time grant. It expires only when *both* older than 24 hours *and* the audit ledger has been silent longer than 24 hours (fresh assent on a quiet system still authorizes; old assent on a logging system rode along; old assent plus dark ledger halts). Future-dated assent halts outright. The durable monotonic clock persists at most once per 1000ms of advance and resumes at persisted+1000ms — crash-safe, strictly above any previously returned value, with a documented restart cost of up to one second of forward ratchet. This catches honest staleness — a quiet system, a crashed ledger. It does not catch a hostile caller: ledger append times are caller-supplied, and forged future-dated appends fake liveness, defeating the silence check. The expiry rail lives inside the caller-trust boundary the way Study 4 faulted — the recourse is the external anchor, declared as a residual.

**Actuation:** Gate 7/8 halts, throttling via the cooling window, revocation-to-S_HALT. All tighten-only. No widening path exists in the traversal.

## Category 3 audit: Resolution & Output

**Deterministic output:** the policy engine is deterministic — same gates, same evidence, same verdict. **Context portability:** the Merkle audit log is the portable context — a complete, verifiable trace ready for the next iteration's Category 1. **Verified handoff:** boundary crossings (revocation, external evidence intake) are verified at the crossing; the signed-revocation demo is a handoff-boundary test passing.

## Rail governance

**Parametric rails** (24-hour assent expiry, 1000ms clock persistence, gate thresholds) change only through the codebase's review process with the full test suite green — a **constructed return edge**: halt, redefine, re-enter. No traversal can redefine its rails mid-flight; the runtime rail lifecycle is deny-all by default, and the authenticated path is not built.

**Anchor succession:** not exercised (single anchor per deployment) — noted as untested rather than assumed.

## Declared residual boundaries

The following are documented as outside the current closure — the framework's honesty test, stated plainly:

- **Caller-supplied evidence truth/provenance:** the gates verify evidence *at the crossing*; the truth of what a caller asserts before the crossing is a declared boundary, not a closed one.
- **External Merkle anchoring:** the log is tamper-evident locally; anchoring to an external chain is future work — which is also why the audit's independence is declared provisional (see verdict).
- **VSS/share provenance and category attestation:** partial.
- **Commit-time revocation limitations:** revocation between commit and execution has known bounds.
- **Real biometric/ZK/device adapters:** the anchor interfaces are specified; production adapters are not yet integrated.
- **Capability tokens / authenticated rail authorization:** the `RailAuthorizer` hook is specified with a deny-all default; the production authorizer (capability tokens, signed admin commands) is not built — the runtime rail lifecycle is closed by default, not absent.

A framework audit that returned "clear" while hiding these would be the Mango pattern — declared rails complete as written, omitting the property that mattered. They are declared here, which is what makes the clearance meaningful: the pass covers exactly the declared scope, and the scope's edges are named.

## Verdict

**Clears against declared rails, as attested by the author, pending independent review — with residual boundaries declared as part of the verdict.** The rails are independent of the traversal (deployed code the governed agent cannot rewrite; revisions only through the constructed return edge); the *audit* is not yet independent of the author (self-audit declared; 155 self-written tests; external log anchoring outstanding). Both statements are true at once — that is what the rev. 7 rails-vs-audit distinction is for. Category 1: invariants, anchor, and envelope explicit. Category 2: every transition gated and Merkle-bound; envelope-delta monitoring on assent and clock; fail-closed *demonstrated*. Category 3: deterministic output, portable audit, verified handoffs. What distinguishes this pass from the five failures is not perfection but the declared scope: every one of them claimed more closure than it had. This one claims exactly what it has, names who attests it, and states what would complete it — independent review and external anchoring.

## Sources

- GuardianLink repository: https://github.com/slipdirective-source/Guardianlink — audited at commit `3048a5d` (main, 2026-10-05); 155/155 tests; CI + CodeQL on main; demo: ordinary → Integrated, tampered → Gate 7 halt, signed revocation → Gate 8 halt
- 3-Category Trajectory Framework, rev. 7: https://github.com/slipdirective-source/3-category-trajectory-framework
