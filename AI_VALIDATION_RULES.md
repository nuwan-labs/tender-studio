# AI VALIDATION RULES
## TenderStudio — AI/System Stress-Testing Ruleset
### National Procurement Manual 2024 (Sri Lanka) — Goods, Works and Non-Consulting Services

**Version**: 1.0
**Date**: 2026-02-26
**Authority**: National Procurement Manual 2024, effective 01.01.2025
**Spec Reference**: TenderStudio SYSTEM_SPECIFICATION.md v3.0 / v4.0
**Purpose**: Every data entry and decision point must pass these rules before the system allows progression. AI-assisted fields are validated against the same rules as manual entries.

---

## How to Read This Document

### Severity Levels

| Severity | Symbol | Behaviour |
|----------|--------|-----------|
| **BLOCK** | 🔴 | Action is prevented. User cannot proceed until condition is resolved. No override available. |
| **WARN** | 🟡 | Action is allowed after explicit acknowledgment. User must check a confirmation box and optionally enter a justification note. Acknowledgment is logged in the audit trail. |
| **INFO** | 🔵 | Informational only. System displays message; no user action required. Logged silently. |

### Rule ID Format
`AVR-[MODULE]-[NNN]` where MODULE is a 3-5 letter module code and NNN is a 3-digit sequence number.

### Columns

| Column | Description |
|--------|-------------|
| Rule ID | Unique identifier |
| Severity | BLOCK / WARN / INFO |
| Trigger Event | The user action or system event that fires this rule |
| Condition | The boolean condition that makes this rule fire (true = rule fires) |
| Message | Exact text displayed to the user (plain language, cites manual) |
| Manual Citation | Chapter and section of National Procurement Manual 2024 |
| Resolution | How the user resolves this or what the system does automatically |

---

## MODULE: TENDER — Tender Creation and Setup

### Section T1 — Basic Tender Information

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-TNDR-001 | 🔴 BLOCK | Save Tender Draft | `tender_title` is blank or fewer than 10 characters | "Tender title is required and must be descriptive (minimum 10 characters). This appears in all procurement documents and committee records." | Ch 05 §5.1 | Enter a meaningful tender title. |
| AVR-TNDR-002 | 🔴 BLOCK | Save Tender Draft | `tce_value` is zero or negative | "Total Cost Estimate (TCE) must be a positive value. The TCE determines the required Procurement Committee type and bidding period thresholds." | Ch 04 §4.2 | Enter a valid TCE amount in LKR. |
| AVR-TNDR-003 | 🔴 BLOCK | Save Tender Draft | `tce_prepared_by` = current user (PM) | "The officer who prepared the TCE cannot be the same officer managing this tender. TCE must be prepared by an independent technical officer." | Ch 04 §4.4 | Assign a different user as TCE Preparer. |
| AVR-TNDR-004 | 🔴 BLOCK | Save Tender Draft | `tce_prepared_by` is blank | "TCE Preparer field is required. Enter the name/ID of the technical officer who prepared the cost estimate." | Ch 04 §4.4 | Assign a TCE Preparer. |
| AVR-TNDR-005 | 🟡 WARN | PC Type selected | Selected `pc_type` has lower authority than required by `tce_value` and `funding_source` | "WARNING: Selected PC type ([selected]) has authority up to LKR [max] for [funding] funded procurements. This TCE value (LKR [tce]) requires at minimum a [required_pc_type]. Proceeding with a lower-authority PC is a compliance violation (Ch 02 §2.1). To proceed, document your justification." | Ch 02 §2.1 | Select the correct PC type or enter a justification note. |
| AVR-TNDR-006 | 🔵 INFO | TCE value entered | `tce_value` entered | "Based on this TCE (LKR [amount], [funding] funded), the recommended PC type is: [HLPC/SHLPC/MPC/DPC/RPC]. Thresholds: RPC ≤50Mn (GOSL) / ≤100Mn (foreign); DPC/PPC ≤400Mn / ≤800Mn; MPC ≤750Mn / ≤1,500Mn; HLPC/SHLPC >750Mn / >1,500Mn." | Ch 02 §2.1 | Informational. Select PC type accordingly. |
| AVR-TNDR-007 | 🔴 BLOCK | Save Tender Draft | `procurement_method` = Shopping AND `tce_value` > shopping limit for selected `pc_type` | "Shopping method cannot be used for this value. Shopping limits: MPC ≤LKR 25Mn (Goods/Services GOSL), DPC/PPC ≤LKR 20Mn, RPC ≤LKR 6Mn. This TCE (LKR [amount]) exceeds the Shopping limit for [pc_type]." | Ch 02 §2.9, Ch 03 §3.1.5 | Change procurement method to NCB or ICB, or reduce scope to within Shopping limit. |
| AVR-TNDR-008 | 🟡 WARN | Procurement Method = Direct Contracting | `procurement_method` = 'Direct Contracting' | "Direct Contracting requires documented justification (single source, standardization, emergency, etc.) and prior CAO/AO approval. This is an exceptional method. Ensure approval is obtained before proceeding. Enter the justification reason." | Ch 03 §3.1.6 | Enter justification. Approval document reference number required. |
| AVR-TNDR-009 | 🔴 BLOCK | Save Tender Draft | `bidding_procedure` = Two-Envelope AND `procurement_method` = Shopping | "Two Envelope bidding procedure cannot be used with Shopping method. Shopping uses sealed quotations, not separate technical/financial envelopes." | Ch 03 §3.3.2 | Change bidding procedure to Single Stage One Envelope for Shopping. |
| AVR-TNDR-010 | 🟡 WARN | Tender type = similar to recent tenders | Count of tenders by same department, same goods category, within 12 months is ≥ 2 and aggregate value would require higher PC type | "POTENTIAL ARTIFICIAL SLICING DETECTED: This PE has [N] similar procurements in the past 12 months in the same category. Aggregate value: LKR [total]. The combined value would require a [higher_pc_type]. Splitting procurements to avoid higher authority is prohibited (Ch 04 §4.3). Document justification if genuinely separate." | Ch 04 §4.3 | Enter justification for separate procurement. System Admin notified. |
| AVR-TNDR-011 | 🔴 BLOCK | Submit Tender for PC Budget Approval | `spec_preparer_id` is blank | "Specification Preparer must be identified before the tender is submitted for PC approval. The officer who prepared technical specifications cannot later serve as a BEC member." | Ch 01 §1.2 | Identify the specification preparer. |
| AVR-TNDR-012 | 🔵 INFO | Repeat Order flag = true | `is_repeat_order` = true | "Repeat Order conditions: additional quantity must be <50% of original contract quantity, must be placed within 6 months of original contract award date, and the original PC must concur. Verify these conditions before proceeding." | Ch 03 §3.2 | Verify conditions. System checks against original contract record. |
| AVR-TNDR-013 | 🔴 BLOCK | Repeat Order: Save | `is_repeat_order` = true AND (`repeat_quantity_pct` > 50 OR `months_since_original_award` > 6) | "Repeat Order conditions not met: [quantity exceeds 50% of original / order is being placed more than 6 months after original award]. Repeat Orders are not permitted under these circumstances (Ch 03 §3.2). Consider a new procurement." | Ch 03 §3.2 | Correct the quantity or use a new procurement process. |

