# Case Study 05 (DRAFT): Auditing the Auditors — The Frontier Labs' Safety Frameworks

**Target:** The published self-governance frameworks of Anthropic (Responsible Scaling Policy v3.0, effective Feb 24, 2026), OpenAI (Preparedness Framework v2, Apr 2025), Google DeepMind (Frontier Safety Framework v3.1, Apr 17, 2026)
**Incident class:** Prospective audit — the industry's own rails, stress-tested before the failure
**Author:** Caleb Aaron Meadows — *draft pending author review*
**Framework:** 3-Category Trajectory Framework, rev. 6

## Why this audit

Twelve frontier companies have now published voluntary safety frameworks. These documents are the industry's declared rails. The trajectory framework asks of any rail system: are the invariants explicit, is there an anchor, is the audit complete, does the envelope hold, is redefinition governed? This case study runs those questions against the three most developed frameworks. The Future of Life AI Safety Index (July 7, 2026; evidence window closed June 3, 2026) graded Anthropic **C+ (2.66)**, OpenAI **C (2.28)**, DeepMind **C (2.01)** — no lab above average. The question is *why*, structurally.

## Category 1 audit: Seed & Constraints

**Invariants:** All three frameworks declare capability thresholds — Anthropic's ASL tiers with CBRN-3/4 and AI R&D-4/5 triggers; OpenAI's High/Critical tiers (severe harm = >1,000 deaths or >$100B damage); DeepMind's TCL→CCL ladder across CBRN, cyber, manipulation, and ML R&D domains (Tracked Capability Levels added in FSF v3.1, April 17, 2026; the Harmful Manipulation CCL was added earlier, in v3.0, September 22, 2025). These are the most explicit invariant declarations in the industry. Anthropic pre-commits specific safeguards per tier (most auditable); DeepMind commits to a mitigation plan without pre-committing its contents ("we will take sufficient action" is hard to fail against); OpenAI commits to "sufficient" safeguards (least falsifiable).

**The anchor problem:** Every framework's ultimate authority is the party being constrained. Anthropic: CEO + Responsible Scaling Officer decide deployments; the RSP's ambiguity clause states that "in cases where this policy is unintentionally ambiguous, we will act in accordance with the RSO or CEO's judgment" — executive discretion fills gaps. OpenAI's Preparedness Framework v2 states that "OpenAI Leadership" — defined as "the CEO or a person designated by them" — "is responsible for making all final decisions," "can approve or reject" SAG recommendations outright, and "can also make decisions without the SAG's participation, i.e., the SAG does not have the ability to 'filibuster'." (This is the internal Safety Advisory Group — strictly separate from the board-level Safety and Security Committee, from which Sam Altman stepped down in September 2024; the SSC is now an independent board committee chaired by Zico Kolter with authority to delay releases.) DeepMind: the FLI panel's key criticism is that it is "unclear which internal body has the authority to halt a deployment independently of executive leadership." **Verification terminates in the executive being constrained — no anchor independent of the traversal.** This is the rev. 6 anchor-independence rail failed at the highest level of the industry.

## Category 2 audit: Traversal & Adaptation

**Thresholds crossed without halting.** Anthropic's Opus 4.6 achieved a 427x kernel-optimization speedup against a 300x ASL-4 autonomy threshold (42% overshoot) yet shipped under ASL-3 — on the basis of a survey in which zero of 16 employees believed it could replace an entry-level researcher within three months (Anthropic's Sabotage Risk Report, via PulseMark; Anthropic's own RSP entry concedes "confidently ruling out this threshold is becoming increasingly difficult, and doing so requires assessments that are more subjective than we would like"). OpenAI's Astra was assessed at Critical for cybersecurity — 100% on ExploitBench (vs 78.5% for GPT-5.6 Sol), two previously unknown zero-days discovered and chained in internal evaluation — and development continued toward a gated "Daybreak" release.

Precision on the central empirical claim: the Critical trigger **did** produce actuation. On August 7, 2026, OpenAI paused internal Astra activities not meeting strengthened security controls; on August 18 it paused frontier RL training for two weeks ("Pacing model development in an era of cyber-critical capabilities"); testing moved to isolated environments with restricted network access, universal monitoring, and sandboxed execution. What it did **not** produce was a halt: development continued, and the model proceeded toward gated release. **Actuation without fail-closed.** The framework's constitutive rail is absent at every lab; the tightening that occurred is exactly what actuation permits — and exactly what it cannot substitute for.

**Audit:** Self-attested throughout. Anthropic's annual third-party review assesses **procedural compliance only**, not substantive outcomes. OpenAI publishes Capabilities and Safeguards Reports reviewed by its own SAG. DeepMind publishes FSF Reports at CCL crossings — the strongest transparency mechanism of the three — but third-party validation is "we expect" language, and risk models are unpublished. No external auditor has enforcement or subpoena power at any lab. The audit function records; nothing it records can stop the traversal.

## Category 3 audit: Resolution & Output

The handoff — a deployed frontier model — is **trusted, not verified** by anyone outside the lab. Voluntary pre-deployment eval access for AI Safety Institutes exists, but institutes cannot block deployment. The receiving iteration (the public, the enterprise customer, the downstream developer) cannot verify the anchor because there is no independent anchor to verify against — only the lab's published say-so.

## Rail governance: the moving goalpost

