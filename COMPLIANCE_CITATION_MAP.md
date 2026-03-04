# COMPLIANCE CITATION MAP
## TenderStudio v3.0 — National Procurement Manual 2024 (Sri Lanka)
### Goods, Works and Non-Consulting Services

**Version**: 1.0
**Date**: 2026-02-26
**Manual Reference**: National Procurement Manual 2024 (Goods, Works and Non-Consulting Services), effective 01.01.2025
**Spec Reference**: TenderStudio SYSTEM_SPECIFICATION.md v3.0

---

## How to Read This Document

| Column | Description |
|--------|-------------|
| Rule ID | Unique identifier: CH[chapter]-[sequence] |
| Manual Citation | Chapter, section, and annexure reference |
| Rule Text | Exact or paraphrased rule from the manual |
| TenderStudio Feature | System feature that implements this rule |
| Screen / Field | Specific screen and/or field name |
| Status | IMPLEMENTED / GAP / PARTIAL / N/A |

**Status Definitions**:
- **IMPLEMENTED**: Fully covered in v3.0 spec
- **PARTIAL**: Partially addressed; missing detail or edge cases
- **GAP**: Not in spec; needs to be added
- **N/A**: Not applicable to software (physical process, legal obligation of humans, etc.)

---

## CHAPTER 01 — ETHICS, CONFLICT OF INTEREST, AND PROHIBITED PRACTICES

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH01-001 | Ch 01 §1.1 | All procurement officers must adhere to a code of ethics encompassing integrity, transparency, impartiality, accountability, and professionalism | User onboarding acknowledgment | User Management → Terms acceptance checkbox | GAP |
| CH01-002 | Ch 01 §1.2 / Annexure I | Every PC member must sign a Conflict of Interest Declaration (Annexure I format) at the first meeting of each procurement | COI declaration workflow | Committee → COI Declaration screen | PARTIAL |
| CH01-003 | Ch 01 §1.2 / Annexure II | Every staff officer (Secretary, support staff, technical advisors) supporting a PC must sign COI Declaration (Annexure II format) at the first meeting | COI declaration workflow | Committee → Staff COI Declaration screen | GAP |
| CH01-004 | Ch 01 §1.2 | An officer who prepared the technical specifications for a procurement CANNOT be a BEC member for that same procurement | Committee assignment validation | BEC Assignment → Member eligibility check | GAP |
| CH01-005 | Ch 01 §1.2 | If a PC/BEC member has a COI, they must disclose and recuse themselves immediately | COI recusal workflow | Committee → COI → Recusal action | GAP |
| CH01-006 | Ch 01 §1.2 | COI declarations must be signed before any evaluation work commences | Pre-evaluation gate | Evaluation → COI completion gate | GAP |
| CH01-007 | Ch 01 §1.3 / Annexure III | Every bidder must submit a Non-Collusion Affidavit (Annexure III format), sworn before a Justice of the Peace or Commissioner of Oaths | NCA in document checklist | Document Checklist → NCA checkbox | PARTIAL |
| CH01-008 | Ch 01 §1.3 | NCA must be sworn — a signed letter is NOT sufficient; must be before JP or Commissioner of Oaths | NCA validation | Document Checklist → NCA status notes | GAP |
| CH01-009 | Ch 01 §1.4 | Prohibited practices: corrupt, fraudulent, collusive, coercive, obstructive conduct — all grounds for bid rejection and debarment | Prohibited practice reporting | Audit Trail → Prohibited practice flag | GAP |
| CH01-010 | Ch 01 §1.5 | Officers must report suspected prohibited practices to the Director General of the Department of Public Finance | Reporting workflow | Audit Trail → Report to authority action | N/A |
| CH01-011 | Ch 01 Annexure I | COI Declaration form for PC/BEC members: name, designation, procurement reference, nature of potential COI, declaration of impartiality, date, signature | COI form fields | Committee → COI form fields | GAP |
| CH01-012 | Ch 01 Annexure II | COI Declaration form for staff officers: same fields as Annexure I but role identified as "Staff Officer/Secretary" | Staff COI form | Committee → Staff COI form fields | GAP |
| CH01-013 | Ch 01 Annexure III | Non-Collusion Affidavit: sworn statement that bid price not fixed with other bidders, no communication of bid price, no arrangement made for benefit of any bidder, no inducement offered | NCA form template | Document Checklist → NCA template reference | GAP |

---

## CHAPTER 02 — PROCUREMENT COMMITTEES

### 2.1 — PC Types and Thresholds

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH02-001 | Ch 02 §2.1 | HLPC: handles all procurements exceeding LKR 750 million (GOSL-funded) or LKR 1,500 million (foreign-funded) | PC type configuration | Tender Creation → PC Type selection | PARTIAL |
| CH02-002 | Ch 02 §2.1 | SHLPC (Standing HLPC): same authority as HLPC; constituted on permanent basis for high-volume entities | PC type configuration | Tender Creation → PC Type → SHLPC option | PARTIAL |
| CH02-003 | Ch 02 §2.1 | MPC: handles procurements up to LKR 750 million (GOSL) or LKR 1,500 million (foreign) | PC type configuration | Tender Creation → PC Type → MPC option | PARTIAL |
| CH02-004 | Ch 02 §2.1 | DPC: handles procurements up to LKR 400 million (GOSL) or LKR 800 million (foreign) | PC type configuration | Tender Creation → PC Type → DPC option | PARTIAL |
| CH02-005 | Ch 02 §2.1 | PPC: project-level committee, same authority as DPC (≤LKR 400 Mn GOSL, ≤LKR 800 Mn foreign) | PC type configuration | Tender Creation → PC Type → PPC option | GAP |
| CH02-006 | Ch 02 §2.1 | RPC: handles procurements up to LKR 50 million (GOSL) or LKR 100 million (foreign) | PC type configuration | Tender Creation → PC Type → RPC option | PARTIAL |
| CH02-007 | Ch 02 §2.2 | PC type is determined by the estimated value of the procurement (TCE) | Automatic PC type suggestion | Tender Creation → PC Type auto-suggest from TCE | GAP |

### 2.2 — PC Composition Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH02-008 | Ch 02 §2.3 | HLPC composition: Cabinet-appointed members, minimum specified by Cabinet decision | PC composition validation | PC Formation → Member count validation | GAP |
| CH02-009 | Ch 02 §2.3 | MPC: typically 5 members including Chair; exact composition by institutional order | PC composition | PC Formation → MPC composition | PARTIAL |
| CH02-010 | Ch 02 §2.3 | DPC: typically 3-5 members; Chair is head of institution or authorized deputy | PC composition | PC Formation → DPC composition | PARTIAL |
| CH02-011 | Ch 02 §2.3 | RPC: minimum 3 members | PC composition | PC Formation → RPC member count | PARTIAL |
| CH02-012 | Ch 02 §2.4 | Quorum for PC meeting: majority of total members must be present | Quorum validation | PC Meeting → Attendance → Quorum check | GAP |
| CH02-013 | Ch 02 §2.4 | Decisions by simple majority; Chair has casting vote in case of tie | Decision recording | PC Meeting → Decision → Majority check | GAP |

### 2.3 — BEC Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH02-014 | Ch 02 §2.5 | BEC for HLPC/SHLPC: minimum 5 members | BEC composition validation | BEC Assignment → Member count validation | PARTIAL |
| CH02-015 | Ch 02 §2.5 | BEC for MPC (major procurements): minimum 5 members | BEC composition validation | BEC Assignment → Member count for MPC major | GAP |
| CH02-016 | Ch 02 §2.5 | BEC for MPC (minor procurements): maximum 3 members | BEC composition validation | BEC Assignment → Member count for MPC minor | GAP |
| CH02-017 | Ch 02 §2.5 | BEC for DPC: minimum 3 members | BEC composition validation | BEC Assignment → Member count for DPC | PARTIAL |
| CH02-018 | Ch 02 §2.5 | BEC for RPC: minimum 3 members | BEC composition validation | BEC Assignment → Member count for RPC | PARTIAL |
| CH02-019 | Ch 02 §2.5 | BEC members must be from the BEC Eligibility Pool | Eligibility enforcement | BEC Assignment → Pool membership check | IMPLEMENTED |
| CH02-020 | Ch 02 §2.5 | Procurement Department staff cannot be BEC members | Role separation enforcement | BEC Assignment → Procurement staff exclusion | IMPLEMENTED |
| CH02-021 | Ch 02 §2.5 | One BEC officer cannot serve on more than 6 BECs simultaneously | Concurrent BEC limit | BEC Assignment → Active BEC count check | GAP |
| CH02-022 | Ch 02 §2.5 | BEC must have a Chair and a Secretary designated | Role assignment | BEC Assignment → Chair and Secretary designation | IMPLEMENTED |
| CH02-023 | Ch 02 §2.6 | BEC quorum: majority of members must be present at sessions | Session quorum validation | Session Controls → Attendance → Quorum check | PARTIAL |
| CH02-024 | Ch 02 §2.6 | 3 consecutive unexplained absences: BEC member removed from eligibility pool | Absence tracking | Attendance → Consecutive absence counter | GAP |
| CH02-025 | Ch 02 §2.6 | 2 absences in a single procurement: notify CAO | Absence notification | Attendance → CAO notification trigger | GAP |
| CH02-026 | Ch 02 §2.6 | First unexcused absence in single procurement: 40% payment reduction | Payment tracking | BEC Payment → Absence penalty calculation | GAP |