### Section T2 — Bidding Period Validation

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-TNDR-020 | 🔴 BLOCK | Set bid closing date | `procurement_method` = ICB AND `bidding_period_days` < 42 | "ICB bidding period must be at least 42 calendar days from the date of first publication. Currently set to [N] days — insufficient." | Ch 06 §6.3 | Set bid closing date to at least 42 days after publication date. |
| AVR-TNDR-021 | 🔴 BLOCK | Set bid closing date | `procurement_method` = NCB AND `tce_value` > 50_000_000 AND `bidding_period_days` < 28 | "NCB bidding period for contracts above LKR 50 million must be at least 28 calendar days. Currently set to [N] days." | Ch 06 §6.3 | Extend bid closing date to at least 28 days from publication. |
| AVR-TNDR-022 | 🔴 BLOCK | Set bid closing date | `procurement_method` = NCB AND `tce_value` <= 50_000_000 AND `bidding_period_days` < 14 | "NCB bidding period for contracts below LKR 50 million must be at least 14 calendar days. Currently set to [N] days." | Ch 06 §6.3 | Extend bid closing date to at least 14 days from publication. |
| AVR-TNDR-023 | 🔴 BLOCK | Set bid closing date | `procurement_method` = LIB AND `bidding_period_days` < 21 | "LIB bidding period must be at least 21 calendar days. Currently set to [N] days." | Ch 06 §6.3 | Extend bid closing date. |
| AVR-TNDR-024 | 🔴 BLOCK | Set bid closing date | `procurement_method` = Shopping AND `bidding_period_days` < 7 | "Shopping quotation submission period must be at least 7 calendar days. Currently set to [N] days." | Ch 06 §6.3 | Extend quotation closing date to at least 7 days. |
| AVR-TNDR-025 | 🔴 BLOCK | Set bid closing date (Two-Stage) | `bidding_procedure` = 'Two Stage' AND `bidding_period_days` < 28 | "Two-Stage bidding: Stage 1 submission period must be at least 28 calendar days from invitation date." | Ch 03 §3.3.3 | Extend Stage 1 closing date. |
| AVR-TNDR-026 | 🔵 INFO | Bid closing date set | Bid closing date is set | "Bid validity period must be set. Recommended: Goods <LKR 5Mn → 49 days; Goods LKR 5–50Mn → 91 days; Goods LKR 50–750Mn → 105 days; Goods >LKR 750Mn → 119 days. Bid security must cover validity period + 28 days (NCB) or + 56 days (ICB)." | Ch 05 §5.3 | Set bid validity period accordingly. |

### Section T3 — Bid Validity and Security

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-TNDR-030 | 🔴 BLOCK | Publish tender | `bid_security_type` = 'None' AND `tce_value` >= 20_000_000 | "Bid security is required for procurements of LKR 20 million or more. For procurements below LKR 20 million, a Bid Securing Declaration (BSD) may be used instead. Set bid security type." | Ch 05 §5.4 | Add bid security requirement or select BSD if below threshold. |
| AVR-TNDR-031 | 🔴 BLOCK | Publish tender | `bid_security_pct` > 2 | "Bid security must not exceed 2% of the Total Cost Estimate. Currently set to [N]%. This would impose excessive burden on bidders." | Ch 05 §5.4 | Reduce bid security to maximum 2% of TCE. |
| AVR-TNDR-032 | 🔴 BLOCK | Enter bid security validity | `bid_security_validity_days` < (`bid_validity_days` + 28) AND `procurement_method` = NCB | "Bid security validity must extend at least 28 days beyond the bid validity period for NCB procurements. Bid validity: [N] days. Minimum security validity: [N+28] days. Currently set to [N] days." | Ch 05 §5.4 | Extend bid security validity period. |
| AVR-TNDR-033 | 🔴 BLOCK | Enter bid security validity | `bid_security_validity_days` < (`bid_validity_days` + 56) AND `procurement_method` = ICB | "Bid security validity must extend at least 56 days beyond the bid validity period for ICB procurements. Minimum security validity: [bid_validity + 56] days. Currently set to [N] days." | Ch 05 §5.4 | Extend bid security validity period. |

---

## MODULE: CMTE — Committee Assignment

### Section C1 — BEC Assignment

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-CMTE-001 | 🔴 BLOCK | Add member to BEC | Nominated member is not in the BEC Eligibility Pool | "This officer ([name]) is not in the BEC Eligibility Pool. Only pre-approved pool members may serve on a BEC. Contact System Administrator to review pool membership." | Ch 02 §2.5 | Select a different member from the eligibility pool. |
| AVR-CMTE-002 | 🔴 BLOCK | Add member to BEC | Nominated member is a Procurement Department staff member | "Procurement Department staff cannot serve as BEC members. This is a strict role separation rule (Ch 02 §2.5). [Name] is identified as [Procurement Manager/Procurement Assistant]. Remove and select a non-procurement officer." | Ch 02 §2.5 | Select a non-Procurement-Department officer. |
| AVR-CMTE-003 | 🔴 BLOCK | Add member to BEC | Nominated member's active BEC count = 6 | "This officer ([name]) is already serving on 6 active BECs — the maximum allowed (Ch 02 §2.5). Current active BECs: [list]. Cannot assign additional BEC membership until at least one BEC is concluded." | Ch 02 §2.5 | Select a different member, or wait until one of their active BECs concludes. |
| AVR-CMTE-004 | 🔴 BLOCK | Add member to BEC | Nominated member is identified as `spec_preparer_id` for this tender | "This officer ([name]) prepared or contributed to the technical specifications for this tender. An officer who prepared specifications cannot evaluate the same procurement (Ch 01 §1.2). This is a COI exclusion." | Ch 01 §1.2 | Select a different BEC member. |
| AVR-CMTE-005 | 🔴 BLOCK | Add member to BEC | Nominated member is already a PC member for this same tender | "This officer ([name]) is already a member of the Procurement Committee for this tender. An officer cannot serve on both the PC and the BEC for the same procurement." | Ch 02 §2.5 | Select a different BEC member. |
| AVR-CMTE-006 | 🔴 BLOCK | Save BEC Assignment | BEC member count < minimum required for `pc_type` | "BEC size is insufficient for this procurement type. Required: [N] members minimum for [pc_type] ([HLPC/SHLPC → min 5; MPC major → min 5; MPC minor → max 3; DPC/RPC → min 3]). Currently assigned: [N] members." | Ch 02 §2.5 | Add additional BEC members to meet minimum requirement. |
| AVR-CMTE-007 | 🔴 BLOCK | Save BEC Assignment | `bec_chair_id` is blank | "BEC Chair must be designated before assignment is complete. All BECs must have a formally designated Chair." | Ch 02 §2.5 | Designate a BEC Chair from the assigned members. |
| AVR-CMTE-008 | 🔴 BLOCK | Save BEC Assignment | `bec_secretary_id` is blank | "BEC Secretary must be designated before assignment is complete. The Secretary is the primary system operator during evaluation sessions." | Ch 02 §2.5 | Designate a BEC Secretary. |
| AVR-CMTE-009 | 🔵 INFO | BEC Assignment complete | BEC assigned | "BEC assigned successfully. Reminder: All BEC members AND supporting staff must sign COI Declarations (Annexure I and Annexure II of Chapter 01) at the FIRST BEC meeting, before any evaluation work commences." | Ch 01 §1.2 | Ensure COI declarations are completed at first session. |
| AVR-CMTE-010 | 🟡 WARN | Add member to BEC | Nominated member has 5 active BECs | "This officer ([name]) is currently serving on 5 active BECs. Adding them to this BEC will reach the maximum limit of 6 (Ch 02 §2.5). If this BEC runs alongside those, they will be at maximum capacity." | Ch 02 §2.5 | Proceed with awareness, or select another member. |

### Section C2 — BOC Assignment

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-CMTE-020 | 🔴 BLOCK | Proceed to Bid Opening | BOC not yet assigned | "Bid Opening Committee (BOC) must be constituted and assigned before bid opening can begin. BOC must be in place prior to the bid opening event." | Ch 02 §2.7, Ch 06 §6.4 | Assign BOC members before scheduling bid opening. |
| AVR-CMTE-021 | 🔴 BLOCK | Save BOC Assignment | BOC member count < 3 | "BOC must have a minimum of 3 members. Currently [N] assigned." | Ch 02 §2.7 | Add members to BOC. |
| AVR-CMTE-022 | 🟡 WARN | Save BOC Assignment | `boc_chair_independence` = 'Internal' | "BOC Chair ([name]) is identified as Internal to the Procuring Entity. Wherever possible, the BOC Chair should be independent of the PE (Ch 02 §2.7). Document justification if external Chair is not available." | Ch 02 §2.7 | Appoint an external Chair if possible, or enter justification. |

### Section C3 — PC Assignment

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-CMTE-030 | 🔴 BLOCK | Submit for PC Budget Approval | PC not yet constituted | "Procurement Committee has not been constituted for this tender. PC approval is required before bid document preparation can begin." | Ch 02 §2.1 | Constitute and assign the PC before submission. |
| AVR-CMTE-031 | 🟡 WARN | Add PC member | PC member is also a BEC member for any other current tender that is in the same period | "This officer is serving on a BEC for another active tender ([tender ref]). While not prohibited, dual PC/BEC service across different tenders should be reviewed for workload capacity." | Ch 02 §2.4 | Proceed with awareness. |

