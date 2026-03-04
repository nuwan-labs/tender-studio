# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TenderStudio is an intelligent procurement and tender management system designed to assist procurement committees in managing the complete tender evaluation lifecycle. The system transforms paper-heavy procurement processes into a streamlined digital workflow enhanced by locally-hosted AI models.

**Project Status**: Specification phase - no code has been written yet. The `SYSTEM_SPECIFICATION.md` contains the complete functional specification (v2.4).

**Scale**: Single organization, <20 users, <10 concurrent tenders, English only.

## Key Architecture Decisions

- **Client-Server Model**: Web-based client with centralized backend server
- **Session-Based Collaboration**: BEC evaluations occur during physical meetings with shared screen; Secretary operates software
- **Full Offline Support**: Work offline with automatic sync on reconnection
- **Local AI Processing**: All AI models run on backend server (data sovereignty)
- **Strict Role Separation**: Procurement Department and BEC have non-overlapping permissions
- **Forensic Audit Trail**: Immutable, timestamped logs; no deletion; exportable for auditors

## Organizational Structure

### System Administrator (Separate from all procurement)
- Manages user accounts and departments
- Maintains BEC Eligibility Pool (pre-approved list)
- Configures PM visibility settings for BEC work
- Cannot participate in any tender activities

### Procurement Department
- **Procurement Manager**: Creates tenders, assigns committees, monitors progress (READ-ONLY for BEC work), sends clarifications, tracks post-award delivery
- **Trainee Procurement Executive / Procurement Assistant**: Support roles

### Committees (Per-Tender Assignment)

| Committee | Members From | Key Responsibilities |
|-----------|--------------|---------------------|
| BOC | Can include Procurement staff | Witness bid opening, record bidder totals, verify bid security |
| BEC | BEC Eligibility Pool only (never Procurement) | Technical/financial evaluation, report generation |
| DPC | Assigned per-tender | Budget approval (start) and final approval (end) |

## Complete Tender Lifecycle

1. **Creation** (PM) → 2. **DPC Budget Approval** → 3. **Bid Document Prep** → 4. **Publication** → 5. **Pre-Bid Meeting** → 6. **Bid Opening** (BOC) → 7. **BEC Assignment** → 8. **BEC Evaluation** → 9. **Clarifications** → 10. **Report Generation** → 11. **BEC Certification** → 12. **PM Review** → 13. **DPC Final Approval** → 14. **Award** → 15. **Post-Award Tracking** → 16. **Archive**

## Critical Business Rules

### Evaluation Grid Logic
- Requirements marked **"C"** = Compulsory; blank = Non-Compulsory
- Each cell (Bidder × Requirement) decision: **Accept** or **Reject**
- Bidder must provide **Yes declaration + supporting evidence**; BEC verifies both
- Cell decision BLOCKED while clarification threads are active
- Item cannot complete until ALL cells have decisions
- Compulsory Reject = Bidder Non-Responsive for that item

### Evaluation Cell Structure (AI-Assisted)
Each cell contains structured evidence fields:
| Field | Source |
|-------|--------|
| Declaration | Manual (Yes/No dropdown) |
| Offered Value | AI auto-populated from bidder docs, human verified |
| Document Reference | AI auto-populated |
| Page/Section | AI auto-populated |
| Extracted Text | AI auto-populated (RAG from OCR) |
| Evaluator Notes | Manual entry |

### Clarification Threads (Two Levels)
- **Two types**: Bidder (to supplier) and Requestor (to internal equipment requestor)
- **Two levels**: Requirement-level (cell) OR Item-level (general queries for whole item)
- Threads can have multiple back-and-forth messages
- Thread closure requires **Satisfied/Not Satisfied** decision
- Flow: Secretary drafts → Chair approves → PM sends → PM enters response → BEC reviews

### Mandatory Information Fields
- Make, Model, Country of Origin must be provided
- If missing: clarification sent; closure requires Satisfied/Not Satisfied
- Not Satisfied on mandatory info = Non-Responsive

