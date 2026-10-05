# 3-Category Trajectory Framework (rev. 5)

**Author:** Caleb Aaron Meadows
**Published:** 2026-10-05
**License:** CC BY 4.0 — share and adapt freely with attribution.
**Status:** Living document. Revisions are governed by the framework's own rail-governance rules below.

> *Things are known by the trajectory they produce, not by static description.*

---

A system moves from seed state to sovereign execution, then returns to a seed state for the next iteration. It applies to software execution layers, governance architectures, and cognitive loops.

## The categories (function layer)

### 1. Dynamic Primitive: Seed & Constraints

Everything that must be true before execution begins.

- **Invariant Enforcement:** hard boundaries, fail-closed rules, non-negotiable constraints
- **State & Environment Primaries:** initial inputs, execution-layer interfaces, the declared capability envelope
- **Identity & Intent Anchor:** the root against which everything later is verified
- **Anchor-mismatch abort:** if handed-in context fails verification against the active anchor, Category 1 hard-aborts. Nothing enters Category 2.

### 2. Feedback Loop: Traversal & Adaptation

Live evaluation, where the trajectory is actually produced.

- **Traversal:** active paths evaluated against the rails as they unfold
- **Audit:** every state transition recorded and verifiable
- **Adaptive Mediation:** rail actuation within existing rails

### 3. Sovereign Endpoint: Resolution & Output

Completion, validation, and handoff.

- **Deterministic Output:** validated, policy-compliant result or decision state
- **Context Portability:** serialization, audit closure, handoff
- **Autonomous Integrity:** every dependency crossing the boundary is verified at the crossing

## The cycle

Category 1 → Category 2 → Category 3 → return edge → Category 1

## Rail governance

**Core vs. parametric.** Core rails are constitutive of the framework: revising one exits the framework. Parametric rails are contingent settings within it (capability limits, thresholds, scope).

Core rails:

- **Non-contradiction**
- **Fail-closed** (constitutive of this framework, not a universal law; a fail-open system is possible, it just isn't this one)
- **Audit-completeness** (every transition recorded and verifiable; without it the root axiom fails)
- **Anchor-existence** (verification must terminate in a root)
- **Envelope-integrity** (see below)

**Envelope-integrity.** Effective capability may not exceed the declared envelope without detection. Implicit widening counts as a breach and triggers fail-closed. It includes:

- Scope creep through unbounded mediation
- Unverified context imports
- Silent capability inheritance
- Recursive exception handling that escalates privilege

Explicit widening happens only through redefinition at the return edge. Fail-closed is the response to a breach, and envelope-integrity is what makes the breach visible, so it is named separately.

**Actuation vs. redefinition.** Actuation is the rails working: narrowing scope, throttling, path isolation, fail-closed halts, circuit breakers. It is Category 2 business, always permitted, and may only tighten, never widen. Redefinition is the rails changing. It happens only at the return edge, and only for parametric rails.

**Authorship and validation.** A redefinition is authored through the Identity & Intent Anchor. The closed audit trace justifies it by documenting the failure that motivated it, but it doesn't validate the new value. The anchor's intent does.

**Anchor succession.** The anchor itself changes only by explicit succession:

- The incumbent anchor authorizes the successor.
- The succession event is recorded in the closing audit trace.
- The receiving iteration verifies the continuity chain at handoff.

An anchor that can't prove continuity from its predecessor is treated as a new anchor, and the handoff boundary applies in full. Two failure cases are distinct: unproven anchor continuity makes the anchor new, and the handoff is treated as a full boundary crossing. Context that fails verification against the active anchor is the anchor-mismatch hard abort.

**Handoff boundary.** The receiving iteration counts as inside the closure only after it re-enters Category 1 and verifies the handed-off context against its own anchor. Until then, the handoff is a boundary crossing and is treated as one.

## Phases

- **I. Invariant Initialization:** set the capability envelope and rails, verify handed-in context and anchor continuity, and abort on anchor mismatch.
- **II. Continuous Evaluation:** live cycles with every transition audit-bound, envelope-integrity monitored, and rail actuation as needed.
- **III. Terminal Resolution:** verify trace integrity, emit portable context, and submit any parametric redefinitions or anchor succession for the next Phase I.

## Mechanism layer (separate from the above)

Interchangeable implementations of the category functions:

- **Audit:** Merkle-tree logging, zero-knowledge verification
- **Anchor:** cryptographic root of trust, state proofs
- **Enforcement:** deterministic policy engine, graph evaluation, envelope-delta monitoring for envelope-integrity
- **Cognitive and governance cases:** the same functions with domain-appropriate mechanisms

---

*Developed through adversarial review across multiple AI systems (Muse, Copilot, Gemini, Claude), October 2026. Nothing here is trusted — only observed. If it's trusted, it's dead.*