### 2.4 — BOC Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH02-027 | Ch 02 §2.7 | BOC witnesses bid opening; can include Procurement staff | BOC composition | BOC Assignment → Members (Procurement allowed) | IMPLEMENTED |
| CH02-028 | Ch 02 §2.7 | BOC minimum 3 members including Chair | BOC composition validation | BOC Assignment → Member count | GAP |
| CH02-029 | Ch 02 §2.7 | BOC Chair is independent of the procuring entity where possible | BOC Chair designation | BOC Assignment → Chair field | GAP |
| CH02-030 | Ch 02 §2.8 | BOC must be constituted before bid opening | BOC pre-opening check | Bid Opening → BOC assignment gate | GAP |

### 2.5 — Payment Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH02-031 | Ch 02 §2.9 / Annexure IX | BEC payment: 75% on submission of final report with firm recommendation; 25% after contract award | BEC payment tracking | BEC Payment → Payment schedule | GAP |
| CH02-032 | Ch 02 §2.9 | BEC payment rate: if evaluation completed within PTS = 100% of allowance; outside PTS = 50% of allowance | Payment rate calculation | BEC Payment → PTS compliance flag | GAP |
| CH02-033 | Ch 02 §2.9 | Maximum payment per officer per year cannot exceed their annual basic salary | Annual payment cap | BEC Payment → Annual salary cap check | GAP |
| CH02-034 | Ch 02 §2.9 / Annexure X | Staff officer (Secretary) payment schedule: separate from BEC member payment | Secretary payment | BEC Payment → Staff officer payment | GAP |
| CH02-035 | Ch 02 §2.9 | 40% deduction from first meeting's attendance allowance for first unexcused absence | Absence penalty | BEC Payment → Absence deduction | GAP |
| CH02-036 | Ch 02 §2.9 | No payment for sessions where quorum is not met due to member's own absence | Payment eligibility | BEC Payment → Quorum-related payment | GAP |

### 2.6 — PC Annexure Formats

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH02-037 | Ch 02 Annexure I | PC meeting minutes format: procurement details, members present, items discussed, decisions, next meeting | PC minutes template | PC Meeting → Minutes template | GAP |
| CH02-038 | Ch 02 Annexure II | BEC meeting minutes format: evaluation session details, attendance, decisions per item | BEC session minutes | Session Controls → Session minutes export | GAP |
| CH02-039 | Ch 02 Annexure III | BEC summary report (short form BER) | Short-form BER | Report Generation → Short-form template | GAP |
| CH02-040 | Ch 02 Annexure IV | Member absence notification form: to CAO | Absence notification form | Attendance → CAO notification form | GAP |
| CH02-041 | Ch 02 Annexure V | HLPC appointment format | HLPC appointment letter | Admin → PC appointment letter template | GAP |
| CH02-042 | Ch 02 Annexure VI | SHLPC approval format | SHLPC approval letter | Admin → SHLPC letter template | GAP |
| CH02-043 | Ch 02 Annexure VII | MPC/DPC/PPC/RPC appointment format | PC appointment letters | Admin → PC appointment letter templates | GAP |
| CH02-044 | Ch 02 Annexure VIII | PC member payment schedule form | PC payment schedule | Admin → PC payment forms | GAP |
| CH02-045 | Ch 02 Annexure IX | BEC member payment schedule form | BEC payment schedule | BEC Payment → Payment form export | GAP |
| CH02-046 | Ch 02 Annexure X | Staff officer payment schedule form | Staff payment schedule | BEC Payment → Staff payment form export | GAP |

---

## CHAPTER 03 — PROCUREMENT METHODS

### 3.1 — Method Selection Thresholds

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH03-001 | Ch 03 §3.1 | ICB (International Competitive Bidding): for large-value, complex, or where insufficient local supply; no fixed threshold — PE discretion with PC approval | Method selection | Tender Creation → Procurement Method | PARTIAL |
| CH03-002 | Ch 03 §3.1 | LIB (Limited International Bidding): ICB but with pre-selected international bidders; requires PC approval | Method selection | Tender Creation → Method → LIB option | GAP |
| CH03-003 | Ch 03 §3.1 | NCB (National Competitive Bidding): standard for Goods/Works procurement when ICB not required | Method selection | Tender Creation → Method → NCB option | IMPLEMENTED |
| CH03-004 | Ch 03 §3.1 | LNB (Limited National Bidding): NCB but with pre-selected national bidders; requires documented justification | Method selection | Tender Creation → Method → LNB option | GAP |
| CH03-005 | Ch 03 §3.1 | Shopping (Goods/Services): for low-value, readily available goods; minimum 3 quotations required | Method selection | Tender Creation → Method → Shopping option | PARTIAL |
| CH03-006 | Ch 03 §3.1 | Direct Contracting: single source; requires PC approval and documented justification | Method selection | Tender Creation → Method → Direct Contracting | GAP |
| CH03-007 | Ch 03 §3.1 | Emergency Procurement: disasters, unforeseen; requires CEO/Head approval; post-facto PC ratification within 30 days | Method selection | Tender Creation → Method → Emergency option | GAP |
| CH03-008 | Ch 03 §3.1 | Community Participation: for small community infrastructure; CBO directly contracted | Method selection | Tender Creation → Method → CBO option | GAP |
| CH03-009 | Ch 03 §3.1 | Force Account: PE's own workforce and equipment; requires PC approval; for remote locations or maintenance | Method selection | Tender Creation → Method → Force Account | GAP |
| CH03-010 | Ch 03 §3.1 | Repeat Order: additional quantity <50% of original within 6 months of original award | Repeat order management | Tender Creation → Repeat Order flag | GAP |

### 3.2 — Bidding Procedures

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH03-011 | Ch 03 §3.2 | Single Stage One Envelope: standard procedure; technical and financial in same envelope; opened simultaneously | Bidding procedure | Tender Creation → Bidding Procedure → Single Stage 1E | IMPLEMENTED |
| CH03-012 | Ch 03 §3.2 | Single Stage Two Envelope: technical envelope (Envelope A) and financial envelope (Envelope B) submitted together; Envelope B of technically responsive bids only opened after technical evaluation | Bidding procedure | Tender Creation → Bidding Procedure → Single Stage 2E | PARTIAL |
| CH03-013 | Ch 03 §3.2 | Two Stage: Stage 1 (no price, technical proposal only); Stage 2 (revised technical + financial); for complex/turnkey | Bidding procedure | Tender Creation → Bidding Procedure → Two Stage | GAP |
| CH03-014 | Ch 03 §3.2 | Two Envelope: separate bid opening events — first Envelope A (technical), then Envelope B (financial) for responsive bids only | Two-envelope workflow | Bid Opening → Envelope A opening event; Bid Opening → Envelope B opening event | PARTIAL |
| CH03-015 | Ch 03 §3.3 | Pre-qualification: for complex, high-value procurements; bidders evaluated before RFP issued; approved by PC | Pre-qualification management | Pre-qualification → Application review → PC approval | GAP |
| CH03-016 | Ch 03 §3.3 | Pre-qualification criteria: financial capacity, technical experience, equipment availability, litigation history | Pre-qualification criteria | Pre-qualification → Criteria setup | GAP |
| CH03-017 | Ch 03 §3.3 | Post-qualification: most common; qualification checked for only the lowest evaluated bidder AFTER financial evaluation | Post-qualification | Stage 4 Evaluation → Post-qualification screen | PARTIAL |