## Key Data Entities

| Entity | Purpose |
|--------|---------|
| Bid Security | Track per-bidder: amount, bank, guarantee number, validity |
| Document Checklist | Standard + custom documents required per bidder |
| Requestor | Per-item (can have multiple); contacted for requirement clarifications |
| Price Schedule | Full breakdown: unit price, transport, VAT, totals |
| Lots/Packages | Optional grouping of items (lot-based or item-based evaluation) |
| Post-Award Tracking | Delivery, installation, commissioning status per item |
| Performance Security | 10% of contract value, tracked after award |

## AI Capabilities (Assist, Not Decide)

| Feature | Behavior |
|---------|----------|
| Document Extraction | Extract specs from tender docs and bidder quotations |
| Evidence Auto-Population | **RAG on OCR'd documents** - AI finds and drafts evidence per requirement; human verifies |
| Verification | **Maker-Checker required**: Two people must verify AI extractions |
| Compliance Analysis | **Highlight relevant text only** - NO auto-suggestions for Accept/Reject |
| Clarification Drafting | **Template-based only** - AI fills placeholders, human writes content |
| Report Generation | **Hybrid** - Tables auto-generated; AI drafts narratives; human edits and verifies |

## Key UI Features

- **Full-Screen Evaluation Grid**: Specs in rows, bidders in columns, per-item view (optimized for projector)
- **Color-Coded Decisions**: Green (Accept), Red (Reject), Gray (Pending), Orange border (clarification active)
- **Price Visibility Toggle**: BEC Chair can show/hide prices during technical evaluation (default: hidden)
- **Session Controls**: Start/end session, attendance, temp write grants, live timer
- **PM Dashboard**: Comprehensive read-only view of all tender progress, financials, deadlines
- **Specification Library**: Reusable specs organized by category with search
- **Bidder Database**: Organization-wide bidder history across tenders
- **Pre-Bid Meeting Management**: Schedule, attendance, Q&A tracking, addendum issuance
- **Post-Award Tracking**: Delivery/installation/commissioning milestones

## Report Generation

**BEC Evaluation Report** (14 sections):
1. Cover Page → 2. Introduction (committees) → 3. Package Summary → 4. Items Table → 5. Process Summary → 6. Suppliers List → 7. Bid Opening → 8. Evaluation of Quotations → 9. Technical Compliance (per item) → 10. Price Schedules (per item) → 11. Recommendations → 12. Clarifications → 13. Conclusion → 14. Signature Table

**Key Report Features**:
- Tables auto-generated from evaluation data (eliminates transcription errors)
- AI drafts narratives; human edits before submission
- Amounts in words + numbers: `[amount_words] (LKR [amount_numeric])`
- Deviation notes appear below relevant compliance table
- Signature table left blank for physical signing
- 5 standard annexures auto-generated (Pre-bid, Clarifications, Bid Opening, Prices, Documents)
- Export: Both Word (.docx) and PDF
- Built-in template editor with placeholder system

## Reference Materials

The `references/` directory contains sample procurement documents from a real tender (PR 211-1: Laboratory Equipment for SLIBTEC):

| File | Content | Used For |
|------|---------|----------|
| `tender_document.txt` | Full tender document | Specification extraction, requirement structure |
| `tender_report.txt` | Completed BEC evaluation report | Report template design (14 sections, 20 tables) |
| `Final_PR 211-1...xlsx` | Excel evaluation workbook | Understanding current manual process |

**Excel Workbook Structure** (current manual evaluation process):
- Sheets 1-5: One per item (requirements in rows, 11 bidders in columns)
- Minutes sheet: Session tracking
- Clarifications sheet: Item-level clarification tracking

**Key Insights from References**:
- 5 items, 11 bidders, 24-42 requirements per item
- Priority "C" = Compulsory requirement
- Manual color coding (Green/Yellow/Red) for pass/needs clarification/fail
- Report requires both Word and PDF export with organizational template matching
