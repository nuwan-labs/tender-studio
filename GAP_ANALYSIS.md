# TenderStudio — Gap Analysis
## Alignment with National Procurement Manual 2024 (Goods, Works & Non-Consulting Services)

**Date:** 2026-02-26 (updated 2026-02-26, 2nd iteration)
**Based on:** `SYSTEM_SPECIFICATION.md` v2.4 and 13 HTML wireframes
**Reference:** `Goods_Manual_English_updated-on-01.01.2025-1.md` (National Procurement Commission, Sri Lanka)

---

## Executive Summary

The current TenderStudio specification was designed based on a real tender document and evaluation report (PR 211-1). The National Procurement Manual 2024 ("the Guidelines") is the authoritative legal standard. This analysis identifies:

- **18 Missing Features** — required by the Guidelines but absent from the spec/wireframes
- **6 Underspecified Areas** — partially covered but needing enhancement
- **3 Unsupported Features** — in the spec but not in the Guidelines (review required)
- **Terminology gaps** — mismatched naming between spec and Guidelines

The core workflow (BEC evaluates → PC/DPC approves → Award) is correctly captured. The primary gaps are in the **evaluation methodology** (deviation classification, the 14-step price adjustment sequence, the 4-stage evaluation structure with exact Annexure table formats), the **award process** (standstill period, appeals, debriefing), and **committee governance** details (declarations, bidding procedure types, VRC post-award).

---

## Part 1 — Terminology Mapping

The spec uses different names for some entities defined in the Guidelines. This must be harmonized throughout the system.

| TenderStudio Spec Term | Guidelines Term | Notes |
|---|---|---|
| Procurement Manager (PM) | Accounting Officer (AO) / PMD Officer | PM maps to AO; PMD = Procurement Management Division |
| DPC (Departmental Procurement Committee) | PC (Procurement Committee) — specifically DPC | Correct type but Guidelines define 6 PC types (HLPC, SHLPC, MPC, DPC, PPC, RPC) |
| BEC | BEC (Bid Evaluation Committee) | ✓ Same |
| BOC | BOC (Bid Opening Committee) | ✓ Same |
| "Responsive" / "Non-Responsive" | "Substantially Responsive" / "Non-Substantially Responsive" | Guidelines use "substantially responsive" |
| "Compulsory" requirement | No direct equivalent | Guidelines use Major Deviation = rejection criterion; see Gap #5 |
| "Requestor" (equipment requestor) | Not defined in Guidelines | See Unsupported Feature #1 |
| "Bidder clarification" | "Clarification" (only one type defined) | Guidelines don't define Requestor clarifications separately |
| Award | "Award of Contract" | ✓ Same concept |

---

## Part 2 — Missing Features (Must Add)

### GAP-01: Standstill Period Before Contract Award
**Guideline Reference:** Section 8.4
**Requirement:** After the PC issues a "Notification of Intention to Award," a mandatory standstill period of **at least 10 working days** must pass before the contract is actually awarded. During this period:
- Any unsuccessful bidder may request a **debriefing** (must be within 3rd working day; PE concludes by 5th working day)
- Any unsuccessful bidder may submit an **appeal** to the relevant appeal body

**Current spec:** No standstill period. DPC approval → Award directly (Stages 13→14).
**Impact on UI:** Award workflow needs an intermediate "Intention to Award" state, a standstill timer, and debriefing/appeal intake.

---

### GAP-02: Debriefing of Unsuccessful Bidders
**Guideline Reference:** Section 7.2, 8.4
**Requirement:** After evaluation, unsuccessful bidders must be informed of **why their bid was not selected** at a debriefing. Request must be made by bidder within 3rd working day of standstill; PE must conclude by 5th working day.

**Current spec:** No debriefing feature mentioned.
**Impact on UI:** New screen/panel needed for debriefing scheduling and recording.

---

### GAP-03: Procurement Appeal Process
**Guideline Reference:** Sections 8.5–8.5.7
**Requirement:** Any unsuccessful bidder may appeal to the relevant appeal body (PAB for HLPC/SHLPC; MPAC for MPC; DPAC for DPC; PPAC for PPC; RPAC for RPC) during the standstill period. The appeal body reviews and makes a recommendation; the final approval authority (Cabinet or CAO/AO) then acts.

**Current spec:** No appeal mechanism. DPC "Send Back" is the only revision mechanism.
**Impact on UI:** Need an "Appeals" section in the award workflow. At minimum: record that an appeal was filed, track appeal status, and record outcome.

---

### GAP-04: Publication of Contract Award
**Guideline Reference:** Section 8.8
**Requirement:** PE must publish award details on its website. For contracts > LKR 750 Mn., must also publish in the Government Gazette in all three official languages and on the General Treasury website. Required published fields:
- Description of items/works/services
- Total number of bids received
- Name of successful bidder
- Contract price
- If awarded to foreign bidder: local agent details

**Current spec:** No award publication feature.
**Impact on UI:** Post-award screen needs a "Publish Award" action that generates a structured publication record.

