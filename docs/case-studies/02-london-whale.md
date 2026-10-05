# Case Study 02 (DRAFT): The London Whale — Redefinition During Traversal at JPMorgan Chase

**Target:** JPMorgan Chase, Chief Investment Office (CIO), London — Synthetic Credit Portfolio
**Incident:** $6.2B trading losses, 2012. U.S. Senate Permanent Subcommittee on Investigations report, March 15, 2013 (~300 pages, ~90,000 documents).
**Author:** Caleb Aaron Meadows — *draft pending author review*
**Framework:** 3-Category Trajectory Framework, rev. 6

## The facts

The CIO's Synthetic Credit Portfolio, traded by Bruno Iksil ("London Whale") under Achilles Macris and CIO head Ina Drew, began souring in early 2012. In January the portfolio breached the firmwide Value-at-Risk limit. Rather than reducing positions, management raised the limits and replaced the risk-measurement model mid-crisis so the breaches would disappear from the reports — while the positions kept growing. Losses compounded to $6.2B by year-end. The bank paid ~$920M in coordinated regulatory penalties (SEC $200M with admission of securities-law violations, FCA ~£137.6M, plus Federal Reserve, OCC, and later CFTC $100M). Two traders were criminally charged with concealing losses. Figures below are from the Senate report unless noted.

## Category 1 audit: Seed & Constraints

**Declared rails:** Firmwide 95% 10Q VaR limit ($125M); CIO-level VaR limits; credit-spread risk limits (CS01, CSW10%); stress-loss limits; stop-loss advisories. The Senate found **all five risk limits were breached for sustained periods** in Q1 2012.

## Category 2 audit: Traversal & Adaptation

The load-bearing fact is the direction of the response: **a sustained breach was answered by widening, never by tightening.** Actuation under the framework may only tighten; widening is redefinition, and redefinition requires a return edge. Every response here widened:

- **January 20, 2012:** Irvin Goldman (CIO) emailed CRO John Hogan projecting that a new VaR model, based on January 18 data, would cut CIO VaR **44%, to $57M** — "with CIO being well under its overall limits" (Senate report pp. 175–176, fn. 985).
- **January 23, 2012:** Market Risk Management emailed CEO Jamie Dimon and Hogan requesting a *temporary* increase of the firmwide 95% 10Q VaR limit from **$125M to $140M**, expiring January 31 — to buy time until the new model was approved.
- **January 30, 2012:** the new model was approved. Its actual overnight effect exceeded the projection: **VaR cut 50%, from $132M to $66M**, while actual positions kept growing (report p. 180, fn. 1009). Per the Senate, the model was built by an analyst working for the traders (not risk management), with no VaR-modeling experience, rushed and under pressure, dependent on manual daily data uploads. The OCC later called the implementation "shocking" and "absolutely unacceptable."
- **February 2012:** a separate set of breaches reached **270% of allowable**; Drew approved raising that limit too as "old and outdated."
- **April 17, 2012:** internal email recorded a key limit breached by **1,074%, for 71 days**.
- In parallel, valuation practices were altered to reduce reported losses (mismarking — the "two sets of books" that drew criminal charges), and separate models for VaR, CRM, and RWA were manipulated to lower reported risk and capital requirements without reducing risky assets.
- A Comprehensive Risk Measure (CRM) projection at end-February 2012 put SCP losses at **$6.3B**. CIO Chief Market Risk Officer Pete Weiland dismissed it — "difficult for us to imagine" and "garbage" (report p. 187; he later told the Subcommittee he meant "unreliable"). Actual losses reached $6.2B: the discarded model was accurate within ~2%.

**Envelope-integrity:** breached structurally. The declared envelope (risk as measured by VaR) no longer measured reality — reported risk halved while actual risk grew. The system's self-description diverged from its state, with no detection, because the detector was what got redefined.

**Audit:** present in form, inverted in substance. The limit-raise requests, breach-percentage emails, and model-approval records all exist — which is what makes the record damning rather than exculpatory. Audit-completeness without fail-closed is a transcript of the disaster, not a control on it.