---

## MODULE: COI — Conflict of Interest Declarations

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-COI-001 | 🔴 BLOCK | Start any evaluation work (first session) | Any BEC member has not signed COI Declaration (Annexure I) | "COI Declarations have not been completed for all BEC members. The following members have not signed: [list]. All members MUST sign Conflict of Interest Declarations at the first meeting, before any evaluation commences (Ch 01 §1.2)." | Ch 01 §1.2, Annexure I | Record COI declarations for all pending members. Evaluation is blocked until all are complete. |
| AVR-COI-002 | 🔴 BLOCK | Start any evaluation work (first session) | BEC Secretary has not signed Staff COI Declaration (Annexure II) | "The BEC Secretary ([name]) has not signed the Staff Officer COI Declaration (Annexure II of Chapter 01). All staff supporting the committee must sign before evaluation commences." | Ch 01 §1.2, Annexure II | Record Secretary's COI declaration. |
| AVR-COI-003 | 🔴 BLOCK | Start any evaluation work (first session) | Any support staff attached to BEC has not signed Annexure II | "The following support staff have not signed COI Declarations: [list]. All officers assisting the BEC (technical advisors, administrative support) must sign Annexure II at the first meeting." | Ch 01 §1.2, Annexure II | Record declarations for all support staff. |
| AVR-COI-004 | 🔵 INFO | COI Declaration recorded | Member marks 'No COI' on declaration | "COI Declaration recorded for [name]. Signed: [date]. Status: No conflict declared." | Ch 01 §1.2 | No action needed. Logged in audit trail. |
| AVR-COI-005 | 🟡 WARN | Member discloses COI mid-evaluation | `coi_disclosed` = true for a member | "COI Disclosure received from [name] for this procurement. This member must immediately recuse themselves from all further deliberations. Their previous votes/decisions in this evaluation will be reviewed. BEC Chair and PM will be notified." | Ch 01 §1.2 | Confirm recusal. Review affected decisions. System Admin notified. |
| AVR-COI-006 | 🔴 BLOCK | Recused member attempts to make decision | `member_status` = 'Recused' AND user attempts to enter cell decision | "You have been recused from this procurement due to a conflict of interest. You cannot make evaluation decisions. Contact the BEC Chair if you believe this is an error." | Ch 01 §1.2 | Recusal can only be reversed by System Admin. |
| AVR-COI-007 | 🔵 INFO | COI form field: relationship type | User enters any value in COI form | "COI Declaration (Annexure I) must capture: (1) name and designation, (2) nature of potential conflict (financial interest, family relationship, previous employment, etc.), (3) declaration of impartiality, (4) date, (5) signature. Retain original signed copy." | Ch 01 Annexure I | Complete all required fields. |

---

## MODULE: DOCS — Document Checklist

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-DOCS-001 | 🔴 BLOCK | Complete Stage 1 (Document Completeness) | Any bidder has NCA status ≠ 'Received and Verified' AND procurement value ≥ LKR 1Mn | "Non-Collusion Affidavit (NCA) is missing or unverified for bidder [name]. NCA is a mandatory document for all bids (Ch 01 §1.3)." | Ch 01 §1.3, Annexure III | Record NCA receipt or mark bidder Non-Responsive for incomplete documents. |
| AVR-DOCS-002 | 🔴 BLOCK | Mark NCA as received | `nca_sworn_before_jp_coo` = false | "The Non-Collusion Affidavit MUST be sworn before a Justice of the Peace or Commissioner of Oaths. A signed letter is NOT sufficient (Ch 01 §1.3). This NCA is marked as not sworn. Bidder [name] cannot be marked compliant for this document." | Ch 01 §1.3 | Confirm the NCA was properly sworn, or flag as deficient (which triggers a clarification or Non-Responsive ruling). |
| AVR-DOCS-003 | 🟡 WARN | Mark NCA as received | `nca_jp_coo_name` or `nca_jp_coo_registration` is blank | "NCA received but JP/Commissioner of Oaths details are incomplete. Record: Name, designation, and registration number of the JP/CoO who witnessed the affidavit." | Ch 01 §1.3 | Enter JP/CoO details. |
| AVR-DOCS-004 | 🔴 BLOCK | Complete Stage 1 | Bidder has submitted bid security amount below required (< TCE × bid_security_pct) | "Bid security for bidder [name] is insufficient. Required: LKR [amount] ([pct]% of TCE LKR [tce]). Submitted: LKR [submitted]. Bidder must be marked Non-Responsive unless this was a bank rounding." | Ch 05 §5.4 | Mark as Non-Responsive or initiate clarification if minor rounding. |
| AVR-DOCS-005 | 🔴 BLOCK | Complete Stage 1 | Bidder's bid security expiry date < (bid_closing_date + bid_validity_days + 28) for NCB | "Bid security for bidder [name] expires [date], which does not cover the required period (bid validity [N] days + 28 days buffer = [required_date]). This is a material deficiency — bidder is Non-Responsive." | Ch 05 §5.4 | Mark bidder Non-Responsive. |
| AVR-DOCS-006 | 🔴 BLOCK | Complete Stage 1 | Bidder's bid security expiry date < (bid_closing_date + bid_validity_days + 56) for ICB | "Bid security for bidder [name] does not cover the required ICB coverage period (bid validity + 56 days). Bidder is Non-Responsive." | Ch 05 §5.4 | Mark bidder Non-Responsive. |
| AVR-DOCS-007 | 🟡 WARN | Enter bid security bank details | `bid_security_bank` is a foreign bank for NCB procurement | "Bid security from bidder [name] is issued by a foreign bank. For NCB procurements, verify that this foreign bank is an acceptable issuing institution under the bid document conditions." | Ch 05 §5.4 | Verify bank acceptability with legal/procurement review. |
| AVR-DOCS-008 | 🔴 BLOCK | Complete Stage 1 | A document marked as 'Compulsory' (C) in the document checklist is missing | "Compulsory document '[doc_name]' is missing for bidder [name]. Missing compulsory documents result in Non-Responsive determination." | Ch 07 §7.3 | Mark bidder Non-Responsive, or initiate clarification if the omission is minor/correctable. |
| AVR-DOCS-009 | 🔵 INFO | Enter document checklist for Two-Envelope | `bidding_procedure` = Two-Envelope | "For Two Envelope bidding: Document checklist is part of Envelope A (technical). Do NOT open Envelope B (financial) for any bidder whose Envelope A is technically non-responsive. Envelope B of non-responsive bidders must be returned unopened after contract award." | Ch 03 §3.3.2 | Follow Two-Envelope opening sequence. |

---