### 3.3 — CBO Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH03-018 | Ch 03 §3.4 | CBO direct award ceiling: LKR 2 million aggregate per CBO per year | CBO contract tracking | CBO Management → Annual aggregate tracker | GAP |
| CH03-019 | Ch 03 §3.4 | CBO competitive bidding ceiling: up to LKR 10 million; 5% preference margin over commercial contractors | CBO preference | CBO Bid → Preference margin application | GAP |
| CH03-020 | Ch 03 §3.4 | CBO advance payment: maximum LKR 200,000 | CBO advance | CBO Contract → Advance amount cap | GAP |
| CH03-021 | Ch 03 §3.4 | CBO maximum 3 concurrent contracts | CBO limit enforcement | CBO Management → Concurrent contract count | GAP |
| CH03-022 | Ch 03 §3.4 | CBO cannot subcontract work | CBO restrictions | CBO Contract → No-subcontract flag | GAP |
| CH03-023 | Ch 03 §3.4 | CBO must be registered for at least 1 year before eligibility | CBO eligibility | CBO Registration → Registration date validation | GAP |

---

## CHAPTER 04 — PROCUREMENT PLANNING

### 4.1 — Procurement Plan Types

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH04-001 | Ch 04 §4.1 | Master Procurement Plan (MPP): 3-5 year strategic plan; approved by CAO | Procurement planning | Admin → MPP upload/tracking | GAP |
| CH04-002 | Ch 04 §4.1 | Annual Procurement Plan (APP): derived from MPP; covers one financial year; approved by AO/CAO | Procurement planning | Admin → APP upload/tracking | GAP |
| CH04-003 | Ch 04 §4.1 | Departmental APP (DAPP): individual department's annual plan; feeds into APP | Procurement planning | Admin → DAPP upload/tracking | GAP |
| CH04-004 | Ch 04 §4.1 | Procurement Package Plan (PP): detailed plan per procurement package | Procurement planning | Admin → PP per tender | GAP |
| CH04-005 | Ch 04 §4.2 | Procurement Time Schedule (PTS): detailed activity-by-activity timeline; prepared per procurement | PTS management | Tender Creation → PTS setup | PARTIAL |
| CH04-006 | Ch 04 §4.2 | PTS must cover all activities from tendering through contract award | PTS completeness | PTS → Activity coverage validation | GAP |
| CH04-007 | Ch 04 §4.2 | PTS is used as basis for BEC payment determination (within/outside PTS) | PTS-payment linkage | BEC Payment → PTS reference | GAP |

### 4.2 — Slicing and Packaging

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH04-008 | Ch 04 §4.3 | Artificial slicing/splitting to circumvent PC authority limits is prohibited | Anti-slicing check | Tender Creation → Estimated value aggregation check | GAP |
| CH04-009 | Ch 04 §4.3 | Lots and packages may be used for efficiency; each lot evaluated separately | Lot-based evaluation | Tender → Lots/Packages management | IMPLEMENTED |
| CH04-010 | Ch 04 §4.3 | A bidder may be awarded multiple lots; award to minimize overall cost for PE | Multi-lot award optimization | Report → Multi-lot recommendation | GAP |

### 4.3 — TCE Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH04-011 | Ch 04 §4.4 | Total Contract Estimate (TCE): basis for PC type selection, bid security, bidding method | TCE field | Tender Creation → TCE field | IMPLEMENTED |
| CH04-012 | Ch 04 §4.4 | TCE must be prepared by an independent technical officer, not the procurement officer | TCE preparer field | Tender Creation → TCE preparer designation | GAP |
| CH04-013 | Ch 04 §4.4 | TCE is confidential — not disclosed to bidders before bid closing | TCE confidentiality | Tender Creation → TCE visibility (PM-only) | GAP |
| CH04-014 | Ch 04 §4.4 | TCE includes VAT; bid security computed on TCE excluding VAT | TCE vs bid security base | Bid Security → Amount from TCE excl. VAT | PARTIAL |

### 4.4 — Publication Requirements

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH04-015 | Ch 04 §4.5 | General Procurement Notice (GPN): published annually for all planned procurements exceeding thresholds | GPN tracking | Admin → GPN publication management | GAP |
| CH04-016 | Ch 04 §4.5 | Annual Procurement Plan Publication (APPP): public notice of annual plan | APPP tracking | Admin → APPP publication | GAP |

### 4.5 — Time Frame Annexures

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH04-017 | Ch 04 Annexure VIII | Detailed PTS time frames for each method and PC type (NCB Goods, ICB Goods, etc.) — specific working day counts per activity | PTS template library | PTS → Method-specific template | GAP |
| CH04-018 | Ch 04 Annexure IX | PTS form format: activity, responsibility, planned date, actual date, variance | PTS form | PTS → Activity tracking form | GAP |
| CH04-019 | Ch 04 Annexure X | Standard PTS timelines by procurement category | PTS standard timelines | PTS → Standard timeline lookup | GAP |

---

## CHAPTER 05 — PROCUREMENT DOCUMENTS AND CONTRACT TERMS

### 5.1 — Standard Procurement Documents (SPD)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-001 | Ch 05 §5.1 | SPD must be used for all NCB and above procurements; PE cannot unilaterally modify SPD conditions | Document management | Bid Document → SPD compliance flag | GAP |
| CH05-002 | Ch 05 §5.1 | Bid Data Sheet (BDS) contains procurement-specific data that supplements or modifies the SPD | BDS management | Bid Document → BDS field editor | PARTIAL |
| CH05-003 | Ch 05 §5.1 | Special Conditions of Contract (SCC) supplements General Conditions of Contract (GCC) | Contract document | Contract → SCC/GCC management | GAP |
| CH05-004 | Ch 05 §5.2 | For Works contracts: must use ICTAD/SCA standard conditions unless approved deviation | Works contract | Contract → ICTAD reference | GAP |

### 5.2 — Bid Validity

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-005 | Ch 05 §5.3 / Table | Bid validity for Goods <LKR 5Mn: 49 days | Bid validity validation | Tender Creation → Bid validity field | PARTIAL |
| CH05-006 | Ch 05 §5.3 / Table | Bid validity for Goods LKR 5-50Mn (GOSL): 91 days | Bid validity validation | Tender Creation → Bid validity field | PARTIAL |
| CH05-007 | Ch 05 §5.3 / Table | Bid validity for Goods LKR 50-750Mn (GOSL): 105 days | Bid validity validation | Tender Creation → Bid validity field | PARTIAL |
| CH05-008 | Ch 05 §5.3 / Table | Bid validity for Goods >LKR 750Mn (GOSL): 119 days | Bid validity validation | Tender Creation → Bid validity field | PARTIAL |
| CH05-009 | Ch 05 §5.3 / Table | Bid validity for Works <LKR 5Mn: 49 days | Bid validity validation | Tender Creation → Bid validity field (Works) | PARTIAL |
| CH05-010 | Ch 05 §5.3 / Table | Bid validity for Works LKR 5-50Mn: 91 days | Bid validity validation | Tender Creation → Bid validity field (Works) | PARTIAL |
| CH05-011 | Ch 05 §5.3 / Table | Bid validity for Works LKR 50-500Mn (GOSL): 119 days | Bid validity validation | Tender Creation → Bid validity field (Works) | PARTIAL |
| CH05-012 | Ch 05 §5.3 / Table | Bid validity for Works >LKR 500Mn (GOSL): 147 days | Bid validity validation | Tender Creation → Bid validity field (Works) | PARTIAL |
| CH05-013 | Ch 05 §5.3 / Table | Bid validity for Works ≥LKR 3,000Mn (foreign-funded): 203 days | Bid validity validation | Tender Creation → Bid validity field (Works, foreign) | PARTIAL |
| CH05-014 | Ch 05 §5.3 | Bid validity extension: requires bidder's consent; bid security must be extended accordingly | Bid validity extension | Tender → Bid validity extension workflow | GAP |
| CH05-015 | Ch 05 §5.3 | Bidder may refuse validity extension without losing bid security | Extension refusal | Tender → Extension refusal recording | GAP |