---

### GAP-05: Evaluation Deviation Classification (Major / Minor / Debatable)
**Guideline Reference:** Sections 7.7, Stage 2, Step 2
**Requirement:** The evaluation process must classify bid deviations from procurement document requirements into three categories:
- **Major Deviations** → Bid is non-responsive; rejected from further evaluation. Examples: failure to sign form of bid, failure to submit bid security, failure to meet critical specifications, conditional bids.
- **Minor Deviations** → Bid is substantially responsive; deviation is quantified monetarily and loaded to bid price for comparison only. Examples: failure to initial all pages, minor non-compliance with no price impact.
- **Debatable Deviations** → Committee judgment required; if accepted as minor, monetarily quantified.

**Current spec:** Uses binary "Compulsory" (C) = reject / "Non-Compulsory" = accept-with-note. This partially captures major/minor but misses:
- The "Debatable" category requiring explicit committee deliberation
- The monetary loading of minor deviations to bid price
- The concept that non-Compulsory rejection does NOT automatically mean minor (it could be debatable)

**Impact on UI:** Evaluation grid cells need a deviation-type field (Major/Minor/Debatable) in addition to Accept/Reject. Financial view needs to show price-loaded amounts. Report must include deviation tables.

---

### GAP-06: Price Adjustment Mechanisms in Financial Evaluation
**Guideline Reference:** Section 7.7 Stage 3; Annexure VI(a) and VI(b) of Chapter 07
**Requirement:** The detailed (Stage 3) evaluation must support a structured sequence of 14 price adjustment steps to arrive at the "Evaluated Price." The official Annexure VI table lists these explicitly:
1. **Exclude VAT, contingencies, and provisional sums** — base price for comparison
2. **Arithmetic error correction** — unit price prevails; amount in words prevails over figures ✓ (partially in spec)
3. **Discount application** — only unconditional discounts announced at bid opening; cross-discounts for multi-lot awards
4. **Bid price adjustment for omissions** — missing line items priced at average of other bids
5. **Adjustment for acceptable departures** — minor deviations monetarily quantified and loaded (plus/minus)
6. **Adjustment for delivery period** — deviation > 10% = non-responsive; accepted deviations quantified
7. **Adjustment for inland transportation** — for imported goods when CIF/FOB pricing used
8. **Operational cost and life cycle costing** — if specified in procurement documents (operational + maintenance NPV)
9. **Conversion to common currency** — using CBSL selling rate as of bid closing date or 28 days prior
10. **Domestic preference** — 20% loaded to CIF price of foreign goods (GOSL-funded); 15% for foreign Works
11. **Reassess ranking order** — after all adjustments applied
12. **After-sales services** — point-based evaluation if specified
13. **Examine bidding for unbalance** — flag if unit prices > 50% higher than average AND > 5% of total bid
14. **Clarifications during evaluation** — priced effect of any clarification obtained

Each step must be applied per bidder and the result per step recorded. Final output is a comparison table:
`Quoted Price → [Adjustments 1-14] → Total Evaluated Price → Rank`

**Current spec:** Only captures: arithmetic correction (unit price prevails), VAT exclusion. No support for steps 3–14.
**Impact on UI:** Financial evaluation view needs a structured multi-step price adjustment workflow with a row per adjustment type, per bidder column, showing plus/minus amounts and cumulative effect. Final "Evaluated Price" column shown alongside "Quoted Price."

---

### GAP-07: Four-Stage Structured Evaluation Workflow
**Guideline Reference:** Section 7.7; Annexures I–VIII of Chapter 07
**Requirement:** Evaluation must follow a mandatory sequential 4-stage process. The Chapter 07 annexures define the exact table formats for each sub-step:

**Stage 1 — Data Collection** (Annexures I, II, III):
- **1a.** Basic Data Table (Annexure I): Procurement metadata (method, dates, bid validity, bid security amounts, number of bids)
- **1b.** Bid Opening Records (Annexure II): BEC prepares its own bid opening table (separate from BOC's report), recording: bidder name, bid security amount, bid amount per package, alternative bids, remarks
- **1c.** Document Completeness Table (Annexure III): Y/N/NA grid per bidder for each required document (original bid, form of bid, price schedules, bid security, power of attorney, signatures, eligibility, joint venture agreement, bid validity)

**Stage 2 — Substantial Responsiveness** (Annexures IV, V):
- **2a.** Commercial Terms Responsiveness (Annexure IV-a/b): C (Compliant) / NC (Non-Compliant) per requirement per bidder. Conclusion: R (Responsive) / NR (Non-Responsive). Requirements: power of attorney, signatures, JV confirmation, eligibility, bid validity, bid security (amount/validity/format), contract type, payment terms.
- **2b.** Technical Requirements Responsiveness (Annexure V): C/NC per technical requirement per bidder; classify deviations as Major/Minor/Debatable. Conclusion: R/NR.

**Stage 3 — Detailed Evaluation** (Annexures VI, VII):
- **3a.** Price comparison table with the 14-step adjustment sequence (see GAP-06)
- **3b.** Final ranking after domestic preference application (Annexure VII)

**Stage 4 — Post-Qualification** (Annexure VIII, Section 5.6):
- Post-qualification of the substantially responsive lowest evaluated bidder only (see GAP-08)

**Official BER Report Structure (Annexure VIII)**:
The manual's suggested BER structure has 9 sections: Introduction, Completeness Examination, Substantive Responsiveness, Detailed Evaluation, Lowest Evaluated Bids, Conclusions & Recommendation, [section 7 implied], Appendixes, Other. Note: BEC "has the liberty to design a suitable format" — TenderStudio's 14-section format is acceptable if all required content is covered.

**Current spec:** Has a general BEC evaluation flow but does not enforce this structured 4-stage sequence. Stage 1b (BEC's own bid opening record) and Stage 2a (commercial terms responsiveness table separate from technical grid) are missing.
**Impact on UI:** Evaluation workspace needs distinct stage tabs or sections: Stage 1 (Data), Stage 2 (Responsiveness with two sub-tables), Stage 3 (Price Adjustments), Stage 4 (Post-Qualification). Stage 2a preliminary commercial checks must precede the main technical evaluation grid.

---

### GAP-08: Post-Qualification of Lowest Evaluated Bidder (Stage 4)
**Guideline Reference:** Section 7.7, Stage 4
**Requirement:** After Stage 3 ranking, the **lowest evaluated substantially responsive bidder** must be assessed for post-qualification:
- For Goods: Manufacturer's Authorization Certificate, maintenance/spare parts capability, financial capability, past experience
- For Works: CIDA registration, similar project experience, cash flow arrangement, key equipment, staff qualifications
- If lowest bidder fails post-qualification, proceed to the next lowest and repeat
- Unbalanced bid analysis (unit prices > 50% higher than average for same items AND > 5% of total bid) must be flagged

**Current spec:** No post-qualification stage. Responsiveness is determined only by technical compliance grid.
**Impact on UI:** Need a post-qualification checklist panel for the recommended bidder. Need unbalanced bid detection in the financial view.

---

### GAP-09: Specific Bid Security Rules
**Guideline Reference:** Section 5.9
**Requirement:** System must enforce and display:
- **Amount:** 1–2% of estimated procurement value (excluding VAT)
- **Validity for National (NCB/LNB):** 28 days beyond bid validity period
- **Validity for International (ICB/LIB):** 56 days beyond bid validity period
- **Three options:** (1) Bank Guarantee, (2) Refundable Cash Deposit, (3) Bid Securing Declaration (only for contracts < LKR 20 Mn.)
- **Foreign bank guarantee:** Must be confirmed by a local bank operating in Sri Lanka (exception: EXIM Banks)
- **Rejection rule:** Cannot reject a bid due to minor deviations unrelated to value, period, or legality of bid security

**Current spec:** Tracks bid security (amount, bank, guarantee number, validity, type) but does not:
- Calculate/validate the 28/56-day rule
- Support Bid Securing Declaration as a separate type with LKR 20 Mn. threshold
- Alert when foreign bank guarantee lacks local confirmation
**Impact on UI:** Bid security entry screen needs these validation rules and alert logic.

---

### GAP-10: Bidding Period Minimum Rules
**Guideline Reference:** Section 6.3
**Requirement:** System must enforce minimum bidding periods by procurement method:
- ICB/LIB: 42 calendar days
- NCB/LNB ≥ LKR 50 Mn.: 21 calendar days
- NCB/LNB < LKR 50 Mn.: 14 calendar days
- Shopping/RFQ National: 7 calendar days
- Shopping/RFQ International: 14 calendar days

**Current spec:** Tracks publication and closing dates but no minimum period validation or warnings.
**Impact on UI:** Tender creation / dates configuration screen needs minimum period validation and warnings.

---

### GAP-11: Pre-Bid Meeting Rules
**Guideline Reference:** Section 5.10
**Requirement:**
- Pre-bid meeting must be held at least **10 calendar days** before bid closing (for procurements > LKR 50 Mn.)
- Minutes must be distributed to **all bidders who purchased documents** (not just attendees)
- Any addendum resulting from the meeting must be issued to all document purchasers and at least **3 working days** before bid closing

**Current spec:** Has pre-bid meeting management but does not enforce the 10-day rule, and does not distinguish between "attendees" and "all document purchasers."
**Impact on UI:** Pre-bid meeting screen needs date validation, a "document purchasers" registry separate from meeting attendees, and addendum tracking with 3-day rule enforcement.

---

### GAP-12: BOC Detailed Opening Procedure
**Guideline Reference:** Section 6.6
**Requirement:** BOC opening procedure has specific steps not captured:
- Bids opened **within 30 minutes** of closing deadline
- BOC must handle **withdrawal letters** — if authenticated, bid not opened; kept for return after award
- BOC must handle **modification envelopes** — opened first, then the original
- BOC members must **initial** specific documents: Form of Bid, Summary of BOQ, Bid Security cover letter, discount letter, modifications
- BOC must **cover Rate and Item Total columns** of BOQ/Price Schedule with adhesive transparent tape
- After opening, originals resealed and handed to PE/PMD → PMD hands originals to **PC/BEC Chairman** (not directly to BEC)
- Duplicates kept in secure custody separately
- **Samples** (if requested): BOC ensures received and recorded; requesting samples with bids is discouraged

**Current spec:** Records bidder name, total bid price, bid security. Does not capture: withdrawal handling, modification envelopes, BOC member initials, tape-covering procedure, handover chain.
**Impact on UI:** Bid opening screen needs withdrawal/modification workflow, checklist for BOC initials, and a formal handover record.

---

### GAP-13: Six Procurement Committee Types Support
**Guideline Reference:** Sections 2.7.1–2.7.6
**Requirement:** The system must support all 6 PC types as the approving body, not just DPC:
- HLPC (> LKR 750 Mn. GOSL)
- SHLPC (standing version of HLPC)
- MPC (up to LKR 750 Mn.)
- DPC (up to LKR 400 Mn.)
- PPC (up to LKR 400 Mn.)
- RPC (up to LKR 50 Mn.)

Each has different composition requirements (minimum members, chairman qualification, alternate members). The authority limits determine which type is used.

**Current spec:** Only DPC is modelled. For SLIBTEC/DPC-level organization this may be sufficient, but the system design should accommodate configuration of PC type.
**Impact on UI:** Tender creation should allow selection of PC type. Committee management must enforce composition rules per type.

---

---

### GAP-14: Bidding Procedure Type — Single Stage Two Envelope Support
**Guideline Reference:** Sections 3.3.1–3.3.3
**Requirement:** Three distinct bidding procedures are defined. Each has a fundamentally different workflow:
- **Single Stage One Envelope** (most common): Technical + financial proposals submitted together; opened together at BOC; evaluated together by BEC.
- **Single Stage Two Envelope**: Technical and financial proposals submitted in separate sealed envelopes in one outer envelope. BOC opens outer envelopes only. BEC evaluates technical proposals first. Financial envelopes opened ONLY for substantially responsive bidders (in their presence). Financial envelopes of non-responsive bidders returned unopened after contract award.
- **Two Stage**: Stage I — draft documents with functional specs, bidders submit technical proposals; BEC evaluates; PE may issue addendum. Stage II — amended final technical + financial proposals invited; combined evaluation.

**Current spec:** Assumes Single Stage One Envelope throughout. No procedure type field on tenders. No separate technical-envelope-opening workflow for Two Envelope procedure.
**Impact on UI:** Tender creation must capture bidding procedure type. BOC opening screen must branch based on procedure: Two Envelope requires a separate "Technical Envelopes Opening" event, then a "Financial Envelopes Opening" event restricted to responsive bidders only.

---

### GAP-15: Non-Collusion Affidavit in Bidder Document Checklist
**Guideline Reference:** Section 1.5; Annexure III of Chapter 01
**Requirement:** Each bidder must submit a Non-Collusion Affidavit (sworn before a Justice of the Peace or Commissioner of Oaths) with their bid. The affidavit declares:
- No combination/collusion agreement with any person regarding bid price
- No steps taken to prevent or deter other bidders
- Bid made without reference to or agreement with any other bid
- No rebate, gift, or commission paid or to be paid in connection with the bid

This document must be tracked per bidder as part of the document checklist (Y/N submitted, and whether it is properly sworn/authenticated).

**Current spec:** Document checklist does not specifically include Non-Collusion Affidavit as a standard required document.
**Impact on UI:** Bidder document checklist (in evaluation Stage 1c) must include Non-Collusion Affidavit as a standard line item. Alert if missing.

---

### GAP-16: Variation Review Committee (VRC) for Post-Award Contract Variations
**Guideline Reference:** Section 9.2
**Requirement:** Contract variations beyond contingency provisions require review by a **Variation Review Committee (VRC)**, appointed by CAO/AO with ≥3 members (engineering, finance, procurement specialists). VRC confirms:
- Variation could not have been foreseen at time of award
- Change is justifiable in scope, quantity, specifications, and financial commitment

**Approval authority thresholds:**
- Aggregate variation ≤7.5% of Contract Award Price AND amount < DPC/PPC threshold → AO/HD/PD approves (on VRC recommendation)
- Aggregate variation ≤10% or amount exceeds DPC threshold → CAO approves
- Aggregate variation >10% or amount exceeds MPC threshold → Cabinet of Ministers approval required

**Format:** Standard variation approval form (Annexure I, Chapter 09) tracks: original TCE, original contract price, contingency %, aggregate variations to date, detailed comparison of original vs. variation, revised contract price.

**Current spec:** Post-award tracking covers delivery/installation/commissioning. No variation request workflow or VRC support.
**Impact on UI:** Post-award screen needs a "Contract Variations" tab with: variation request form, VRC recommendation field, approval chain tracking (AO/CAO/Cabinet) based on thresholds, running aggregate variation %.

---

### GAP-17: Bidder Debarment / Blacklisting Tracking
**Guideline Reference:** Sections 10.1–10.3
**Requirement:** Suppliers, contractors, and service providers can be sanctioned/debarred/blacklisted by appropriate authorities at three levels:
- **National level** (all GOSL institutions)
- **Ministry level** (specific Ministry)
- **PE level** (specific Procuring Entity)

Grounds include: false information submitted, bid withdrawal after closing, refusal to accept award, performance failure, ethics violations, collusion.

Debarment process: complaint → written notice to bidder → show cause → independent committee (3 members) → hearing → recommendation → authority decision → appeal (5 working days; 21-day appeal committee). During debarment, bidder cannot participate in new procurement but must complete existing contracts.

**Current spec:** Bidder Database exists but no debarment status field, no debarment history, no automatic blocking of debarred bidders from new tenders.
**Impact on UI:** Bidder Database must include: debarment status (none/active/expired), debarment level, debarment period, debarment grounds. Alert when a debarred bidder attempts to participate in a new tender.

---

### GAP-18: Conflict of Interest Declarations — Committee Member Forms
**Guideline Reference:** Section 1.5; Annexures I, II, III of Chapter 01
**Requirement:** At the **first meeting** of every PC and BEC, all members and supporting staff must sign formal declarations:
- **Annexure I (Ch.01):** PC and BEC member declaration — no conflict of interest, no shareholding/ownership/kin links to bidders, will maintain confidentiality, will disassociate from procurement if conflict arises. Signed by ALL committee members at first meeting.
- **Annexure II (Ch.01):** Staff officer declaration — any officer (PMD staff, secretaries) assisting the committee must also sign a similar declaration.
- **Annexure III (Ch.01):** Non-Collusion Affidavit — bidders submit this with their bid (see GAP-15).

Additional rule: Officers who **prepared the specifications** for a procurement **cannot serve as BEC evaluators** for that procurement (conflict of interest).

**Current spec:** No mechanism for recording declaration signing at first meeting. No conflict-of-interest check when assigning committee members.
**Impact on UI:** BEC/BOC session management must have a "First Meeting — Declarations" step where each member is marked as having signed. Committee member assignment screen should flag if an assigned person prepared the specifications for this tender.

---

## Part 3 — Underspecified Areas (Enhance Existing)

### UNDER-01: Clarification Rules — What Cannot Be Clarified
**Guideline Reference:** Section 7.7, Stage 3; Section 3.212
**Current spec** states clarifications "Cannot change bid substance (price, core specifications)." The Guidelines are more specific:
Clarification **shall NOT involve modification to:** contract price, delivery period, conditions of contract, specifications, or any other substance of the bid.
**Enhancement:** Clarification form must display this restriction prominently and the system should flag if a received response appears to modify these elements.

---

### UNDER-02: Single Substantially Responsive Bid Handling
**Guideline Reference:** Section 7.10
**Guidelines state:** Lack of competition shall NOT be determined solely by number of bids. Even a **single substantially responsive bid** shall be evaluated for responsiveness and price reasonableness. If found responsive and price is reasonable, it shall be considered for award.
**Current spec:** No explicit guidance on single-bidder scenarios.
**Enhancement:** System should not block evaluation when only one bidder is responsive. Add a "Price Reasonableness Assessment" step when a single responsive bidder exists.

---

### UNDER-03: Tie-Breaking When Lowest Evaluated Prices Are Equal
**Guideline Reference:** Section 7.9C
**Guideline options when two or more bids have equal lowest evaluated price:**
1. Split the contract among equal lowest bidders (based on nature of procurement)
2. Request fresh sealed bids from the tied bidders
3. Select via fair tossing/drawing lots

**Current spec:** No tie-breaking mechanism described.
**Enhancement:** Financial evaluation view needs a tie-detection alert and the three options presented to the committee.

---

### UNDER-04: Bid Validity Extension Procedure
**Guideline Reference:** Section 7.4–7.5
**Requirement:** If evaluation overruns bid validity, PE must request all bidders to extend. Rules:
- Bidders who extend cannot modify substance or price
- Bidders who refuse → their bid security returned after award; bid excluded
- Extension is unfavorable (risk of cost increase) — system should flag

**Current spec:** Has "Bid validity period" field but no extension workflow.
**Enhancement:** Add bid validity expiry tracking with warnings; add extension request workflow that records which bidders agreed/refused and handles exclusion of refusing bidders.

---

### UNDER-05: BEC Chairman Participation in PC Meetings
**Guideline Reference:** Sections 2.7.3, 2.7.4, 2.7.5
**Requirement:** BEC Chairman (or nominee) **participates as a non-member** at PC meetings to provide clarifications on the BEC report. This is a formal role, not ad hoc.
**Current spec:** PM reviews report (read-only). No mechanism for BEC Chair to present/clarify at PC/DPC meeting.
**Enhancement:** Meeting management must support "non-member participant" role. BEC Chair should be able to attend DPC meetings in a non-voting clarification capacity.

---

### UNDER-06: Procurement Committee (PC) Separate Role
**Guideline Reference:** Sections 2.4 B, 8.1
**Requirement:** The PC has specific powers beyond final approval:
- PC can **partially accept** BEC recommendations (accept some items, reject others)
- PC can **reject** BEC recommendations and request reconsideration
- PC can **accept one or more bids, or parts of bids** (split awards)
- PC decides to re-invite bids if needed
- If PC rejects BEC recommendations, it must record reasons in minutes
- A majority decision prevails if PC members disagree; dissents recorded

**Current spec:** DPC has Approve/Reject/Send Back but no partial acceptance, no split-award decision mechanism, no dissent recording.
**Enhancement:** DPC decision screen must support partial acceptance per item, split award decisions, dissenting vote recording.

---

## Part 4 — Unsupported Features (Review Required)

### UNSUP-01: "Requestor Clarification" (Internal Equipment Requestor)
**Current spec:** Two clarification types — Bidder (external) and Requestor (internal equipment requestor who requested the procurement). Requestor is contacted to ask if deviations are acceptable.
**Guidelines:** Define only one clarification type — bidder clarification through PE. The Guidelines do not describe a mechanism to consult internal requestors.
**Assessment:** This is an **addition** (not a contradiction) to the Guidelines. It is a practical workflow that may occur informally in organizations. However:
- The Guidelines state that the BEC is solely responsible for evaluation judgments
- If the requestor's opinion allows a deviation to be accepted, BEC must still make the formal decision and record justification
- The system should make clear that requestor input is **advisory only** — BEC makes the final decision
**Recommendation:** **Retain** this feature with clarified labeling: rename "Requestor Clarification" to "Internal Technical Consultation" and add a prominent note that BEC is solely responsible for the compliance decision regardless of requestor input.

---

### UNSUP-02: AI Maker-Checker Verification as a Workflow Requirement
**Current spec:** AI extractions require two-person verification (maker-checker) before being used.
**Guidelines:** No mention of AI or maker-checker verification.
**Assessment:** This is a TenderStudio design decision to ensure data accuracy. It is **not contradicted** by the Guidelines.
**Recommendation:** **Retain** — this is a sound quality control mechanism aligned with the Guidelines' emphasis on accuracy and accountability.

---

### UNSUP-03: Session-Based Access Control (Write Access Only During Sessions)
**Current spec:** BEC members have read-only access outside official sessions; write access only during sessions when granted by Secretary.
**Guidelines:** Do not prescribe this level of technical access control. They focus on who is responsible for decisions, not when they can type.
**Assessment:** This is a TenderStudio design decision.
**Recommendation:** **Retain** — it enforces the Guidelines' requirement for formal session-based deliberation and maintains the audit trail integrity.

---

## Part 5 — Report Structure Gaps

The current 14-section BEC Evaluation Report must be enhanced to include tables required by the Guidelines.

| Required Table (Guidelines Sec. 7.9) | Current Spec | Status |
|---|---|---|
| Basic data sheet (procurement method, dates, bid validity, bid security amount) | Section 5 "Process Summary" | Partially covered |
| Bid opening records + attendance | Section 7 "Bid Opening Summary" | ✓ Covered |
| Completeness table (per bidder) | Section 8 "Evaluation of Quotations" | ✓ Covered |
| Eligibility table (per bidder) | Section 8 "Evaluation of Quotations" | ✓ Covered |
| Bid security compliance table | Not present | **MISSING** |
| Major/minor deviation classification table | Not present (only Accept/Reject per req.) | **MISSING** |
| Arithmetic error correction table | Section 10 "Price Schedules" (partial) | Partially covered |
| Discount application table | Not present | **MISSING** |
| Price adjustments table (delivery, deviations, transportation) | Not present | **MISSING** |
| Currency conversion table | Not present | **MISSING** |
| Domestic preference table | Not present | **MISSING** |
| Step-by-step: Quoted Price → Evaluated Price table | Not present | **MISSING** |
| Post-qualification verification table (for lowest bidder) | Not present | **MISSING** |
| Record of all clarifications | Section 12 "Clarifications" | ✓ Covered |
| Award recommendation (with reasons for rejection of others) | Section 11 "Recommendations" | ✓ Covered |
| Narrative section | Section 13 "Conclusion" | ✓ Covered |
| Signature table | Section 14 "Signature Table" | ✓ Covered |

---

## Part 6 — Wireframe-Specific Gaps

| Wireframe | Missing Elements |
|---|---|
| `evaluation-grid.html` | No deviation type column (Major/Minor/Debatable); no Stage 1/2/3/4 stage indicator; no "preliminary check" section before grid; no BEC bid opening record table (Annexure II); no document completeness table (Annexure III); no separate commercial terms responsiveness table (Annexure IV) |
| `bid-opening.html` | No withdrawal handling; no modification envelope workflow; no initials checklist; no tape-covering step; no handover record to PC/BEC Chairman; no Two Envelope procedure branch (separate technical envelope opening event) |
| `pm-dashboard.html` | No standstill period timer; no appeal status indicator; no award publication status |
| `report-preview.html` | Missing tables: bid security compliance, deviation classification, price adjustments (14-step), currency conversion, domestic preference, Quoted→Evaluated Price steps, post-qualification; BEC bid opening records table |
| `clarification-panel.html` | No restriction notice (cannot modify price/delivery/specs); requestor type needs "advisory only" label |
| `tender-creation.html` | No PC type selection (DPC/MPC/PPC/RPC/HLPC/SHLPC); no bidding procedure type (Single Stage One/Two Envelope, Two Stage); no minimum bidding period validation; no document purchasers registry |
| `pre-bid-meeting.html` | No 10-day minimum validation; no "document purchasers" list separate from attendees; no 3-day addendum rule |
| `bidder-detail.html` | No bid security option: Bid Securing Declaration; no 28/56-day validity rule; no foreign bank confirmation tracking; no Non-Collusion Affidavit in document checklist; no debarment status display |
| `session-management.html` (or equivalent) | No "First Meeting — Declarations" step; no member conflict-of-interest declaration tracking |
| *(missing)* | No **post-qualification** screen for lowest bidder |
| *(missing)* | No **standstill period** + **debriefing** + **appeal** workflow screen |
| *(missing)* | No **price adjustment** (evaluated price) screen in financial evaluation |
| *(missing)* | No **contract variations** screen with VRC workflow (post-award) |
| *(missing)* | No **bidder debarment management** screen in Bidder Database |

---

## Part 7 — Priority Summary

### Priority 1 — Legal Compliance (Must have before go-live)
1. **GAP-05:** Deviation classification (Major/Minor/Debatable) — core evaluation methodology
2. **GAP-07:** 4-stage structured evaluation (the Guidelines' mandated process), including BEC's own Stage 1 bid opening record, document completeness table, and separate commercial terms responsiveness table
3. **GAP-06:** Price adjustment mechanisms — all 14 steps from Annexure VI, producing Quoted→Evaluated Price
4. **GAP-08:** Post-qualification stage for lowest evaluated bidder
5. **GAP-01:** Standstill period (10 working days) before award
6. **GAP-03:** Appeal intake recording
7. **GAP-18:** Conflict of interest declarations at first BEC/BOC meeting

### Priority 2 — Process Compliance (Should have)
8. **GAP-02:** Debriefing of unsuccessful bidders
9. **GAP-04:** Award publication
10. **GAP-09:** Bid security rules (validity calculation, declaration type)
11. **GAP-10:** Bidding period minimum validation
12. **GAP-11:** Pre-bid meeting rules (10-day, 3-day addendum)
13. **GAP-12:** BOC detailed opening procedure
14. **GAP-14:** Bidding procedure type (Single Stage Two Envelope has different opening workflow)
15. **GAP-15:** Non-Collusion Affidavit in bidder document checklist
16. **GAP-16:** VRC for post-award contract variations

### Priority 3 — Enhanced Governance
17. **GAP-13:** Multiple PC types support
18. **GAP-17:** Bidder debarment/blacklisting tracking in Bidder Database
19. **UNDER-05:** BEC Chair as non-member in PC meetings
20. **UNDER-06:** PC partial acceptance, dissent recording

---

## Part 8 — Action Plan

| Action | Affects | Priority |
|---|---|---|
| Update `SYSTEM_SPECIFICATION.md` — Add 4-stage evaluation framework | Sections 6.7, 12.1, 13.2 | P1 |
| Update spec — Add deviation classification (Major/Minor/Debatable) | Section 5.8, 6.7, 13.2 | P1 |
| Update spec — Add price adjustment workflow (evaluated price) | Sections 5.10, 6.9, 12.1 | P1 |
| Update spec — Add post-qualification stage | Sections 5.7, 6.7, 12.1 | P1 |
| Update spec — Add standstill period + debriefing + appeals | Sections 4.1, 12.1, new section | P1 |
| Update spec — Add bid security detailed rules | Section 5.9 | P2 |
| Update spec — Add bidding period validation | Section 5.5, 6.4 | P2 |
| Update spec — Enhance pre-bid meeting rules | Section 5.12 | P2 |
| Update spec — Enhance BOC procedure | Section 6.6, 12.1 | P2 |
| Update spec — Add award publication | Section 5.16, 12.1 | P2 |
| Update spec — Add PC type configuration | Sections 3.3, 3.5.3 | P3 |
| Update spec — BEC Chair non-member role in PC meetings | Sections 3.5, 6.14 | P3 |
| Update spec — PC partial acceptance + dissent recording | Section 6.13, 12.1 | P3 |
| Relabel "Requestor Clarification" → "Internal Technical Consultation" | Sections 5.9, 6.8, 8.3, 13.4 | P2 |
| Update spec — Add declaration forms tracking for first BEC/BOC meeting | Section 3.4, 6.2, new | P1 |
| Update spec — Add bidding procedure type (One/Two Envelope, Two Stage) | Sections 4.2, 5.4, 6.6 | P2 |
| Update spec — Add Non-Collusion Affidavit to standard document checklist | Section 5.9 | P2 |
| Update spec — Add VRC (Variation Review Committee) for post-award variations | Section 5.16, new | P2 |
| Update spec — Add bidder debarment tracking in Bidder Database | Section 7.2, new | P3 |
| **New wireframes:** Post-qualification screen | New | P1 |
| **New wireframes:** Standstill/Debriefing/Appeal screen | New | P1 |
| **New wireframes:** Price adjustment 14-step evaluated price screen | New | P1 |
| **New wireframes:** Contract Variations (VRC) screen for post-award | New | P2 |
| **New wireframes:** Bidder Debarment management screen | New | P3 |
| **Update wireframes:** evaluation-grid.html — add Stage 1/2/3/4 tabs, BEC bid opening record, document completeness table, commercial terms responsiveness table, deviation types | Existing | P1 |
| **Update wireframes:** bid-opening.html — Two Envelope branch, withdrawal, modification, initials, handover | Existing | P2 |
| **Update wireframes:** report-preview.html — add missing tables (14-step price, BEC opening record, commercial terms responsiveness, post-qualification) | Existing | P1 |
| **Update wireframes:** tender-creation.html — PC type, bidding procedure type, bidding period validation | Existing | P2 |
| **Update wireframes:** pm-dashboard.html — standstill timer, appeal status | Existing | P1 |
| **Update wireframes:** bidder-detail.html — bid security rules, Non-Collusion Affidavit, debarment status | Existing | P2 |
| **Update wireframes:** pre-bid-meeting.html — document purchasers, addendum rule | Existing | P2 |
| **Update wireframes:** session-management — First Meeting declaration signing step | Existing | P1 |

---

## Part 9 — Second Iteration Findings Summary

*Chapters newly read or re-read: Ch.01 (Annexure III), Ch.03 (full), Ch.04 (full), Ch.05 §§5.16–5.19, Ch.07 Annexures I–VIII, Ch.09 (full), Ch.10 (full)*

### New Gaps Identified (GAP-14 through GAP-18)

| Gap ID | Summary | Priority |
|---|---|---|
| GAP-14 | Bidding Procedure Type (Single Stage One/Two Envelope, Two Stage) — Two Envelope requires separate technical envelope opening event | P2 |
| GAP-15 | Non-Collusion Affidavit — standard required bidder document, currently missing from document checklist | P2 |
| GAP-16 | VRC (Variation Review Committee) — post-award contract variation review with approval thresholds (7.5% AO, 10% CAO, >10% Cabinet) | P2 |
| GAP-17 | Bidder Debarment/Blacklisting — 3-level system (national/ministry/PE), must be tracked in Bidder Database | P3 |
| GAP-18 | Conflict of Interest Declarations — all PC/BEC members AND support staff must sign at first meeting; officer who prepared specs cannot evaluate | P1 |

### Key Refinements to Existing Gaps

| Gap ID | Refinement |
|---|---|
| GAP-06 | Expanded from 10 to 14 adjustment steps (confirmed by Annexure VI(a) of Ch.07). Added: Exclude VAT/contingencies first, Reassess ranking, After-sales services, Examine for unbalanced bidding, Clarifications during evaluation |
| GAP-07 | Stage 1 now has 3 confirmed sub-steps (basic data, BEC's own bid opening record, document completeness table). Stage 2 confirmed as two separate tables: commercial terms (Annexure IV) + technical (Annexure V). BER official structure has 9 sections (Annexure VIII), not 14 — spec's 14-section format is acceptable since "BEC has liberty to design suitable format." |
| GAP-08 | Confirmed: post-qualification appears as Section 5.6 in official BER Annexure VIII structure |
| GAP-12 | BEC prepares its own bid opening record (Annexure II) separate from BOC's report — this is Stage 1b of evaluation, not just BOC documentation |

### Key Technical Relationships Confirmed

- **Performance Security timing:** Bid Securities are released ONLY AFTER performance security submitted AND contract agreement signed (§5.19)
- **Advance Payments for Goods:** Max 30% of contract value; advance payment guarantee required
- **Repeat Orders:** Additional purchases from same supplier — within 6 months of award, <50% of original quantity, same PC must agree
- **PTS approval:** Must occur at PC's FIRST meeting specifically (not just "at some point")
- **Price Adjustments (§5.16):** Applicable when contract period exceeds 3 months; CIDA formula for locally-funded projects
- **Single Substantially Responsive Bid:** Even if only one bid is received, it shall be evaluated if responsive and price is reasonable (§7.10)