## MODULE: BOC — Bid Opening

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-BOC-001 | 🔴 BLOCK | Open any bid | BOC assignment is incomplete (< 3 members, or Chair/Secretary not designated) | "Bid opening cannot proceed. BOC is not fully constituted (minimum 3 members required, with Chair and Secretary designated)." | Ch 02 §2.7, Ch 06 §6.4 | Complete BOC assignment before opening. |
| AVR-BOC-002 | 🔴 BLOCK | Open any bid | Current time is before bid closing date/time | "Bids cannot be opened before the bid closing date and time ([datetime]). Opening must occur at or after the advertised closing time in the presence of BOC and any attending bidders." | Ch 06 §6.4 | Wait until the bid closing time has passed. |
| AVR-BOC-003 | 🔴 BLOCK | Record bid in BOC register | Bid amount entered as zero for a non-withdrawn bid | "Bid amount cannot be zero for an active bid. If the bidder submitted a bid, record the actual amount. If no bid was submitted, mark as 'No Bid Received'." | Ch 06 §6.4 | Correct the bid amount. |
| AVR-BOC-004 | 🟡 WARN | Record all bids (after all envelopes opened) | Only 1 bid received | "Only one bid has been received. Single-bid procurement: this bid must still be evaluated normally. Lack of competition does not, by itself, disqualify a bid. Evaluation proceeds; BEC must verify the bid is substantially responsive and the price is reasonable against TCE." | Ch 07 §7.1 | Proceed with evaluation. Explain single bid in BER. |
| AVR-BOC-005 | 🔴 BLOCK | Open Envelope B (Two-Envelope) | `bidding_procedure` = Two-Envelope AND Stage 2b (technical evaluation) is not complete | "Financial envelopes (Envelope B) cannot be opened until technical evaluation of Envelope A is complete and substantial responsiveness determined for all bidders. Complete Stage 2b first." | Ch 03 §3.3.2 | Complete Stage 2b technical evaluation. |
| AVR-BOC-006 | 🔴 BLOCK | Open Envelope B for a bidder | Bidder is marked as technically non-responsive | "Bidder [name] was found non-responsive in technical evaluation. Envelope B must NOT be opened. It will be returned to the bidder unopened after contract award." | Ch 03 §3.3.2 | Do not open. Mark as 'To Return'. |
| AVR-BOC-007 | 🟡 WARN | Complete BOC session | Any bid not initialled on all pages (initialling not recorded) | "BOC minutes should confirm that all bid documents were initialled by the BOC Chair on each page. Confirm that initialling was completed before closing the bid opening session." | Ch 06 §6.4 | Confirm initialling done. |
| AVR-BOC-008 | 🔵 INFO | BOC opening complete | All bids recorded | "BOC opening complete. Bid Opening Record must include: all bids received (bidder name, bid amount, bid security details, any modifications/withdrawals). This record is auto-generated and must be signed by all BOC members. Print for physical signatures." | Ch 06 §6.4 | Print and sign BOC opening record. |
| AVR-BOC-009 | 🔴 BLOCK | Accept late bid | Bid received after bid closing time | "Late bids must not be accepted or opened. This bid from [bidder name] was received at [time], which is [N] minutes/hours after the closing time of [closing_time]. Mark as 'Late — Rejected'. Return to bidder unopened." | Ch 06 §6.4 | Reject and return unopened. |
| AVR-BOC-010 | 🔵 INFO | Record bid withdrawal | Bidder submitted a withdrawal request | "Bid withdrawal recorded for [bidder name]. Withdrawal requests received before bid closing time are valid. The withdrawal notification should be opened and read aloud at bid opening, but the original bid envelope is returned unopened." | Ch 06 §6.4 | Record withdrawal. Return original bid. |

---

## MODULE: EVAL — Evaluation Grid

### Section E1 — Stage Gates

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-EVAL-001 | 🔴 BLOCK | Enter any evaluation cell | COI declarations not complete for all members and staff | "Evaluation cannot commence. COI declarations are pending for: [list]. All BEC members and staff must sign COI declarations at the first meeting before any evaluation work." | Ch 01 §1.2 | Complete all COI declarations. |
| AVR-EVAL-002 | 🔴 BLOCK | Begin Stage 2a (Commercial) | Stage 1 (Document Completeness) not marked complete | "Stage 2a (Commercial Terms) cannot begin until Stage 1 (Document Completeness) is marked complete by the BEC Secretary. Complete Stage 1 for all bidders first." | Ch 07 §7.3 | Complete Stage 1. |
| AVR-EVAL-003 | 🔴 BLOCK | Begin Stage 2b (Technical) | Stage 2a (Commercial) not complete | "Stage 2b (Technical Requirements) cannot begin until Stage 2a (Commercial Terms) is complete. Complete Stage 2a evaluation for all bidders." | Ch 07 §7.4 | Complete Stage 2a. |
| AVR-EVAL-004 | 🔴 BLOCK | Begin Stage 3 (Price Adjustment) | Stage 2b (Technical) not complete | "Stage 3 (Price Adjustment) cannot begin until Stage 2b (Technical) is complete for all bidders and items." | Ch 07 §7.5 | Complete Stage 2b. |
| AVR-EVAL-005 | 🔴 BLOCK | Begin Stage 4 (Post-Qualification) | Stage 3 (Price Adjustment) not complete | "Stage 4 (Post-Qualification) cannot begin until Stage 3 (Price Adjustment / Evaluated Price calculation) is complete." | Ch 07 §7.6 | Complete Stage 3. |
| AVR-EVAL-006 | 🔴 BLOCK | Mark item as complete | Any cell in the item still has status = 'Pending' | "Item [item_name] cannot be marked complete. The following cells still have no decision: [list of Bidder × Requirement]. All cells must be decided (Accept or Reject) before an item can be completed." | Ch 07 §7.4 | Decide all pending cells. |
| AVR-EVAL-007 | 🔴 BLOCK | Mark item as complete | Any open clarification thread is attached to this item | "Item [item_name] has open clarification threads: [list]. All clarification threads must be closed (Satisfied or Not Satisfied) before an item can be completed." | Ch 07 §7.4 | Close all open clarification threads. |

### Section E2 — Cell-Level Decisions

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-EVAL-010 | 🔴 BLOCK | Enter decision in cell | Active clarification thread exists for this exact cell | "This cell is blocked: an open clarification thread is pending for [Bidder] × [Requirement]. Resolve the clarification thread first. Cell decisions are blocked while threads are active." | Ch 07 §7.4 | Close the clarification thread first. |
| AVR-EVAL-011 | 🔴 BLOCK | Enter 'Reject' decision for a 'C' (Compulsory) requirement | `requirement_type` = 'C' AND decision = 'Reject' AND deviation_type not yet selected | "This is a Compulsory (C) requirement. Rejecting it automatically creates a Major Deviation. Confirm: once you submit this rejection, bidder [name] will be marked Non-Responsive for item [item_name]. This cannot be reversed without BEC Chair authorisation." | Ch 07 §7.4 | Confirm rejection. System auto-sets deviation_type = Major, bidder_status = Non-Responsive for this item. |
| AVR-EVAL-012 | 🔴 BLOCK | Enter 'Reject' for a non-compulsory requirement | `requirement_type` = blank/NC AND decision = 'Reject' AND `deviation_type` is blank | "This is a Non-Compulsory requirement. You must classify the deviation type before saving: Minor (quantifiable, monetary loading applied) or Debatable (BEC Chair resolves with documented reasoning)." | Ch 07 §7.4 | Select deviation type: Minor or Debatable. |
| AVR-EVAL-013 | 🔴 BLOCK | Save Minor Deviation | `deviation_type` = 'Minor' AND `monetary_loading` is blank or zero | "Minor deviations require a monetary loading value to be calculated and entered. This loading is added to the Evaluated Price for comparison purposes. Enter the loading amount in LKR." | Ch 07 §7.5 | Enter monetary loading amount. |
| AVR-EVAL-014 | 🔴 BLOCK | Save Debatable Deviation | `deviation_type` = 'Debatable' AND `chair_resolution` is blank | "Debatable deviations require the BEC Chair to enter a documented resolution decision. The Chair must record their reasoning before this decision can be saved." | Ch 07 §7.4 | BEC Chair enters resolution reasoning. |
| AVR-EVAL-015 | 🔵 INFO | AI auto-populates Offered Value | AI field populated | "Offered Value auto-populated by AI from document [doc_name], page [N], section [section]. This is a draft — human verification is required. A second BEC member must verify this extraction (maker-checker rule)." | Ch 07 §7.4 | Verify and confirm the AI extraction. |
| AVR-EVAL-016 | 🔴 BLOCK | Accept AI-extracted evidence | `maker_checker_verified` = false | "AI extraction must be verified by a second BEC member before it can be marked as confirmed evidence. Current verifier: [name]. Awaiting verification from a different member." | Ch 07 §7.4 | Second member reviews and confirms or corrects the AI extraction. |
| AVR-EVAL-017 | 🔵 INFO | AI generates compliance analysis | Any requirement cell | "AI compliance highlight: The following text from bidder [name]'s document appears relevant to this requirement. This is a reference only — AI does NOT suggest Accept or Reject. BEC must make all decisions independently." | Ch 07 §7.4 | Use as reference only. |
| AVR-EVAL-018 | 🟡 WARN | BEC Member (non-Chair, non-Secretary) enters decision | Session is active AND member does not have 'Temporary Write' grant | "You do not have write access in this session. Only the BEC Secretary (and members granted temporary write access by the Secretary) can enter decisions. Request temporary write access from the Secretary." | SYSTEM_SPEC §6.3 (Session-based access) | Secretary grants temporary write access if appropriate. |