## Category 3 audit: Resolution & Output

The outputs — risk reports, earnings — were **trusted, not verified**. Q1 2012 earnings were restated (additional $660M recognized, July 13, 2012). The handoff to regulators and shareholders carried numbers produced by an instrument swapped mid-measurement. The bank's own Management Task Force (January 16, 2013) admitted the January 2012 VaR model "was not properly vetted or approved."

## Rail governance

This is the case the rev. 6 **anchor-independence** rail was written for. The January 23 raise *looked* governed: temporary, time-boxed, requested through formal process, approved by the top authority. Under rev. 5's letter it was arguably a valid redefinition. What distinguishes it is independence: the redefinition relieved the very traversal that had breached, authored through a chain whose beneficiary was the breaching activity. The judged wrote the judgment.

And there was **no constructed return edge**. A continuous trading book must halt-and-re-enter before new rails take effect; here the new model took effect mid-traversal on January 30 while positions grew. Authority does not substitute for the return edge — and the episode shows why the framework requires both: formal approval without a halt is redefinition during traversal with better paperwork. The Senate report's Section V ("Disregarding Limits") walks the sequence stepwise: develop a new VaR model → breach the VaR limit → raise the VaR limit temporarily → win approval of the new model → *use the new model to increase risk* → fail to lower the VaR limit.

The regulator was inside the information flow and still missed it: the OCC was notified *in advance* of the projected 44% VaR drop and raised no concerns; the bank told the OCC it planned to *reduce* the SCP, then increased it. The gate watched the reports, and the reports were what got redefined.

## Verdict

**Rail-governance violation: a sustained breach answered by widening instead of tightening, via redefinition during traversal that was formally authorized but failed anchor-independence — the constrained activity's chain authored its own relief — with no constructed return edge, producing an envelope-integrity breach in the measuring instrument itself.** The governance machinery ran throughout; every element was inverted in substance. The January 23 email remains the artifact: a written request to move the fence because the activity had already crossed it.

## The fix, in framework terms

1. **Tighten-only actuation:** a limit breach triggers narrowing, throttling, or halt — never redefinition. Widening in response to breach is the precise signature of rail capture.
2. **Anchor-independence:** the authority authoring a rail change must be independent of the traversal the rail constrains — personal or temporal, but never the presently-constrained party authoring its own relief.
3. **Constructed return edge:** continuous operations halt-and-re-enter (Terminal Resolution → Invariant Initialization) before new rails take effect. The model swap of January 30 was the redefinition; the missing halt was the violation.
4. **Fail-closed on sustained breach:** the constitutive rail whose absence cost $6.2B.

## Sources

- U.S. Senate PSI, *JPMorgan Chase Whale Trades* (Mar 15, 2013): https://www.hsgac.senate.gov/wp-content/uploads/imo/media/doc/REPORT%20-%20JPMorgan%20Chase%20Whale%20Trades%20%283-26-13%29.pdf (44%/$57M: pp. 175–176, fn. 985; 50%/$132M→$66M: p. 180, fn. 1009; CRM $6.3B/"garbage": p. 187)
- JPMorgan Management Task Force report (Jan 16, 2013), via Senate report
- Motley Fool, "48 Damning Pieces of Evidence" (May 7, 2013): https://www.fool.com/investing/general/2013/05/07/48-damning-pieces-of-evidence-from-the-jpmorgan-wh.aspx
- NPR, "Whale Of A Fine: JPMorgan Chase To Pay $920M" (Sept 19, 2013): https://www.nprillinois.org/2013-09-19/whale-of-a-fine-jpmorgan-chase-to-pay-920m-in-penalties
- NPR, "JPMorgan In Hot Seat Over London Whale Losses" (Mar 15, 2013): https://www.wcbe.org/2013-03-15/jpmorgan-in-hot-seat-over-london-whale-losses
- Yale Journal of Financial Crises, "London Whale H: Cross-Border Regulation": https://elischolar.library.yale.edu/cgi/viewcontent.cgi?article=1021&context=journal-of-financial-crises
