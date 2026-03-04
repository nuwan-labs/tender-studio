# TenderStudio — Complete System Specification

**Version 5.1** | The Definitive Developer Reference
**Date:** 2026-02-27
**Status:** Authoritative — Self-Contained — No Companion Documents Required
**Legal Authority:** National Procurement Manual 2024, National Procurement Commission, Sri Lanka

---

## Table of Contents

1. [System Purpose and Scope](#1-system-purpose-and-scope) — incl. §1.5 Procurement Principles, §1.6 Contract Categories & Scope, §1.7 FFA Guidelines
2. [Legal and Compliance Framework](#2-legal-and-compliance-framework) — incl. §2.17 Ethics, COI Definition & NCA
3. [System Architecture](#3-system-architecture)
4. [Organizational Roles and Permissions](#4-organizational-roles-and-permissions)
5. [Complete Data Model](#5-complete-data-model)
6. [Tender Lifecycle — 18 Stages](#6-tender-lifecycle--18-stages)
7. [Evaluation System — 4 Stages](#7-evaluation-system--4-stages)
8. [Price Adjustment Worksheet — 14 Steps](#8-price-adjustment-worksheet--14-steps)
9. [Clarification System](#9-clarification-system)
10. [Report Generation](#10-report-generation)
11. [AI Features](#11-ai-features)
12. [User Interface Specification](#12-user-interface-specification)
13. [Session and Access Control](#13-session-and-access-control)
14. [Audit Trail](#14-audit-trail)
15. [System Administration](#15-system-administration)
16. [Validation Rules](#16-validation-rules)
17. [Glossary](#17-glossary)

---

## 1. System Purpose and Scope

### 1.1 What TenderStudio Is

TenderStudio is a web-based intelligent procurement management system for Sri Lankan public-sector Procuring Entities (PEs). It digitises the complete tender lifecycle — from initial creation through contract award and post-award tracking — in strict compliance with the National Procurement Manual 2024.

The system replaces paper-based processes with structured digital workflows, eliminates transcription errors in evaluation reports, and provides AI-assisted (not AI-decided) evaluation support. All AI models run locally on the backend server; no procurement data leaves the organisation's infrastructure.

### 1.2 What TenderStudio Is Not

- It is not an e-procurement marketplace or supplier portal.
- It does not publish tenders to external websites (it tracks that publication happened).
- It does not communicate directly with bidders (the Procurement Manager does that; TenderStudio tracks the correspondence).
- It does not make award decisions; committees decide and the system records their decisions.

### 1.3 Scale and Constraints

| Constraint | Value |
|---|---|
| Organisation | Single PE |
| Concurrent users | < 20 |
| Concurrent tenders | < 10 |
| Language | English only |
| Network | LAN-first; full offline support |
| Data sovereignty | All data and AI models on local servers |
| Audit | Immutable, no deletion, forensic-grade |

### 1.4 Core Design Principles

1. **Assist, not decide.** Every AI output is a draft for human review. No AI action is final without human confirmation.
2. **Strict role separation.** Procurement Department staff cannot be BEC members. System enforces this absolutely.
3. **Session-based collaboration.** BEC evaluations happen in physical meetings with the Secretary operating software on a shared screen.
4. **Forensic audit trail.** Every state change is timestamped, attributed, and immutable.
5. **Offline-first.** All evaluation work is possible offline; automatic sync on reconnection.
6. **Legal compliance.** Every enforcement rule in this document traces to the National Procurement Manual 2024.

### 1.5 National Procurement Principles (Manual §1.2(c))

TenderStudio is designed to uphold all seven principles mandated by the National Procurement Manual 2024. These principles are the foundation for every rule in this specification:

| # | Principle | What it means for TenderStudio |
|---|---|---|
| 1 | **Economy, Efficiency, Effectiveness, Equity and Value for Money** (while complying with all applicable laws) | Evaluation workflows are structured to compare bids objectively and identify the best value — not merely the lowest price |
| 2 | **Fair, equal and maximum opportunity to compete** | Evaluation criteria are fixed before bid opening; no post-opening changes to requirements; all responsive bidders evaluated under identical criteria |
| 3 | **Expeditious execution of procurement** | System tracks dates and deadlines; alerts PM when milestones are overdue; minimum bidding periods enforced |
| 4 | **Transparency throughout the process** | Audit trail is immutable; evaluation decisions are recorded with reasoning; report auto-generated from evaluation data |
| 5 | **Sustainable Public Procurement (SPP)** | Contract type and evaluation criteria may include sustainability parameters; SPP considerations are a category in the specification library |
| 6 | **Confidentiality** | Evaluation proceedings are restricted to committee members; COI declarations enforce commercial-in-confidence obligations; bid prices hidden from BEC until Chair enables visibility |
| 7 | **Integrity** | COI declarations mandatory for all officers involved; NCA mandatory for all bidders; fraud/corruption grounds for debarment |

### 1.6 Contract Categories and Scope of Application

**Three Contract Categories (Manual §1.2(b)):**

| Category | Definition | Examples |
|---|---|---|
| **Goods** | Commodities, raw materials, products, equipment (and associated services: installation, maintenance, training — incidental to the main supply) | Laboratory equipment, vehicles, computers, reagents |
| **Works** | Construction, reconstruction, demolition, repair, renovation, installation of any plant/equipment where construction is the principal object | Buildings, roads, bridges, plumbing, electrical fit-out |
| **Non-Consulting Services** | Services that do not require specialised professional knowledge and can be evaluated primarily on price | Janitorial/cleaning, security guarding, routine machinery maintenance, cargo clearance, catering, transport/haulage |

> **Consulting Services** (specialist professional/advisory services) are governed by a separate Manual and are **out of scope** for TenderStudio in its current version.

**Institutions to which the Manual applies (Manual §1.4):**
- All Ministries and Government Departments
- Public Corporations and Statutory Bodies
- Local Authorities
- Government Business Undertakings
- Government-owned Companies (>50% government shareholding)
- Provincial Councils (with amendments per Provincial Finance Commission authority)

### 1.7 Foreign Funding Agency (FFA) Guidelines (Manual §1.4.1)

When a tender is financed externally, the following rule applies:

| Funding Type | Rule |
|---|---|
| **Foreign Grant** | PE may follow FFA guidelines where they differ from the Manual, provided the grant agreement permits |
| **Bilateral / Multilateral Loan or Credit** | PE must **review** FFA guidelines against Manual objectives. If FFA guidelines conflict with the Manual, the PE follows the Manual — unless the loan/credit agreement explicitly specifies that FFA guidelines prevail. The PE must document the review and its rationale. |

**In TenderStudio:** When `funding_source = Foreign-Funded`, the Project Setup screen displays a warning panel prompting the PM to confirm whether FFA guidelines apply and to record the outcome of the FFA-vs-Manual review.

---

## 2. Legal and Compliance Framework

### 2.1 Procurement Committees — Types and Thresholds

Six types of Procurement Committees (PCs) exist, determined by contract value and funding source. TenderStudio supports all six; the default is DPC.

| PC Type | Full Name | GOSL-Funded Threshold | Foreign-Funded Threshold |
|---|---|---|---|
| HLPC | High Level Procurement Committee | > LKR 750 Mn | > LKR 1,500 Mn |
| SHLPC | Standing High Level Procurement Committee | > LKR 750 Mn | > LKR 1,500 Mn |
| MPC | Ministry Procurement Committee | ≤ LKR 750 Mn | ≤ LKR 1,500 Mn |
| DPC | Department Procurement Committee | ≤ LKR 400 Mn | ≤ LKR 800 Mn |
| PPC | Project Procurement Committee | ≤ LKR 400 Mn | ≤ LKR 800 Mn |
| RPC | Regional Procurement Committee | ≤ LKR 50 Mn | ≤ LKR 100 Mn |

**Shopping/RFQ upper limits (GOSL-funded, Goods):**

| PC Type | Shopping Upper Limit |
|---|---|
| MPC | ≤ LKR 25 Mn |
| DPC / PPC | ≤ LKR 20 Mn |
| RPC | ≤ LKR 6 Mn |

### 2.2 Procurement Methods

| Method | Code | Scope |
|---|---|---|
| International Competitive Bidding | ICB | Open; all qualified bidders worldwide |
| Limited International Bidding | LIB | Invited; select international firms |
| National Competitive Bidding | NCB | Open; Sri Lanka-registered bidders |
| Limited National Bidding | LNB | Invited; select national firms |
| Shopping / Request for Quotations | RFQ | Low-value; minimum 3 quotations |
| Direct Contracting | DC | Single-source; exceptional justification required |
| Emergency Procurement | EP | Declared emergency; expedited |

### 2.3 Bidding Procedures

| Procedure | Code | Description |
|---|---|---|
| Single-Stage One-Envelope | SS-1E | All technical and financial information in one sealed envelope. Default. |
| Single-Stage Two-Envelope | SS-2E | Envelope A = technical; Envelope B = financial. Financial opened only for technically responsive bidders after Stage 2b is complete. |
| Two-Stage | 2S | Stage 1: non-priced technical proposals; Stage 2: revised technical + financial proposals. |

### 2.4 Minimum Bidding Periods

The bidding period runs from the date Procurement Documents are first made available to the bid/proposal closing date.

| Procurement Method | Minimum Calendar Days |
|---|---|
| ICB / LIB | 42 |
| NCB / LNB — contract ≥ LKR 50 Mn | 21 |
| NCB / LNB — contract < LKR 50 Mn | 14 |
| Shopping / RFQ — National | 7 |
| Shopping / RFQ — International | 14 |
| Emergency Procurement | Reasonable and practical per declared circumstances |

The system calculates the minimum bidding period from these rules and displays a warning if the configured closing date would violate the minimum.

### 2.5 Bid Validity Periods

Recommended minimum bid/proposal validity periods (calendar days). These are recommendations — the system warns if below minimum but does not block.

**Works Contracts:**

| Contract Value (LKR Mn) | GOSL-Funded | Foreign-Funded |
|---|---|---|
| ≥ 3,000 | 189 | 203 |
| ≥ 1,000 to < 3,000 | 175 | 189 |
| ≥ 500 to < 1,000 | 147 | 175 |
| ≥ 250 to < 500 | 119 | 147 |
| ≥ 25 to < 250 | 91 | 119 |
| ≥ 5 to < 25 | 77 | 77 |
| < 5 | 49 | 49 |

**Goods Contracts:**

| Contract Value (LKR Mn) | GOSL-Funded | Foreign-Funded |
|---|---|---|
| ≥ 3,000 | 175 | 189 |
| ≥ 1,000 to < 3,000 | 147 | 175 |
| ≥ 500 to < 1,000 | 119 | 147 |
| ≥ 250 to < 500 | 91 | 119 |
| ≥ 25 to < 250 | 91 | 119 |
| ≥ 5 to < 25 | 77 | 77 |
| < 5 | 49 | 49 |

### 2.6 Bid Security Rules

| Parameter | Rule |
|---|---|
| Amount range | 1% to 2% of estimated procurement value (excluding VAT) |
| Validity — National (NCB/LNB) | Bid validity period + **28** calendar days |
| Validity — International (ICB/LIB) | Bid validity period + **56** calendar days |
| Acceptable forms | Bank Guarantee; Refundable Cash Deposit; Bid Securing Declaration (BSD) |
| BSD permitted threshold | Contracts with estimated value < LKR 20 Mn only |
| Bank Guarantee issuer | Local commercial bank (CBSL-approved); foreign bank confirmed by local CBSL-approved bank; EXIM Bank / Export Credit Agency / reputed international financier (CBSL-acceptable — confirmation not required); Construction Guarantee Fund |
| Minor deviations | A bid must NOT be rejected for minor bid security deviations that do not affect the PE's rights, the security's value, its validity period, or the ability to encash on first written demand |

### 2.7 Performance Security Rules

| Contract Type | Minimum Amount | Validity |
|---|---|---|
| Goods | 10% of contract value | 28 days beyond scheduled supply completion |
| Works | 5% of contract value | 28 days beyond intended completion date |
| Services | 5% of contract value | 28 days beyond intended completion date |
| Contracts < LKR 5 Mn | PE may waive at discretion | — |

Once performance security is submitted and contract is signed, bid securities of all bidders are released.

### 2.8 Advance Payment Rules

| Contract Type | Maximum Advance |
|---|---|
| Works contracts > LKR 50 Mn | 20% of contract value |
| Works contracts ≤ LKR 50 Mn | 30% of contract value |
| Goods contracts | 30% of contract value |

Advance payments require an unconditional on-demand advance payment guarantee from an accepted bank or the Construction Guarantee Fund. Advance must be fully recovered before 90% of net contract payments are made.

### 2.9 Liquidated / Delay Damages

| Contract Type | Rate | Maximum Cap |
|---|---|---|
| Goods | Maximum 1% of contract value per week | 10% of contract value |
| Works | Maximum 0.05% of contract value per day | 10% of contract value |

When delay damages reach the cap, the PE may initiate contract termination.

### 2.10 Retention Money (Works Contracts)

- Minimum 5% of contract value retained from interim payments.
- On completion: 50% may be released; remaining 50% released on satisfactory completion of Defects Liability Period (or replaced with equivalent unconditional bank guarantee valid 28 days beyond Defects Liability Period).

### 2.11 Standstill Period

A mandatory standstill of at least **10 working days** must elapse between the date the PE sends the Notification of Intention to Award and the actual award (LOA issuance).

**Exceptions — standstill does NOT apply:**
- Only a single bid/proposal was received in competitive bidding
- Direct contracting
- Shopping / Request for Quotations
- Emergency procurement declared by relevant Government Authorities

**Rights during standstill:**
- Unsuccessful bidders may request a debriefing before the end of the **3rd working day**.
- PE must complete the debriefing before the end of the **5th working day**.
- Any bidder wishing to appeal must do so before the standstill expires.

### 2.12 Procurement Appeal Bodies

| Appeal Body | Handles Procurements Of | Members | Cash Deposit | Resolution Target |
|---|---|---|---|---|
| PAB (Procurement Appeal Board) | HLPC / SHLPC | 3 + 1 alternate | LKR 100,000 | 10 working days |
| MPAC (Ministry PAC) | MPC | 3 + 1 alternate | LKR 25,000 | 10 working days |
| DPAC (Department PAC) | DPC | 3 + 1 alternate | LKR 10,000 | 10 working days |
| PPAC (Project PAC) | PPC | 3 + 1 alternate | LKR 10,000 | 10 working days |
| RPAC (Regional PAC) | RPC | 3 + 1 alternate | LKR 5,000 | 5 working days |

The system auto-sets the correct appeal body and cash deposit amount based on the tender's configured PC type.

### 2.13 Contract Award Publication

The PE must publish award details on its official website for **all** awarded contracts.

Required published information:
- Description of Goods/Works/Services procured
- Total number of bids/proposals received
- Name of the successful bidder
- Contract price
- Details of the local agent (if a foreign bidder is awarded)

**If contract value > LKR 750 Mn:** Additionally publish in the Government Gazette (all three official languages) and on the General Treasury website.

### 2.14 Formal Contract Agreement Thresholds

| Contract Type | Formal Agreement Required When |
|---|---|
| Works | Value > LKR 500,000 |
| Goods or Services | Value > LKR 1,000,000 |
| Below thresholds | PE discretion; purchase order acceptable |

Until the formal contract is signed, the LOA serves as the binding agreement between parties.

### 2.15 Contract Variation Approval Authority

All variation requests must first be examined by the Variation Review Committee (VRC — minimum 3 members: specialists in engineering, finance, and procurement). VRC confirms: (a) variation could not have been foreseen at contract award, and (b) change is justifiable.

| Aggregate Variation Level | Aggregate Amount Condition | Approving Authority |
|---|---|---|
| ≤ 7.5% of contract award price | AND aggregate amount < DPC/PPC threshold | AO / HD / PD |
| ≤ 7.5% of contract award price | BUT aggregate amount ≥ DPC/PPC threshold | Forward to CAO |
| > 7.5% and ≤ 10% | AND aggregate amount < MPC threshold | CAO |
| ≤ 10% | BUT aggregate amount ≥ MPC threshold | Cabinet of Ministers |
| > 10% | Any amount | Appropriate PC (not necessarily original PC) + Cabinet if > MPC threshold |

### 2.16 Debarment

Debarment restricts a bidder/supplier/contractor from public procurement for a specified period. Three levels:
1. **National:** All Government institutions to which Guidelines apply
2. **Ministry-level:** Within the specific Ministry
3. **PE-level:** Within the specific Procuring Entity

**Grounds for debarment:** BSD declaration default; false eligibility/qualification information or falsified documents; unauthorized use of another's name for bidding; bid withdrawal after closing; refusal to accept award without justification; post-award inability to perform or failure to submit performance security; breach of contract terms.

**During active debarment:** The bidder cannot participate in new procurements but must continue performing any already-awarded contracts.

**Debarment appeal:** Within 5 working days of determination. If CAO made determination → appeal to Secretary to General Treasury. If AO made determination → appeal to CAO. Appeal committee concludes within 21 calendar days.

### 2.17 Procurement Ethics and Integrity (Manual §1.5)

#### 2.17.1 General Integrity Obligation

All officers involved in any aspect of a procurement process must conduct themselves with the **highest degree of integrity** throughout. This obligation covers the entire lifecycle — from inception of requirements through contract close-out.

#### 2.17.2 Conflict of Interest — Definition and Grounds

An officer has a conflict of interest in any of the following four situations (Manual §1.5.4):

| Ground | Description |
|---|---|
| **(a) Financial / Ownership** | The officer, their close relatives, or business associates have ownership, shareholding, employment, or any other direct or indirect financial interest in a bidding entity — or in any entity connected to the outcome of the procurement |
| **(b) Prior Involvement** | The officer was involved in preparing the technical specifications, bid documents, or evaluation criteria for this procurement — or will be involved in implementing the resulting contract |
| **(c) Organisational Relationship** | The officer's employing organisation has a close business or family relationship with key personnel of the PE who are involved in or influence the procurement decision |
| **(d) Other Specified Situations** | Any other situation described in the Procurement Documents as constituting a conflict of interest for this specific procurement |

**Obligation on declaration:** An officer who identifies a conflict must immediately declare it to the appointing authority and **disassociate** from all further involvement in that procurement.

#### 2.17.3 COI Declaration Forms — Annexure I and Annexure II

**Who must sign what, and when:**

| Form | Who Signs | When | On Conflict Declared |
|---|---|---|---|
| **Annexure I** (PC/BEC Member COI) | All PC members, all BEC members, BOC Chairman | **Before commencing** any role in the procurement process | Member must **request to be replaced** and disassociate immediately |
| **Annexure II** (Staff Officer / Supporting Staff COI) | All PMD staff, BEC secretaries, technical assistants, and any other officer providing supporting services to any committee | **Before commencing** any supporting role in the procurement process | Staff officer must **inform the appointing authority**; the appointing authority decides on replacement |

> **Key distinction:** Annexure I members (committee members) must actively request their own replacement when a conflict arises. Annexure II staff (supporting officers) only need to inform the appointing authority — the authority decides whether to replace them.

**Annexure I — Five Undertakings:**
1. Not to discuss evaluation proceedings with any party other than other PC/BEC members **officially involved in this tender** — commercial-in-confidence obligation
2. No shareholding, close relative relationship, or direct/indirect financial interest in any bidder
3. To declare and **request replacement** if a conflict of interest arises during the process
4. Maintain strict confidentiality of proceedings, bids, evaluation decisions, and all tender information
5. Not to use their position or information obtained for personal gain or to advantage any party

**Annexure II — Five Undertakings (same as Annexure I except item 3):**
1. Not to discuss evaluation proceedings with any party other than officially involved PC/BEC members
2. No shareholding, close relative relationship, or direct/indirect financial interest in any bidder
3. To **inform the appointing authority** if a conflict of interest arises (no request for replacement — the authority decides)
4. Maintain strict confidentiality of proceedings, bids, evaluation decisions, and all tender information
5. Not to use their position or information obtained for personal gain or to advantage any party

**"Before commencing" rule:** COI declarations must be signed **before the officer begins any procurement-related work** — not only at the first formal committee meeting. For BEC members, this means before any evaluation session. For PC members, before budget approval discussion. For BOC members, before entering the bid opening venue.

#### 2.17.4 Non-Collusion Affidavit (NCA) — Bidder Obligation

The NCA (Annexure III of Manual Chapter 01) is required from all bidders in competitive procurements. The bidder swears or affirms, **under penalty for perjury**:

1. The bid price was **not arrived at through combination, collusion, or price-fixing** with any other bidder
2. The bidder **did not prevent or induce** any other person from bidding or from withdrawing their bid
3. The bid was **made without reference** to any other bid submitted for the same procurement

Additionally, the bidder confirms that **no rebate, gift, commission, or other inducement** has been or will be paid in connection with the bid.

**Execution requirement:** The affidavit must be **sworn** before a **Justice of the Peace** or **Commissioner of Oaths** — a signature alone is insufficient. The system tracks `nca_sworn` (boolean) and `nca_sworn_before` (name and designation of JP or Commissioner).

#### 2.17.5 Sanctions for Prohibited Practices

Bidders or officers found to have engaged in fraud, corruption, collusion, coercion, or obstruction in connection with any procurement may be subject to debarment under Chapter 10 (see §2.16). The PE's CAO/AO must investigate any allegation of prohibited practices and may impose sanctions following the Chapter 10 process. TenderStudio's immutable audit trail supports such investigations.

---

## 3. System Architecture

### 3.1 Technical Model

- **Client:** Web browser (no installed client software required)
- **Server:** Centralised backend with local AI models
- **Storage:** Server-side relational database
- **AI:** All inference on backend server — no external cloud API calls for AI
- **Offline:** Full offline support via local cache; automatic sync on reconnection
- **Conflict resolution:** Last-write-wins for non-contentious fields; manual resolution required for conflicting evaluation decisions

### 3.2 Deployment

The system runs within the PE's local network. Remote access, if required, is managed by the PE's own network infrastructure.

---

## 4. Organizational Roles and Permissions

### 4.1 Role Summary

| Role | Tender Access | BEC Access | Administration |
|---|---|---|---|
| System Administrator | None | None | Full |
| Procurement Manager (PM) | Full (read-only after BEC assignment) | None | None |
| Trainee Procurement Executive (TPE) | Assigned tenders, limited | None | None |
| Procurement Assistant (PA) | Support functions | None | None |
| BOC Chairman | Bid opening only | None | None |
| BOC Member | Bid opening only | None | None |
| BEC Chair | None (creates no tenders) | Full: approve decisions, certify report, manage SS-2E envelope opening | None |
| BEC Secretary | None | Full write during active sessions | None |
| BEC Member | None | Write only if session active AND Secretary grants temporary access | None |
| PC Member | Budget approval, final approval views only | None | None |

### 4.2 System Administrator

**Responsibilities:**
- Create and manage user accounts and role assignments
- Maintain the BEC Eligibility Pool (pre-approved officers eligible for BEC service)
- Configure system-level settings
- View audit logs (read-only)

**Hard restrictions:**
- Cannot create, view, or participate in any tender activity
- Cannot be assigned to BOC, BEC, or PC for any tender
- Cannot view evaluation data

### 4.3 Procurement Manager

**Responsibilities:**
- Create and configure tenders (items, requirements, bidder list, document checklist)
- Prepare bid documents (tracked in system)
- Assign BOC and BEC (from eligibility pool) with PM nomination, Admin confirmation
- Manage pre-bid meetings and addenda
- Monitor all tender progress — **read-only after BEC assignment**
- Send clarifications to bidders (after BEC Secretary drafts and BEC Chair approves)
- Enter bidder clarification responses
- Issue Notification of Intention to Award
- Manage standstill period (debriefings, appeals)
- Issue Letter of Acceptance
- Manage post-award tracking and contract variations
- Archive completed tenders

**After BEC assignment:** PM view of BEC evaluation is read-only. PM cannot enter or modify evaluation cells, decisions, or session data.

### 4.4 BEC Eligibility Pool

The System Administrator maintains the pool. Rules enforced by the system at the time of BEC assignment:

1. **Procurement Department exclusion:** No PM, TPE, or PA can appear in the BEC Eligibility Pool. Their role membership prevents pool listing.
2. **Concurrent BEC limit:** Maximum 6 active BEC assignments per officer. Assignment blocked if limit is reached.
3. **Spec preparer exclusion:** The officer who prepared the technical specifications for a tender (`spec_preparer_id`) cannot serve on the BEC for that same tender.
4. **COI obligation:** After assignment, each member must sign a COI declaration before any evaluation work begins.

### 4.5 BEC Chair

**During evaluation sessions:**
- Approve/override cell decisions
- Resolve Debatable deviations (Accept-as-Minor or Reject-as-Major, with recorded reasoning)
- Enable/disable price visibility on shared screen
- Open Financial Envelopes in SS-2E procedure
- Certify the final evaluation report

**Outside sessions:** Read-only access to all evaluation data for their assigned tenders.

### 4.6 BEC Secretary

**During evaluation sessions:** Primary system operator. Enters all cell data, deviation classifications, clarification drafts, session attendance. Grants and revokes temporary write access to individual members.

**Outside sessions:** Read-only access.

### 4.7 BEC Member

**During sessions:** Read-only by default. If the BEC Secretary grants temporary write access for a specific cell, the member may enter data in that cell only. The grant is revoked by the Secretary or automatically at session end.

**Outside sessions:** Read-only.

### 4.8 BOC

Appointed by PE with PC concurrence. Chairman + at least one member. Can include Procurement Department staff. Cannot be BEC members for the same tender. Access limited to Bid Opening module for their assigned tender only.

### 4.9 PC Members

Assigned per tender for two approval stages: (1) budget approval at start, (2) final approval at end. Cannot access BEC evaluation data. Receive the complete BER for review before final approval.

---

## 5. Complete Data Model

### 5.1 Tender

| Field | Type | Notes |
|---|---|---|
| `tender_id` | UUID | System-generated |
| `tender_number` | String | PE's reference number |
| `title` | String | Official procurement title |
| `description` | Text | Scope summary |
| `contract_type` | Enum | Goods \| Works \| Services \| Information-Systems |
| `tender_type` | Enum | ICB \| LIB \| NCB \| LNB \| Shopping \| Direct \| Emergency |
| `bidding_procedure` | Enum | SS-1E \| SS-2E \| 2S (default: SS-1E) |
| `pc_type` | Enum | HLPC \| SHLPC \| MPC \| DPC \| PPC \| RPC (default: DPC) |
| `funding_source` | Enum | GOSL \| Foreign-Funded |
| `tce_value` | Decimal (LKR) | Tender Cost Estimate — CONFIDENTIAL. Never visible to bidders. Not visible to BEC until BEC Chair enables price visibility after bid opening. |
| `tce_prepared_by` | UserID | Must differ from `pm_id` — enforced by system |
| `spec_preparer_id` | UserID | Officer who prepared technical specifications; checked against all BEC assignments |
| `is_repeat_order` | Boolean | |
| `original_contract_ref` | String | If is_repeat_order = true |
| `lcc_required` | Boolean | Life Cycle Costing required as evaluation criterion |
| `domestic_preference_applicable` | Boolean | |
| `pm_id` | UserID | Assigned Procurement Manager |
| `status` | Enum | Draft \| PC-Approval-Pending \| Active \| BEC-Evaluation \| BEC-Report \| PC-Final-Pending \| Standstill \| Awarded \| Archived \| Cancelled |
| `publication_date` | Date | Date of IFB/RFB advertisement |
| `pre_bid_meeting_date` | Date | Nullable |
| `bid_closing_datetime` | DateTime | Submission deadline |
| `bid_opening_datetime` | DateTime | ≥ bid_closing_datetime |
| `bid_validity_days` | Integer | Required bid validity in calendar days |
| `bid_validity_expiry_date` | Date | Computed: bid_opening_datetime + bid_validity_days |
| `bid_security_min_pct` | Decimal | 1–2% of TCE |
| `bid_security_max_pct` | Decimal | 2% cap |
| `bid_security_amount_lkr` | Decimal | Computed: bid_security_min_pct × tce_value |
| `bid_security_validity_beyond_days` | Integer | 28 (NCB/LNB/RFQ) or 56 (ICB/LIB); auto-set from tender_type |
| `standstill_start_date` | Date | Set when Notification of Intention to Award is sent |
| `standstill_expiry_date` | Date | Computed: standstill_start_date + 10 working days |
| `standstill_exception` | Boolean | True if standstill does not apply |
| `standstill_exception_reason` | Enum | Single-Bid \| Direct-Contracting \| Shopping \| Emergency |
| `award_cleared` | Boolean | True when standstill expired with no blocking appeals |
| `loa_date` | Date | Date LOA issued |
| `contract_price_lkr` | Decimal | Contract price at award |
| `performance_security_pct` | Decimal | 10% (Goods) or 5% (Works/Services) — auto-set from contract_type |
| `performance_security_amount_lkr` | Decimal | Computed: performance_security_pct × contract_price_lkr |
| `performance_security_due_date` | Date | As specified in LOA |
| `created_at` | Timestamp | |
| `updated_at` | Timestamp | |

### 5.2 Package / Lot

| Field | Type | Notes |
|---|---|---|
| `package_id` | UUID | |
| `tender_id` | UUID | |
| `package_number` | Integer | Sequential |
| `title` | String | |
| `evaluation_basis` | Enum | Lot-based \| Item-based |
| `status` | Enum | Active \| Evaluated \| Recommended \| Awarded |

### 5.3 Item

| Field | Type | Notes |
|---|---|---|
| `item_id` | UUID | |
| `package_id` | UUID | |
| `item_number` | Integer | Sequential within package |
| `description` | String | |
| `unit_of_measure` | String | |
| `quantity` | Decimal | |
| `status` | Enum | Draft \| Active \| Evaluating \| Complete |

An item is **Complete** only when: all evaluation cells have final decisions, all Debatable deviations are resolved, all clarification threads are Closed, and all mandatory information fields (Make, Model, Country of Origin for equipment) are resolved.

### 5.4 Requirement (Technical Specification)

| Field | Type | Notes |
|---|---|---|
| `requirement_id` | UUID | |
| `item_id` | UUID | |
| `sequence_number` | Integer | Display order in evaluation grid |
| `description` | Text | Specification text |
| `is_compulsory` | Boolean | True = "C" (Compulsory); False = Non-Compulsory |
| `source` | Enum | Manual \| Library |
| `library_ref_id` | UUID | Nullable |

**Compulsory rule:** A Compulsory (C) requirement that receives a Reject decision is automatically classified as a **Major** deviation. This is system-enforced and cannot be overridden. The bidder becomes Non-Responsive for that item.

### 5.5 Requestor (Internal Equipment Requestor — Record Only)

The Requestor entity records the user department contact who originated the procurement request. This information is used for:
- Pre-bid meeting notification (did the requestor attend?)
- Cross-referencing which department needs the equipment
- Report context (Section 2 Introduction identifies the requesting department)

| Field | Type | Notes |
|---|---|---|
| `requestor_id` | UUID | |
| `item_id` | UUID | One item can have multiple requestors |
| `name` | String | |
| `designation` | String | |
| `department` | String | |
| `email` | String | Contact for pre-bid / addendum notification only |
| `phone` | String | |

> **Evaluation restriction:** The Requestor (user department) has **no role in BEC evaluation**. Contacting the requestor for input during BEC evaluation would constitute a Conflict of Interest under Ground (b) (§2.17.2) if they prepared the specifications. BEC cannot consult them. The only advisory mechanism available to BEC is External Technical Expert Advisory with PC approval (§9.1).

### 5.6 Bidder (Organisation-wide Master Record)

| Field | Type | Notes |
|---|---|---|
| `bidder_id` | UUID | |
| `company_name` | String | |
| `registration_number` | String | |
| `address` | Text | |
| `contact_person` | String | |
| `email` | String | |
| `phone` | String | |
| `country` | String | |
| `debarment_status` | Enum | None \| Active \| Expired |
| `debarment_level` | Enum | None \| PE \| Ministry \| National |
| `debarment_start_date` | Date | Nullable |
| `debarment_end_date` | Date | Nullable |
| `debarment_grounds` | Text | Nullable |
| `debarment_source` | String | Authority that imposed debarment |
| `created_at` | Timestamp | |

If a bidder with `debarment_status = Active` is added to any tender, the system displays a persistent alert banner on that bidder's entry. All alert views are logged in the audit trail.

### 5.7 Tender Bidder (Participation Record per Tender)

| Field | Type | Notes |
|---|---|---|
| `tender_bidder_id` | UUID | |
| `tender_id` | UUID | |
| `bidder_id` | UUID | |
| `internal_reference` | String | E.g., "Bidder 1", "Bidder 2" — for display during blind evaluation |
| `date_documents_purchased` | Date | For addendum distribution tracking |
| `submission_received` | Boolean | |
| `submission_datetime` | DateTime | Nullable |
| `is_late_submission` | Boolean | Computed: submission_datetime > bid_closing_datetime |
| `withdrawal_received` | Boolean | |
| `withdrawal_datetime` | DateTime | Nullable |
| `withdrawal_authenticated` | Boolean | BOC confirmed withdrawal letter is authentic |
| `modification_received` | Boolean | |
| `overall_responsiveness` | Enum | Pending \| Responsive \| Non-Responsive |
| `elimination_stage` | String | Nullable — stage at which bidder was eliminated |
| `elimination_reason` | Text | Nullable |
| `post_qualification_result` | Enum | Pending \| Pass \| Fail \| Not-Evaluated |
| `final_rank` | Integer | Nullable — assigned after Stage 3 |
| `recommended_for_award` | Boolean | |

### 5.8 Bid Security

| Field | Type | Notes |
|---|---|---|
| `bid_security_id` | UUID | |
| `tender_bidder_id` | UUID | |
| `security_type` | Enum | Bank-Guarantee \| Cash-Deposit \| BSD |
| `issuing_bank_institution` | String | |
| `is_foreign_bank` | Boolean | |
| `foreign_bank_confirmed_by_local` | Boolean | Required if is_foreign_bank AND not EXIM/Export-Credit |
| `is_exim_or_export_credit` | Boolean | If true, local confirmation not required |
| `guarantee_number` | String | |
| `amount_lkr` | Decimal | |
| `amount_original_currency` | Decimal | If foreign currency |
| `original_currency` | String | ISO code |
| `issue_date` | Date | |
| `validity_expiry_date` | Date | |
| `required_validity_date` | Date | Computed: bid_validity_expiry_date + 28 or 56 days |
| `validity_status` | Enum | Valid \| Expires-Soon (≤ 7 days) \| Expired \| Not-Submitted |
| `boc_confirmed` | Boolean | BOC confirmed presence at opening |
| `bec_verified` | Boolean | BEC verified at Stage 1 |

**BSD restriction:** System blocks `security_type = BSD` selection when `tender.tce_value ≥ LKR 20,000,000`.

### 5.9 Document Checklist

One record per (tender_bidder, document_type) pair.

| Field | Type | Notes |
|---|---|---|
| `checklist_id` | UUID | |
| `tender_bidder_id` | UUID | |
| `document_type` | Enum | See list below |
| `is_required` | Boolean | |
| `submitted` | Enum | Yes \| No \| NA |
| `verified_by_bec` | Boolean | |
| `verification_notes` | Text | |

**Standard document types:**

| # | Document |
|---|---|
| 1 | Form of Bid |
| 2 | Price Schedule |
| 3 | Bid Security |
| 4 | Power of Attorney |
| 5 | Joint Venture Agreement |
| 6 | Eligibility Declaration — Bidder |
| 7 | Eligibility Declaration — Goods/Services |
| 8 | Technical Documentation |
| 9 | Audited Financial Statements |
| 10 | Non-Collusion Affidavit (NCA) |
| 11+ | Custom documents added by PM per tender |

**NCA special verification fields:**

| Field | Type | Notes |
|---|---|---|
| `nca_sworn` | Boolean | Whether NCA is sworn (not merely signed) |
| `nca_sworn_before` | String | Name of Justice of the Peace or Commissioner of Oaths |
| `nca_jp_coo_designation` | String | JP \| Commissioner-of-Oaths |
| `nca_jp_coo_registration` | String | Registration number |

An NCA that is signed but not sworn before a JP or Commissioner of Oaths is a document deficiency. The BEC must record this at Stage 1. Its effect on responsiveness depends on whether the Procurement Documents specified NCA as a rejection ground.

### 5.10 Evaluation Cell (per Bidder × Requirement)

| Field | Type | Notes |
|---|---|---|
| `cell_id` | UUID | |
| `tender_bidder_id` | UUID | |
| `requirement_id` | UUID | |
| `declaration` | Enum | Yes \| No \| Pending |
| `offered_value` | Text | AI pre-populated; human verified |
| `document_reference` | String | AI pre-populated; human verified |
| `page_section` | String | AI pre-populated; human verified |
| `extracted_text` | Text | RAG-extracted; AI pre-populated; human verified |
| `evaluator_notes` | Text | Always manual |
| `ai_populated` | Boolean | |
| `ai_verified_by` | UserID | Maker — first to verify AI content |
| `ai_second_verified_by` | UserID | Checker — second independent verifier |
| `decision` | Enum | Accept \| Reject \| Pending |
| `deviation_type` | Enum | null \| Major \| Minor \| Debatable |
| `monetary_loading_lkr` | Decimal | Required if deviation_type = Minor or Debatable (accepted as Minor) |
| `debatable_resolution` | Enum | Pending \| Accept-as-Minor \| Reject-as-Major |
| `debatable_resolved_by` | UserID | BEC Chair |
| `debatable_resolution_notes` | Text | BEC Chair's reasoning (mandatory when resolving) |
| `active_clarification_thread_id` | UUID | Nullable — BLOCKS cell decision when set |
| `locked` | Boolean | True once item is Complete |

**Deviation Classification Logic:**

| Situation | Classification | Effect |
|---|---|---|
| Compulsory (C) + Reject | **Major** (auto, non-overridable) | Bidder Non-Responsive for item |
| Non-Compulsory + Reject + material unquantifiable impact | **Major** | Bidder Non-Responsive for item |
| Non-Compulsory + Reject + minor, financially quantifiable impact | **Minor** | Bidder Responsive; load to bid price in Stage 3 |
| Non-Compulsory + Reject + unclear | **Debatable** | Chair must resolve; cell locked until resolved |
| Accept | null | No deviation |

For Minor deviations: if no financial impact, `monetary_loading_lkr = 0.00` must be explicitly confirmed — the system requires the BEC to confirm this rather than leave it blank.

**Cell blocking:** When `active_clarification_thread_id` is non-null, the `decision` field is locked. The cell cannot be decided or changed until the thread is closed.

### 5.11 Price Schedule

| Field | Type | Notes |
|---|---|---|
| `price_schedule_id` | UUID | |
| `tender_bidder_id` | UUID | |
| `package_id` | UUID | |
| `item_id` | UUID | Nullable if package-level |
| `unit_price` | Decimal | |
| `quantity` | Decimal | From tender setup |
| `line_item_total` | Decimal | Computed: unit_price × quantity |
| `inland_transport_cost` | Decimal | For imported goods on CIF/FOB basis |
| `vat_amount` | Decimal | Stated separately by bidder |
| `total_bid_price_excl_vat` | Decimal | Submitted total excluding VAT |
| `discount_amount` | Decimal | Unconditional discount |
| `discount_type` | Enum | Percentage \| Lump-Sum \| Item-Specific \| Cross-Discount |
| `discount_announced_at_opening` | Boolean | Must be true for discount to be valid |
| `incoterm` | Enum | EXW \| FOB \| CIF \| CIP \| DDP |
| `currency` | String | ISO currency code |

### 5.12 Price Adjustment Worksheet

One record per (tender_bidder, item, step).

| Field | Type | Notes |
|---|---|---|
| `adjustment_id` | UUID | |
| `tender_bidder_id` | UUID | |
| `item_id` | UUID | |
| `step_number` | Integer | 1–14 |
| `step_name` | String | |
| `adjustment_amount_lkr` | Decimal | Positive = added to bid price; Negative = deducted |
| `basis` | Text | How amount was calculated |
| `entered_by` | UserID | |
| `entered_at` | Timestamp | |

**Computed outputs per bidder per item:**
- `quoted_price` = price_schedule total excluding VAT, with unconditional discounts applied
- `total_adjustments` = sum of Steps 1–14
- `evaluated_price` = quoted_price + total_adjustments

**Critical rule:** The evaluated price is for comparison only. The contract is signed at the original quoted price minus unconditional discounts. The evaluated price is never the contract price.

### 5.13 Pre-Bid Meeting

| Field | Type | Notes |
|---|---|---|
| `prebid_id` | UUID | |
| `tender_id` | UUID | |
| `meeting_date` | DateTime | |
| `location` | String | |
| `attendees` | List of {name, designation, organisation} | Officials + bidder representatives at the meeting |
| `document_purchasers` | List of {company_name, contact, purchase_date} | All who purchased bid documents — separate from attendees |
| `questions_raised` | List of {question_text, raised_by, answer_text} | |
| `addenda_issued` | List of links to Addendum records | |
| `minutes_finalized` | Boolean | |
| `minutes_distributed` | Boolean | To all document purchasers |

**Rules:**
- For tenders with `tce_value > LKR 50,000,000`: pre-bid meeting must be held ≥ 10 calendar days before `bid_closing_datetime`. System warns if violated.
- Addenda arising from the meeting must be distributed to all `document_purchasers` (not just meeting attendees).
- Addendum distribution must be ≥ 3 working days before bid closing. System warns if violated.

### 5.14 Addendum

| Field | Type | Notes |
|---|---|---|
| `addendum_id` | UUID | |
| `tender_id` | UUID | |
| `addendum_number` | Integer | Sequential |
| `issue_date` | Date | |
| `subject` | String | |
| `content` | Text | |
| `pc_approved` | Boolean | All addenda require PC approval |
| `distributed_to_all_purchasers` | Boolean | |
| `distribution_date` | Date | |
| `days_before_closing` | Integer | Computed: bid_closing_date − distribution_date |

System warns if `days_before_closing < 3`.

### 5.15 COI Declaration Record

| Field | Type | Notes |
|---|---|---|
| `coi_id` | UUID | |
| `tender_id` | UUID | |
| `committee_type` | Enum | BEC \| BOC \| PC |
| `user_id` | UserID | |
| `declaration_form` | Enum | Annexure-I-Member \| Annexure-II-Staff |
| `signed_date` | Date | |
| `conflict_declared` | Boolean | |
| `conflict_description` | Text | Required if conflict_declared = true |
| `recused` | Boolean | |
| `recusal_date` | DateTime | |
| `recusal_reason` | Text | |
| `prior_decisions_flagged` | Boolean | System flags prior decisions by this officer |

**COI Rules:**

1. **Mandatory before commencing:** All committee members (BEC, BOC, PC) sign Annexure I before their first act in the procurement (before first BEC session, before bid opening venue entry, before budget approval discussion). All supporting staff (PMD officers, BEC secretaries, technical assistants) sign Annexure II before beginning any supporting work. The system blocks progression past each lifecycle stage until required COI records have `signed_date` set:
   - Stage 8 (BEC Evaluation): all BEC Annexure I and Annexure II records must be signed
   - Bid Opening: all BOC Annexure I records must be signed
   - PC approval events: all PC Annexure I records must be signed

2. **Annexure I vs Annexure II on conflict:** When `conflict_declared = true`: Annexure I officers (members) must request their own replacement — system marks `recused = true` and notifies PM and Admin to arrange a replacement. Annexure II officers (staff) only need to inform the appointing authority — system notifies Admin; the appointing authority decides on replacement. The distinction is enforced in the UI: Annexure I conflict triggers a mandatory replacement workflow; Annexure II conflict triggers a notification workflow only.

3. **Mid-evaluation conflict:** If a conflict is declared after evaluation has begun: (a) the officer is immediately set to read-only; (b) all their prior cell decisions are flagged for BEC Chair review; (c) PM and System Administrator are notified; (d) an immutable audit log entry is created; (e) BEC Chair determines whether prior decisions need reconsidering.

4. **Spec preparer check:** The system checks every BEC nominee against `tender.spec_preparer_id`. If matched, a conflict warning (Ground (b) — prior involvement in spec preparation) is raised before the assignment is confirmed. Assignment cannot proceed without override documented by PM.

5. **Commercial-in-confidence obligation:** All Annexure I signatories undertake not to discuss evaluation proceedings with any person outside the officially involved committee for this tender. The system enforces this by restricting evaluation data access to assigned members only. Any unauthorised sharing of evaluation data is logged as a security event.

6. **BOC COI:** BOC members also sign Annexure I COI declarations before entering the bid opening venue. BOC supporting staff sign Annexure II. Same before-commencing rule applies.

### 5.16 BOC Opening Record

| Field | Type | Notes |
|---|---|---|
| `boc_record_id` | UUID | |
| `tender_id` | UUID | |
| `opening_datetime` | DateTime | |
| `boc_chairman_id` | UserID | |
| `boc_member_ids` | List of UserIDs | |
| `opening_venue` | String | |
| `bids_received_on_time` | Integer | |
| `bids_received_late` | Integer | Returned unopened after contract award |
| `minutes_certified` | Boolean | |
| `minutes_certified_datetime` | DateTime | |

**Per-bidder BOC entry:**

| Field | Type | Notes |
|---|---|---|
| `bidder_id` | UUID | |
| `withdrawal_authenticated` | Boolean | |
| `modification_submitted` | Boolean | |
| `bid_announced_amount_lkr` | Decimal | Total amount read aloud |
| `bid_amount_per_package` | List of {package_id, amount} | If lot/package-based |
| `bid_security_present` | Boolean | |
| `bid_security_issuer` | String | |
| `bid_security_confirmed_by_local` | Boolean | If foreign bank |
| `discount_announced` | Boolean | |
| `discount_details` | Text | |
| `alternative_bid_submitted` | Boolean | |
| `samples_received` | Boolean | |
| `rate_columns_taped` | Boolean | BOC covered Rate and Item Total columns with transparent adhesive tape |
| `boc_members_initialled` | Boolean | Form of Bid, BOQ summary, bid security cover letter, discount letter, modification envelope |
| `boc_remarks` | Text | |

### 5.17 Clarification Thread

Only Bidder-type clarification threads are supported (see §9.1). Internal Technical Consultation has been removed — it has no basis in the National Procurement Manual 2024 (Chapter 07).

| Field | Type | Notes |
|---|---|---|
| `thread_id` | UUID | |
| `tender_id` | UUID | |
| `thread_type` | Enum | Bidder (only) |
| `level` | Enum | Requirement \| Item |
| `requirement_id` | UUID | Nullable; set if level = Requirement |
| `item_id` | UUID | |
| `bidder_id` | UUID | The bidder to whom the clarification is addressed |
| `status` | Enum | Draft \| Pending-Chair-Approval \| Sent \| Response-Received \| Closed |
| `closure_decision` | Enum | Pending \| Satisfied \| Not-Satisfied |
| `messages` | List of ClarificationMessage | |

**Clarification Message:**

| Field | Type | Notes |
|---|---|---|
| `message_id` | UUID | |
| `direction` | Enum | Outgoing \| Incoming |
| `content` | Text | |
| `drafted_by` | UserID | BEC Secretary |
| `approved_by` | UserID | BEC Chair (required before PM sends) |
| `sent_by` | UserID | PM |
| `sent_at` | DateTime | |
| `response_entered_by` | UserID | PM (for incoming) |
| `response_received_at` | DateTime | |

### 5.18 Standstill Record

| Field | Type | Notes |
|---|---|---|
| `standstill_id` | UUID | |
| `tender_id` | UUID | |
| `intention_sent_date` | Date | When PM sends Notification of Intention to Award |
| `standstill_expiry_date` | Date | Computed: intention_sent_date + 10 working days |
| `standstill_exception` | Boolean | |
| `exception_reason` | Enum | Single-Bid \| Direct-Contracting \| Shopping \| Emergency |
| `standstill_status` | Enum | Active \| Expired-No-Appeals \| Expired-With-Resolved-Appeals \| Blocked-By-Appeals |
| `award_cleared` | Boolean | |
| `debriefing_requests` | List of DebriefingRequest | |
| `appeal_records` | List of AppealRecord | |

**Debriefing Request:**

| Field | Notes |
|---|---|
| `bidder_id` | |
| `request_date` | Must be ≤ 3rd working day of standstill period |
| `debriefing_scheduled_date` | Must be ≤ 5th working day of standstill period |
| `debriefing_notes` | |
| `completed` | Boolean |

**Appeal Record:**

| Field | Notes |
|---|---|
| `bidder_id` | |
| `appeal_date` | Must be within standstill period |
| `appeal_body` | Auto-set from tender's pc_type |
| `deposit_amount_lkr` | Auto-set: PAB=100K, MPAC=25K, DPAC/PPAC=10K, RPAC=5K |
| `appeal_grounds` | Text |
| `appeal_outcome` | Pending \| Upheld \| Dismissed |
| `outcome_date` | Date |

### 5.19 Contract Variation Request (VRC)

| Field | Type | Notes |
|---|---|---|
| `variation_id` | UUID | |
| `tender_id` | UUID | |
| `requested_by` | UserID | |
| `request_date` | Date | |
| `description` | Text | |
| `original_contract_price_lkr` | Decimal | |
| `aggregate_variations_to_date_lkr` | Decimal | Sum of all previously approved variation amounts |
| `current_variation_amount_lkr` | Decimal | This request |
| `aggregate_after_this_lkr` | Decimal | Computed: aggregate_to_date + current_variation_amount |
| `aggregate_percentage` | Decimal | Computed: aggregate_after_this / original_contract_price × 100 |
| `vrc_recommendation` | Text | |
| `vrc_members` | List of {name, designation} | ≥ 3 members |
| `vrc_date` | Date | |
| `approval_level_required` | Enum | AO \| CAO \| Cabinet (computed from thresholds in Section 2.15) |
| `approval_status` | Enum | Pending \| Approved \| Rejected |
| `approved_by` | String | |
| `approval_date` | Date | |
| `revised_contract_price_lkr` | Decimal | Computed if approved |

### 5.20 Post-Award Tracking

| Field | Type | Notes |
|---|---|---|
| `tracking_id` | UUID | |
| `tender_id` | UUID | |
| `commencement_date` | Date | From LOA |
| `scheduled_completion_date` | Date | |
| `delivery_milestones` | List of {name, scheduled_date, actual_date, status} | |
| `installation_status` | Enum | Not-Started \| In-Progress \| Complete |
| `commissioning_status` | Enum | Not-Started \| In-Progress \| Complete |
| `goods_receipt_certificate_issued` | Boolean | |
| `goods_receipt_date` | Date | |
| `performance_security_status` | Enum | Not-Submitted \| Submitted \| Valid \| Expires-Soon \| Expired \| Released |
| `performance_security_validity_date` | Date | |
| `defects_liability_period_end` | Date | |
| `final_payment_certified` | Boolean | |
| `contract_closure_date` | Date | |

### 5.21 Session Record

| Field | Type | Notes |
|---|---|---|
| `session_id` | UUID | |
| `tender_id` | UUID | |
| `session_type` | Enum | BEC-Evaluation \| BOC-Opening \| PC-Approval \| Report-Review |
| `start_time` | Timestamp | |
| `end_time` | Timestamp | Nullable until ended |
| `secretary_id` | UserID | |
| `attendees` | List of {user_id, role, joined_at, left_at} | |
| `write_grants` | List of {user_id, cell_id, granted_at, revoked_at} | |
| `duration_minutes` | Integer | Computed |

---

## 6. Tender Lifecycle — 18 Stages

### Overview

| # | Stage | Owner | Key Gate |
|---|---|---|---|
| 1 | Tender Creation | PM | Configuration complete; submit for PC approval |
| 2 | PC Budget Approval | PC | PC approves budget and scope |
| 3 | Bid Document Preparation | PM + BEC | BEC approves documents with PC concurrence |
| 4 | Publication | PM | IFB/RFB published; documents available for purchase |
| 5 | Pre-Bid Meeting | PM | Minutes distributed; addenda issued if needed |
| 6 | Bid Closing | PM | Submission deadline; late bids rejected |
| 7 | Bid Opening (BOC) | BOC | All bids publicly opened; BOC minutes certified |
| 8 | BEC Assignment | PM + Admin | COI declarations signed; PTS approved |
| 9 | BEC Evaluation | BEC | 4-stage evaluation; all cells decided; all threads closed |
| 10 | Clarifications | BEC + PM | (Continuous throughout Stage 9) |
| 11 | Report Generation | BEC Secretary + Chair | Draft BER produced; narratives edited |
| 12 | BEC Certification | BEC Chair | Report certified; forwarded to PM |
| 13 | PM Review | PM | PM submits to PC |
| 14 | PC Final Approval | PC | PC approves recommendation |
| 15 | Notification of Intention to Award | PM | Standstill begins |
| 16 | Standstill Period | PM | ≥ 10 working days; debriefings + appeals handled |
| 17 | Contract Award (LOA) | PM | LOA issued; performance security obtained |
| 18 | Post-Award Tracking | PM | Delivery / installation / commissioning; archive |

### Stage 1 — Tender Creation

1. PM creates tender record: title, number, contract type, tender type, bidding procedure, PC type, funding source.
2. PM enters TCE value and confirms it was prepared by a different officer (`tce_prepared_by ≠ pm_id` — enforced).
3. PM records `spec_preparer_id`.
4. PM configures packages and items.
5. PM adds requirements per item (manually or from Specification Library).
6. PM sets dates (publication, pre-bid if planned, bid closing, bid opening).
7. System validates minimum bidding period; warns if violated.
8. PM configures document checklist (standard + custom documents).
9. PM sets bid security parameters (1–2% range; system auto-blocks BSD if TCE ≥ LKR 20 Mn).
10. PM submits for PC budget approval → status: `PC-Approval-Pending`.

### Stage 2 — PC Budget Approval

1. PC receives tender for review (scope, TCE, procurement approach).
2. PC records decision: Approve / Return-for-Revision / Reject.
3. If Approved → status: `Active` (proceed to Stage 3).

### Stage 3 — Bid Document Preparation

1. PM prepares draft Procurement Documents.
2. BEC reviews draft for Guidelines compliance.
3. BEC approves with PC concurrence.
4. Any decision on non-standard SPD (FIDIC, FFA documents) recorded in PC minutes.

### Stage 4 — Publication

1. PM records publication date and advertisement details.
2. PM registers each entity that purchases bid documents (`document_purchasers` list).
3. Bidding period tracking begins.

### Stage 5 — Pre-Bid Meeting

1. PM creates pre-bid meeting record; records attendance and document purchasers separately.
2. PM records questions raised and official answers.
3. If Procurement Document amendments result: PM prepares addendum; PC approves; PM distributes to all document purchasers ≥ 3 working days before bid closing.
4. Minutes distributed to all document purchasers.

**Validations:**
- If TCE > LKR 50 Mn: meeting must be ≥ 10 calendar days before closing. System warns.
- Addendum distribution: ≥ 3 working days before closing. System warns.

### Stage 6 — Bid Closing

1. At bid_closing_datetime: system locks new submission registrations.
2. PM confirms all bids received and records submission timestamps.
3. Late bids flagged `is_late_submission = true` — returned unopened after contract award (do NOT open at BOC).

### Stage 7 — Bid Opening (BOC)

**BOC composition:** Chairman + ≥ 1 member. Appointed by PE with PC concurrence. May include Procurement Department staff. Cannot be BEC members for this tender.

**Process (must begin within 30 minutes of bid closing):**

1. BOC sorts bids before opening:
   - Identify authenticated withdrawal letters → set aside; original bid NOT opened; stored sealed for return after award.
   - Identify modification envelopes; these will be opened first (then the original) for the relevant bidder.
   - Separate originals from duplicate copies.
   - Affix official BOC seal on all envelopes; each BOC member initials all envelopes.

2. For each bidder (modifications opened first, then original):
   - BOC covers Rate and Item Total columns with adhesive transparent tape to prevent alteration.
   - BOC members initial: Form of Bid, BOQ/Price Schedule summary, bid security cover letter, discount letter, modification envelope.
   - Read aloud: bidder name and address; bid amount (total; by package/lot if lot-based); bid security presence and issuing entity (and confirmation letter if foreign bank); discounts (if bidder brought an unread discount to BOC's attention, BOC reads it); whether alternative bid submitted; whether modification submitted.

3. If alternative bid envelopes are present: noted in minutes but NOT opened at BOC stage.

4. **For SS-2E procedure:** At BOC, only outer envelopes are opened to verify two inner envelopes (A=Technical, B=Financial) are present. Neither inner envelope is opened at BOC. Both are resealed and handed to PMD.

5. After opening: all originals and duplicates resealed separately. Originals handed to PE/PMD → PMD hands originals to PC/BEC Chair. Duplicates in separate secure custody.

6. BOC has no authority to reject any bid at this stage, except late submissions.

7. BOC prepares minutes (Attendance Sheet + Opening Record) and certifies.

### Stage 8 — BEC Assignment

1. PM nominates BEC Chair, Secretary, and members from Eligibility Pool.
2. System validates each nominee:
   - Not Procurement Department staff (**BLOCK** if yes).
   - Not already on 6 active BECs (**BLOCK** if yes).
   - Not the tender's spec_preparer (**BLOCK** if matched).
3. PM records all supporting staff assisting the BEC.
4. System sends COI declaration prompts to all BEC members (Annexure I) and supporting staff (Annexure II).
5. **BLOCK on all evaluation work until all required COI records have `signed_date` set.**
6. BEC Chair approves PTS at the first BEC meeting.

### Stage 9 — BEC Evaluation

See Section 7 for the complete 4-stage evaluation specification.

### Stages 10 — Clarifications

Clarifications are managed continuously throughout Stage 9. See Section 9 for the complete clarification system specification.

### Stage 11 — Report Generation

**Pre-condition:** All evaluation items are Complete (all cells decided, all threads closed).

1. System auto-generates all 14+ tables from evaluation data.
2. AI drafts narrative sections (Introduction, Process Summary, Recommendations, Conclusions).
3. BEC Secretary and Chair review and edit all narrative sections.
4. Full BER preview available.

See Section 10 for the complete report structure.

### Stage 12 — BEC Certification

1. BEC Chair reviews complete report.
2. All BEC members sign (signature table left blank for physical signing; system records digital confirmation).
3. Certified report forwarded to PM.

### Stage 13 — PM Review

1. PM reviews certified BER.
2. PM submits BER to PC Secretary for circulation.
3. If PM identifies omissions: PM may return to BEC for amendment, with documented reasons.

### Stage 14 — PC Final Approval

1. PC Secretary distributes BER to all PC members within 1 week of receipt.
2. PC meets and reviews.
3. PC may: Fully Accept / Partially Accept / Reject (with reasons) / Return to BEC for reconsideration.
4. Disagreements among PC members: majority decision prevails; dissenting views recorded in minutes.
5. PC Chair informs:
   - HLPC/SHLPC/MPC → Secretary to the relevant Ministry.
   - DPC/PPC/RPC → AO / Head of Department / Project Director.

### Stage 15 — Notification of Intention to Award

1. PM sends written notification to **all** bidders (successful and unsuccessful).
2. System sets `standstill_start_date` and computes `standstill_expiry_date`.
3. If standstill exception applies: system records reason and allows skipping to Stage 17.
4. Notification must inform unsuccessful bidders of: right to request debriefing (within 3rd working day), right to appeal (within standstill period), and the correct appeal body.
5. Status → `Standstill`.

### Stage 16 — Standstill Period

1. System displays countdown timer to standstill expiry.
2. PM manages debriefing requests:
   - Validates: request_date ≤ 3rd working day.
   - Schedules debriefing ≤ 5th working day.
   - Records debriefing notes and marks complete.
3. PM manages any appeals:
   - System auto-sets appeal body and cash deposit from pc_type.
   - PM records grounds and tracks outcome.
4. When standstill expires with no blocking appeals: `award_cleared = true` → proceed to Stage 17.
5. If appeal is upheld requiring re-evaluation: system returns to appropriate stage.

### Stage 17 — Contract Award (LOA)

**Pre-conditions:** `award_cleared = true`. If contract > LKR 750 Mn: Cabinet Memorandum with PAC/PAB report required.

1. **Final debarment check:** System checks recommended bidder's `debarment_status`. If Active: **BLOCK** warning displayed. PM must document justification in audit trail to override.
2. PM issues LOA:
   - Must be within bid validity period.
   - Must not introduce new conditions not in Procurement Documents.
   - Must state contract price.
3. System records `loa_date` and `contract_price_lkr`.
4. PM tracks performance security submission. Once confirmed:
   - Bid securities released to all bidders.
   - Formal contract signed (if Works > LKR 500K or Goods/Services > LKR 1 Mn).
5. PM records contract award publication on PE website.
6. If contract > LKR 750 Mn: PM confirms Gazette and General Treasury website publication.
7. Status → `Awarded`.

### Stage 18 — Post-Award Tracking

1. PM enters commencement date, delivery milestones.
2. PM updates delivery/installation/commissioning status.
3. PM issues goods receipt certificate on satisfactory delivery.
4. PM manages variation requests through VRC workflow.
5. PM monitors performance security validity; requests extension if needed before expiry.
6. PM certifies final payment.
7. PM closes contract at end of Defects Liability Period.
8. Status → `Archived`.

---

## 7. Evaluation System — 4 Stages

### 7.1 Overview

The BEC evaluation follows a mandatory 4-stage sequential process. No stage is skippable.

| Stage | Name | Purpose | Can Eliminate Bidders? |
|---|---|---|---|
| 1 | Data Collection | Verify facts; record opening; document completeness | No |
| 2a | Commercial Responsiveness | Assess commercial terms compliance | Yes (Non-Responsive on commercial) |
| 2b | Technical Responsiveness | Assess technical specification compliance | Yes (Major deviation) |
| 3 | Detailed Price Evaluation | Compute evaluated prices; rank bidders | No (but flags unbalanced bids) |
| 4 | Post-Qualification | Verify recommended bidder's capacity | Yes (if PQ fails; move to next) |

### 7.2 Stage 1 — Data Collection

**Sub-stage 1a — Basic Data Sheet**

Auto-populated from tender settings; BEC verifies and confirms:
- Institution, bid number, contract title, procurement method
- Bid closing/opening dates and times
- Number of bids received (on-time, late)
- Bid validity period and expiry date
- Bid security requirement (amount, validity)
- Evaluation criteria (as disclosed in Procurement Documents)
- Whether clarifications, addenda, or a pre-bid meeting occurred

**Sub-stage 1b — BEC Bid Opening Record**

The BEC prepares its own supplementary record (separate from the BOC's official minutes):
- Per bidder: bid security details; bid amounts (total and per package); whether alternative bid submitted; BEC remarks on any notable issues not captured by BOC.
- Notes on any clarifications or discounts noted at opening.
- The BOC's official opening report is attached as reference.

**Sub-stage 1c — Document Completeness Grid**

Grid: documents in rows, bidders in columns. BEC verifies submission status:
- Y = Submitted and complete
- N = Not submitted / incomplete
- NA = Not applicable

BEC verifies NCA sworn status (nca_sworn, sworn_before fields).

**Stage 1 gate:** All verification complete, minutes confirmed. No bidder is eliminated at Stage 1 alone.

### 7.3 Stage 2a — Commercial Terms Responsiveness

Grid: commercial requirements in rows, bidders in columns.

| Cell value | Meaning |
|---|---|
| C | Complied |
| NC | Not Complied |

Remarks column records: the specific deviation and its effect (Major or Minor and monetisable).

Standard commercial requirements (all must be assessed per bidder):

| # | Requirement |
|---|---|
| 1 | Power of attorney / signature authority |
| 2 | Signatures on Form of Bid and Price Schedule |
| 3 | JV confirmation and letter of intent (if applicable) |
| 4 | Bidder eligibility |
| 5 | Goods/services eligibility |
| 6 | Bid validity period meets specified requirement |
| 7 | Bid security — amount |
| 8 | Bid security — validity period |
| 9 | Bid security — correct format |
| 10 | Fixed price contract compliance (if specified) |
| 11 | Payment terms compliance |
| 12+ | Additional requirements from Procurement Documents |

**Conclusion row:** R (Responsive) or NR (Non-Responsive) per bidder.

A bidder marked NR in Stage 2a is eliminated from Stage 3 onwards. Stage 2b may still be partially reviewed for record purposes.

**For SS-2E procedure:** Stage 2a is assessed from Envelope A content for commercial terms included therein. Financial envelope remains sealed.

### 7.4 Stage 2b — Technical Requirements Responsiveness

The full evaluation grid (requirements in rows, bidders in columns). Only Stage-2a Responsive bidders are fully evaluated.

**Cell workflow:**
1. BEC Secretary enters evidence fields (Declaration, Offered Value, Document Reference, Page/Section, Extracted Text — AI pre-populated fields reviewed and confirmed).
2. Maker verifies AI-populated fields. Checker independently verifies Maker's confirmation.
3. Secretary enters Evaluator Notes.
4. Secretary records Decision (Accept or Reject) — subject to BEC approval.
5. If Reject: BEC classifies deviation (Major / Minor / Debatable).
6. If Major: bidder flagged Non-Responsive for item. If Compulsory, Major is auto-set and non-overridable.
7. If Minor: monetary loading entered (LKR; explicitly confirm zero if no impact).
8. If Debatable: cell locked until BEC Chair resolves (Accept-as-Minor or Reject-as-Major with recorded reasoning).

**Substantially Responsive:** A bidder is Substantially Responsive if they passed Stage 2a AND have no Major deviation in Stage 2b. Bidders with only Minor deviations remain Substantially Responsive.

**Item completion gate (all must be true):**
- All cells for the item have final decisions (no Pending)
- All Debatable deviations resolved
- All clarification threads for the item are Closed
- All mandatory information fields (Make, Model, Country of Origin for equipment) verified or resolved via thread

**For SS-2E — Financial Envelope Opening:**
- BEC Chair marks Stage 2b complete after all substantially responsive bidders are identified.
- System unlocks "Open Financial Envelopes" button for BEC Chair only.
- BEC Chair opens Financial Envelopes of Substantially Responsive bidders only at a BEC session.
- Financial envelopes of Non-Responsive bidders returned unopened after contract award.
- Financial amounts recorded by Secretary; read aloud and confirmed.
- Stage 3 proceeds after financial opening.

### 7.5 Stage 3 — Detailed Price Evaluation

See Section 8 for the complete 14-step price adjustment worksheet.

Only Substantially Responsive bidders proceed to Stage 3. At this stage, no bidder is rejected. The purpose is to produce an evaluated price for comparison and determine the ranking.

**Key outputs:**
- Evaluated Price per bidder per item (post all 14 adjustments)
- Final ranking (lowest evaluated price = Rank 1, recommended for Stage 4)

**Tie resolution:** When two or more Substantially Responsive bidders share the lowest evaluated price, the PC/BEC may: (a) split the contract proportionately, (b) request fresh sealed quotations from the tied bidders, or (c) select through a fair draw of lots.

### 7.6 Stage 4 — Post-Qualification

Applied to the lowest evaluated substantially responsive bidder (Rank 1 from Stage 3).

**Step A — Unbalanced Bid Check**

A bid is potentially unbalanced when any unit price is >50% above the average price of other bidders for the same item, AND the cumulative impact of such items exceeds 5% of the total bid price.

| Contract Value | Procedure |
|---|---|
| > LKR 750 Mn | Compute NPV of all unbalanced items using the work programme; add NPV difference to bid price for comparison only |
| ≤ LKR 750 Mn | Request rate analysis and capacity justification from bidder; if acceptable → proceed; if unacceptable → request higher performance security; if bidder refuses → reject (without forfeiting bid security) |

An unbalanced finding does not automatically disqualify. BEC documents findings and remedial action.

**Step B — Post-Qualification Assessment**

For Goods contracts:

| Criterion | Assessment Basis |
|---|---|
| Manufacturer's Authorisation Certificate (MAC) | Verify bidder is authorised to supply these specific Goods |
| After-sales and maintenance capability | Verify maintenance, repair, spare parts capacity |
| Financial capability | Audited financial statements review |
| Relevant past experience | Similar Goods supplied within specified period and quantity |

For Works contracts:

| Criterion | Assessment Basis |
|---|---|
| CIDA Registration | Valid registration at required grade for contract value |
| Similar project experience | Main contractor experience in similar construction (typically last 5–10 years, ≥70% completion) |
| Cash flow arrangements | Evidence of financing for minimum 3-month period |
| Key equipment | Acquisition plan (own, lease, hire) |
| Key staff | Qualifications and experience of contract management team |

**Result:** Pass → recommend for award. Fail → move to next-lowest evaluated Substantially Responsive bidder and repeat PQ. If all bidders fail PQ → BEC recommends re-invitation.

**Debarment check:** At Stage 4, system checks recommended bidder's debarment_status. If Active → BLOCK (see Stage 17).

### 7.7 Single Substantially Responsive Bid

A single substantially responsive bid must be evaluated normally for responsiveness and price reasonableness. Lack of competition is NOT determined solely by the number of bids received. One responsive bid at a reasonable price shall be considered for award.

### 7.8 Rejection of All Bids

Justifiable grounds for rejecting all bids:
- Lack of effective competition (not solely on number of bids)
- No substantially responsive bids
- All prices unreasonably high, unrealistic, or substantially above TCE
- Procurement Documents found to be defective

**Procedure:** PC (with BEC assistance) reviews grounds; BEC/PC recommends remedial actions (revise specifications, scope, conditions, or combination); all bidders informed with reasons; bid securities returned.

**Prohibited:** Bids cannot be rejected and re-invited solely to obtain lower prices. If prices substantially exceed TCE, the TCE must be reviewed first to identify the cause.

---

## 8. Price Adjustment Worksheet — 14 Steps

Applied to each substantially responsive bidder. Produces the evaluated price for comparison.

**CRITICAL RULE:** The evaluated price is for ranking and comparison purposes ONLY. The contract price is the original quoted price minus unconditional discounts. No evaluated price adjustment is reflected in the contract.

### Step 1 — Exclude VAT, Contingencies, and Provisional Sums

Deduct: VAT component; provisional sums; contingency amounts.
Retain: all other taxes and levies.

VAT amounts are stated separately by bidders in the Price Schedule and tracked in `vat_amount` field. The system auto-deducts based on this field.

### Step 2 — Correction of Arithmetical Errors

**Priority rules:**
1. Amount in words vs. figures conflict → **words prevail**.
2. Unit rate × quantity ≠ line-item total → **unit rate prevails**.
   - Exception: if there is an obvious gross misplacement of the decimal point in the unit rate → line-item total prevails; unit rate is corrected.
3. Lump Sum contracts: total bid price vs. column-price sum discrepancy → **column price prevails**.

After correction: the adjusted amount is binding on the bidder.

BEC must provide detailed explanation in the BER for each arithmetical correction.

If a bidder's price changes after correction, the PM notifies the bidder of the corrected amount in writing. The bidder must confirm acceptance. If the bidder does not confirm, their bid is rejected (without forfeiting bid security).

### Step 3 — Application of Unconditional Discounts

**Valid discounts:** Only discounts announced at bid opening. If a discount was not read due to BOC oversight and the bidder requests it at opening, BOC reads it — then it is valid.

**Conditional discounts:** Ignored entirely.

**Application of unconditional discounts:**
- Item-specific discount → applied to those items only.
- Percentage of total → applied proportionately to all items.
- Lump-sum of total → distributed proportionately across all items.
- Cross-discounts (multi-lot) → applied per bidder's stated terms.

### Step 4 — Adjustment for Omissions / Missing Items

If a bid omits certain items but is otherwise substantially responsive:
- Price the missing items using: average of other bidders' prices for the same item, OR prevailing market prices.
- The adjusted amount is for evaluation comparison only.
- If this bidder is selected, they remain obligated to supply missing items within their original quoted price.

Only applicable when: (a) missing items are not critical to the procurement, and (b) their values are insignificant relative to total.

### Step 5 — Adjustment for Delivery Period Deviations

If a bidder proposed a delivery period different from that specified:
- Quantify the deviation in monetary terms (additional storage, insurance, delays, etc.) as specified in Procurement Documents.
- No advantage is given for early delivery.
- If the price adjustment required for late delivery exceeds **10% of the bid price**, the bid is treated as Non-Responsive at Stage 2 (not here in Stage 3).

### Step 6 — Adjustment for Minor Deviations

Load the `monetary_loading_lkr` values from all Stage 2b evaluation cells where `deviation_type = Minor` or `deviation_type = Debatable` with `debatable_resolution = Accept-as-Minor`.

System auto-links these from Stage 2b. BEC confirms each amount is correct.

### Step 7 — Adjustment for Inland Transportation

Applicable only for imported Goods on CIF/FOB basis where bidder omitted inland transport to project site. The cost for local handling and inland transportation from port-of-entry to project site is estimated at prevailing market rates and added.

Not applicable for: Works contracts; Goods on EXW basis (no entitlement to inland transport); Goods on DDP basis (already included).

### Step 8 — Operational Costs and Life Cycle Costing

Applicable only when `tender.lcc_required = true` and the methodology is specified in the Procurement Documents.

**LCC components:**
- Initial purchase price (already in quoted price)
- Operational cost
- Maintenance cost
- Disposal cost

**Method:** Calculate Net Present Value (NPV) for each component using the methodology and discount rate specified in the Procurement Documents. The NPV difference between bidders is added/deducted.

### Step 9 — Conversion to Common Currency

For bids submitted in foreign currencies, convert all amounts to Sri Lankan Rupees.

**Exchange rate:** CBSL (Central Bank of Sri Lanka) weighted average selling rate.

**Reference date:** The bid closing day, OR 28 days prior to the bid opening — whichever is specified in the Procurement Documents (the 28-days-prior date is preferred when specified). The PM enters the applicable CBSL rate(s) for each currency at the correct reference date.

### Step 10 — Application of Domestic Preference

Applicable only when `tender.domestic_preference_applicable = true` AND domestic preference was stated in the Procurement Documents.

**The preference loading is added to the non-entitled group's price — NOT subtracted from entitled group's price.**

**Goods:**

| Funding | Preference Loading | Applied To |
|---|---|---|
| GOSL-funded | 20% of CIF bid price | Foreign/non-entitled bids for comparison |
| Foreign-funded (FFA) | As per loan/credit agreement (< 15%) | Foreign/non-entitled bids for comparison |

**Eligibility for preference (Goods — "preference entitled" group):**
- ≥ 30% domestic value addition (Sri Lankan labour + locally sourced raw materials) of EXW price
- Registered under Companies Act No. 7 of 2007
- > 50% owned by Sri Lankan nationals
- Affidavit declaring value addition ≥ 30% of EXW price
- Certified audited financial statements confirming value addition

"Preference not entitled" group includes: Sri Lankan-made goods with < 30% value addition, and all imported goods.

**Works:**

| Funding | Preference Loading | Applied To |
|---|---|---|
| GOSL-funded | 15% | Foreign/non-entitled bids for comparison |
| Foreign-funded (FFA) | As per loan/credit agreement (< 10%) | Foreign/non-entitled bids for comparison |

**Works domestic eligibility:**
- Individual/sole proprietor: must be Sri Lankan.
- Partnership: > 50% Sri Lankan ownership.
- Company: registered in Sri Lanka + > 50% Sri Lankan ownership + ≤ 10% subcontracted to foreign contractors.
- JV: all constituent firms meet the above criteria + JV registered in Sri Lanka.

**Process:** System classifies bidders into entitled/not-entitled groups based on domestic preference data in bidder profiles. PM confirms classification. System computes the loading for comparison purposes. Award price is the original evaluated price — the preference loading has no effect on contract price.

### Step 11 — Re-rank

After all Steps 1–10: compute total adjustments and evaluated price for each substantially responsive bidder. Rank from lowest to highest evaluated price.

**System output:** Ranking table — Bidder | Quoted Price | Total Adjustments | Evaluated Price | Rank.

Rank 1 (lowest evaluated price) proceeds to Stage 4 Post-Qualification.

### Step 12 — After-Sales Services

Applicable only if after-sales service is an evaluation criterion specified in the Procurement Documents.

**Method (as specified in Procurement Documents):** Point-based scoring of factors such as proximity of service facilities, number and expertise of available staff, spare parts stock levels, service duration. Points converted to monetary equivalent per the methodology in the Procurement Documents.

### Step 13 — Unrealistic Bid Price Assessment

**For Goods/Services:** If the lowest evaluated price is substantially different from the TCE:

The BEC assesses reasonableness based on:
- Last purchase price accepted as reasonable
- Prevailing market price from market survey or published indexes
- Price of similar/equivalent Goods
- Component estimate (for machinery: assembling parts separately)

A bid is not rejected solely because it exceeds the TCE.

**For Works:** A bid is not rejected solely because it is above or below the engineer's estimate. If marginally low: request rate analysis; if explanation is unacceptable and bidder refuses increased performance security → reject without forfeiting bid security.

Unbalanced unit prices (>50% above average AND impact >5% of total) are formally flagged here and addressed in Stage 4 Step A.

### Step 14 — Clarification Price Effects

If any clarification obtained during evaluation has a price effect (e.g., confirming a discount or resolving a price ambiguity), document the effect here.

**Clarifications must NOT modify contract price, delivery period, conditions of contract, specifications, or any other substantive element of the bid.** This restriction is displayed in the clarification drafting interface.

---

## 9. Clarification System

### 9.1 Clarification Thread — One Type Only

The National Procurement Manual 2024 (Chapter 07) permits BEC to seek clarification **from bidders only**, through the PE (PM). Direct BEC-to-bidder correspondence is not permitted. The Manual does not provide for internal clarification from equipment requestors or specification preparers; in fact, those officers are excluded from evaluation involvement under COI Ground (b) (§2.17.2).

**The only clarification thread type:**

| Type | Purpose | Direction |
|---|---|---|
| **Bidder Clarification** | Seek clarification from a bidder on their submission | BEC Secretary drafts → BEC Chair approves → PM sends to bidder → bidder responds → PM enters response → BEC reviews and closes |

**External Technical Expert Advisory (separate workflow — not a clarification thread):**

The Manual permits BEC to obtain advice from external technical experts when the BEC's collective expertise is insufficient to evaluate a specific technical matter. This is **not** a clarification to a bidder; it is a formal consultation with an independent external expert.

| Requirement | Rule |
|---|---|
| PC approval | Required before engaging any external expert |
| Expert independence | The external expert must have no COI with any bidder and cannot be the tender's specification preparer |
| Advisory nature | Expert's opinion is recorded and considered by BEC; BEC Chair's decision is final |
| Documentation | Expert's credentials, opinion, and PC approval are recorded in the audit trail and included in the BER |

> **Removed from this version:** "Internal Technical Consultation" (advisory from requestor/user department) has been removed as it has no basis in the National Procurement Manual 2024. Requestor/user department involvement in evaluation would constitute a conflict of interest under COI Ground (b) if they prepared the specifications.

### 9.2 Two Levels of Threads

| Level | Scope | Cell Effect |
|---|---|---|
| Requirement-level | Specific requirement (one cell) | Sets `cell.active_clarification_thread_id` → BLOCKS that cell's decision |
| Item-level | Entire item (general query) | Does not block individual cells |

### 9.3 Clarification Restrictions

Clarification requests must NOT involve:
- Modification of the contract price
- Changes to delivery period
- Modification of conditions of contract
- Changes to technical specifications
- Any other substantive alteration to the bid

The system displays this restriction prominently in the clarification drafting screen.

### 9.4 Thread Workflow

1. Secretary creates thread (type, level, linked requirement or item).
2. Secretary drafts message. AI fills standard template placeholders (bidder name, tender number, requirement reference). BEC Secretary writes the substantive question content — AI cannot write this.
3. BEC Chair reviews and approves (or returns for revision).
4. PM sends to bidder/requestor.
5. PM enters response.
6. BEC reviews response.
7. BEC closes thread: **Satisfied** or **Not-Satisfied**.
8. Requirement-level threads: `cell.active_clarification_thread_id` cleared; cell decision is unblocked.

### 9.5 Mandatory Information Fields

For equipment procurement, Make, Model, and Country of Origin are mandatory. If not provided in a bid:
- System flags the deficiency automatically.
- A clarification thread must be opened.
- Thread closure requires Satisfied or Not-Satisfied.
- **Not-Satisfied = Non-Responsive for the item.**

### 9.6 Multiple Rounds

A thread supports multiple back-and-forth messages. The thread remains Open until formally closed. Each additional message round requires Chair approval before PM sends.

---

## 10. Report Generation

### 10.1 BER Structure — 15 Sections

| # | Section | Source |
|---|---|---|
| 1 | Cover Page | Auto: tender details, committee names, date |
| 2 | Introduction | AI-drafted + manual edit: general intro, bid submission, bid opening summary, basic data, BEC composition |
| 3 | Package Summary | Auto: packages, lots, items table |
| 4 | Items Table | Auto: item descriptions, quantities, units |
| 5 | Process Summary | AI-drafted + manual edit: timeline, meetings, clarifications issued, addenda |
| 6 | Suppliers List | Auto: all bidders who submitted (name, country, basic details) |
| 7 | Bid Opening | Auto tables: BOC attendance + BOC minutes; BEC's opening record (Stage 1b output) |
| 8a | Examination of Document Completeness | Auto: Y/N/NA grid (Stage 1c output) |
| 8b | Commercial Terms Responsiveness | Auto: C/NC grid with R/NR conclusion (Stage 2a output) |
| 8c | Eligibility Assessment | Auto: eligibility findings per bidder |
| 9 | Technical Compliance | Per-item: evaluation grid (Accept/Reject/deviation type per requirement per bidder); deviation summary table below each item |
| 10 | Price Schedules | Per substantially responsive bidder: quoted price schedule; 14-step Price Adjustment Table; ranking summary |
| 10a | Post-Qualification | Checklist per PQ criterion for recommended bidder (and next bidder if first failed) |
| 11 | Recommendations | Auto-structured + AI-drafted narrative: recommended bidder, evaluated price, ranking, PQ result |
| 12 | Clarifications | Auto: all clarification threads, status, closure decisions |
| 13 | Conclusion | AI-drafted + manual edit |
| 14 | Signature Table | Auto: names and designations; left blank for physical signing |

**5 Auto-generated Annexures:**

| Annexure | Content |
|---|---|
| I | Pre-bid meeting minutes |
| II | Clarifications register (all threads, all rounds) |
| III | Bid opening record |
| IV | Price schedules (full detail all bidders) |
| V | Document checklist (all bidders) |

### 10.2 Key Formatting Rules

1. **Monetary amounts:** Written as `[Amount in words] (LKR [amount in figures])` — e.g., "Sri Lanka Rupees Twelve Million Five Hundred Thousand (LKR 12,500,000.00)".
2. **Deviation notes:** Appear immediately below the relevant compliance table for each deviation found.
3. **Rejected bidder tracking:** Each eliminated bidder is shown at the stage and step of elimination.
4. **Signature table:** Left blank for physical signing. System records digital certification separately in audit trail.
5. **Export:** Word (.docx) and PDF.
6. **Template editor:** Built-in editor with placeholder system for customising narrative templates.

### 10.3 Price Adjustment Table Format

Columns: Step | Step Name | Bidder 1 | Bidder 2 | … (only substantially responsive bidders)

Row values: ± LKR amounts for each of the 14 steps

Final rows:
- Total Adjustments (sum of all steps)
- Quoted Price (price_schedule total − VAT + discounts)
- **Evaluated Price** (Quoted Price + Total Adjustments)
- **Rank** (1 = lowest evaluated price = recommended)

### 10.4 Domestic Preference Table Format (when applicable)

Columns: Bidder | Origin | Group (Entitled/Not-Entitled) | Evaluated Price (pre-preference) | Domestic Preference Loading | Adjusted Price for Comparison | Rank after Preference

---

## 11. AI Features

### 11.1 Core Principle

AI assists evaluation but never decides. Every AI output is explicitly marked as a draft. No AI action is final without human confirmation. Every AI output is logged in the audit trail.

### 11.2 Document Extraction

AI processes uploaded documents (bidder quotations, tender documents) using OCR and RAG to extract text.

**Outputs:** Pre-populates evaluation cell fields (Offered Value, Document Reference, Page/Section, Extracted Text) and requirement descriptions (from tender documents).

**Maker-Checker verification:** Two distinct persons must verify each AI extraction:
- **Maker:** Reviews and marks AI content as verified (`ai_verified_by`).
- **Checker:** Independently confirms the Maker's verification (`ai_second_verified_by`).
- The same person cannot serve as both Maker and Checker for the same cell.

### 11.3 Compliance Analysis

AI highlights relevant text in documents related to a specific requirement.

**Rule:** AI highlights only — it does NOT suggest Accept or Reject. The system must never display any AI output that implies a compliance recommendation ("this appears to comply", "this does not meet the specification", or similar). Any such output is a system defect.

### 11.4 Clarification Drafting

AI fills standard template placeholders: bidder name, tender number, item description, requirement reference number. AI does NOT write the substantive content of the query. The BEC Secretary writes the substantive question.

### 11.5 Report Narrative Drafting

AI drafts narrative text for: Introduction, Process Summary, Recommendations, Conclusions.

All AI-drafted sections are clearly labelled as drafts. The system preserves the original AI draft alongside the human-edited version in the audit trail. BEC Secretary and Chair must review and edit all narratives before the report can be certified.

### 11.6 Specification Library Suggestions

When the PM adds requirements to a new tender item, AI suggests relevant specifications from the Specification Library based on the item description and contract type. PM selects from suggestions — AI does not auto-add.

### 11.7 Prohibited AI Actions (System Must Never Do These)

1. Output an automatic Accept or Reject decision for any evaluation cell.
2. Display any text implying a compliance recommendation.
3. Send communications to bidders or requestors.
4. Modify evaluation decisions.
5. Access TCE value before bid closing.
6. Draft clarification content that would modify specifications, contract terms, or bid price.
7. Produce a report section without retaining the original AI draft in audit trail.

---

## 12. User Interface Specification

### 12.1 Navigation Structure

**PM Dashboard (default landing):**
- Active tenders as summary cards: title, status, next action, deadline countdown.
- Quick access: New Tender, Pending Clarifications, Post-Award Items.
- System-wide notifications.

**Tender Detail View:**
- Left sidebar: 18-stage progress indicator (current stage highlighted, completed stages checked).
- Main area: context-specific panel for the current stage.
- Right panel: recent audit events for current stage.

### 12.2 Project Setup (Stage 1)

**Basic Info panel:** Title, number, description, contract type, tender type, bidding procedure, PC type, funding source, TCE (masked until bid opening), tce_prepared_by picker, spec_preparer picker.

**Dates panel:** Publication date, pre-bid date, bid closing datetime, bid opening datetime, bid validity days. Computed display: bid validity expiry, bid security validity deadline. Warning banners for minimum bidding period and recommended bid validity period.

**Bid Security panel:** Percentage entry (1–2% range), computed LKR amount, accepted security types (checklist), BSD availability indicator (hidden if TCE ≥ LKR 20 Mn).

**Items and Requirements panel:** Hierarchical view — Packages → Items → Requirements. Inline C/Non-C toggle for each requirement. Specification Library search.

### 12.3 Bidders View

- List view: all registered bidders with submission status, responsiveness, rank, and debarment badge.
- Add bidder: search Bidder Database or create new.
- Record document purchase date.
- Per-bidder expanded view: Document Checklist; Bid Security; Price Schedule.

### 12.4 BOC Bid Opening View

- Session controls (start/end).
- Attendance recording.
- Per-bidder row:
  - Withdrawal: authenticated (checkbox)
  - Modification: received (checkbox)
  - Bid security: present (checkbox), issuer (text), foreign confirmation (checkbox)
  - Announced amount: total + per-package
  - Discounts: announced (checkbox), details (text)
  - Alternative bid (checkbox)
  - BOC tape applied (checkbox)
  - BOC initials confirmed (checkbox)
  - Remarks
- Minutes preview and certification button.

### 12.5 Evaluation View (Full-Screen Session Mode)

**Full-screen layout for projector use.** Left panel: requirement rows (with C/Non-C indicator, colour-coded by decision status). Top panel: bidder columns. Central area: grid cells.

**Stage strip (horizontal tabs):** Stage 1 Data | Stage 2a Commercial | Stage 2b Technical | Stage 3 Price | Stage 4 Post-Qual.

For SS-2E tenders: Stage 3 and Stage 4 tabs are locked with a padlock icon until BEC Chair performs Financial Envelope Opening.

**Colour coding:**
- Green cell: Accept
- Red cell: Reject
- Grey cell: Pending
- Orange border: active clarification thread on this cell

**Price Visibility Toggle (BEC Chair only):** Default OFF (prices hidden). In Stage 3: always ON. In SS-2E: locked OFF until Financial Envelope Opening.

**Cell expanded view on click:**
- Declaration (Yes/No/Pending dropdown)
- Offered Value (text; "AI" badge if AI-populated; Maker/Checker status shown)
- Document Reference (text; AI badge)
- Page/Section (text; AI badge)
- Extracted Text (expandable text area; AI badge)
- Evaluator Notes (text; always manual)
- Decision (Accept / Reject buttons)
- If Reject: Deviation Type selector (Major / Minor / Debatable)
- If Minor: Monetary Loading (LKR; "Confirm zero impact" button if zero)
- If Debatable: Chair Resolution panel (Accept-as-Minor / Reject-as-Major + reasoning text)
- Linked clarification thread (if any; link to thread panel)
- Maker verification button; Checker verification button

#### Stage 1 Sub-tabs

**1a Basic Data:** Form view; auto-populated; BEC confirms each field.

**1b Opening Record:** Table (bidder rows × data columns); Secretary enters BEC observations.

**1c Document Completeness:** Grid (document rows × bidder columns); Y/N/NA toggles; NCA sworn verification fields.

#### Stage 2a Commercial

Grid: commercial requirements × bidders. C/NC toggles. Remarks per cell. Conclusion row: R/NR (auto-computed; manual override with reason).

#### Stage 2b Technical

Full evaluation grid as described above.

#### Stage 3 Price Adjustment

14-row worksheet. Columns: Step Number | Step Name | Bidder 1 | Bidder 2 | …

Per cell: +/– LKR entry + Basis text.

Step 1 (VAT exclusion) and Step 9 (currency) partially auto-populated from price schedule data.

Step 6 auto-linked from Stage 2b deviation loadings.

Summary rows: Quoted Price | Total Adjustments | Evaluated Price | Rank (auto-computed).

#### Stage 4 Post-Qualification

Activated for Rank 1 bidder.

Two sub-sections:
- **Unbalanced Check:** Auto-flag if any unit price >50% above average AND impact >5% of total; BEC documents assessment and remedial action.
- **PQ Criteria:** Checklist table — Criterion | Required | Evidence | Status (Pass/Fail/NA) | Notes. Goods: MAC, maintenance capability, financial capability, past experience. Works: CIDA, similar experience, cash flow, equipment, staff.

If Fail: "Move to next bidder" button. System archives the failed PQ result and activates Stage 4 for the next-lowest ranked bidder.

### 12.6 Clarification Panel

Side panel accessible from any evaluation stage.

Single tab: **Bidder Clarifications** (Internal Technical Consultations removed — see §9.1).

Thread list with status badges. Thread detail: full message history, current status, closure decision.

Draft message area: template placeholder auto-fill by AI; substantive content typed by Secretary. Restriction notice displayed prominently:
> "Clarifications must not modify bid price, delivery period, conditions of contract, technical specifications, or any substantive aspect of the bid."

Approval status: Draft → Awaiting Chair Approval → Approved → Sent → Response Received → Closed.

**External Technical Expert Advisory** (separate section below thread list): Lists any PC-approved expert consultations for this item. Fields: Expert name, credentials, PC approval reference, opinion summary, date. Read-only in this panel; managed from the Evaluation Session Controls.

### 12.7 Report Preview

Live preview of full BER (all 15 sections + 5 annexures).

Editable narrative sections with change tracking vs. original AI draft.

Tables are read-only (from evaluation data; cannot be edited directly).

Section-by-section certification checklist (Chair confirms each section).

Export: Word (.docx) | PDF.

### 12.8 Financials View

Price schedule data entry. Evaluated Price Summary (read-only; Stage 3 output). Unbalanced bid alert. Currency conversion tracker (CBSL rates per currency per reference date).

### 12.9 Pre-Bid Meeting View

Meeting details. Document Purchasers Registry (separate list). Q&A table. Addenda tracking. System warning if meeting is < 10 days before closing (for >LKR 50 Mn tenders).

### 12.10 Standstill View

Countdown timer to standstill expiry. Exception status display. Debriefing requests management with date validation. Appeals management with auto-populated appeal body and deposit amount. Award Cleared indicator.

### 12.11 Post-Award Tracking View

Delivery milestones with status updates. Installation / commissioning status. Performance security tracker with expiry warning. Contract variations log (link to VRC workflow). Goods receipt certificate. Final payment and closure.

### 12.12 Specification Library

Browse by category. Keyword search. Per-spec: description, C/Non-C flag, standard values, usage history. Import into active tender item. Add new specifications.

### 12.13 Bidder Database

Organisation-wide search (name, registration number, country). Bidder profile: participation history, performance notes. Debarment tab: full debarment history. Active debarment warning banner.

### 12.14 Audit Trail View

Chronological event log. Filters: event type, user, date range, stage. All event fields displayed (timestamp, user, role, event type, before/after state, session ID). Export to CSV and PDF.

---

## 13. Session and Access Control

### 13.1 BEC Sessions

A BEC Session must be active for any evaluation data entry. Outside active sessions, all BEC evaluation views are read-only for all roles.

**Session start:** Secretary opens Session Controls; records session type and attendees; system activates write mode for Secretary.

**Temporary write grants:**
- Secretary can grant write access to a specific BEC member for a specific cell.
- Grant is logged: {user_id, cell_id, granted_at}.
- Secretary revokes at any time; all grants auto-revoke at session end.

**Session end:** Secretary ends session; system records end time; all write access revoked; session summary logged.

### 13.2 Price Visibility

| Context | Visibility State |
|---|---|
| Stage 2b (default) | OFF (prices hidden) — BEC Chair can toggle ON |
| Stage 3 | Always ON (prices required for price adjustment) |
| SS-2E before Financial Envelope Opening | Always OFF — cannot be toggled |
| SS-2E after Financial Envelope Opening | Follows normal Stage 3 rules |

### 13.3 Authentication

- Username and password authentication.
- Configurable session timeout on inactivity.
- All login, logout, and failed login events logged in audit trail.

---

## 14. Audit Trail

### 14.1 Immutability

The audit trail is immutable. No entry can be deleted or modified. The trail is forensic-grade — exportable for official auditors.

### 14.2 Event Record Structure

Every event logged with:
- Timestamp (precise)
- User ID, user name, role at time of event
- Event type and category
- Before-state (where applicable)
- After-state
- Session ID (if within a session)
- Additional context (tender_id, cell_id, thread_id, etc.)

### 14.3 Event Categories

| Category | Examples |
|---|---|
| Tender lifecycle | Created, status changed, stage advanced, stage returned |
| Evaluation | Cell decision set, deviation classified, Debatable resolved, item completed |
| AI events | Extraction performed, draft generated, Maker verification, Checker verification |
| Clarification | Thread created, message drafted, Chair approved, PM sent, response entered, thread closed |
| COI | Declaration signed, conflict declared, recusal actioned, prior decisions flagged |
| Session | Started, ended, write grant issued, write grant revoked, attendee joined/left |
| Role/assignment | BEC assigned, eligibility check result (including block reasons), nomination blocked |
| Debarment | Debarment check performed, alert displayed, override recorded |
| Report | Generated, section edited (with diff), report certified |
| Award | Notification sent, standstill started, debriefing scheduled, appeal received, LOA issued |
| Post-award | Milestone updated, variation requested, VRC recommendation, variation approved |
| System | User created, role changed, login, logout, failed login attempt |

---

## 15. System Administration

### 15.1 User Management

- Create user accounts with role assignment.
- Deactivate accounts (preserves all audit history).
- Reset passwords.
- View user activity log.
- Change role assignments (logged in audit trail).

### 15.2 BEC Eligibility Pool

- Maintain list of officers approved for BEC service.
- Each entry: user account, department, expertise areas.
- Computed display: current active BEC count per officer.
- Cannot include Procurement Department role holders.
- Admin adds/removes officers; removal does not affect existing assignments.

### 15.3 System Configuration

| Setting | Notes |
|---|---|
| Organisation name and logo | Appears on all reports |
| Default PC type | Pre-fills tender creation |
| Working days calendar | Used for standstill and deadline computations; excludes weekends and gazetted public holidays |
| Bid validity period tables | Referencing Section 2.5; configurable if NPC updates |
| Performance security percentages | 10% Goods / 5% Works/Services; configurable |
| Currency code | LKR default |
| Document purchaser fee ranges | Reference table from Chapter 06 of the Manual |

### 15.4 Offline Mode

- All evaluation work is available offline using local cache.
- Changes made offline are queued and synced on reconnection.
- **Conflict resolution:** If two users edited the same field offline, the system flags the conflict for manual resolution. Evaluation decisions always require manual resolution — no automatic overwrite.

---

## 16. Validation Rules

### 16.1 BLOCK Rules — System Prevents Action

| Rule ID | Trigger Condition | Blocked Action |
|---|---|---|
| V-CMTE-01 | BEC nominee is Procurement Department staff (PM/TPE/PA) | BEC assignment |
| V-CMTE-02 | BEC nominee already on 6 active BECs | BEC assignment |
| V-CMTE-03 | BEC nominee user_id = tender.spec_preparer_id | BEC assignment |
| V-COI-01 | Not all BEC members have signed COI Annexure I | Any evaluation stage |
| V-COI-02 | Not all supporting staff have signed COI Annexure II | Any evaluation stage |
| V-EVAL-01 | cell.active_clarification_thread_id is non-null | Cell decision entry or change |
| V-EVAL-02 | Debatable deviation pending resolution | Item marked Complete |
| V-EVAL-03 | Any cell Pending | Item marked Complete |
| V-EVAL-04 | Any clarification thread not Closed | Item marked Complete |
| V-EVAL-05 | Compulsory (C) requirement + Reject | Change deviation_type away from Major |
| V-DOCS-01 | tender.tce_value ≥ LKR 20,000,000 | security_type = BSD selection |
| V-TCE-01 | tce_prepared_by = pm_id | Tender configuration save |
| V-SS2E-01 | SS-2E mode: Financial Envelope Opening not performed | Stage 3 tab access |
| V-AWRD-01 | award_cleared ≠ true | LOA issuance |
| V-AWRD-02 | Recommended bidder has debarment_status = Active (override required with documentation) | LOA issuance (soft block; requires documented override) |
| V-MINOR-01 | deviation_type = Minor AND monetary_loading_lkr not confirmed | Stage 3 proceed for this bidder |

### 16.2 WARN Rules — System Warns, User Can Proceed

| Rule ID | Trigger Condition | Warning Message |
|---|---|---|
| W-TNDR-01 | Computed bidding period < minimum for selected procurement method | "Bidding period is below the minimum required for [method]" |
| W-TNDR-02 | Bid validity days < recommended minimum for contract type and value | "Bid validity is below the recommended minimum for this procurement" |
| W-TNDR-03 | bid_security_min_pct outside 1–2% | "Bid security should be 1–2% of TCE per Procurement Manual" |
| W-PREBID-01 | Pre-bid meeting date < 10 calendar days before bid closing AND TCE > LKR 50 Mn | "Pre-bid meeting must be ≥10 days before closing for procurements over LKR 50 Mn" |
| W-ADDN-01 | Addendum distribution_date < 3 working days before bid closing | "Addendum should reach document purchasers ≥3 working days before bid closing" |
| W-BIDR-01 | Bidder with active debarment added to tender | "This bidder has an active debarment. Proceeding is your responsibility. This action is logged." |
| W-PRICE-01 | Any unit price >50% above average AND cumulative impact >5% of total bid | "Unbalanced bid pricing detected — formal assessment required at Stage 4" |
| W-AWRD-01 | Bid validity expiry approaching during evaluation | "Bid validity expires in [X] days. Consider requesting extension before expiry." |
| W-AWRD-02 | Debarment override recorded at LOA | "Active debarment override is logged in the audit trail with your credentials." |

### 16.3 INFO Rules — Informational Display

| Rule ID | Trigger | Information Displayed |
|---|---|---|
| I-TNDR-01 | PC type selected | "Monetary threshold for [PC type]: GOSL [X] / Foreign [Y]" |
| I-TNDR-02 | Contract type and TCE entered | "Recommended minimum bid validity: [X] calendar days" |
| I-TNDR-03 | Tender type and bidding period computed | "Minimum required bidding period: [X] calendar days for [method]" |
| I-BSEC-01 | Bid security amount computed | "Bid security range: LKR [min] to LKR [max] (1–2% of TCE)" |
| I-EVAL-01 | Minor deviation loading confirmed as zero | "Confirmed: no financial loading for this deviation" |
| I-SS2E-01 | Stage 2b complete in SS-2E mode | "Technical evaluation complete. BEC Chair may now open Financial Envelopes." |
| I-STND-01 | Standstill started | "Standstill period: [start date] to [expiry date]. Debriefing requests accepted until [3rd working day]. Appeal deadline: [expiry date]." |
| I-STND-02 | Appeal body auto-set | "Appeals for [PC type] procurements are handled by [appeal body]. Non-refundable deposit: LKR [amount]." |
| I-CONT-01 | LOA issued | "Formal contract required if value > LKR 500,000 (Works) or LKR 1,000,000 (Goods/Services)." |

---

## 17. Glossary

| Term | Definition |
|---|---|
| AO | Accounting Officer; head of a department-level Procuring Entity |
| BEC | Bid Evaluation Committee; evaluates technical and financial aspects of bids |
| BER | Bid Evaluation Report; the formal output of the BEC |
| BOC | Bid Opening Committee; witnesses and records the public bid opening |
| BSD | Bid Securing Declaration; acceptable for contracts with estimated value < LKR 20 Mn |
| CAO | Chief Accounting Officer; head of a Ministry-level Procuring Entity |
| CBSL | Central Bank of Sri Lanka |
| CIDA | Construction Industry Development Authority |
| COI | Conflict of Interest |
| Contract Price | The original quoted price minus unconditional discounts; the price at which the contract is signed. Never equals the evaluated price. |
| DC | Direct Contracting |
| DPC | Department Procurement Committee; threshold ≤ LKR 400 Mn (GOSL) |
| Debatable Deviation | A deviation that cannot be clearly classified as Major or Minor without context; BEC Chair must resolve explicitly |
| Evaluated Price | Quoted price plus all 14 standard adjustments; used for comparison and ranking ONLY — not the contract price |
| FFA | Foreign Funding Agency (e.g., World Bank, ADB) |
| HLPC | High Level Procurement Committee; threshold > LKR 750 Mn (GOSL) |
| ICB | International Competitive Bidding |
| IFB | Invitation for Bids |
| INCOTERM | International Commercial Term of Carriage (EXW, FOB, CIF, CIP, DDP) |
| External Technical Expert Advisory | Advisory consultation with an independent external expert, approved by PC; the expert's opinion is recorded but BEC Chair's decision is final; the requestor/spec preparer cannot serve as the expert (COI Ground (b)) |
| ITB | Instructions to Bidders |
| LCC | Life Cycle Costing |
| LIB | Limited International Bidding |
| LKR | Sri Lanka Rupee |
| LNB | Limited National Bidding |
| LOA | Letter of Acceptance; the formal contract award instrument |
| MAC | Manufacturer's Authorisation Certificate |
| Major Deviation | A departure that materially affects scope, quality, or performance, or cannot be financially quantified; causes Non-Responsive determination |
| Minor Deviation | A deviation accepted with a monetary loading for price comparison; bidder remains Substantially Responsive |
| MPC | Ministry Procurement Committee; threshold ≤ LKR 750 Mn (GOSL) |
| NCB | National Competitive Bidding |
| NCA | Non-Collusion Affidavit; must be SWORN before a Justice of the Peace or Commissioner of Oaths — not merely signed |
| NPC | National Procurement Commission, Sri Lanka |
| NPV | Net Present Value |
| PA | Procurement Assistant |
| PAB | Procurement Appeal Board; handles HLPC/SHLPC procurement appeals |
| PAC | Procurement Appeal Committee (collective term for MPAC/DPAC/PPAC/RPAC) |
| PC | Procurement Committee |
| PE | Procuring Entity |
| PM | Procurement Manager |
| PMD | Procurement Management Division |
| Post-Qualification | Assessment of the lowest evaluated Substantially Responsive bidder's capacity before award recommendation |
| PPC | Project Procurement Committee; threshold ≤ LKR 400 Mn (GOSL) |
| PTS | Procurement Time Schedule |
| RAG | Retrieval-Augmented Generation; AI method for extracting document content |
| RFQ | Request for Quotations (Shopping procedure) |
| RPC | Regional Procurement Committee; threshold ≤ LKR 50 Mn (GOSL) |
| SHLPC | Standing High Level Procurement Committee; threshold > LKR 750 Mn (GOSL) |
| Spec Preparer | The officer who prepared the technical specifications for a tender; excluded from serving on the BEC for the same procurement |
| SPD | Standard Procurement Document |
| SS-1E | Single-Stage One-Envelope bidding procedure |
| SS-2E | Single-Stage Two-Envelope bidding procedure |
| Standstill Period | Mandatory ≥ 10 working days between the Notification of Intention to Award and LOA issuance |
| Substantially Responsive | A bid that passes Stage 2a (commercial) and Stage 2b (technical) with no Major deviations |
| TCE | Tender Cost Estimate; confidential PE estimate; never shared with bidders |
| TPE | Trainee Procurement Executive |
| VRC | Variation Review Committee; reviews post-award contract variations before approval |
| 2S | Two-Stage bidding procedure |

---

*End of TenderStudio System Specification v5.0*

*This document is entirely self-contained. All rules, validation logic, data entities, workflows, and UI requirements are defined herein. A software development team can build the complete TenderStudio system from this document alone, without reference to any companion documents.*
