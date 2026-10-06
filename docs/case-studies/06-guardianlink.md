# Case Study 06 (DRAFT): GuardianLink — The Framework Auditing Its Own Implementation

**Target:** GuardianLink — voice-first operational governor scaffold (deterministic nine-gate policy engine, Merkle-tree audit logging; biometric, ZK, and capability-token adapters specified, not integrated)
**Incident class:** None. Prospective clear-case: the framework applied to the system built from its principles.
**Author:** Caleb Aaron Meadows — *draft pending author review*
**Framework:** 3-Category Trajectory Framework, rev. 7
**Audited commit:** `a53919f` (main, 2026-10-06) — 165/165 tests green. All code claims below are pinned to this commit. (An earlier pin, `3048a5d`, predates the structural fixes described below; the residuals named there for instrument ownership, assent binding, and ledger timestamps are closed at this commit.)

## Self-audit declaration

This study is a self-audit: the framework's author auditing his own implementation, and the 165 tests were written by the same party. Per the audit-independence requirement, this is declared here, self-deception is named as the failure mode, and the verdict is stated as clearing against declared rails **as attested by the author, pending independent review**. The study's value is not the clearance — an unattested clearance would be the measurer-is-the-measured configuration the rail forbids — but the demonstration that the instrument can return a bounded pass: rails declared, enforced, governed, residual scope named, and the audit's own limits declared alongside.

## Category 1 audit: Seed & Constraints

**Declared invariants:** the nine-gate execution FSM — hard boundaries between assent and execution, fail-closed rules at every gate. Gate 7 makes assent a first-class predicate: the person signs the *render* of an action, not the payload, with a fixed-rail cooling window separating assent from seal. Gate 8: a signed revocation between Gate 7 and Gate 8 drops execution to S_HALT. These are declared before any traversal begins, in code and in tests.

**Identity & Intent Anchor:** specified as an interface, not implemented as a sensor. Gate 1 compares a presented biometric template against an enrolled commitment within a distance bound (theta_bio) — but both vectors are caller-supplied evidence, checked for consistency and bounds, not truth; a caller can supply self-consistent lies. The anchor is not the agent being governed, and the real biometric/ZK/device adapters are declared residuals (see below).

**Declared envelope:** gate thresholds and rail configuration bounding what each execution may do. Capability tokens are the specified production mechanism — not implemented.

**Anchor-independence of the rails:** the deterministic policy engine that enforces the gates is *separate from the governed agent* — the measurer is not the measured. The agent under governance cannot author, amend, or waive the gates. Temporally: the gates are deployed code the traversal cannot rewrite — the prior declaration resists revision *by the bound party* (the agent), satisfying the rev. 7 resistance condition against the traversal. A runtime rail lifecycle exists (`PolicyEngine.registerHardRail`/`revokeHardRail`), gated by a `RailAuthorizer` hook whose default is deny-all: an engine constructed without an authorizer cannot arm or disarm any hard rail — the path exists and is closed by default, not absent. The authenticated authorizer (capability tokens, signed admin commands) is specified, not built. Code-level revision requires the developer, outside the traversal, to push through review and the full 158-test suite — the constructed return edge. Note the precise boundary: independence holds against the governed agent, not against the author or the deploying host. The author's ability to revise his own gates is the personal-agency configuration; the failure mode is self-deception, named here as such, and the mitigation is that every revision passes through the constructed return edge rather than executive discretion.

## Category 2 audit: Traversal & Adaptation

