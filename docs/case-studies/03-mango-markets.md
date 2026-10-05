# Case Study 03 (DRAFT): Mango Markets — The Declared Envelope Was Never Exceeded

**Target:** Mango Markets, Solana-based margin-trading and lending DEX
**Incident:** October 11, 2022. ~$110–117M drained via oracle-price manipulation. Attacker: Avraham Eisenberg.
**Author:** Caleb Aaron Meadows — *draft pending author review*
**Framework:** 3-Category Trajectory Framework, rev. 6

## The facts

Eisenberg funded two accounts (~$5–10M USDC), took an outsized long position in MNGO-PERP on one while counter-trading himself on the other, pumping MNGO from **$0.038 to $0.91 (+2,394%)** (CertiK's figures). The protocol's oracles (Switchboard/Pyth) faithfully reported the manipulated market price. Eisenberg then used the inflated unrealized perp profit as collateral to borrow ~$110M+ in BTC, USDT, SOL, mSOL, USDC, and SRM from the treasury — draining all available liquidity. When MNGO crashed back, the protocol was left insolvent. No flash loan. No code bug. He publicly claimed it was a "highly profitable trading strategy" and "legal open market actions, using the protocol as designed."

**The ruling (May 23, 2025, SDNY, Judge Arun Subramanian):** Counts 1–2 (commodities fraud, commodities manipulation) were **vacated on venue** — Eisenberg operated from Puerto Rico with no New York nexus; the judge did not reach misrepresentation on those counts. Count 3 (wire fraud) was **acquitted on the merits** for insufficient evidence of falsity: "Mango Markets was permissionless and automatic," meaning the system could not be deceived in the legal sense, and the platform had no clear rules or prohibitions about borrowing. Prosecutors are appealing. DL News: "'code is law' just won in court."

**The settlement:** Eisenberg posted a DAO proposal demanding the **70M USDC treasury** be used to repay bad debt, remaining debt treated as bug bounty/insurance, and no criminal investigation or fund-freezing — he would return roughly $51M in MSOL/SOL/MNGO and keep ~$65M. He voted for it with **33,282,677 stolen MNGO (9.6% yes)**; it **failed**. The DAO's counter-proposal ("Repay Bad Debt #2") then **passed: 119,821,720 yes (96.3%) vs 4,601,240 no** — $67M returned, **$47M kept as bug bounty**, treasury covering residual bad debt, no criminal pursuit. Mango Labs later sued to void the settlement as made "under duress."

## Category 1 audit: Seed & Constraints

**The declared invariant:** positions must remain over-collateralized; borrows limited by account health, with collateral valued at oracle prices. The code executed *exactly as written* — the health-check math was correct.

**The finding is definitional, not enforcemental.** Under rev. 6's declared-rails scope, the precise statement is: **the declared envelope was never exceeded.** Every parameter stayed in coded bounds throughout; the manipulation didn't violate parameters, it exploited what the parameters measured. The protocol's actual risk property — collateral must reflect *realizable* value — was never declared. Mango's own admission: *"neither oracle providers have any fault here. The oracle price reporting worked as it should have."*

This is a **Category 1 anchor-mechanism mismatch**, not a Category 2 envelope breach. The anchor's declared intent — solvent over-collateralized lending, stated in the protocol's own documentation of its purpose — was implemented by a mechanism (oracle-price × quantity) that did not encode it. The framework does not certify undeclared intent; it names the omission. The omission is the finding: the property whose failure destroyed the protocol was never declared, so no rail governed it, so no breach could be detected. "Code is law" is what a system looks like when the declared rails are complete as written and still produce insolvency — the audit's value here is demonstrating the framework's boundary honestly, not claiming it detects what was never declared.

**The unused return edge:** CertiK stated in its October 2022 post-mortem that this exact vector — thin MNGO/USDC liquidity as the perp price reference — had been raised in Mango's Discord in **March 2022** (Mango declined to confirm the discussion). Seven months, multiple return edges, no redefinition taken. Governance negligence: the mirror image of the Whale's hyperactivity.

## Category 2 audit: Traversal & Adaptation

No fail-closed existed for abnormal price dislocation: no circuit breaker, no borrow caps, no liquidation halt during the manipulation window. Response was entirely manual and post-hoc: front-end deposit disable, third-party freeze requests, negotiation. The chain was fully auditable on-chain in ~30–60 minutes — audit-completeness held, and changed nothing, because there was no fail-closed for the audit to trigger. Audit without fail-closed is a transcript.

## Category 3 audit: Resolution & Output

The settlement was a parametric redefinition negotiated under duress — and the mechanism that performed it fails **anchor-independence**. Eisenberg voted his own proposal with stolen governance tokens; the redefinition process was operated, in part, by the breach's beneficiary. This is not anchor succession (no continuity chain was claimed or needed); it is the precise configuration the independence rail forbids: the judged writing the judgment. "One token, one vote" is an anchor purchasable with breach proceeds. Mango Labs' later suit to void the settlement as coerced is the protocol's own post-hoc admission that its return-edge process was captured.

## Rail governance

Two failures, one of each kind: the seven-month unpatched known gap (return edge available, unused — negligence), and the captured return edge (redefinition mechanism operated by the attacker — independence failure).

## Verdict

**Category 1 declaration failure: the declared envelope (oracle-price collateralization) was never exceeded — the property that mattered (realizable value) was never declared, an anchor-mechanism mismatch the framework names as omission rather than certifying undeclared intent.** No fail-closed existed for the dislocation; a known parametric gap sat unpatched across multiple return edges; and the settlement redefinition was authored in part by the breach's beneficiary, failing anchor-independence. The court's venue-based vacatur of the commodities counts and merits-based acquittal on wire fraud ("permissionless and automatic") judicially confirm the framework's reading: the system did exactly what its declared rails permitted.

## The fix, in framework terms

1. **Declare the property that matters:** the anchor's declared intent (solvent lending against realizable collateral) must be encoded as an explicit invariant — liquidity-weighted, manipulation-resistant valuation. The oracle price is a mechanism; the mechanism is not the invariant.
2. **Fail-closed on envelope delta:** abnormal price dislocation relative to depth triggers halt. Actuation is Category 2 business; its absence was a choice.
3. **Independent return edge:** governance mechanisms that revise the rails must satisfy anchor-independence — votes purchased with breach proceeds are the independence failure, and the handoff boundary applies in full.

## Sources

- DL News, "'Code is law' just won in court": https://www.dlnews.com/articles/defi/code-is-law-won-in-court-in-victory-for-defi/
- DL News, prosecutors appeal: https://www.dlnews.com/articles/defi/prosecutors-appeal-acquittal-of-mango-markets-exploiter/
- TradingView/The Block, venue analysis of the ruling: https://www.tradingview.com/news/the_block:ab738585b094b:0-u-s-judge-overturns-fraud-convictions-of-mango-markets-exploiter-eisenberg-determining-improper-venue/
- Cointelegraph, "Judge overturns Avraham Eisenberg fraud convictions": https://cointelegraph.Com/news/judge-overturns-avraham-eisenberg-fraud-convictions-mango-markets ("permissionless and automatic"; "insufficient evidence of falsity")
- CertiK post-mortem figures via CryptoDaily: https://cryptodaily.co.uk/2022/10/mango-markets-hack-sees-attacker-siphon-off-117-million ($0.038 → $0.91, +2,394%; March 2022 Discord discussion; Mango declined to confirm — caveat via https://cyberdaily.securelayer7.net/information-regarding-the-mango-markets-hack/?noamp=mobile)
- CoinDesk, Eisenberg's proposal: https://www.coindesk.com/markets/2022/10/12/mango-markets-hacker-provides-ultimatum-repay-bad-debt
- Court exhibit, vote screenshot (Realms): https://archive.org/download/gov.uscourts.wyd.72278/gov.uscourts.wyd.72278.14.4.pdf (33,282,677 yes / 9.6%, failed)
- Decrypt, counter-proposal terms: https://decrypt.co/112016/mango-dao-solana-defi-hacker-47m-offer ($67M returned, $47M bounty)
- ChainBulletin, deal approval: https://chainbulletin.com/mango-markets-to-approve-new-deal-with-hacker
- CoinDesk, contemporaneous response (Oct 11, 2022): https://www.coindesk.com/business/2022/10/11/breaking-news-solana-based-defi-platform-mango-hit-by-potential-100-million-exploit
- Blockworks (OtterSec $112,199,876): https://blockworks.com/news/mango-markets-mangled-by-oracle-manipulation-for-112m