### Section E3 — Mandatory Information Fields

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-EVAL-020 | 🔴 BLOCK | Complete Stage 2b for a bidder | `make` is blank for any item submitted by bidder | "Make (manufacturer name) is missing for bidder [name], item [item_name]. Make, Model, and Country of Origin are mandatory information fields. A clarification must be sent to request this information." | Ch 07 §7.4 | Send clarification requesting Make details. Item completion is blocked. |
| AVR-EVAL-021 | 🔴 BLOCK | Complete Stage 2b for a bidder | `model` is blank for any item submitted by bidder | "Model is missing for bidder [name], item [item_name]. A clarification requesting Model information must be sent." | Ch 07 §7.4 | Send clarification. |
| AVR-EVAL-022 | 🔴 BLOCK | Complete Stage 2b for a bidder | `country_of_origin` is blank for any item submitted by bidder | "Country of Origin is missing for bidder [name], item [item_name]. A clarification must be sent." | Ch 07 §7.4 | Send clarification. |
| AVR-EVAL-023 | 🔴 BLOCK | Close clarification for missing mandatory info | Clarification closure = 'Not Satisfied' AND the clarification was for Make/Model/Country of Origin | "Mandatory information clarification closed as Not Satisfied for bidder [name]. Bidder is Non-Responsive for item [item_name] due to failure to provide mandatory information (Make/Model/Country of Origin)." | Ch 07 §7.4 | System auto-sets bidder_item_status = Non-Responsive. |

---

## MODULE: PRICE — Price Evaluation (Stage 3 — 14-Step Adjustment)

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-PRICE-001 | 🔴 BLOCK | Begin price evaluation | Any substantially responsive bidder's price schedule is not fully entered | "Price schedule for bidder [name] is incomplete. All unit prices, quantities, transport, and VAT must be entered before price adjustment can proceed." | Ch 07 §7.5 | Complete price schedule entry. |
| AVR-PRICE-002 | 🔴 BLOCK | Step 1: Exclude VAT | VAT amount not separated in price entry | "Step 1 of price adjustment: VAT must be excluded for comparison. Enter VAT amount separately for each bidder. If bidder did not state VAT, flag for clarification." | Ch 07 §7.5, Step 1 | Separate VAT in price schedule. |
| AVR-PRICE-003 | 🟡 WARN | Step 2: Arithmetic correction | System-calculated total ≠ bidder-stated total (difference > 0.5%) | "Arithmetic error detected for bidder [name]. Bidder-stated total: LKR [stated]. Recalculated total: LKR [calculated]. Difference: LKR [diff] ([pct]%). Corrected figure will be used for evaluation. Bidder will be notified of the correction." | Ch 07 §7.5, Step 2 | Confirm system correction. Notify bidder. |
| AVR-PRICE-004 | 🔵 INFO | Step 2: Arithmetic correction applied | Correction applied | "Arithmetic correction applied for [bidder]: LKR [original] → LKR [corrected]. If the correction is material and the bidder does not accept, the bid may be withdrawn but bid security is forfeited." | Ch 07 §7.5, Step 2 | Note correction in evaluation record. |
| AVR-PRICE-005 | 🔵 INFO | Step 3: Unconditional discounts | Discount offered in bid letter | "Unconditional discount of [amount]% / LKR [amount] offered by bidder [name]. This discount will be applied to the evaluated price in Step 3." | Ch 07 §7.5, Step 3 | Confirm discount application. |
| AVR-PRICE-006 | 🟡 WARN | Step 3: Conditional discounts | Discount offered conditionally (e.g., "if also awarded lot X") | "Bidder [name] offered a conditional discount ([condition]). Conditional discounts can only be applied if all conditions are met in the award scenario being evaluated. Verify applicability before applying." | Ch 07 §7.5, Step 3 | Determine if condition is met. Apply only if all conditions satisfied. |
| AVR-PRICE-007 | 🔴 BLOCK | Step 4: Omissions | Requirement item in spec has no price from bidder | "Bidder [name] has not priced item/requirement: [item]. An omission in the price schedule is a material deviation unless it was designated as 'Optional' in the bid documents. Determine: (a) Optional: ignore; (b) Mandatory: load the estimated cost into evaluated price, OR mark bidder Non-Responsive." | Ch 07 §7.5, Step 4 | Decide treatment for omission. |
| AVR-PRICE-008 | 🔵 INFO | Step 7: Domestic preference | `procurement_method` = ICB AND domestic bidder present | "Step 7: Domestic preference applies for ICB procurements. Eligible domestic bidders receive a 15% preference adjustment on evaluated price for comparison only (not for payment). Verify bidder domestic eligibility before applying." | Ch 07 §7.5, Step 7 | Confirm domestic bidder eligibility. Apply adjustment. |
| AVR-PRICE-009 | 🟡 WARN | Step 9: Life-cycle costing | Item is energy-consuming equipment (flagged as `lcc_required` = true) | "This item is flagged for Life Cycle Cost (LCC) evaluation. Operating costs (energy consumption, maintenance) must be included in the evaluated price per the LCC methodology specified in the bid documents. Enter LCC components for each bidder." | Ch 07 §7.5, Step 9 | Enter LCC components and confirm LCC calculation. |
| AVR-PRICE-010 | 🔴 BLOCK | Complete Stage 3 | `evaluated_price` is blank for any substantially responsive bidder | "Evaluated Price (Step 14 output) must be calculated for all substantially responsive bidders before Stage 3 can be marked complete. Missing for: [list of bidders]." | Ch 07 §7.5 | Complete all 14 adjustment steps for all responsive bidders. |
| AVR-PRICE-011 | 🔵 INFO | Stage 3 complete | Evaluated prices calculated | "Evaluated Price calculation complete. IMPORTANT: Evaluated Price is for comparison purposes only. Contract price (if awarded) will be the bidder's original bid price adjusted per contract terms — NOT the evaluated price. Do not use evaluated price in correspondence with bidders." | Ch 07 §7.5 | Note the distinction between evaluated and contract price. |
| AVR-PRICE-012 | 🟡 WARN | Reviewed price vs TCE | Lowest evaluated price > TCE × 1.15 (more than 15% above estimate) | "All evaluated prices exceed the Total Cost Estimate by more than 15%. Lowest evaluated: LKR [amount] vs TCE: LKR [tce] ([pct]% above). BEC must investigate reasons. Options: proceed and recommend if justified; negotiate (if permitted); or recommend re-invitation." | Ch 07 §7.5 | Investigate and document BEC response to price-TCE gap. |

---