### 5.3 — Bid Security

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-016 | Ch 05 §5.4 | Bid security amount: 1% to 2% of TCE (excluding VAT) | Bid security setup | Tender Creation → Bid Security Amount (% of TCE excl. VAT) | PARTIAL |
| CH05-017 | Ch 05 §5.4 | Bid security form: unconditional bank guarantee, cashier's cheque, or cash | Bid security form | Bid Opening → Bid Security Type field | PARTIAL |
| CH05-018 | Ch 05 §5.4 | NCB/LNB bid security validity: bid validity period + 28 days | Bid security validity | Bid Opening → Bid Security Expiry validation | GAP |
| CH05-019 | Ch 05 §5.4 | ICB/LIB bid security validity: bid validity period + 56 days | Bid security validity | Bid Opening → Bid Security Expiry validation (ICB) | GAP |
| CH05-020 | Ch 05 §5.4 | Bid Securing Declaration (BSD): used instead of bank guarantee for contracts <LKR 20 million | BSD option | Tender Creation → Bid Security Type → BSD option | PARTIAL |
| CH05-021 | Ch 05 §5.4 | Bid security must be from a licensed commercial bank in Sri Lanka | Bid security bank validation | Bid Opening → Issuing Bank → SL licensed bank check | GAP |
| CH05-022 | Ch 05 §5.4 | Bid security is forfeited if bidder withdraws bid during validity, submits false info, refuses contract, or refuses performance security | Forfeiture tracking | Bid Security → Forfeiture recording | GAP |
| CH05-023 | Ch 05 §5.4 | Bid security returned to unsuccessful bidders after notification of award | Bid security return | Award → Bid security return workflow | GAP |
| CH05-024 | Ch 05 §5.4 | Bid security returned to successful bidder upon signing of contract | Bid security return | Contract Award → Bid security return trigger | GAP |

### 5.4 — Performance Security

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-025 | Ch 05 §5.5 | Works contracts: performance security ≥5% of contract value | Performance security | Contract Award → Performance Security % (Works) | PARTIAL |
| CH05-026 | Ch 05 §5.5 | Goods contracts: performance security ≥10% of contract value | Performance security | Contract Award → Performance Security % (Goods) | PARTIAL |
| CH05-027 | Ch 05 §5.5 | Services contracts: performance security ≥5% of contract value | Performance security | Contract Award → Performance Security % (Services) | PARTIAL |
| CH05-028 | Ch 05 §5.5 | Performance security waived for contracts <LKR 5 million | Waiver threshold | Contract Award → PS waiver for <5Mn contracts | GAP |
| CH05-029 | Ch 05 §5.5 | Performance security validity: 28 days beyond contract completion date | PS validity | Contract Award → PS Expiry Date calculation | GAP |
| CH05-030 | Ch 05 §5.5 | Performance security form: unconditional on-demand bank guarantee | PS form type | Contract Award → PS Type → Bank guarantee | PARTIAL |
| CH05-031 | Ch 05 Annexure IX | Performance security guarantee standard form format | PS form template | Contract Award → PS form template | GAP |

### 5.5 — Advance Payment

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-032 | Ch 05 §5.6 | Works contracts >LKR 50 million: advance payment maximum 20% of contract value | Advance payment | Contract → Advance payment cap (Works >50Mn) | GAP |
| CH05-033 | Ch 05 §5.6 | Works contracts <LKR 50 million: advance payment maximum 30% of contract value | Advance payment | Contract → Advance payment cap (Works <50Mn) | GAP |
| CH05-034 | Ch 05 §5.6 | Goods contracts: advance payment maximum 30% of contract value | Advance payment | Contract → Advance payment cap (Goods) | GAP |
| CH05-035 | Ch 05 §5.6 | Advance payment requires unconditional bank guarantee of equal amount | Advance payment guarantee | Contract → AP Guarantee tracking | GAP |
| CH05-036 | Ch 05 §5.6 | Advance payment recovered by deduction from progress payments; percentage deducted equals percentage advanced | Advance recovery | Contract → Payment → Advance recovery tracking | GAP |
| CH05-037 | Ch 05 Annexure VII | Advance payment guarantee standard form | AP guarantee form | Contract → AP guarantee form template | GAP |

### 5.6 — Retention Money (Works)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-038 | Ch 05 §5.7 | Works: retention money minimum 5% of each progress payment | Retention tracking | Contract → Payment → Retention calculation | GAP |
| CH05-039 | Ch 05 §5.7 | First 50% of retention releasable on completion of works (with retention guarantee) | Retention release | Contract → Retention → 50% release trigger | GAP |
| CH05-040 | Ch 05 §5.7 | Remaining 50% of retention releasable after defects liability period | Retention release | Contract → Retention → Final release trigger | GAP |
| CH05-041 | Ch 05 Annexure VIII | Retention money guarantee standard form | Retention guarantee form | Contract → Retention guarantee form | GAP |

### 5.7 — Liquidated Damages

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-042 | Ch 05 §5.8 | Goods: liquidated damages maximum 1% per week of delay | LD calculation | Contract → LD rate (Goods) | GAP |
| CH05-043 | Ch 05 §5.8 | Works: liquidated damages maximum 0.05% per day of delay | LD calculation | Contract → LD rate (Works) | GAP |
| CH05-044 | Ch 05 §5.8 | Liquidated damages capped at 10% of contract value for both Goods and Works | LD cap | Contract → LD cap enforcement | GAP |
| CH05-045 | Ch 05 §5.8 | Bonus for early completion may be specified in contract | Bonus clause | Contract → Early completion bonus | GAP |

### 5.8 — Contract Formation

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-046 | Ch 05 §5.9 | Formal written contract required for Works >LKR 500,000 | Contract threshold | Contract Award → Works contract threshold | GAP |
| CH05-047 | Ch 05 §5.9 | Formal written contract required for Goods/Services >LKR 1,000,000 | Contract threshold | Contract Award → Goods/Services contract threshold | GAP |
| CH05-048 | Ch 05 §5.9 | Contracts <LKR 500,000 (Works) or <LKR 1,000,000 (Goods): purchase order or LOA sufficient | Simplified contract | Contract Award → LOA/PO option for small values | GAP |

### 5.9 — Specifications

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH05-049 | Ch 05 §5.10 | Specifications must be generic/functional; cannot be proprietary or brand-specific unless no alternative exists | Specification review | Spec Library → Proprietary spec flag | GAP |
| CH05-050 | Ch 05 §5.10 | If brand mentioned, must add "or equivalent" | "Or equivalent" enforcement | Spec Editor → Brand → Auto-append "or equivalent" | GAP |
| CH05-051 | Ch 05 §5.10 | Specifications must be technically sound and verifiable | Spec quality | Spec Library → Technical review flag | GAP |

---

## CHAPTER 06 — PUBLICATION AND BID OPENING

### 6.1 — Publication Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH06-001 | Ch 06 §6.1 | IFB/RFQ must be published in at least one national newspaper in Sinhala, Tamil, and English | Publication tracking | Publication → Newspaper publication tracker | GAP |
| CH06-002 | Ch 06 §6.1 | ICB must additionally be published in international media/trade journals | Publication tracking | Publication → International publication tracker | GAP |
| CH06-003 | Ch 06 §6.1 | Award >LKR 750 million: must be published in Gazette in all three official languages | Award publication | Award → Gazette publication trigger (>750Mn) | GAP |
| CH06-004 | Ch 06 §6.1 | All contract awards must be published on PE's website | Award publication | Award → Website publication tracking | GAP |
| CH06-005 | Ch 06 §6.2 | Bid documents must be available for purchase from the day of IFB publication | Document issuance | Publication → Document availability date | GAP |
| CH06-006 | Ch 06 §6.2 | Bid document fee schedule set by NPC (standardized fees) | Document fee | Tender Creation → Document fee field | GAP |
| CH06-007 | Ch 06 §6.2 | Purchasers of bid documents (document purchasers list) separate from pre-bid meeting attendees | Document purchasers list | Publication → Document purchasers list | GAP |

### 6.2 — Bidding Period Minimums

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH06-008 | Ch 06 §6.3 | ICB bidding period: minimum 42 days from IFB publication to bid closing | Bidding period validation | Tender Creation → Closing date → ICB minimum 42 days | GAP |
| CH06-009 | Ch 06 §6.3 | LIB bidding period: minimum 42 days | Bidding period validation | Tender Creation → Closing date → LIB minimum 42 days | GAP |
| CH06-010 | Ch 06 §6.3 | NCB Goods/Works ≥LKR 50Mn: minimum 21 days | Bidding period validation | Tender Creation → Closing date → NCB ≥50Mn minimum 21 days | GAP |
| CH06-011 | Ch 06 §6.3 | NCB Goods/Works <LKR 50Mn: minimum 14 days | Bidding period validation | Tender Creation → Closing date → NCB <50Mn minimum 14 days | GAP |
| CH06-012 | Ch 06 §6.3 | LNB: same as NCB | Bidding period validation | Tender Creation → Closing date → LNB rule | GAP |
| CH06-013 | Ch 06 §6.3 | Shopping (National): minimum 7 days | Bidding period validation | Tender Creation → Closing date → Shopping National 7 days | GAP |
| CH06-014 | Ch 06 §6.3 | Shopping (International): minimum 14 days | Bidding period validation | Tender Creation → Closing date → Shopping International 14 days | GAP |

