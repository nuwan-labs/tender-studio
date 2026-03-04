# SPEC GAPS v3.1
## TenderStudio SYSTEM_SPECIFICATION.md v3.0 — Gaps vs. National Procurement Manual 2024

**Version**: 1.0
**Date**: 2026-02-26
**Based on**: Full word-by-word reading of National Procurement Manual 2024 (Goods, Works and Non-Consulting Services), effective 01.01.2025
**Previous gap analysis**: GAP_ANALYSIS.md (18 gaps, v2.4 spec)
**This document**: Comprehensive gaps for v3.0 spec; supersedes and expands GAP_ANALYSIS.md

---

## How to Read This Document

Each gap entry has:
- **GAP-XX**: Unique identifier
- **Priority**: P1 (Critical — legal/compliance blocker), P2 (Important — significant missing feature), P3 (Minor — detail or edge case)
- **Manual Citation**: Specific chapter, section, annexure
- **Exact Manual Text**: Direct quote or precise paraphrase
- **Current Spec Status**: What v3.0 says (or doesn't say)
- **Required Change**: What must be added/changed in the spec
- **Affected Screens**: Screens/components needing changes

---

## PART 1 — ETHICS AND CONFLICT OF INTEREST

### GAP-19: Staff Officer COI Declaration (New — was not in v2 analysis)

**Priority**: P1 (Critical)
**Manual Citation**: Ch 01 §1.2, Annexure II
**Exact Manual Text**: "Every staff officer, secretary and support staff attached to a Procurement Committee must sign the Conflict of Interest Declaration at the first meeting." (Annexure II provides the specific form for staff officers, distinct from Annexure I for PC/BEC members)
**Current Spec Status**: Spec v3.0 mentions COI declarations for BEC members at first session (GAP-18 was partially addressed in v3.0 for BEC members), but does NOT address COI for the BEC Secretary or other support staff attached to the committee.
**Required Change**: Add a separate COI declaration workflow for staff officers (BEC Secretary, technical advisors, administrative support) at the first BEC meeting. Create Annexure II format form in system. Block evaluation commencement until all staff COI declarations are recorded.
**Affected Screens**: BEC Session (first session) → Staff COI Declaration; BEC Assignment → Secretary COI requirement

---

### GAP-20: Spec Preparer Cannot Evaluate

**Priority**: P1 (Critical)
**Manual Citation**: Ch 01 §1.2
**Exact Manual Text**: "An officer who prepared or contributed to the technical specifications of a procurement shall not be a member of the BEC evaluating that procurement."
**Current Spec Status**: Not mentioned in v3.0. The spec allows any BEC Eligibility Pool member to be assigned to any BEC without checking specification authorship.
**Required Change**: Add a field to each tender "Specification Prepared By" (user reference). During BEC assignment, validate that none of the nominated BEC members authored the specifications for this tender. Display a BLOCK error if a match is found.
**Affected Screens**: Tender Creation → Spec Preparer field; BEC Assignment → Preparer-member conflict check

---

### GAP-21: Non-Collusion Affidavit — Sworn vs. Signed Distinction

**Priority**: P1 (Critical)
**Manual Citation**: Ch 01 §1.3, Annexure III
**Exact Manual Text**: "The Non-Collusion Affidavit must be sworn before a Justice of the Peace or Commissioner of Oaths. A signed letter is not sufficient."
**Current Spec Status**: v3.0 includes NCA in document checklist (from earlier gap work) but treats it as a checkable document without distinction from other documents. The system does not enforce the "sworn before JP/CoO" requirement or capture the JP/CoO details.
**Required Change**: Add NCA as a special document type in the checklist. Add fields: "Sworn before JP/Commissioner of Oaths" (Yes/No), "JP/CoO Name", "JP/CoO registration number". If "No", block Stage 1 completion. Document checklist NCA item must show these sub-fields distinct from a simple "received" checkbox.
**Affected Screens**: Document Checklist → NCA → Sworn status and JP/CoO fields

---

### GAP-22: COI Recusal Workflow

**Priority**: P1 (Critical)
**Manual Citation**: Ch 01 §1.2
**Exact Manual Text**: "Any PC or BEC member who has a conflict of interest must disclose it immediately upon discovery and recuse themselves from all deliberations relating to the conflicted procurement."
**Current Spec Status**: Not in v3.0. System has no mechanism to record mid-evaluation COI disclosures or to handle recusal (marking a member as excluded from decisions without removing them from the committee entirely).
**Required Change**: Add COI recusal action available at any point during evaluation. When triggered: member marked as "Recused" for that tender, their votes/decisions invalidated for that tender, PM and System Admin notified, immutable audit log entry created. Recused member retains read access for transparency but cannot write decisions.
**Affected Screens**: BEC Session → Member → Recusal action; Audit Trail → COI Recusal log

---

## PART 2 — PROCUREMENT COMMITTEES

### GAP-23: PC Type Auto-Suggestion from TCE

**Priority**: P2 (Important)
**Manual Citation**: Ch 02 §2.1, §2.2
**Exact Manual Text**: "The type of Procurement Committee to be constituted shall be determined by the estimated value of the procurement as defined in the TCE."
**Current Spec Status**: v3.0 has a PC type dropdown but does not automatically suggest the correct PC type based on TCE value. The PM must manually select.
**Required Change**: When PM enters TCE value during tender creation, system auto-calculates and suggests the minimum required PC type based on thresholds: RPC (≤50Mn GOSL / ≤100Mn foreign), DPC/PPC (≤400Mn GOSL / ≤800Mn foreign), MPC (≤750Mn GOSL / ≤1,500Mn foreign), HLPC/SHLPC (>750Mn GOSL / >1,500Mn foreign). PM can select a higher-level PC but system warns if lower level selected.
**Affected Screens**: Tender Creation → TCE → PC Type auto-suggest + override warning

---

### GAP-24: BEC Concurrent Assignment Limit

**Priority**: P2 (Important)
**Manual Citation**: Ch 02 §2.5
**Exact Manual Text**: "No officer shall serve on more than six (6) Bid Evaluation Committees at the same time."
**Current Spec Status**: Not in v3.0. System has no check on how many concurrent BECs a person is serving on.
**Required Change**: When adding a member to a BEC, count their currently active BEC assignments across all tenders in the system. If count equals 6, display a BLOCK error: "This officer is already serving on 6 active BECs (maximum allowed by Ch 02 §2.5). Cannot assign additional BEC membership until one is concluded." Display current count for each candidate in the BEC assignment UI.
**Affected Screens**: BEC Assignment → Candidate list → Active BEC count column; Assignment → Block validation

---

### GAP-25: BEC Absence Tracking and Penalties

**Priority**: P2 (Important)
**Manual Citation**: Ch 02 §2.6
**Exact Manual Text**: "Three consecutive unexplained absences from BEC sessions shall result in removal from the BEC Eligibility Pool. Two absences within a single procurement shall require notification to the CAO. The first unexcused absence in a single procurement shall result in a 40% reduction in the attendance allowance for the first meeting's allowance."
**Current Spec Status**: v3.0 has attendance tracking but no automated consequence enforcement — no consecutive absence counter, no CAO notification trigger, no payment penalty calculation.
**Required Change**:
(1) Per-procurement absence counter: track excused vs. unexcused absences per member per tender.
(2) Trigger at 2 unexcused: generate CAO notification form (auto-filled, requires PM to dispatch).
(3) Trigger at 3 consecutive unexcused: flag member for eligibility pool removal (System Admin reviews and confirms).
(4) First unexcused absence: flag 40% attendance penalty on payment calculation.
(5) System-wide consecutive absence counter (across all procurements): if 3 consecutive across any combinations of procurements, flag for pool removal.
**Affected Screens**: Session Controls → Attendance → Absence type (Excused/Unexcused); BEC Payment → Penalty flags; System Admin → Eligibility Pool → Removal flag

---

### GAP-26: BEC Payment Within/Outside PTS Calculation

**Priority**: P2 (Important)
**Manual Citation**: Ch 02 §2.9
**Exact Manual Text**: "Where the evaluation is completed within the Procurement Time Schedule (PTS), the full rate of allowance shall be paid. Where the evaluation is completed outside the PTS, 50% of the allowance rate shall apply. The total payment shall not exceed the officer's annual basic salary."
**Current Spec Status**: v3.0 has post-award tracking but BEC payment calculations are not in scope. PTS is referenced but the payment rate determination against PTS is not modeled.
**Required Change**: Add BEC Payment module with: (a) PTS reference date for evaluation completion, (b) actual evaluation completion date, (c) within/outside PTS determination → 100% or 50% rate, (d) session attendance count per member, (e) per-session allowance from payment schedule (Annexure IX Ch 02), (f) 40% penalty deduction for first unexcused absence, (g) annual salary cap check (requires salary data for members), (h) payment schedule generation (75% on report submission, 25% on contract award). Export as payment schedule form matching Annexure IX format.
**Affected Screens**: New screen: BEC Payment Module; links from: Report Generation (75% trigger), Award screen (25% trigger)

---

### GAP-27: BOC Minimum Size and Chair Independence

**Priority**: P2 (Important)
**Manual Citation**: Ch 02 §2.7
**Exact Manual Text**: "The BOC shall consist of a minimum of three (3) members. The Chair of the BOC shall, wherever possible, be an officer independent of the Procuring Entity."
**Current Spec Status**: v3.0 has BOC assignment but no minimum member count validation and no Chair independence flag.
**Required Change**: Add validation: BOC must have minimum 3 members before bid opening proceeds. Add "Chair Independence" field: dropdown (Internal / External — Independent) with warning if Chair is marked Internal when external alternative is available.
**Affected Screens**: BOC Assignment → Member count validation; BOC Assignment → Chair field → Independence flag

---

## PART 3 — PROCUREMENT PLANNING

### GAP-28: PTS (Procurement Time Schedule) Full Module

**Priority**: P2 (Important)
**Manual Citation**: Ch 04 §4.2, Annexures VIII, IX, X
**Exact Manual Text**: "A Procurement Time Schedule (PTS) shall be prepared for each procurement covering all activities from preparation of procurement documents through contract award. The PTS is used to determine BEC payment rates (within/outside PTS). Activity-level time frames by procurement category are specified in Annexures VIII, IX, X."
**Current Spec Status**: v3.0 mentions PTS conceptually but does not have a full PTS management module — no activity-by-activity tracking, no planned vs. actual date comparison, no variance reporting, no method-specific standard templates.
**Required Change**: Add PTS module with: (a) Activity list with planned dates (standard templates per method from Annexure VIII/IX/X), (b) Responsibility per activity (PM, BEC, BOC, PC, System Admin), (c) Actual completion date entry, (d) Variance calculation (working days), (e) PTS dashboard showing delays highlighted in red, (f) PTS completion date referenced by BEC Payment module. Standard templates: NCB Goods, NCB Works, ICB Goods, ICB Works, Shopping, etc.
**Affected Screens**: New screen: Tender PTS; linked from: Tender Creation → PTS Setup; linked to: BEC Payment → PTS reference

---

### GAP-29: Anti-Slicing Validation

**Priority**: P2 (Important)
**Manual Citation**: Ch 04 §4.3
**Exact Manual Text**: "The artificial division of procurement requirements into smaller packages to circumvent the authority of a higher-level Procurement Committee is prohibited."
**Current Spec Status**: Not in v3.0. System has no mechanism to detect or warn about potential artificial slicing.
**Required Change**: Add aggregate monitoring: when a PM creates multiple tenders for the same category (e.g., same goods type, same department) within a 12-month rolling window, display a WARN: "Multiple procurements of similar type detected. Ensure these are not artificial packages that should be combined. Aggregate value: LKR X. Applicable PC type for aggregate: [PC type]. Current selected PC type: [PC type]. If packages are genuinely separate, document justification." Justification field required to proceed.
**Affected Screens**: Tender Creation → Anti-slicing WARN dialog; Admin → Procurement analytics view

---

### GAP-30: TCE Confidentiality and Preparer Separation

**Priority**: P1 (Critical)
**Manual Citation**: Ch 04 §4.4
**Exact Manual Text**: "The Total Contract Estimate (TCE) must be prepared by an independent technical officer, not the procurement officer responsible for the procurement. The TCE is confidential and must not be disclosed to bidders before bid closing."
**Current Spec Status**: v3.0 has a TCE field but no enforcement of preparer separation and no explicit confidentiality control (i.e., who can see the TCE).
**Required Change**: (a) Add "TCE Prepared By" field (references a user, cannot be same as the PM managing the tender). System validates this is a different user. (b) TCE value visible only to PM, System Admin, and authorized PC members — never to BEC during technical evaluation phase. (c) TCE remains hidden from all public-facing views. (d) Audit log whenever TCE is accessed.
**Affected Screens**: Tender Creation → TCE field with Preparer field; Access control → TCE visibility rules

---

## PART 4 — BID DOCUMENTS AND VALIDITY

### GAP-31: Bid Validity Period Lookup Table

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.3, Table
**Exact Manual Text**: Specific table of recommended bid validity periods:
- Goods <LKR 5Mn: 49 days
- Goods LKR 5-50Mn (GOSL): 91 days
- Goods LKR 50-750Mn (GOSL): 105 days
- Goods >LKR 750Mn (GOSL): 119 days
- Works <LKR 5Mn: 49 days
- Works LKR 5-50Mn: 91 days
- Works LKR 50-500Mn (GOSL): 119 days
- Works >LKR 500Mn (GOSL): 147 days
- Works ≥LKR 3,000Mn (foreign-funded): 203 days
**Current Spec Status**: v3.0 has a bid validity field but no lookup table, no validation against recommended periods, no warning if entered value is below recommended minimum.
**Required Change**: Implement lookup logic: based on Contract Type (Goods/Works) and TCE value and Funding Source (GOSL/Foreign), auto-populate the recommended bid validity period. Allow PM to enter different value with a WARN if below recommended minimum. Display recommended value next to field for reference.
**Affected Screens**: Tender Creation → Bid Validity → Auto-suggest from type/value/funding

---

### GAP-32: Bid Validity Extension Workflow

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.3
**Exact Manual Text**: "If a bid validity extension is required, the PE shall request extension from all bidders. A bidder may refuse the extension without losing their bid security. If a bidder agrees to the extension, the bid security must be extended accordingly. An extended bid security must be obtained before the extended validity period commences."
**Current Spec Status**: Not in v3.0.
**Required Change**: Add bid validity extension workflow: (a) PM initiates extension request → system generates extension request letter for all bidders, (b) per-bidder response: Accept (with new bid security expiry date) / Decline, (c) Declined bidders: bid security returned, bidder excluded from further evaluation, (d) Accepted but bid security not extended by deadline: treat as declined, (e) Extension tracked in audit log with dates.
**Affected Screens**: Tender → Bid Validity Extension screen; Bidder list → Extension response per bidder

---

### GAP-33: Bid Security Validity Rules (NCB vs. ICB)

**Priority**: P1 (Critical)
**Manual Citation**: Ch 05 §5.4
**Exact Manual Text**: "For NCB and LNB procurements, the bid security shall be valid for a period of 28 days beyond the bid validity period. For ICB and LIB procurements, the bid security shall be valid for a period of 56 days beyond the bid validity period."
**Current Spec Status**: v3.0 captures bid security expiry date but does not validate it against the bid validity period + the required extension (28 or 56 days). The validation is not method-aware.
**Required Change**: At bid opening entry, calculate minimum required bid security expiry date = bid validity date + 28 days (NCB/LNB) or + 56 days (ICB/LIB). If entered bid security expiry < minimum: WARN evaluator and log. If significantly short (< bid validity period itself): BLOCK with explanation. Display the calculated minimum in the bid security form.
**Affected Screens**: Bid Opening → Bid Security Entry → Expiry date validation; Stage 1 → Bid security validity check

---

### GAP-34: Bid Security Forfeiture Tracking

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.4
**Exact Manual Text**: "The bid security shall be forfeited if: (a) a bidder withdraws their bid after the bid submission deadline and during the bid validity period; (b) a bidder submits false information; (c) the successful bidder refuses to accept the Letter of Acceptance; (d) the successful bidder fails to provide the required performance security."
**Current Spec Status**: Not in v3.0.
**Required Change**: Add bid security forfeiture action available after bid opening. Forfeiture triggers: (a) Late withdrawal, (b) False information found during evaluation, (c) LOA refusal, (d) Performance security non-submission. For each trigger: log reason, flag bidder record in Bidder Database, generate forfeiture notice, update bid security status to "Forfeited". Integration with Bidder Database to flag forfeiture history.
**Affected Screens**: Bidder record → Bid Security → Forfeiture action + reason; Bidder Database → Forfeiture history flag

---

### GAP-35: Performance Security Waiver for Small Contracts

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.5
**Exact Manual Text**: "Performance security may be waived for contracts with a value of less than LKR 5 million."
**Current Spec Status**: v3.0 has performance security tracking but no automatic waiver logic for small contracts.
**Required Change**: When contract value < LKR 5Mn, display INFO: "Performance security may be waived per Ch 05 §5.5 (contracts <LKR 5Mn)." Add a "Waive Performance Security" checkbox with mandatory justification field. Waiver decision logged in audit trail.
**Affected Screens**: Award → Contract → Performance Security → Waiver option for <5Mn contracts

---

### GAP-36: Advance Payment Rules (All Categories)

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.6
**Exact Manual Text**: "For Works contracts exceeding LKR 50 million, the advance payment shall not exceed 20% of the contract value. For Works contracts not exceeding LKR 50 million, the advance payment shall not exceed 30%. For Goods contracts, the advance payment shall not exceed 30% of the contract value. Advance payment requires an unconditional bank guarantee of equal amount."
**Current Spec Status**: Not in v3.0. Post-award tracking tracks delivery/installation but no advance payment management.
**Required Change**: Add Advance Payment module to post-award tracking: (a) Advance payment amount field with cap validation based on contract type and value, (b) Advance payment guarantee tracking (bank, amount, expiry), (c) Advance recovery tracking — show outstanding balance as contractor submits progress claims, (d) Advance recovery rate = same percentage as advance percentage.
**Affected Screens**: Post-Award → Advance Payment section; Contract → Advance Payment setup

---

### GAP-37: Retention Money Management (Works)

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.7
**Exact Manual Text**: "Retention money of not less than 5% shall be deducted from each progress payment. The first 50% of the accumulated retention may be released upon certification of completion of works, provided a retention money guarantee is submitted. The remaining 50% shall be released after the expiry of the Defects Liability Period."
**Current Spec Status**: Not in v3.0. Post-award tracking covers delivery/commissioning milestones but no retention money management.
**Required Change**: Add Retention Money module for Works contracts: (a) Retention rate (minimum 5%) field, (b) Per-payment deduction tracking, (c) Accumulated retention total, (d) First release trigger (50% on completion + retention guarantee), (e) Final release trigger (50% after DLP expiry), (f) Defects Liability Period end date field, (g) Retention guarantee tracking.
**Affected Screens**: Post-Award → Retention Money section (Works only); Contract → Retention Rate setup

---

### GAP-38: Liquidated Damages Calculation Module

**Priority**: P2 (Important)
**Manual Citation**: Ch 05 §5.8
**Exact Manual Text**: "Liquidated damages for Goods: maximum 1% per week of delay. For Works: maximum 0.05% per day of delay. In both cases, the total LD is capped at 10% of the contract value."
**Current Spec Status**: Not in v3.0.
**Required Change**: Add LD module to post-award tracking: (a) LD rate field pre-populated based on contract type (Goods: 1%/week, Works: 0.05%/day), (b) Delay calculation: actual delivery/completion date vs. contractual date, (c) LD amount = delay period × LD rate × contract value, (d) Cap enforcement: total LD ≤ 10% of contract value, (e) LD applied to payment certification (deducted from final payment), (f) Bonus for early completion field if applicable.
**Affected Screens**: Post-Award → Liquidated Damages section; Payment certification → LD deduction

---

## PART 5 — BID OPENING

### GAP-39: Pre-Opening Procedures (30-Minute Rule)

**Priority**: P2 (Important)
**Manual Citation**: Ch 06 §6.6
**Exact Manual Text**: "The bid opening ceremony shall commence not later than 30 minutes after the bid submission closing time."
**Current Spec Status**: v3.0 has bid opening screen but no validation that opening occurs within 30 minutes of closing time.
**Required Change**: Record bid closing time during tender setup. At bid opening event creation, validate that opening start time ≤ closing time + 30 minutes. Display WARN if 30-minute window is exceeded. Log actual opening start time vs. closing time in audit trail.
**Affected Screens**: Bid Opening → Opening Start Time → 30-minute rule validation

---

### GAP-40: BOC Initialing of Bid Pages

**Priority**: P2 (Important)
**Manual Citation**: Ch 06 §6.6
**Exact Manual Text**: "All members of the BOC shall initial each page of every bid document at the time of opening to confirm authenticity of the document."
**Current Spec Status**: v3.0 captures bid opening details but no BOC initialing confirmation.
**Required Change**: Add per-bidder checklist item: "BOC members initialed all pages" (checkbox, mandatory before closing BOC record for that bidder). System cannot finalize BOC record without this confirmation. Generate BOC Initial Record as a printed checklist for physical use during opening.
**Affected Screens**: Bid Opening → Per-bidder record → BOC Initial Confirmation checkbox

---

### GAP-41: Bid Modification and Withdrawal Processing

**Priority**: P1 (Critical)
**Manual Citation**: Ch 06 §6.5
**Exact Manual Text**: "If modification or withdrawal envelopes are received before bid closing, marked clearly as 'Modification' or 'Withdrawal', they shall be processed at bid opening in the following order: (1) the withdrawal envelope, if present, is opened first — bid is withdrawn and all envelopes returned unopened; (2) the modification envelope is opened and supersedes the original bid for those items modified."
**Current Spec Status**: Not in v3.0. System processes bids but has no modification or withdrawal envelope workflow.
**Required Change**: Add withdrawal and modification handling at bid opening: (a) Per-bidder: "Withdrawal received?" (Y/N) → if Yes: bid excluded, all envelopes returned, audit log entry, (b) "Modification received?" (Y/N) → if Yes: record which items/prices are modified, modification envelope opened first, original bid updated with modification. Only unconditional modifications affecting already-submitted items allowed.
**Affected Screens**: Bid Opening → Per-bidder → Modification/Withdrawal flags

---

### GAP-42: Addendum Distribution to All Purchasers

**Priority**: P2 (Important)
**Manual Citation**: Ch 06 §6.4
**Exact Manual Text**: "Any addendum or clarification issued following a pre-bid meeting or query must be communicated to all persons who have purchased the bid documents, not just those who attended the pre-bid meeting. The addendum must be issued at least 3 working days before the bid closing date."
**Current Spec Status**: v3.0 has pre-bid meeting management and addendum issuance but no document purchasers list and no distribution tracking to all purchasers.
**Required Change**: Add document purchasers list to each tender (name, organization, contact, date purchased). Addendum distribution workflow sends to all purchasers (not just pre-bid attendees). System enforces 3-working-day minimum before closing date. Delivery confirmation per purchaser tracked.
**Affected Screens**: Tender → Document Purchasers List; Pre-Bid Meeting → Addendum → Distribution to purchasers list; Addendum → 3-working-day validation

---

## PART 6 — EVALUATION STAGES

### GAP-43: Stage 1 Formal Output (C/NC Table)

**Priority**: P2 (Important)
**Manual Citation**: Ch 07 §7.1, Annexures I, II, III
**Exact Manual Text**: "The BEC shall complete three tables in Stage 1: (1) Basic Data Table (Annexure I): bid dates, validity, security details; (2) BEC Bid Opening Records (Annexure II): verification of BOC records; (3) Document Completeness Table (Annexure III): per-bidder checklist. Each bidder receives a Compliant (C) or Non-Compliant (NC) status."
**Current Spec Status**: v3.0 has document checklist but no formal three-table Stage 1 output with C/NC per-bidder status. The connection between Stage 1 output and Stage 2 gating is not explicit.
**Required Change**: Create formal Stage 1 evaluation module with three distinct tables matching Annexures I, II, III format. Each table has a per-bidder C/NC column. Overall Stage 1 result per bidder: C = all three tables compliant; NC = any non-compliance. NC bidders flagged for Stage 2 handling (BEC may request clarification for minor issues or recommend rejection). Stage 1 output feeds directly into Stage 2 — NC bidders pre-flagged.
**Affected Screens**: Evaluation → Stage 1 tab → Three table view; Stage 2 → Pre-flag from Stage 1

---

### GAP-44: Stage 2 Commercial Terms Table

**Priority**: P2 (Important)
**Manual Citation**: Ch 07 §7.2, Annexure IV(a), IV(b)
**Exact Manual Text**: "Stage 2 consists of two sub-stages: (a) assessment of commercial terms (payment terms, delivery period, warranty, after-sales service, spare parts availability, country of origin) against bid requirements; (b) assessment of technical requirements. Both must be independently assessed. A bidder is Non-Responsive if either commercial or technical assessment fails."
**Current Spec Status**: v3.0 has a technical evaluation grid (rows = requirements, columns = bidders) but no separate commercial terms assessment table. Commercial terms are not broken out as a distinct Stage 2 sub-stage.
**Required Change**: Add Stage 2a (Commercial Terms) as a distinct evaluation table before Stage 2b (Technical). Commercial Terms table: rows = commercial criteria (payment terms, delivery period, warranty period, after-sales terms, spare parts availability, country of origin, delivery location). Per-bidder assessment: Compliant (C) / Non-Compliant (NC). Deviation type for commercial terms: Major / Minor. Result: Commercially Responsive (R) or Non-Responsive (NR). Both Stage 2a and Stage 2b must be R for bidder to proceed to Stage 3.
**Affected Screens**: Evaluation → Stage 2 → Commercial Terms tab (new); Evaluation → Stage 2 → Technical tab (existing grid)

---

### GAP-45: Debatable Deviation Handling

**Priority**: P2 (Important)
**Manual Citation**: Ch 07 §7.2
**Exact Manual Text**: "A 'Debatable' deviation is one where BEC members cannot reach consensus. In such cases, the BEC Chair shall apply a conservative interpretation (treating it as Major if in doubt) OR refer the matter to the Procurement Committee for guidance before proceeding."
**Current Spec Status**: v3.0 has Major/Minor deviation classification. Debatable deviation is mentioned in CLAUDE.md but the handling workflow (conservative interpretation or PC referral) is not in the spec.
**Required Change**: Add "Debatable" as a third deviation classification option in the evaluation grid. When selected: (a) require BEC Chair's resolution decision: "Apply conservative interpretation (treat as Major)" or "Refer to PC for guidance", (b) if PC referral selected: evaluation of that cell BLOCKED until PC decision received and recorded, (c) PC decision recorded by Secretary (PM dispatches to PC, response entered), (d) Cell then updated to Major or Minor based on PC decision.
**Affected Screens**: Evaluation Grid → Cell → Deviation type → "Debatable" option; Evaluation → PC Referral for Debatable deviation

---

### GAP-46: Combined Responsiveness Gate (Stage 2a + 2b)

**Priority**: P1 (Critical)
**Manual Citation**: Ch 07 §7.2
**Exact Manual Text**: "A bid shall be declared Non-Responsive if it fails either the commercial terms assessment or the technical requirements assessment. Non-Responsive bids shall not be further evaluated in Stage 3."
**Current Spec Status**: v3.0 implies technical NR excludes from price evaluation, but the combined commercial + technical gate and the explicit blocking from Stage 3 is not formally specified.
**Required Change**: Add an explicit combined responsiveness result at end of Stage 2: for each bidder, display: Stage 2a Result (R/NR), Stage 2b Result (R/NR), Combined Result (R if BOTH are R, else NR). NR bidders physically EXCLUDED from Stage 3 table (greyed out, locked). System prevents any price evaluation entries for NR bidders. Stage 3 table only shows R bidders.
**Affected Screens**: Evaluation → Stage 2 → Combined Responsiveness summary; Stage 3 → NR bidder exclusion visual

---

### GAP-47: Arithmetic Correction Module

**Priority**: P1 (Critical)
**Manual Citation**: Ch 07 §7.3, Annexure VI(a) Step 3
**Exact Manual Text**: "Arithmetic corrections shall be made as follows: where there is a discrepancy between unit rate and extended amount, the unit rate shall prevail and the extended amount corrected. Where there is a discrepancy between figures and words, the amount in words shall prevail."
**Current Spec Status**: v3.0 price schedule has unit price and total fields, but no arithmetic correction logic and no rules for which field prevails.
**Required Change**: Add arithmetic correction to price evaluation: (a) Compare unit price × quantity vs. extended amount — if discrepancy > LKR 0 (or configurable threshold), flag and auto-correct extended amount based on unit price, record correction with original and corrected values, (b) Compare numeric field vs. words field — if discrepancy, words prevail, numeric corrected, record both values. All corrections visible in a correction log with "Original: X → Corrected: Y" per line item. Correction total shown separately in price evaluation table.
**Affected Screens**: Price Schedule → Per-line → Arithmetic check fields; Price Evaluation → Correction log; Annexure VI(b) → Correction column

---

### GAP-48: Price Adjustment Steps 4-13 Module

**Priority**: P1 (Critical)
**Manual Citation**: Ch 07 §7.3, Annexure VI(a)
**Exact Manual Text**: Steps 4 through 13 as specified:
- Step 4: Unconditional discounts (announced at opening only)
- Step 5: Omissions (average of other responsive bids)
- Step 6: Delivery period adjustment (>10% late = NR)
- Step 7: Acceptable deviations (monetarily loaded)
- Step 8: Inland transport to destination
- Step 9: Life cycle costing (NPV where specified)
- Step 10: Domestic preference (20% Goods/15% Works GOSL only)
- Step 11: Currency conversion (CBSL selling rate, bid closing date or 28 days prior)
- Step 12: After-sales services factor
- Step 13: Unbalanced bid detection (>50% above weighted average AND >5% of total)
**Current Spec Status**: v3.0 has a basic price evaluation section that captures quoted prices but does not implement any of steps 4-13. This is the most significant functional gap in the evaluation module.
**Required Change**: Implement full 14-step price adjustment module in Stage 3:
- Step 4: Discount entry field (must be recorded as "unconditional, announced at opening" — conditional discounts rejected)
- Step 5: Omission detection (compare line items between bidders; auto-flag missing items; calculate average of other responsive bids as adjustment)
- Step 6: Delivery period field → if > required period × 1.10: auto-flag NR; if ≤ 10% late: calculate monetary penalty per BDS formula
- Step 7: Load minor deviation monetary values from Stage 2 onto bid price
- Step 8: Inland transport cost field (if not included in bid)
- Step 9: NPV life cycle cost field (enabled only if specified in BDS)
- Step 10: DP loading: if foreign goods (GOSL-funded), add 20% to CIF price; if domestic goods, no adjustment; eligibility check for ≥30% local content
- Step 11: Currency conversion: exchange rate entry (CBSL selling rate as of bid closing date or 28 days prior — whichever applies), all prices converted to single currency (LKR)
- Step 12: After-sales service scoring/cost adjustment field
- Step 13: Unbalanced bid: per-line weighted average calculator; flag items >50% above weighted average AND >5% of total evaluated bid price
Final output: Annexure VI(b) table: Bidder | Quoted Price | Steps 2-13 Adjustments | Evaluated Price | Ranking
**Affected Screens**: Evaluation → Stage 3 → 14-Step Price Adjustment module (major new development)

---

### GAP-49: Post-Qualification Full Workflow with Failure Cascade

**Priority**: P2 (Important)
**Manual Citation**: Ch 07 §7.5
**Exact Manual Text**: "Post-qualification shall be applied to the lowest evaluated responsive bidder only. If the lowest evaluated bidder does not satisfy the post-qualification criteria, the PE shall apply post-qualification to the next lowest evaluated bidder, and so on. If no bidder satisfies post-qualification, the procurement shall be cancelled and re-advertised."
**Current Spec Status**: v3.0 has Stage 4 post-qualification referenced but the cascade logic (try next bidder if first fails) and the cancel-and-re-advertise outcome are not specified.
**Required Change**: Stage 4 flow: (a) PQ applied to lowest evaluated bidder (system auto-identifies). (b) PQ criteria checklist: financial capacity (audited financials last 3 years, credit line), technical capacity (equipment, key personnel), relevant experience (similar contracts last 5-10 years), manufacturer authorization (if required). (c) Per-criterion: Satisfactory / Unsatisfactory. (d) If all criteria Satisfactory: bidder passes PQ → recommend award. (e) If any criterion Unsatisfactory: PQ failed → WARN: "Bidder X failed PQ. Proceed to next lowest evaluated bidder?" → BEC Chair confirms → move to next bidder. (f) Track which bidders were PQ'd and results. (g) If all bidders exhausted: WARN: "All evaluated bidders have failed post-qualification. Recommend procurement cancellation and re-advertisement." → generate cancellation recommendation.
**Affected Screens**: Evaluation → Stage 4 → PQ cascade with failure workflow

---

### GAP-50: Unbalanced Bid Investigation Workflow

**Priority**: P2 (Important)
**Manual Citation**: Ch 07 §7.3, Step 13
**Exact Manual Text**: "Where unbalanced bids are detected (individual item price more than 50% above the weighted average of responsive bidders for that item AND more than 5% of the total evaluated bid price), the BEC shall investigate and may seek clarification from the bidder. If the BEC is satisfied the bid poses financial risk to the PE, the bid may be rejected."
**Current Spec Status**: Not in v3.0.
**Required Change**: After Step 13 unbalanced bid detection flags a bidder: (a) System automatically highlights flagged line items, (b) BEC Secretary can initiate a clarification request specifically for "Unbalanced Bid — Item X" (uses existing clarification workflow), (c) After clarification response received: BEC decision — "Satisfied (no financial risk)" → bidder retained; "Not Satisfied (financial risk confirmed)" → bidder rejected (with audit log justification), (d) Rejection at this stage records in Stage 3 results as "Rejected — Unbalanced Bid."
**Affected Screens**: Stage 3 → Unbalanced bid flag → Investigation workflow → Reject/Retain decision

---

## PART 7 — AWARD AND STANDSTILL

### GAP-51: Simultaneous Notification of Intent to Award (All Bidders)

**Priority**: P1 (Critical)
**Manual Citation**: Ch 08 §8.2
**Exact Manual Text**: "Notification of intention to award shall be sent simultaneously to all bidders, both successful and unsuccessful. The notification shall state the name of the intended awardee and their evaluated price."
**Current Spec Status**: v3.0 has standstill period from previous gap work, but the simultaneous notification requirement (including what content must be in the notification) is not fully specified.
**Required Change**: Award notification: (a) System generates notification for ALL bidders simultaneously (one action sends to all), (b) Notification template: "Procurement Reference [X]: It is the intention of [PE] to award to [Bidder Name] at an evaluated price of [LKR amount]. A standstill period of 10 working days applies from the date of this notification. You may request a debriefing within 3 working days." (c) System records dispatch time for ALL bidders (not just winner), (d) Standstill countdown starts from notification dispatch time, (e) Cannot issue LOA until standstill expiry confirmed.
**Affected Screens**: Award → Intent Notification → Simultaneous all-bidder notification; Award → Standstill countdown display

---

### GAP-52: Debriefing Request and Conduct Workflow

**Priority**: P2 (Important)
**Manual Citation**: Ch 08 §8.3
**Exact Manual Text**: "Any unsuccessful bidder may request a debriefing within the 3rd working day from the date of the standstill notification. The PE shall conduct the debriefing and conclude it by the 5th working day. The debriefing shall explain why the bid was unsuccessful without revealing confidential information of other bidders."
**Current Spec Status**: Not in v3.0. No debriefing workflow exists.
**Required Change**: Add debriefing workflow: (a) PM sees "Debriefing Requests" section during standstill period showing which bidders have requested debriefing and on which working day they requested, (b) System tracks 3 working day request window — after 3rd working day, request window closes, (c) Debriefing record per bidder: date conducted, conducted by, notes (what was communicated, what was withheld), (d) Debriefing must be completed by 5th working day — system warns if approaching deadline, (e) Debriefing notes go into audit trail (visible to auditors, not to bidder — only verbal communication).
**Affected Screens**: Award → Standstill → Debriefing Requests tab; PM Dashboard → Pending Debriefings

---

### GAP-53: Appeal Intake Workflow with Body Routing

**Priority**: P2 (Important)
**Manual Citation**: Ch 08 §8.4
**Exact Manual Text**: "Bidders may appeal award decisions to the appropriate appeal body: PAB (HLPC/SHLPC), MPAC (MPC), DPAC/PPAC (DPC/PPC), RPAC (RPC). The applicable appeal fee must accompany the appeal. Upon receipt of an appeal, the standstill period shall be extended until the appeal is resolved."
**Current Spec Status**: v3.0 has appeal tracking from previous gap work but no routing logic, no fee display, and no standstill extension on appeal receipt.
**Required Change**: Add appeal intake: (a) System auto-determines applicable appeal body based on PC type selected for tender, (b) Display appeal body name, address, fee amount prominently in intent-to-award notification, (c) PM can record appeal receipt: bidder name, date received, fee confirmation, (d) On appeal receipt: standstill automatically extends (no automatic LOA block already handles this), (e) Record appeal outcome: Upheld / Dismissed, fee returned/forfeited, award decision: proceed / cancel / re-evaluate, (f) Appeal deadline tracking: PAB/MPAC/DPAC/PPAC 10 working days, RPAC 5 working days — system shows deadline and warns PM.
**Affected Screens**: Award → Appeals tab; Award Notification → Appeal body info display

---

### GAP-54: Award Timing Against Bid Validity

**Priority**: P1 (Critical)
**Manual Citation**: Ch 08 §8.1
**Exact Manual Text**: "The Letter of Acceptance must be issued within the bid validity period. If the bid validity period has expired before LOA can be issued, the PE must first obtain a bid validity extension."
**Current Spec Status**: v3.0 does not validate LOA issuance against bid validity expiry.
**Required Change**: Before LOA generation: validate LOA date ≤ bid validity expiry date for the awardee. If bid validity has already expired: BLOCK LOA issuance with message "Bid validity for [Bidder] expired on [date]. You must obtain a bid validity extension before issuing LOA. Initiating extension workflow." Trigger bid validity extension workflow (GAP-32).
**Affected Screens**: Award → LOA → Bid validity expiry gate

---

## PART 8 — CONTRACT MANAGEMENT

### GAP-55: VRC Full Module (with Cumulative Tracking)

**Priority**: P2 (Important)
**Manual Citation**: Ch 09 §9.2, Annexure I
**Exact Manual Text**: "A Variation Review Committee (VRC) of minimum 3 members (at least 1 technical expert) shall review all variations. Cumulative variations tracked as percentage of original contract value determines approval authority: ≤7.5% AND <DPC threshold → AO/HD/PD; >7.5%≤10% OR >DPC but <MPC threshold → CAO; >10% OR >MPC threshold → Cabinet."
**Current Spec Status**: v3.0 has post-award tracking and VRC referenced from earlier gap work (GAP-16 in v2 analysis) but the full VRC module with cumulative tracking, approval routing, and the standard variation form is not in the spec.
**Required Change**: Add VRC module: (a) VRC formation: member list (min 3, at least 1 technical), (b) Variation form (Annexure I Ch 09 fields): original TCE, original contract price, contingency %, aggregate variations to date (amount and % of original), itemized variation table (original qty/unit/amount vs. variation qty/unit/amount, reason), revised contract price, (c) Auto-calculate cumulative variation % after each variation, (d) Auto-route for approval based on thresholds (display required approver: AO/CAO/Cabinet), (e) Approval decision recorded with date, (f) Contract value automatically updated to revised contract price after approval, (g) Dashboard shows contract value vs. variations vs. remaining contingency.
**Affected Screens**: Post-Award → Variations tab → VRC module; Post-Award → Contract Summary → Live contract value

---

### GAP-56: Debarment Database Integration

**Priority**: P2 (Important)
**Manual Citation**: Ch 10 §10.1
**Exact Manual Text**: "The Procuring Entity must check the national debarment register maintained by the NPC before making any award. A debarred bidder cannot be awarded a contract during the period of debarment but must complete existing contracts."
**Current Spec Status**: v3.0 has a Bidder Database but no debarment status field and no pre-award debarment check.
**Required Change**: (a) Add debarment status to Bidder Database: active/inactive, level (national/ministry/PE), start date, end date, grounds, (b) Before award: system auto-checks if intended awardee has active debarment — if yes, BLOCK award with message, (c) During BEC assignment and BOC assignment: check if any member has debarment-related restrictions, (d) NPC debarment register integration: either API (if available) or manual entry with date-stamped verification, (e) Debarment history visible to System Admin and PM, (f) PE-initiated debarment workflow: show-cause → hearing → decision → report to NPC.
**Affected Screens**: Bidder Database → Debarment status; Award → Pre-award debarment check; Admin → Debarment Management module

---

## PART 9 — BIDDING PERIOD AND PUBLICATION

### GAP-57: Bidding Period Minimum Validation (All Methods)

**Priority**: P1 (Critical)
**Manual Citation**: Ch 06 §6.3
**Exact Manual Text**: Method-specific minimum bidding periods:
- ICB: 42 days
- LIB: 42 days
- NCB/LNB ≥LKR 50Mn: 21 days
- NCB/LNB <LKR 50Mn: 14 days
- Shopping (National): 7 days
- Shopping (International): 14 days
**Current Spec Status**: v3.0 has bid closing date field but no automatic validation against method-specific minimum bidding periods.
**Required Change**: When PM sets bid closing date: calculate days between IFB publication date and closing date. Compare against method-specific minimum. If below minimum: BLOCK with message "Bidding period [X] days is below the minimum [Y] days required for [method] per Ch 06 §6.3." Display minimum and recommended periods next to closing date field based on selected method and value.
**Affected Screens**: Tender Creation → Bid Closing Date → Method-specific minimum validation

---

### GAP-58: Award Publication Rules

**Priority**: P2 (Important)
**Manual Citation**: Ch 06 §6.1 / Ch 08 §8.6
**Exact Manual Text**: "All contract awards must be published on the PE's website. Awards exceeding LKR 750 million must additionally be published in the Gazette in all three official languages (Sinhala, Tamil, English)."
**Current Spec Status**: Not in v3.0. No award publication tracking exists.
**Required Change**: After LOA issuance: (a) Mandatory: record website publication date. If contract >LKR 750Mn: additionally require Gazette publication date entry in all three languages, (b) System generates standard award notice text (in English) pre-populated with contract details, (c) Publication tracking: date published, medium (website/Gazette), language (for Gazette: all three), confirmation by PM.
**Affected Screens**: Award → Post-LOA → Publication Tracking section

---

## PART 10 — SUSTAINABILITY AND CIDA

### GAP-59: CIDA Registration Validation (Works)

**Priority**: P2 (Important)
**Manual Citation**: Ch 11 §11.2
**Exact Manual Text**: CIDA registration required for Works contractors. Grades: CS2-C9 (civil), EM1-EM5 (electro-mechanical), SP1-SP5 (specialized), GP-P, GP-B1 through GP-B4 (general). Minimum grade required depends on contract value per CIDA grading table.
**Current Spec Status**: Not in v3.0.
**Required Change**: For Works contracts, add CIDA Registration check in Stage 4 Post-Qualification: (a) CIDA registration number field, (b) CIDA grade field (dropdown from valid grades), (c) System validates if grade is sufficient for contract value (lookup from CIDA grade table), (d) If grade insufficient: PQ fails on this criterion.
**Affected Screens**: Stage 4 PQ → CIDA Registration fields (Works only)

---

## PART 11 — REPORTING AND DOCUMENTATION

### GAP-60: BEC Minutes of Sessions

**Priority**: P2 (Important)
**Manual Citation**: Ch 02 Annexure II
**Exact Manual Text**: BEC meeting minutes format: evaluation session details, members present, items discussed, decisions per item, next session date, Chair's signature, all members' signatures.
**Current Spec Status**: v3.0 has session controls but no formal session minutes generation.
**Required Change**: At end of each evaluation session, system auto-generates draft session minutes: session number, date, location, members present, start/end time, items evaluated, decisions made (Accept/Reject per cell), clarifications initiated, any PC referrals, next session date. Minutes exported as Word document. Chair must confirm/edit before sign-off. Minutes retained in audit trail.
**Affected Screens**: Session Controls → End Session → Minutes generation; Audit Trail → Session Minutes log

---

### GAP-61: Five Standard BER Annexures

**Priority**: P2 (Important)
**Manual Citation**: Ch 07 §7.7, Annexure VIII
**Exact Manual Text**: "The BER shall include five standard annexures: (1) Pre-bid Meeting Records, (2) Clarification Records, (3) Bid Opening Records (BOC), (4) Price Schedules (per bidder), (5) Document Checklists (per bidder)."
**Current Spec Status**: v3.0 has report generation but the five annexures are not explicitly mapped to system-generated content with their exact content requirements.
**Required Change**: Auto-generate all 5 annexures from system data: (1) Pre-bid Q&A, attendance, addenda from pre-bid meeting module, (2) All clarification threads (bidder and requestor) with dates and responses, (3) BOC financial and general info records, (4) Per-bidder price schedule breakdown as entered in price evaluation, (5) Per-bidder document checklist with C/NC status. Each annexure formatted per manual guidance. Included in BER Word export.
**Affected Screens**: Report Generation → BER Annexures → Auto-generation from linked modules

---

## PRIORITY SUMMARY

| Priority | Count | Gap IDs |
|----------|-------|---------|
| P1 — Critical | 12 | GAP-19, 20, 21, 22, 30, 33, 41, 43 (gate), 46, 47, 48, 51, 54, 57 |
| P2 — Important | 29 | GAP-23 through 29, 31, 32, 34 through 40, 42, 44, 45, 49, 50, 52, 53, 55, 56, 58, 59, 60, 61 |
| **Total new gaps** | **43** | GAP-19 through GAP-61 |

*Note: GAP-01 through GAP-18 were identified in the previous GAP_ANALYSIS.md (v2.4 spec). This document adds GAP-19 through GAP-61 (43 new gaps) identified from full manual reading against v3.0 spec.*

---

## GAPS CARRIED FORWARD FROM V2.4 ANALYSIS (Verify if still open in v3.0)

The following gaps were identified in the original GAP_ANALYSIS.md. They should be verified as still open in v3.0:

| Original Gap | Description | Verify Status |
|-------------|-------------|--------------|
| GAP-01 | Standstill Period (≥10 working days) | PARTIAL — timer exists but notification workflow incomplete (see GAP-51) |
| GAP-02 | Debriefing workflow | Not in v3.0 — see GAP-52 |
| GAP-03 | Appeal intake with body routing | Not in v3.0 — see GAP-53 |
| GAP-04 | Award publication tracking | Not in v3.0 — see GAP-58 |
| GAP-05 | Deviation classification (Major/Minor/Debatable) | PARTIAL in v3.0 — Debatable still missing (see GAP-45) |
| GAP-06 | 14-step price adjustment | Not in v3.0 — see GAP-48 |
| GAP-07 | 4-stage evaluation structure | PARTIAL — Stages exist but Stage 1 formal output and Stage 2a commercial still missing |
| GAP-08 | Post-qualification full workflow | PARTIAL — see GAP-49 |
| GAP-09 | Bid security validity rules (28/56 day) | Not in v3.0 — see GAP-33 |
| GAP-10 | Bidding period minimums | Not in v3.0 — see GAP-57 |
| GAP-11 | Pre-bid meeting 10-day rule and purchasers list | PARTIAL — see GAP-42 |
| GAP-12 | BOC detailed procedure (initialing, tape rule) | Not in v3.0 — see GAP-39, 40, 41 |
| GAP-13 | 6 PC types with thresholds | PARTIAL in v3.0 |
| GAP-14 | Two-envelope procedure | PARTIAL in v3.0 |
| GAP-15 | NCA in document checklist | PARTIAL — sworn requirement not enforced (see GAP-21) |
| GAP-16 | VRC module | Not in v3.0 — see GAP-55 |
| GAP-17 | Bidder debarment database | Not in v3.0 — see GAP-56 |
| GAP-18 | COI declarations | PARTIAL — Staff COI still missing (see GAP-19); recusal still missing (see GAP-22) |

---

*End of SPEC_GAPS_v3_1.md v1.0*
*This document identifies 43 new gaps (GAP-19 through GAP-61) plus 18 carried-forward gaps (GAP-01 through GAP-18) for a total of 61 tracked gaps.*
*Generated from word-by-word reading of National Procurement Manual 2024 cross-referenced with SYSTEM_SPECIFICATION.md v3.0*