## MODULE: PQ — Post-Qualification (Stage 4)

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-PQ-001 | 🔴 BLOCK | Begin Stage 4 | Lowest evaluated bidder is not identified | "Post-qualification applies to the lowest evaluated bidder only. Stage 3 must be complete and the lowest evaluated bidder identified before Stage 4 begins." | Ch 07 §7.6 | Complete Stage 3 and identify lowest evaluated bidder. |
| AVR-PQ-002 | 🔵 INFO | Stage 4 begins | Stage 4 starts | "Post-Qualification initiated for lowest evaluated bidder: [bidder name] (Evaluated Price: LKR [amount]). Verify: (1) Financial capacity, (2) Technical experience in similar contracts, (3) Equipment and manpower availability, (4) Manufacturer authorisation (if applicable), (5) CIDA registration (if Works contract), (6) No debarment." | Ch 07 §7.6 | Complete all PQ checks. |
| AVR-PQ-003 | 🔴 BLOCK | Complete PQ | `pq_financial_capacity` not assessed | "Financial capacity of lowest evaluated bidder must be assessed and recorded in Stage 4. Upload supporting evidence (audited accounts, bank statements, line of credit)." | Ch 07 §7.6 | Enter financial capacity assessment. |
| AVR-PQ-004 | 🔴 BLOCK | Complete PQ | `pq_technical_experience` not assessed | "Technical experience of lowest evaluated bidder must be assessed. Verify similar contract experience (type, value, timeframe) as required by bid document qualification criteria." | Ch 07 §7.6 | Enter technical experience assessment. |
| AVR-PQ-005 | 🔴 BLOCK | Complete PQ | `pq_manufacturer_auth` not assessed AND item requires manufacturer authorisation | "Manufacturer authorisation check required for [bidder name] on [item]. Bidder must provide valid manufacturer authorisation for the offered make/model." | Ch 07 §7.6 | Enter manufacturer authorisation status. |
| AVR-PQ-006 | 🔴 BLOCK | Complete PQ for Works contract | `pq_cida_registration` not verified AND `tender_type` includes Works | "CIDA registration must be verified for the lowest evaluated bidder for Works contracts. Required CIDA grade for contract value LKR [amount]: [grade]. Verify CIDA certificate validity." | Ch 11 §11.2 | Enter CIDA grade and certificate details. |
| AVR-PQ-007 | 🔴 BLOCK | Mark bidder as post-qualified | `npc_debarment_check` = not completed | "NPC Debarment Register check not recorded for [bidder name]. The national debarment register must be checked before any contract award recommendation. Record the outcome (clear / debarred)." | Ch 10 §10.1 | Perform and record NPC debarment check. |
| AVR-PQ-008 | 🔴 BLOCK | Recommend award | Bidder is on debarment register | "BLOCKED: Bidder [name] is on the NPC Debarment Register. Debarred suppliers cannot be awarded contracts. Move to the next lowest evaluated bidder and conduct post-qualification." | Ch 10 §10.1 | Move to next bidder. Document reason. |
| AVR-PQ-009 | 🟡 WARN | PQ result = Fail | Lowest evaluated bidder fails PQ | "Lowest evaluated bidder [name] has failed post-qualification. BEC must now conduct post-qualification on the next lowest evaluated bidder ([next_bidder], Evaluated Price LKR [amount]). Document reasons for first bidder's failure in BER." | Ch 07 §7.6 | Move to next bidder. Record failure reasons. |
| AVR-PQ-010 | 🔵 INFO | All bidders fail PQ | No bidder passes post-qualification | "All substantially responsive bidders have failed post-qualification. BEC must recommend to PC: re-invite bids. PC has authority to: (a) accept a bid, (b) accept part of bids, (c) reject all bids, (d) re-invite. BEC cannot recommend award in this scenario." | Ch 07 §7.6 | Refer to PC with recommendation to re-invite. |

---

## MODULE: CLAR — Clarifications

### Section CL1 — Bidder Clarifications

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-CLAR-001 | 🔴 BLOCK | Send clarification to bidder | PM has not reviewed/approved the clarification draft | "Clarification to [bidder name] has not been approved by the Procurement Manager. Workflow: Secretary drafts → BEC Chair approves → PM sends. PM approval is required before dispatch." | Ch 07 §7.4 | BEC Chair approves; PM reviews and sends. |
| AVR-CLAR-002 | 🔴 BLOCK | Close clarification thread | `closure_decision` is blank (neither Satisfied nor Not Satisfied) | "Clarification thread cannot be closed without a Satisfied / Not Satisfied decision by the BEC. Select closure status before closing." | Ch 07 §7.4 | BEC Chair selects closure decision. |
| AVR-CLAR-003 | 🔵 INFO | Close thread as Not Satisfied (mandatory info) | Thread was for missing Make/Model/Country of Origin and closed as Not Satisfied | "Clarification closed as Not Satisfied for mandatory information. System will automatically mark bidder [name] as Non-Responsive for item [item] (missing mandatory information = Non-Responsive). This action is irreversible without BEC Chair override documented in audit log." | Ch 07 §7.4 | Confirm Non-Responsive ruling. |
| AVR-CLAR-004 | 🟡 WARN | BEC attempts to request new information from bidder | New clarification type = 'Request for New Information' (not clarification of existing) | "Clarifications must seek to clarify ambiguities in submitted bids — they cannot be used to request new information that was not provided in the original bid. Confirm this clarification is a genuine request for clarification, not a request for supplementary submission." | Ch 07 §7.4 | Confirm that the request is for clarification only. |
| AVR-CLAR-005 | 🔴 BLOCK | Enter bidder's response | Response received after clarification response deadline | "Response from bidder [name] received on [date], which is after the response deadline of [deadline]. Late responses may be disregarded by the BEC. BEC Chair must decide: accept (document justification) or disregard." | Ch 07 §7.4 | BEC Chair decision required. |

### Section CL2 — Internal Technical Consultation

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-CLAR-010 | 🔵 INFO | Create Internal Technical Consultation | Any user | "Internal Technical Consultation is advisory only. The Requestor's response provides technical guidance but the BEC makes the final evaluation decision. Requestor advice cannot override BEC judgment." | Ch 07 §7.4 | Note advisory-only nature. |
| AVR-CLAR-011 | 🟡 WARN | Cell decision disagrees with Requestor recommendation | Cell decision entered after Requestor thread closed as Satisfied, but decision differs from Requestor's recommendation | "BEC decision ([Accept/Reject]) differs from the Internal Technical Consultant's recommendation ([recommendation]). This is permitted — BEC decides independently. Enter the BEC's documented reasoning for the departure from technical advice." | Ch 07 §7.4 | Enter BEC reasoning. Logged in audit trail. |

---

## MODULE: RPT — Report Generation

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-RPT-001 | 🔴 BLOCK | Generate BER | Any item is not marked complete | "Bid Evaluation Report cannot be generated. The following items are not yet complete: [list]. All items must be fully evaluated (all cells decided, all threads closed) before report generation." | Ch 07 §7.7 | Complete all pending items. |
| AVR-RPT-002 | 🔴 BLOCK | Generate BER | Stage 4 (Post-Qualification) is not marked complete | "BER cannot be generated until Post-Qualification (Stage 4) is complete and a recommended bidder is identified." | Ch 07 §7.7 | Complete Stage 4. |
| AVR-RPT-003 | 🟡 WARN | Generate BER | Recommendation is for a bidder who is NOT the lowest evaluated bidder | "The recommended bidder ([name], LKR [amount]) is NOT the lowest evaluated bidder. Lowest evaluated bidder is [name], LKR [amount]. BER MUST explain why the lowest evaluated bidder is not being recommended. Unexplained departure is a compliance violation." | Ch 07 §7.7 | Enter detailed explanation for non-lowest recommendation. |
| AVR-RPT-004 | 🔴 BLOCK | Finalize BER section (narratives) | AI-drafted narrative section has not been reviewed/edited by a BEC member | "This BER section was AI-drafted and has not been reviewed by a BEC member. At least one BEC member must confirm review of each AI-drafted section before it can be included in the final report." | Ch 07 §7.7 | BEC member reviews and confirms each section. |
| AVR-RPT-005 | 🔵 INFO | BER narrative: amounts in words | Any monetary amount entered in report | "Monetary amounts in the BER must be expressed as: '[amount in words] (LKR [numeric])'. Example: 'Two Million Five Hundred Thousand Rupees (LKR 2,500,000)'. Check all tables and narratives for consistent format." | Ch 07 §7.7 | Format all amounts correctly. |
| AVR-RPT-006 | 🟡 WARN | Export BER | Signature table is pre-filled | "The BER signature table should be left blank for physical signing. Do not pre-fill signatures. The exported document will have blank signature lines." | Ch 07 §7.7 | Confirm blank signature table. |
| AVR-RPT-007 | 🔴 BLOCK | BEC certify BER | Number of BEC members who have reviewed and confirmed BER < quorum | "BER cannot be certified unless a quorum of BEC members has reviewed and confirmed the report. Required: [quorum_count]. Confirmed: [confirmed_count]." | Ch 07 §7.7 | Ensure all BEC members review BER. |
| AVR-RPT-008 | 🔵 INFO | Auto-generate BER Annexures | BER export triggered | "Auto-generating 5 standard annexures: (1) Pre-bid Meeting Records, (2) Clarification Records, (3) Bid Opening Records, (4) Price Schedules (per bidder), (5) Document Checklists (per bidder). Review auto-generated content before export." | Ch 07 §7.7 | Review all 5 annexures. |
| AVR-RPT-009 | 🔴 BLOCK | Include deviation table | `deviation_type` = 'Major' for any non-compulsory requirement | "A non-compulsory requirement has been classified as 'Major' deviation. Only Compulsory requirements automatically generate Major deviations. Non-Compulsory deviations must be Minor or Debatable. Reclassify." | Ch 07 §7.4 | Reclassify deviation correctly. |