### 6.3 — Pre-Bid Meeting

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH06-015 | Ch 06 §6.4 | Pre-bid meeting must be held at least 10 calendar days before bid closing date (for procurements >LKR 50Mn) | Pre-bid timing validation | Pre-Bid Meeting → Date → 10-day rule | PARTIAL |
| CH06-016 | Ch 06 §6.4 | Attendance at pre-bid meeting is NOT mandatory for bidders (unless specified in BDS) | Pre-bid attendance | Pre-Bid Meeting → Attendance recording (optional flag) | PARTIAL |
| CH06-017 | Ch 06 §6.4 | Questions raised at pre-bid meeting must be documented | Pre-bid Q&A | Pre-Bid Meeting → Q&A tracking | IMPLEMENTED |
| CH06-018 | Ch 06 §6.4 | All responses (clarifications/addenda) circulated to ALL document purchasers, not just attendees | Addendum distribution | Pre-Bid Meeting → Addendum → All purchasers distribution | GAP |
| CH06-019 | Ch 06 §6.4 | Addendum must be issued at least 3 working days before bid closing | Addendum timing | Pre-Bid Meeting → Addendum → 3 working day minimum | GAP |
| CH06-020 | Ch 06 §6.4 | Document purchasers list maintained separately; used for addendum distribution | Document purchasers list | Pre-Bid Meeting → Purchasers list link | GAP |

### 6.4 — Bid Receipt

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH06-021 | Ch 06 §6.5 | Bids received must be date/time stamped and stored securely until opening | Bid receipt | Bid Opening → Receipt log | GAP |
| CH06-022 | Ch 06 §6.5 | Bids received after closing time must be returned unopened | Late bid rejection | Bid Opening → Late bid rejection log | GAP |
| CH06-023 | Ch 06 §6.5 | Modification letters: if sealed envelope has "Modification" or "Withdrawal" clearly marked, processed accordingly | Modification/withdrawal | Bid Opening → Modification and withdrawal processing | GAP |
| CH06-024 | Ch 06 §6.5 | Withdrawal letters must be received before bid closing to be valid | Withdrawal timing | Bid Opening → Withdrawal timing validation | GAP |

### 6.5 — Bid Opening Procedure

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH06-025 | Ch 06 §6.6 | Bid opening commences no more than 30 minutes after bid closing time | Opening timing | Bid Opening → Opening start time (30-min rule) | GAP |
| CH06-026 | Ch 06 §6.6 | Opening must be public; bidder representatives may attend | Public opening | Bid Opening → Attendee registration | PARTIAL |
| CH06-027 | Ch 06 §6.6 | BOC Chair announces each bidder name, bid price, any discount, bid security amount and validity | Reading at opening | Bid Opening → BOC reading procedure | PARTIAL |
| CH06-028 | Ch 06 §6.6 | All BOC members must initial each page of the bid documents | BOC initials | Bid Opening → BOC initial recording (per bidder) | GAP |
| CH06-029 | Ch 06 §6.6 | Corrections to original text on bid forms: covered with transparent tape and initialed by BOC Chair | Tape and initial rule | Bid Opening → Correction recording | GAP |
| CH06-030 | Ch 06 §6.6 | After opening: original bids handed to BEC Secretary; copies retained by BOC | Handover chain | Bid Opening → Handover to BEC Secretary | GAP |
| CH06-031 | Ch 06 §6.6 | BOC keeps signed attendance sheet and signed copy of all bid opening records | BOC record retention | Bid Opening → BOC records signed copies | GAP |
| CH06-032 | Ch 06 §6.6 | No bid prices shall be revealed to any bidder before award notification | Price confidentiality | Bid Opening → Price visibility restriction | PARTIAL |
| CH06-033 | Ch 06 §6.6 | For Two-Envelope: Envelope B (financial) of non-responsive bidders returned unopened | Two-envelope rejection | Bid Opening → Envelope B return for NR bidders | GAP |

### 6.6 — BOC Annexure Formats

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH06-034 | Ch 06 Annexure I | BOC attendance sheet format: date, procurement ref, members present, bidder representatives | BOC attendance form | Bid Opening → BOC Attendance form | PARTIAL |
| CH06-035 | Ch 06 Annexure II | BOC general information record: procurement details, BOC members, bid received/valid/rejected count | BOC general info | Bid Opening → BOC General Info form | PARTIAL |
| CH06-036 | Ch 06 Annexure III | BOC financial information record: bidder name, total bid price, currency, discount, bid security amount/bank/validity | BOC financial record | Bid Opening → BOC Financial Record table | PARTIAL |

---

## CHAPTER 07 — EVALUATION

### 7.1 — Stage 1: Basic Data, BOC Records, Document Completeness

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-001 | Ch 07 §7.1 / Annexure I | Stage 1 table: bidder name, bid date, bid validity (required vs. offered), bid security (required vs. offered), number of copies | Stage 1 evaluation | Evaluation → Stage 1 → Basic Data table | PARTIAL |
| CH07-002 | Ch 07 §7.1 / Annexure II | BEC bid opening records: verify BOC records for each bidder (prices, discounts, bid security) | Stage 1 evaluation | Evaluation → Stage 1 → BOC Record Verification | GAP |
| CH07-003 | Ch 07 §7.1 / Annexure III | Document completeness table: per-bidder checklist of all required documents | Stage 1 evaluation | Evaluation → Stage 1 → Document Completeness table | PARTIAL |
| CH07-004 | Ch 07 §7.1 | Document checklist items (Annexure III Ch 07): original bid + 1 copy, completeness of bid form, price schedules, bid security, power of attorney, signature on bid form, authority to sign, bidder eligibility, goods/services eligibility, JV agreement, bid validity dates | Document checklist | Document Checklist → Standard items | PARTIAL |
| CH07-005 | Ch 07 §7.1 | Non-Collusion Affidavit in document checklist (Annexure III Ch 07) | NCA in checklist | Document Checklist → NCA item | PARTIAL |
| CH07-006 | Ch 07 §7.1 | Stage 1 result: each bidder = Compliant (C) or Non-Compliant (NC) for each document | Stage 1 result | Evaluation → Stage 1 → C/NC status per bidder | GAP |
| CH07-007 | Ch 07 §7.1 | Stage 1 failure is grounds for bid rejection in Stage 2 (but PE may request clarifications for minor Stage 1 issues) | Stage 1 rejection | Evaluation → Stage 1 → Rejection flag | GAP |

### 7.2 — Stage 2: Commercial Terms and Technical Responsiveness

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-008 | Ch 07 §7.2 / Annexure IV(a) | Commercial terms table: per-bidder assessment of payment terms, delivery period, warranty, after-sales, spare parts availability, country of origin | Stage 2 commercial | Evaluation → Stage 2 → Commercial Terms table | GAP |
| CH07-009 | Ch 07 §7.2 / Annexure IV(b) | Commercial terms result: Responsive (R) or Non-Responsive (NR) for commercial terms overall | Commercial responsiveness | Evaluation → Stage 2 → Commercial R/NR | GAP |
| CH07-010 | Ch 07 §7.2 / Annexure V | Technical responsiveness table: per-requirement, per-bidder assessment; rows = requirements, columns = bidders | Evaluation grid | Evaluation Grid → Stage 2 → Technical table | IMPLEMENTED |
| CH07-011 | Ch 07 §7.2 | Deviation classification: MAJOR, MINOR, or DEBATABLE | Deviation classification | Evaluation Grid → Requirement → Deviation type | PARTIAL |
| CH07-012 | Ch 07 §7.2 | MAJOR deviation: materially affects scope, quality, or performance; bidder = Non-Responsive | Major deviation | Evaluation → Deviation → Major → NR flag | PARTIAL |
| CH07-013 | Ch 07 §7.2 | MINOR deviation: does not materially affect scope; can be accepted or quantified | Minor deviation | Evaluation → Deviation → Minor → Accept/Quantify | PARTIAL |
| CH07-014 | Ch 07 §7.2 | DEBATABLE deviation: BEC cannot agree; seeks PC guidance or applies conservative interpretation | Debatable deviation | Evaluation → Deviation → Debatable → PC referral | GAP |
| CH07-015 | Ch 07 §7.2 | Compulsory (C) requirement: Major deviation = Non-Responsive for whole item | C-requirement rejection | Evaluation Grid → C flag → NR propagation | IMPLEMENTED |
| CH07-016 | Ch 07 §7.2 | Technical responsiveness result: Responsive (R) or Non-Responsive (NR) per item per bidder | Technical result | Evaluation → Stage 2 → Technical R/NR | IMPLEMENTED |
| CH07-017 | Ch 07 §7.2 | Combined result: both commercial and technical must be R for bidder to proceed to Stage 3 | Combined responsiveness | Evaluation → Stage 2 → Combined R/NR gate | GAP |
| CH07-018 | Ch 07 §7.2 | BEC must not proceed to Stage 3 for NR bidders | NR gate enforcement | Evaluation → Stage 3 → NR bidder exclusion | GAP |