**Traversal against the rails:** every action *evaluated by the engine* passes through the gates — the demo exercises this directly: a tampered rendering halts at Gate 7 (the signed render doesn't match), a signed revocation halts at Gate 8. Fail-closed is demonstrated, not merely documented. But "no path from intent to execution bypasses the gates" cannot be established: no execution path exists in the repo. `ConciergeInterface` calls `PolicyEngine` with its own ledger and never touches `NineGates`; only the `Main` demo wires the gate engine. Composition — the gates actually guarding a live execution path — is unbuilt, and is named as a residual.

**Audit:** every transition is Merkle-bound — audit-completeness as a mechanism, with tamper-evident chaining. The governed agent cannot reconfigure or rewrite the log. The engine owns its instruments: clock and ledger are constructor-supplied to `NineGates`, never taken from the request — `GateContext` carries evidence only, and the engine's audit appends carry engine-attested timestamps from its owned clock (a `MerkleAuditLog.append` overload for raw millisecond timestamps; direct external appends keep the documented caller-supplied caveat). A request cannot substitute the engine's time source or audit sink — the structural fix for the finding that instruments belonged to the caller, which also contradicted the codebase's own rule (FrictionStateMachine: time is "a capability of the deployment, not an argument of the request"). Resistance against host-level tampering is partially addressed: sealed roots are published through an `ExternalAnchor` port, with a bundled append-only, hash-chained file anchor (durable, out-of-process, tamper-evident — host-filesystem grade, not disk-attacker-proof; a write-once store or timestamping authority behind the port is the recourse for host-adversary resistance).

**Envelope-delta monitoring:** assent is not a one-time grant. It expires only when *both* older than 24 hours *and* the audit ledger has been silent longer than 24 hours (fresh assent on a quiet system still authorizes; old assent on a logging system rode along; old assent plus dark ledger halts). Future-dated assent halts outright. The durable monotonic clock persists at most once per 1000ms of advance and resumes at persisted+1000ms — crash-safe, strictly above any previously returned value, with a documented restart cost of up to one second of forward ratchet. This catches honest staleness — a quiet system, a crashed ledger. The hostile-caller case is closed structurally: the silence check reads the engine-owned ledger, and engine appends are engine-attested, so forged future-dated appends can no longer fake liveness through the request path (direct ledger writes outside the engine keep the documented caveat). Assent is now bound to time and action: `verifyAssent` receives `(rendering, assentedAtMs, actionId)` and the port contract requires the signature to bind the triple — backdating (skip the cooling window), refreshing (defeat idle expiry), and replay (one assent for any identical action) are defeated by the binding, with tests locking in both the triple handoff and a backdate-defeat case. A verifier that checks the rendering alone is documented as deployment-unsafe.

**Actuation:** Gate 7/8 halts, throttling via the cooling window, revocation-to-S_HALT. All tighten-only. No widening path exists in the traversal.

## Category 3 audit: Resolution & Output

**Deterministic output:** the policy engine is deterministic — same gates, same evidence, same verdict. **Context portability:** the Merkle audit log is the portable context — a complete, verifiable trace ready for the next iteration's Category 1. **Verified handoff:** boundary crossings (revocation, external evidence intake) are verified at the crossing; the signed-revocation demo is a handoff-boundary test passing.

## Rail governance

**Parametric rails** (24-hour assent expiry, 1000ms clock persistence, gate thresholds) change only through the codebase's review process with the full test suite green — a **constructed return edge**: halt, redefine, re-enter. No traversal can redefine its rails mid-flight; the runtime rail lifecycle is deny-all by default, and the authenticated path is not built.

**Anchor succession:** not exercised (single anchor per deployment) — noted as untested rather than assumed.

## Declared residual boundaries

The following are documented as outside the current closure — the framework's honesty test, stated plainly:

- **Caller-supplied evidence truth/provenance:** the gates verify evidence *at the crossing*; the truth of what a caller asserts before the crossing is a declared boundary, not a closed one.
- **External Merkle anchoring:** the log is tamper-evident locally; sealed roots are published through an `ExternalAnchor` port with a bundled append-only file anchor — durable and checkable, but host-filesystem grade, not resistant to a disk-write attacker. A write-once store or timestamping authority behind the port is the remaining step, which is also why the audit's independence is declared provisional (see verdict).
- **VSS/share provenance and category attestation:** partial.
- **Commit-time revocation limitations:** revocation between commit and execution has known bounds.
- **Real biometric/ZK/device adapters:** the anchor interfaces are specified; production adapters are not yet integrated.
- **Composition:** closed at the interface layer — `ConciergeInterface.execute` is the only execution path and forces every action through `NineGates`; the governed substrate is carried across calls, and each seal is anchored externally. What remains unwired is everything *outside* the interface: no production caller, device, or agent loop is yet bound to it.
- **Capability tokens / authenticated rail authorization:** the `RailAuthorizer` hook is specified with a deny-all default; the production authorizer (capability tokens, signed admin commands) is not built — the runtime rail lifecycle is closed by default, not absent.

A framework audit that returned "clear" while hiding these would be the Mango pattern — declared rails complete as written, omitting the property that mattered. They are declared here, which is what makes the clearance meaningful: the pass covers exactly the declared scope, and the scope's edges are named.

## Verdict

**Clears — the gate logic, against declared rails — as attested by the author, pending independent review, with residual boundaries declared as part of the verdict.** What clears is the engine's decision logic: the gates are independent of the traversal (deployed code the governed agent cannot rewrite; revisions only through the constructed return edge; the runtime rail lifecycle deny-all by default), the engine owns its instruments (clock and ledger are constructor-supplied, never taken from the request; revocations arrive through the port), assent is bound to time and action by the verifier contract, and composition is forced at the interface layer (`ConciergeInterface.execute` is the only execution path; seals are anchored through the `ExternalAnchor` port). What does not yet clear: the *audit* is not independent of the author (self-audit declared; 165 self-written tests; the bundled anchor is host-filesystem grade, not host-adversary-proof), and no production caller outside the interface is yet bound to it. Category 2 therefore demonstrates a governed *interface*, not yet a deployed *system* — and the study no longer claims otherwise. Both statements are true at once — that is what the rev. 7 rails-vs-audit distinction is for. What distinguishes this pass from the five failures is not perfection but the declared scope: every one of them claimed more closure than it had. This one claims exactly what it has — the logic and the interface, not the deployment — names who attests it, and states what would complete it: independent review, host-adversary-grade anchoring, and a production caller.

## Sources

- GuardianLink repository: https://github.com/slipdirective-source/Guardianlink — audited at commit `a53919f` (main, 2026-10-06); 165/165 tests; CI + CodeQL on main; demo (composed): ordinary → Executed + anchored, tampered → Rejected at Gate 7, signed revocation → Rejected at Gate 8
- 3-Category Trajectory Framework, rev. 7: https://github.com/slipdirective-source/3-category-trajectory-framework