---

## MODULE: AWARD — Award Process and Standstill

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-AWRD-001 | 🔴 BLOCK | Proceed to Award | PC Final Approval not recorded | "Contract cannot be awarded without PC Final Approval. Record the PC's decision and meeting reference before proceeding." | Ch 08 §8.1 | Record PC Final Approval. |
| AVR-AWRD-002 | 🔴 BLOCK | Proceed to Award | BEC Report not certified by BEC Chair | "BER has not been certified by BEC Chair. BEC certification is required before PM Review and PC Final Approval." | Ch 07 §7.7 | BEC Chair certifies the BER. |
| AVR-AWRD-003 | 🔴 BLOCK | Issue Letter of Award (LoA) | Standstill period has not elapsed (< 10 working days since Notification of Intention to Award) | "BLOCKED: Standstill period has not elapsed. Notification of Intention to Award was sent on [date]. Minimum standstill period: 10 working days. Standstill expires on: [date]. You cannot issue the Letter of Award before [date]." | Ch 08 §8.2 | Wait until standstill period expires. |
| AVR-AWRD-004 | 🔴 BLOCK | Issue Letter of Award | Any open appeal has not been resolved | "An appeal has been lodged (received [date], appeal body: [body]). The appeal must be resolved before the LoA can be issued." | Ch 08 §8.3 | Resolve the appeal. Record outcome. |
| AVR-AWRD-005 | 🔵 INFO | Send Notification of Intention to Award | LoA target is set | "Notification of Intention to Award sent to [bidder name] on [date]. Standstill period: 10 working days (until [calculated_date]). All unsuccessful bidders must also be notified simultaneously. System will block LoA issuance until [date]." | Ch 08 §8.2 | Notify all unsuccessful bidders as well. |
| AVR-AWRD-006 | 🟡 WARN | Send Notification of Intention to Award | Number of substantially responsive bidders = 1 | "Only one substantially responsive bidder exists. Standstill period still applies. Notify the single bidder of the intention to award. Even single-bid situations require the standstill before formal award." | Ch 08 §8.2 | Proceed normally with standstill. |
| AVR-AWRD-007 | 🔴 BLOCK | Issue LoA | `recommended_bidder_id` has debarment flag | "BLOCKED: The recommended bidder has been flagged as debarred since the BEC report was prepared. A debarred entity cannot receive a contract award. Refer back to BEC for re-evaluation of the next eligible bidder." | Ch 10 §10.1 | Refer to BEC. Do not award. |
| AVR-AWRD-008 | 🟡 WARN | Issue LoA | Bid validity period will expire within 14 days | "Bid validity of [bidder name] expires on [date] — within 14 days. Request a bid validity extension before issuing the LoA to ensure the bid remains valid through contract signing." | Ch 05 §5.3 | Request bid validity extension. |
| AVR-AWRD-009 | 🔴 BLOCK | Issue LoA | Contract award value exceeds TCE by more than 15% without documented PC approval | "LoA value (LKR [amount]) exceeds TCE (LKR [tce]) by [pct]%. This requires documented PC concurrence before award. PC approval for above-estimate award not yet recorded." | Ch 08 §8.1 | Record PC concurrence. |
| AVR-AWRD-010 | 🔵 INFO | Debriefing requested by unsuccessful bidder | Unsuccessful bidder requests debriefing | "Debriefing requested by [bidder name] on [date]. PEs are encouraged to provide debriefing to unsuccessful bidders. Schedule debriefing within 10 working days of request. Debriefings are informational — do not reveal other bidders' confidential information." | Ch 08 §8.4 | Schedule and record debriefing session. |

---

## MODULE: POST — Post-Award Tracking

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-POST-001 | 🔴 BLOCK | Record contract signing | `performance_security_received` = false AND contract value ≥ performance_security_threshold | "Performance Security must be received from [contractor] before contract signing can be recorded. Required: 10% of contract value = LKR [amount]. Document is not yet recorded." | Ch 08 §8.5 | Record receipt of Performance Security. |
| AVR-POST-002 | 🟡 WARN | Record delivery milestone | Delivery date is past due (today > `planned_delivery_date`) | "Delivery milestone overdue for [item]: planned [date], today is [today]. [N] days overdue. Consider issuing a reminder or formal notice to supplier." | Ch 09 §9.1 | Issue delivery reminder. Document delay. |
| AVR-POST-003 | 🟡 WARN | Record variation request | Variation amount ≤ 7.5% of contract value | "Variation within Accounting Officer (AO) authority (≤7.5% of contract value). AO may approve directly. VRC review may not be required but document the variation basis thoroughly." | Ch 09 §9.2 | Record variation. Obtain AO approval. |
| AVR-POST-004 | 🟡 WARN | Record variation request | 7.5% < variation amount ≤ 10% of contract value | "Variation within Chief Accounting Officer (CAO) authority (>7.5% to ≤10% of contract value). CAO approval required. VRC review recommended." | Ch 09 §9.2 | Obtain CAO approval. Consider VRC review. |
| AVR-POST-005 | 🔴 BLOCK | Approve variation | Variation amount > 10% of contract value AND Cabinet approval not recorded | "Variation exceeds 10% of contract value (Cabinet approval threshold). This variation (LKR [amount] = [pct]% of contract) cannot be approved without Cabinet of Ministers approval. Record Cabinet approval reference before proceeding." | Ch 09 §9.2 | Obtain and record Cabinet approval. |
| AVR-POST-006 | 🔵 INFO | VRC constituted | VRC = Variation Review Committee | "VRC constituted for contract [ref]. VRC has authority to review and recommend variations. VRC recommendation is advisory to the approving authority (AO/CAO/Cabinet based on variation size). VRC should include at least one independent technical member." | Ch 09 §9.2 | Proceed with VRC review. |
| AVR-POST-007 | 🔴 BLOCK | Archive tender | Outstanding post-award milestones are incomplete | "Tender cannot be archived while post-award milestones are incomplete. Outstanding: [list of incomplete milestones]. Complete all delivery, installation, and commissioning records before archiving." | SYSTEM_SPEC §12 | Complete all milestones. |

---

## MODULE: ADMIN — User Management and System Administration

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-ADMN-001 | 🔴 BLOCK | Assign Procurement Manager role to a user | User is already in the BEC Eligibility Pool | "This user ([name]) is in the BEC Eligibility Pool. A Procurement Department officer cannot be in the BEC Eligibility Pool. Assign the PM role OR keep them in the BEC pool — not both." | Ch 02 §2.5 | Remove from BEC pool first, or do not assign PM role. |
| AVR-ADMN-002 | 🔴 BLOCK | Add user to BEC Eligibility Pool | User has an active Procurement Manager or Procurement Assistant role | "Users with Procurement Department roles cannot be in the BEC Eligibility Pool. Role separation is mandatory (Ch 02 §2.5). Remove procurement role before adding to BEC pool." | Ch 02 §2.5 | Remove procurement role first. |
| AVR-ADMN-003 | 🟡 WARN | Flag user for BEC pool removal | User has 3 consecutive unexcused BEC absences | "ELIGIBILITY POOL REMOVAL RECOMMENDED: [name] has 3 consecutive unexcused absences across BEC meetings (Ch 02 §2.6). Manual review required. Confirm removal from BEC Eligibility Pool?" | Ch 02 §2.6 | System Admin confirms removal. Audit logged. |
| AVR-ADMN-004 | 🔵 INFO | Record BEC member absence | Absence recorded | "Absence recorded for [name] in [procurement]. Type: [Excused/Unexcused]. Running totals: [N] unexcused in this procurement; [N] consecutive across all procurements. [If 2 unexcused in this procurement]: CAO notification form generated — PM must send." | Ch 02 §2.6 | Send CAO notification if required. |
| AVR-ADMN-005 | 🔴 BLOCK | Assign System Admin role | System Admin is also assigned any tender-related role (PM, BEC, BOC, PC) | "System Administrator cannot participate in any tender activities. The Admin role is strictly separated from all procurement functions. Remove tender role assignments before assigning System Admin." | SYSTEM_SPEC §3 | Remove all tender roles. |
| AVR-ADMN-006 | 🔵 INFO | TCE accessed | Any user accesses TCE value | "TCE access logged: [user] accessed TCE for tender [ref] at [datetime]. TCE is confidential and must not be disclosed to bidders before bid closing. BEC members should not see TCE during technical evaluation unless the BEC Chair enables financial visibility." | Ch 04 §4.4 | Audit log recorded. No action required. |