### 7.3 — Stage 3: Price Evaluation (14-Step Adjustment)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-019 | Ch 07 §7.3 / Annexure VI(a) | Step 1: Follow criteria established in the PD | Price evaluation | Price Evaluation → Step 1 reference | PARTIAL |
| CH07-020 | Ch 07 §7.3 / Annexure VI(a) | Step 2: Exclude VAT, contingencies, and provisional sums from bid price | Price exclusions | Price Evaluation → VAT exclusion field | PARTIAL |
| CH07-021 | Ch 07 §7.3 / Annexure VI(a) | Step 3: Correct arithmetic errors; unit price prevails over extended amount; words prevail over figures | Arithmetic correction | Price Evaluation → Arithmetic correction module | GAP |
| CH07-022 | Ch 07 §7.3 / Annexure VI(a) | Step 4: Apply unconditional discounts announced at bid opening; conditional discounts excluded | Discount application | Price Evaluation → Discount field (unconditional only) | PARTIAL |
| CH07-023 | Ch 07 §7.3 / Annexure VI(a) | Step 5: Adjust for omissions (required items not priced); adjustment = average of other responsive bidders' prices for that item | Omission adjustment | Price Evaluation → Omission adjustment calculation | GAP |
| CH07-024 | Ch 07 §7.3 / Annexure VI(a) | Step 6: Delivery period adjustment; delivery >10% beyond required = Non-Responsive | Delivery period check | Price Evaluation → Delivery period validation | GAP |
| CH07-025 | Ch 07 §7.3 / Annexure VI(a) | Step 7: Adjust for acceptable minor deviations (monetarily quantified); loaded onto bid price | Deviation adjustment | Price Evaluation → Minor deviation monetary loading | GAP |
| CH07-026 | Ch 07 §7.3 / Annexure VI(a) | Step 8: Adjust for inland transportation costs to destination (if not included in bid) | Transport adjustment | Price Evaluation → Inland transport adjustment | GAP |
| CH07-027 | Ch 07 §7.3 / Annexure VI(a) | Step 9: Operational cost and life-cycle costing using NPV methodology where specified in PD | Life cycle cost | Price Evaluation → Life cycle cost (NPV) | GAP |
| CH07-028 | Ch 07 §7.3 / Annexure VI(a) | Step 10: Apply domestic preference (20% for GOSL-funded Goods; 15% for Works) | Domestic preference | Price Evaluation → Domestic preference loading | GAP |
| CH07-029 | Ch 07 §7.3 / Annexure VI(a) | Step 11: Currency conversion to single currency; use CBSL selling rate on bid closing date or 28 days prior (whichever lower for PE) | Currency conversion | Price Evaluation → Currency conversion | GAP |
| CH07-030 | Ch 07 §7.3 / Annexure VI(a) | Step 12: After-sales service costs adjustment (spare parts availability, service network quality) | After-sales adjustment | Price Evaluation → After-sales factor | GAP |
| CH07-031 | Ch 07 §7.3 / Annexure VI(a) | Step 13: Examine for unbalanced bids; unbalanced = individual item >50% above weighted average AND >5% of total evaluated bid price | Unbalanced bid detection | Price Evaluation → Unbalanced bid check | GAP |
| CH07-032 | Ch 07 §7.3 / Annexure VI(a) | Unbalanced bid: BEC must flag and investigate; may seek clarification; if confirmed, may reject | Unbalanced bid workflow | Price Evaluation → Unbalanced bid → Investigate/reject | GAP |
| CH07-033 | Ch 07 §7.3 / Annexure VI(a) | Step 14: Clarifications during evaluation: BEC may seek written clarification; no change to price or substance allowed | Clarification during evaluation | Evaluation → Clarification workflow | IMPLEMENTED |
| CH07-034 | Ch 07 §7.3 / Annexure VI(b) | Price comparison table: quoted price, adjustments steps 2-13, evaluated price; all in common currency | Price comparison table | Price Evaluation → Price comparison table (Annexure VI(b)) | GAP |
| CH07-035 | Ch 07 §7.3 | Evaluated price: final price after all 14 adjustments; basis for ranking | Evaluated price | Price Evaluation → Evaluated price field | PARTIAL |
| CH07-036 | Ch 07 §7.3 | Ranking: bidder with lowest evaluated price is first ranked (among technically responsive bidders) | Bid ranking | Price Evaluation → Ranking → Lowest evaluated | PARTIAL |

### 7.4 — Domestic Preference Rules

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-037 | Ch 07 §7.4 / Annexure VII(a) | Domestic preference for Goods (GOSL): 20% loaded onto CIF price of foreign goods | DP calculation | Price Evaluation → DP → 20% foreign goods loading | GAP |
| CH07-038 | Ch 07 §7.4 / Annexure VII(b) | Domestic preference for Works (GOSL): 15% loaded onto bid of foreign contractors | DP calculation | Price Evaluation → DP → 15% foreign contractor loading | GAP |
| CH07-039 | Ch 07 §7.4 | Domestic preference eligibility: Goods must have ≥30% local value addition | DP eligibility | Price Evaluation → DP → 30% local content check | GAP |
| CH07-040 | Ch 07 §7.4 | Domestic preference: NOT applied for foreign-funded procurements (World Bank, ADB, etc.) | DP applicability | Tender Creation → Funding source → DP toggle | GAP |

### 7.5 — Stage 4: Post-Qualification

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-041 | Ch 07 §7.5 | Post-qualification applied ONLY to lowest evaluated bidder | PQ target | Stage 4 → PQ → Lowest evaluated bidder only | PARTIAL |
| CH07-042 | Ch 07 §7.5 | PQ criteria: financial capability (audited financial statements, credit lines) | PQ financial | Stage 4 → PQ → Financial criteria checklist | PARTIAL |
| CH07-043 | Ch 07 §7.5 | PQ criteria: technical capability (equipment, key personnel, experience) | PQ technical | Stage 4 → PQ → Technical criteria checklist | PARTIAL |
| CH07-044 | Ch 07 §7.5 | PQ criteria: relevant experience (completed similar contracts in last 5-10 years) | PQ experience | Stage 4 → PQ → Experience criteria checklist | PARTIAL |
| CH07-045 | Ch 07 §7.5 | PQ criteria: manufacturer authorization (if required in PD) | PQ manufacturer auth | Stage 4 → PQ → Manufacturer authorization | GAP |
| CH07-046 | Ch 07 §7.5 | If lowest evaluated bidder fails PQ: move to second lowest, repeat PQ | PQ failure handling | Stage 4 → PQ → Failure → Next bidder workflow | GAP |
| CH07-047 | Ch 07 §7.5 | If all bidders fail PQ: procurement cancelled and re-advertised | PQ total failure | Stage 4 → PQ → All fail → Cancel workflow | GAP |

### 7.6 — Tie-Breaking and Special Situations

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-048 | Ch 07 §7.6 | Tie (identical evaluated prices): apply domestic preference as tie-breaker if one is domestic | Tie-breaking | Price Evaluation → Tie → DP tie-breaker | GAP |
| CH07-049 | Ch 07 §7.6 | Tie: if still tied after DP, PC determines by lot (drawing) | Tie by lot | Price Evaluation → Tie → PC lot drawing | GAP |
| CH07-050 | Ch 07 §7.6 | Single responsive bid: obtain PC approval to proceed; confirm price is competitive (compare to TCE) | Single bid handling | Evaluation → Single bid → PC approval workflow | GAP |
| CH07-051 | Ch 07 §7.6 | If single bid price >TCE by more than acceptable margin: negotiation or re-tender | Single bid over TCE | Evaluation → Single bid → Over-TCE workflow | GAP |