All three frameworks carry **competitor-contingent clauses** — and precision matters here: these are pre-declared conditional rails, not mid-traversal redefinitions, so they are formally legitimate under the framework. Anthropic's RSP footnote permits lowered Required Safeguards if another frontier actor passes a Capability Threshold without equivalent measures. OpenAI's v2: "If another frontier AI developer releases a high-risk system without comparable safeguards, we may adjust our requirements." DeepMind's v2 made protocol adoption conditional on field-wide adoption (softened in later versions; residual-risk reasoning against rivals' models remains).

The failure is not the conditional form — it is **who invokes the condition and which direction the ratchet moves**. Invocation sits with the constrained party (executive discretion, no independent adjudicator — the anchor-independence failure again), and the adjustment is downward-only: no clause triggers *tightening* when a rival behaves well. A conditional rail whose condition is judged by the constrained party and whose adjustment only loosens is a parametric rail with a built-in drift mechanism.

Meanwhile the pause commitments — the nearest thing to non-revisable rails — were removed or weakened across 2025–2026. Anthropic's v3.0 (effective Feb 24, 2026) **removed the pause commitment**: instead of pausing when capabilities outstrip safeguards, the framework publishes "Frontier Safety Roadmaps" with goals Anthropic grades its own progress against (v3.1, April 2026: "we have achieved two of the goals we had set"). Precision: the *commitment* was removed; the *option* to pause remains ("we remain free to take measures such as pausing... in any circumstances in which we deem appropriate"). A rail the constrained party may invoke at its discretion is not a rail. Dated version releases (v1→v2→v3) are formally valid return-edge redefinitions — which is precisely why the independence rail matters: valid in form, authored by the constrained party, drifting downward.

The FLI expert panel called the pattern a "moving goalpost" that "undermined safety frameworks across the board."

## Verdict

**No non-revisable commitments and no independent anchor: every threshold is parametric, every redefinition is authored by the constrained party, the audit is self-attested and procedure-only, and fail-closed is absent (actuation occurred at Astra; halt did not).** The frameworks are most valuable as honesty exhibits — they document, in the labs' own words, that no external enforcement exists, that thresholds are conditioned on competitive pressure, and that the highest-tier triggers have produced tightening but never a halt. The industry's rails describe the traversal; they do not govern it.

## The fix, in framework terms

The framework does not prescribe policy, but it specifies what a genuine rail system would require — the missing pieces are structural, not rhetorical:

1. **Non-revisable core:** at least one commitment whose revision exits the framework (e.g., an externally-held halt authority). Without it, all rails are parametric and all parametric rails drift toward the competitor.
2. **Independent anchor:** verification terminating in a root outside the traversal — an auditor with enforcement power, not advisors the CEO can override or committees that review after the fact.
3. **Governed redefinition with independence:** threshold changes at defined return edges (fixed review cycles with external sign-off), authored by a party independent of the constrained traversal — never mid-traversal at executive discretion.
4. **Fail-closed restoration:** a Critical-tier trigger halts deployment until safeguards are demonstrated — actuation that can tighten to zero, not merely to "gated."

## Sources

- Anthropic RSP (current; v3.0 effective Feb 24, 2026): https://www.anthropic.com/rsp
- RSP v3.0 analysis: https://www.ninetwothree.co/blog/anthropic-responsible-scaling-policy ("The brake was gone...")
- RSP version history: https://en.wikipedia.org/wiki/Anthropic%27s_Responsible_Scaling_Policy
- Opus 4.6 autonomy threshold: https://pulsemark.ai/anthropic-sabotage-risk-report-claude-opus-4-6-autonomy-threshold/ (427x vs 300x; 0/16 survey)
- OpenAI Preparedness Framework v2 (official): https://openai.com/index/updating-our-preparedness-framework/?trk=article-ssr-frontend-pulse_little-text-block
- SAG/Leadership provisions (framework text quoted): https://absolutedigitalpublishers.com/articles/openai-never-rewrote-the-rulebook-before-astra ("can approve or reject... outright"; no filibuster)
- Astra pause (Aug 7, 2026): https://www.infosecurity-magazine.com/news/openai-pauses-development-astra/
- Astra RL pause + framework evolution (Aug 18, 2026): https://www.infosecurity-magazine.com/news/openai-tightens-ai-safeguards/
- Astra Critical confirmation + Daybreak (Sept 2026): https://nationalcybersecurity.com/openai-to-limit-release-of-its-astra-model-due-to-hacking-concerns-hacking-cybersecurity-infosec-comptia-pentest-ransomware/
- Altman/SSC (Sept 2024): https://techcrunch.com/?p=2879180 ; http://WWW.Engadget.com/ai/openais-new-safety-board-has-more-power-and-no-sam-altman-230113547.html
- DeepMind FSF v3.1 (TCL layer, Apr 17, 2026): https://ppc.land/google-raises-security-to-level-2-for-3-types-of-dangerous-ai-capability/
- SaferAI DeepMind tracker: https://tracker.safer-ai.org/company/deepmind/
- FLI AI Safety Index (July 7, 2026; C+ 2.66 / C 2.28 / C 2.01): https://www.techtimes.com/articles/320959/20260719/ai-safety-grades-are-no-lab-tops-c-best-ones-are-retreating.htm ; https://www.eweek.com/news/2026-ai-safety-index/
- Cross-lab comparison: https://futureagi.substack.com/p/frontier-safety-frameworks-govern