---

## MODULE: SESS — Session Controls

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-SESS-001 | 🔴 BLOCK | Start evaluation session | Quorum not met (< majority of BEC members present in attendance) | "Session cannot start without quorum. BEC has [total_members] members; quorum requires [quorum_count] present. Currently marked present: [present_count]. Mark additional members as present before starting." | Ch 02 §2.6 | Mark additional members as present. |
| AVR-SESS-002 | 🔵 INFO | Session started | Session begins | "Session [N] started at [time]. Present: [list]. Quorum: Met. Timer started. Any BEC member who has not yet signed COI declarations must do so before participating in evaluation." | Ch 02 §2.6 | Session underway. |
| AVR-SESS-003 | 🟡 WARN | Price visibility toggle | BEC Chair enables price visibility | "Financial proposal prices are now visible to all session participants. For Two-Envelope bids, ensure that Envelope B has been formally opened at a separate BOC event before prices are revealed. Confirm this is appropriate at the current evaluation stage." | Ch 07 §7.5 | Confirm price visibility is appropriate at this stage. |
| AVR-SESS-004 | 🔵 INFO | End session | Session ends | "Session ended at [time]. Duration: [N] hours [N] minutes. Items evaluated: [N]. Session minutes auto-generated — BEC Chair must review, edit if needed, and confirm before minutes are locked in the audit trail." | Ch 02 Annexure II | Review and confirm session minutes. |
| AVR-SESS-005 | 🔴 BLOCK | Non-Secretary member enters decision | Member does not have `temp_write_grant` AND is not the Secretary | "Write access required. Only the BEC Secretary and members with temporary write access can enter evaluation decisions during a session. Request temporary write access from the Secretary." | SYSTEM_SPEC §6.3 | Secretary grants temporary write access. |

---

## MODULE: AI — AI-Specific Validation Rules

| Rule ID | Severity | Trigger Event | Condition | Message | Manual Citation | Resolution |
|---------|----------|---------------|-----------|---------|----------------|------------|
| AVR-AI-001 | 🔴 BLOCK | AI Accept/Reject suggestion attempt | AI model attempts to output an Accept/Reject decision | "AI suggestion BLOCKED. AI is not permitted to suggest Accept or Reject decisions for evaluation cells. AI may highlight relevant text and auto-populate evidence fields, but all compliance decisions must be made by the BEC independently." | Ch 07 §7.4, SYSTEM_SPEC §14 | AI output is suppressed. Human decision required. |
| AVR-AI-002 | 🔵 INFO | AI highlights text | AI marks text in bid document as relevant to requirement | "AI Relevance Highlight: The following text appears relevant to requirement [req_name]. This is an AI-generated reference only. BEC determines Accept/Reject independently. Highlighted text: '[excerpt]' — Location: [doc_name], Page [N]." | Ch 07 §7.4 | Use as reference. No decision implication. |
| AVR-AI-003 | 🔴 BLOCK | AI-populated field used in decision | `maker_verified` = false AND `checker_verified` = false for AI-extracted field | "AI-extracted evidence has not been verified. Both maker-check and checker-verification required before AI extraction can be used as confirmed evidence. Current status: Unverified." | Ch 07 §7.4 | Maker verifies; a different member (checker) verifies. |
| AVR-AI-004 | 🟡 WARN | AI-populated field: low confidence | AI confidence score < 0.7 for an extraction | "AI confidence for this extraction is low ([score]). The extracted value may be incorrect. Manual verification is especially important for low-confidence extractions. Human must carefully review the source document before confirming." | Ch 07 §7.4 | Carefully verify before confirming. |
| AVR-AI-005 | 🔴 BLOCK | AI draft clarification: fill custom content | AI attempts to draft the substantive content of a clarification | "AI cannot draft the substantive content of clarification letters. AI may fill template placeholders (bidder name, requirement reference, date) but the technical inquiry text must be written by the BEC Secretary or a BEC member." | Ch 07 §7.4 | BEC Secretary writes clarification content. |
| AVR-AI-006 | 🔵 INFO | AI draft narrative (BER) | AI produces BER narrative section draft | "AI-generated BER narrative draft for Section [N]. This is a draft only — a BEC member must review, edit, and confirm before this section is included in the final report. AI-generated narratives cannot be published without human review." | Ch 07 §7.7 | BEC member reviews and edits. |

---

## SUMMARY: RULE COUNT BY MODULE AND SEVERITY

| Module | BLOCK (🔴) | WARN (🟡) | INFO (🔵) | Total |
|--------|-----------|----------|----------|-------|
| TNDR — Tender Creation | 10 | 3 | 2 | 15 |
| TNDR — Bidding Period | 6 | 0 | 1 | 7 |
| TNDR — Bid Security | 4 | 1 | 0 | 5 |
| CMTE — BEC Assignment | 6 | 1 | 1 | 8 |
| CMTE — BOC Assignment | 1 | 1 | 0 | 2 |
| CMTE — PC Assignment | 1 | 1 | 0 | 2 |
| COI — Declarations | 3 | 1 | 3 | 7 |
| DOCS — Document Checklist | 6 | 2 | 1 | 9 |
| BOC — Bid Opening | 4 | 1 | 5 | 10 |
| EVAL — Stage Gates | 7 | 0 | 0 | 7 |
| EVAL — Cell Decisions | 5 | 2 | 3 | 10 |
| EVAL — Mandatory Info | 3 | 0 | 0 | 3 |
| PRICE — Price Evaluation | 5 | 4 | 3 | 12 |
| PQ — Post-Qualification | 7 | 1 | 2 | 10 |
| CLAR — Clarifications | 4 | 2 | 1 | 7 |
| RPT — Report Generation | 5 | 3 | 3 | 11 |
| AWRD — Award | 5 | 3 | 2 | 10 |
| POST — Post-Award | 3 | 3 | 1 | 7 |
| ADMN — Administration | 3 | 1 | 2 | 6 |
| SESS — Session Controls | 2 | 1 | 2 | 5 |
| AI — AI Rules | 3 | 1 | 3 | 7 |
| **TOTAL** | **93** | **31** | **35** | **159** |

---

## IMPLEMENTATION NOTES FOR DEVELOPERS

1. **BLOCK rules are non-negotiable**: No UI override, no admin bypass. The condition must be genuinely resolved.
2. **WARN rules require acknowledgment log**: System must store: rule ID, user, timestamp, acknowledgment text, any justification entered. This is part of the forensic audit trail.
3. **INFO rules are silent in audit trail**: Only log when the user explicitly clicks "Acknowledge" or when the info is material to a gate.
4. **AI rules (AVR-AI-001, AVR-AI-005)**: These are architectural constraints — the AI model must be configured at the prompt/output level to never produce Accept/Reject verdicts or substantive clarification content.
5. **Rule sequencing**: Rules fire in the order listed within each module. If a BLOCK fires, no further rules in that trigger event are evaluated.
6. **Real-time vs. submit-time**: Cell-level rules (EVAL, PRICE) fire in real-time as fields are filled. Gate rules (stage transitions) fire on explicit "Complete Stage X" button press.
7. **Cross-module dependencies**: BLOCK rules in EVAL-001 reference COI module completion. All cross-module dependencies are noted in "Condition" column.

---

*End of AI_VALIDATION_RULES.md v1.0*
*159 validation rules across 21 modules*
*Cross-referenced with: COMPLIANCE_CITATION_MAP.md (301 rules), SPEC_GAPS_v3_1.md (61 gaps), SYSTEM_SPECIFICATION.md v3.0*