### 7.7 — BEC Evaluation Report (BER) Format

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH07-052 | Ch 07 Annexure VIII | BER format: BEC has liberty to design suitable format; Annexure VIII is a template | Report structure | Report Generation → BER template flexibility | IMPLEMENTED |
| CH07-053 | Ch 07 Annexure VIII | BER must include: basic info, evaluation methodology, Stage 1-4 results, recommendation | BER completeness | Report Generation → BER sections completeness | PARTIAL |
| CH07-054 | Ch 07 Annexure VIII | BER must contain all 5 standard annexures: pre-bid meeting records, clarifications, bid opening records, price schedules, document checklist | BER annexures | Report Generation → 5 auto-generated annexures | PARTIAL |
| CH07-055 | Ch 07 §7.7 | BER must be certified by all BEC members before submission to PC | BEC certification | BER → Certification → All members sign | IMPLEMENTED |
| CH07-056 | Ch 07 §7.7 | BER certification requires signature of Chair, all members, and Secretary | Certification fields | BER → Signature table | IMPLEMENTED |

---

## CHAPTER 08 — AWARD OF CONTRACT

### 8.1 — Award Decision

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH08-001 | Ch 08 §8.1 | Award authority: PC approves award recommendation from BER | Award approval | Award → PC Approval workflow | PARTIAL |
| CH08-002 | Ch 08 §8.1 | Award must be to lowest evaluated responsive bidder who passes post-qualification | Award rule | Award → Lowest evaluated responsive winner | PARTIAL |
| CH08-003 | Ch 08 §8.1 | Award must be within bid validity period | Award timing | Award → Bid validity expiry check | GAP |
| CH08-004 | Ch 08 §8.1 | DPC Final Approval required before LOA issuance (for DPC-level procurements) | DPC approval | Award → DPC Final Approval → LOA trigger | IMPLEMENTED |

### 8.2 — Standstill Period

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH08-005 | Ch 08 §8.2 | Standstill period: minimum 10 working days between notification of intention to award and actual award | Standstill period | Award → Standstill period timer (10 working days) | PARTIAL |
| CH08-006 | Ch 08 §8.2 | Notification of intention to award sent to ALL bidders simultaneously | Intent notification | Award → Notification → All bidders simultaneously | GAP |
| CH08-007 | Ch 08 §8.2 | Notification must state: name of intended awardee, their evaluated price | Notification content | Award → Notification content fields | GAP |
| CH08-008 | Ch 08 §8.2 | LOA cannot be issued until standstill period expires | LOA timing gate | Award → LOA → Standstill expiry gate | GAP |
| CH08-009 | Ch 08 §8.2 | If appeal received during standstill: standstill extended until appeal resolved | Standstill extension | Award → Appeal → Standstill extension | GAP |

### 8.3 — Debriefing

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH08-010 | Ch 08 §8.3 | Unsuccessful bidder may request debriefing within 3rd working day of standstill notification | Debriefing request | Award → Debriefing request window (3 working days) | GAP |
| CH08-011 | Ch 08 §8.3 | PE must conclude debriefing by 5th working day of standstill | Debriefing deadline | Award → Debriefing → 5 working day completion | GAP |
| CH08-012 | Ch 08 §8.3 | Debriefing: explain why bid was unsuccessful; must not reveal other bidders' confidential info | Debriefing content | Award → Debriefing notes | GAP |

### 8.4 — Appeal Bodies and Process

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH08-013 | Ch 08 §8.4 | Appeal body for HLPC/SHLPC procurements: Procurement Appeal Board (PAB) | Appeal routing | Award → Appeal → PAB (HLPC/SHLPC) | GAP |
| CH08-014 | Ch 08 §8.4 | Appeal body for MPC procurements: Ministerial Procurement Appeal Committee (MPAC) | Appeal routing | Award → Appeal → MPAC (MPC) | GAP |
| CH08-015 | Ch 08 §8.4 | Appeal body for DPC/PPC procurements: Departmental/Provincial Procurement Appeal Committee (DPAC/PPAC) | Appeal routing | Award → Appeal → DPAC/PPAC (DPC/PPC) | GAP |
| CH08-016 | Ch 08 §8.4 | Appeal body for RPC procurements: Regional Procurement Appeal Committee (RPAC) | Appeal routing | Award → Appeal → RPAC (RPC) | GAP |
| CH08-017 | Ch 08 §8.4 | PAB appeal fee: LKR 100,000 | Appeal fee | Award → Appeal → Fee field (PAB: 100,000) | GAP |
| CH08-018 | Ch 08 §8.4 | MPAC appeal fee: LKR 25,000 | Appeal fee | Award → Appeal → Fee field (MPAC: 25,000) | GAP |
| CH08-019 | Ch 08 §8.4 | DPAC/PPAC appeal fee: LKR 10,000 | Appeal fee | Award → Appeal → Fee field (DPAC/PPAC: 10,000) | GAP |
| CH08-020 | Ch 08 §8.4 | RPAC appeal fee: LKR 5,000 | Appeal fee | Award → Appeal → Fee field (RPAC: 5,000) | GAP |
| CH08-021 | Ch 08 §8.4 | PAB must report within 10 working days of receiving appeal | Appeal timeline | Award → Appeal → PAB 10-day deadline | GAP |
| CH08-022 | Ch 08 §8.4 | MPAC must report within 10 working days | Appeal timeline | Award → Appeal → MPAC 10-day deadline | GAP |
| CH08-023 | Ch 08 §8.4 | DPAC/PPAC must report within 10 working days | Appeal timeline | Award → Appeal → DPAC/PPAC 10-day deadline | GAP |
| CH08-024 | Ch 08 §8.4 | RPAC must report within 5 working days | Appeal timeline | Award → Appeal → RPAC 5-day deadline | GAP |
| CH08-025 | Ch 08 §8.4 | Appeal fee returned if appeal is upheld; forfeited if dismissed | Fee handling | Award → Appeal → Fee return/forfeit | GAP |

### 8.5 — Letter of Acceptance (LOA)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH08-026 | Ch 08 §8.5 / Annexure I | LOA for Supply contracts: standard format fields — bidder name, procurement reference, awarded lot/items, contract value, delivery date, payment terms | LOA generation | Award → LOA → Supply contract template | GAP |
| CH08-027 | Ch 08 §8.5 / Annexure II | LOA for Works contracts: standard format fields — contractor name, project description, contract value, completion period, performance security requirement | LOA generation | Award → LOA → Works contract template | GAP |
| CH08-028 | Ch 08 §8.5 | LOA must be issued within standstill expiry date and bid validity | LOA timing | Award → LOA → Timing validation | GAP |
| CH08-029 | Ch 08 §8.5 | LOA acceptance: contractor must return signed copy within specified days | LOA acceptance | Award → LOA → Acceptance tracking | GAP |

---

## CHAPTER 09 — CONTRACT MANAGEMENT

### 9.1 — Contract Administration

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH09-001 | Ch 09 §9.1 | Contract administrator (Project Manager/Consultant) appointed for each contract | Contract admin | Post-Award → Contract administrator assignment | GAP |
| CH09-002 | Ch 09 §9.1 | Contract variations must be documented and approved before execution | Variation management | Post-Award → Variation → Pre-approval requirement | PARTIAL |
| CH09-003 | Ch 09 §9.1 | Payment claims: contractor submits; Project Manager certifies; AO approves; Finance disburses | Payment workflow | Post-Award → Payment claim workflow | GAP |

### 9.2 — Variation Review Committee (VRC)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH09-004 | Ch 09 §9.2 | VRC formation: minimum 3 members; at least 1 technical expert | VRC formation | Post-Award → VRC → Committee setup | GAP |
| CH09-005 | Ch 09 §9.2 | Variation ≤7.5% of contract AND <DPC threshold: approved by AO/HD/PD | VRC threshold | Post-Award → Variation → AO approval (≤7.5%) | GAP |
| CH09-006 | Ch 09 §9.2 | Variation >7.5%≤10% of contract OR >DPC threshold but <MPC threshold: approved by CAO | VRC threshold | Post-Award → Variation → CAO approval (7.5-10%) | GAP |
| CH09-007 | Ch 09 §9.2 | Variation >10% of contract OR >MPC threshold: approved by Cabinet of Ministers | VRC threshold | Post-Award → Variation → Cabinet approval (>10%) | GAP |
| CH09-008 | Ch 09 §9.2 | Aggregate variations tracked; cumulative total determines applicable threshold | Aggregate variation | Post-Award → Variation → Cumulative % tracker | GAP |
| CH09-009 | Ch 09 Annexure I | Variation form: original TCE, original contract price, contingency %, aggregate variations to date (amount and % of original contract price), itemized variation table (original qty/amount vs variation qty/amount), revised contract price | Variation form | Post-Award → Variation → Standard form fields | GAP |

### 9.3 — Dispute Resolution

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH09-010 | Ch 09 §9.3 | Disputes: first by negotiation; then DAB (Dispute Adjudication Board); then arbitration | Dispute tracking | Post-Award → Dispute → Resolution pathway | GAP |
| CH09-011 | Ch 09 §9.3 | DAB decision binding unless challenged within 28 days | DAB tracking | Post-Award → DAB → Decision tracking | GAP |

---

## CHAPTER 10 — DEBARMENT AND BLACKLISTING

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH10-001 | Ch 10 §10.1 | Debarment levels: national (all government), ministry, PE (institution-level) | Debarment levels | Bidder Database → Debarment status field | GAP |
| CH10-002 | Ch 10 §10.1 | Grounds for debarment: false information, unauthorized bid withdrawal, refusal to accept award, performance failure, ethics violations, collusion, prohibited practices | Debarment grounds | Bidder Database → Debarment → Grounds list | GAP |
| CH10-003 | Ch 10 §10.1 | Debarment procedure: written show-cause notice → 2-week response period → independent committee (1 or 3 members, at least 1 external) → hearing within 10 working days → report → authority decision | Debarment process | Admin → Debarment → Process workflow | GAP |
| CH10-004 | Ch 10 §10.1 | Debarment appeal: appeal within 5 working days of decision | Debarment appeal | Admin → Debarment → Appeal window | GAP |
| CH10-005 | Ch 10 §10.1 | Debarment appeal committee: 1 or 3 members, at least 2 external for 3-member committee | Appeal committee | Admin → Debarment → Appeal committee | GAP |
| CH10-006 | Ch 10 §10.1 | Debarment appeal committee must conclude within 21 calendar days | Appeal timeline | Admin → Debarment → 21-day appeal deadline | GAP |
| CH10-007 | Ch 10 §10.1 | During debarment: bidder cannot bid but must complete existing contracts | Debarment enforcement | Bidder Database → Debarment → Bid exclusion | GAP |
| CH10-008 | Ch 10 §10.1 | National debarment register maintained by NPC; PE must check before contract award | Debarment check | Award → Bidder → NPC debarment check | GAP |
| CH10-009 | Ch 10 §10.1 | PE-level debarment maintained by PE; shared with NPC | PE debarment | Admin → Debarment → NPC reporting | GAP |

---

## CHAPTER 11 — ESSENTIAL REQUIREMENTS

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH11-001 | Ch 11 §11.1 | Essential requirements: minimum non-negotiable standards that all procurements must meet | System-wide compliance | All screens → Compliance baseline | N/A |
| CH11-002 | Ch 11 §11.1 | Essential requirement: competition maintained; no restriction unless technically justified | Competition monitoring | Evaluation → Competition check | N/A |
| CH11-003 | Ch 11 §11.1 | Essential requirement: transparency — all decisions documented and auditable | Audit trail | Audit Trail → All decisions logged | IMPLEMENTED |
| CH11-004 | Ch 11 §11.1 | Essential requirement: value for money — lowest evaluated cost wins | Award basis | Award → Lowest evaluated cost | IMPLEMENTED |
| CH11-005 | Ch 11 §11.2 | CIDA registration required for Works contractors (grade-specific by contract value): CS2-C9 for civil, EM1-EM5 for electro-mechanical, SP1-SP5 for specialized, GP-P, GP-B1 through GP-B4 for general | Contractor CIDA check | Stage 4 → PQ → CIDA grade verification | GAP |

---

## CHAPTER 12 — IS PROCUREMENT

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH12-001 | Ch 12 §12.1 | IS (Information Systems) procurement: special rules for hardware, software, consulting | IS tender type | Tender Creation → IS procurement flag | GAP |
| CH12-002 | Ch 12 §12.1 | IS specifications: functional requirements preferred over technical specifications | IS specifications | Spec Library → IS category → Functional spec | GAP |
| CH12-003 | Ch 12 §12.1 | Software procurement: total cost of ownership (TCO) evaluation | IS price evaluation | Price Evaluation → TCO methodology | GAP |

---

## CHAPTER 13 — ELECTRONIC GOVERNMENT PROCUREMENT (e-GP)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH13-001 | Ch 13 §13.1 | e-GP platform: government's national e-procurement system; PE must use for above-threshold procurements | e-GP integration | System → e-GP integration | GAP |
| CH13-002 | Ch 13 §13.1 | e-GP: publication of IFB through national e-procurement portal | Publication | Publication → e-GP portal link | GAP |
| CH13-003 | Ch 13 §13.1 | e-GP: bid submission through portal for eligible procurements | Bid submission | Bid Opening → e-GP bid import | GAP |

---

## CHAPTER 14 — PUBLIC-PRIVATE PARTNERSHIPS (PPP)

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH14-001 | Ch 14 §14.1 | PPP: special regime; separate PPP Act governs; procurement manual provisions supplementary | PPP flag | Tender Creation → PPP flag | N/A |
| CH14-002 | Ch 14 §14.1 | PPP: BOI involved for foreign investment component | BOI flag | Tender Creation → BOI flag | N/A |

---

## CHAPTER 15 — SUSTAINABLE PROCUREMENT

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CH15-001 | Ch 15 §15.1 | Sustainable procurement: consider environmental, social, and economic sustainability factors | Sustainability criteria | Spec Library → Sustainability criteria | GAP |
| CH15-002 | Ch 15 §15.1 | Environmental criteria: energy efficiency, emissions, recyclability, use of hazardous materials | Env criteria | Spec Library → Environmental criteria | GAP |
| CH15-003 | Ch 15 §15.1 | Social criteria: fair labor practices, local employment, gender considerations | Social criteria | Spec Library → Social criteria | GAP |
| CH15-004 | Ch 15 §15.1 | Life cycle costing: mandatory for energy-consuming equipment (HVAC, IT, vehicles) | LCC mandate | Tender Creation → LCC flag for energy equipment | GAP |

---

## CROSS-CUTTING: AUDIT TRAIL AND RECORD KEEPING

| Rule ID | Manual Citation | Rule Text | TenderStudio Feature | Screen / Field | Status |
|---------|----------------|-----------|---------------------|----------------|--------|
| CC-001 | Ch 01 / Ch 07 / Ch 08 | All evaluation decisions must be documented with rationale | Audit trail | Audit Trail → Decision rationale logging | IMPLEMENTED |
| CC-002 | Ch 07 §7.7 | BEC evaluation records retained for minimum 5 years after contract completion | Record retention | Archive → Retention policy | GAP |
| CC-003 | Ch 08 §8.6 | All procurement records must be available for external audit on demand | Audit access | Audit Trail → External auditor export | GAP |
| CC-004 | Ch 04 §4.6 | Procurement records classified: confidential (TCE, individual evaluations) vs. public (award info) | Record classification | Audit Trail → Classification flags | GAP |
| CC-005 | Ch 07 §7.8 | No phone/verbal communication for official evaluation purposes; all must be written | Written comm enforcement | Clarification → Written channel only | IMPLEMENTED |

---

## SUMMARY TABLE: STATUS COUNT BY CHAPTER

| Chapter | IMPLEMENTED | PARTIAL | GAP | N/A | Total |
|---------|-------------|---------|-----|-----|-------|
| Ch 01 Ethics | 0 | 1 | 11 | 1 | 13 |
| Ch 02 Committees | 4 | 10 | 30 | 0 | 44 |
| Ch 03 Methods | 1 | 3 | 14 | 0 | 18 |
| Ch 04 Planning | 1 | 3 | 15 | 0 | 19 |
| Ch 05 Documents | 3 | 14 | 30 | 0 | 47 |
| Ch 06 Publication/Opening | 2 | 10 | 26 | 0 | 38 |
| Ch 07 Evaluation | 7 | 14 | 31 | 0 | 52 |
| Ch 08 Award | 1 | 4 | 25 | 0 | 30 |
| Ch 09 Contract Mgmt | 0 | 2 | 9 | 0 | 11 |
| Ch 10 Debarment | 0 | 0 | 9 | 0 | 9 |
| Ch 11-15 Other | 2 | 0 | 9 | 4 | 15 |
| Cross-Cutting | 3 | 0 | 2 | 0 | 5 |
| **TOTAL** | **24** | **61** | **211** | **5** | **301** |

**Overall compliance readiness**: 24 IMPLEMENTED (8%) + 61 PARTIAL (20%) = 28% addressed; **72% gap**

---

*End of Compliance Citation Map v1.0*
*Generated from word-by-word reading of National Procurement Manual 2024 (Goods, Works and Non-Consulting Services)*
*Cross-referenced with TenderStudio SYSTEM_SPECIFICATION.md v3.0*
