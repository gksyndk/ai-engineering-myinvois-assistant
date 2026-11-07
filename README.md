# MyInvois e-Invoice Guide Assistant

> Answers Malaysian e-Invoice questions for small businesses, accountants, and students, grounded in official LHDN documents.

## Problem Statement
E-Invoice rules are spread across long, versioned LHDN documents that keep changing. Existing software helps submit e-Invoices but does not explain the rules, and general AI assistants are not grounded in the current guideline. This assistant gives source-cited, version-aware answers using retrieval over the official documents.

## Evidence of the Problem
- LHDN's briefing notes four updates to the guidelines and FAQs in the first half of 2025 (CTOS SME BIZ Day 2025 slides).
- Specific Guideline v4.7 replaced v4.6 in April 2026 (VATupdate, 23 April 2026).

## Initial Related Work / Existing Approaches
- Official LHDN guidelines, FAQ, and help channels — authoritative, but long, versioned, and dependent on staff availability.
- E-invoicing software and providers (e.g. B2Brouter, IRIS MyInvois) — create and submit e-Invoices, but do not explain which rules apply.
- General-purpose AI assistants and AI agent tools — flexible, but not grounded in the current official guideline version.

## Engineering Gap / Limitation
Initial search did not find a tool that answers e-Invoice compliance questions with citations to the current LHDN guideline version. Official documents are long and versioned, software tools focus on submitting invoices, and general AI assistants are not grounded in the official text.

## Project Objectives
- O1: To design a document-grounded AI assistant that answers Malaysian e-Invoice compliance questions with citations to the current LHDN guideline version.
- O2: To develop it with retrieval, version-aware citation, and guardrails.
- O3: To evaluate accuracy, groundedness, latency, and safety, and compare with a general-purpose AI assistant.

## Target Users
Small business owners, accountants, and students learning about Malaysian e-Invoicing.

## Data Sources
- LHDN e-Invoice Guideline v4.8 (30 Aug 2026) — https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf
- LHDN e-Invoice Specific Guideline v4.7 (20 Apr 2026) — https://www.hasil.gov.my/wp-content/uploads/irbm-e-invoice-specific-guideline.pdf
- MyInvois SDK — https://sdk.myinvois.hasil.gov.my/

## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |
| W2 | Requirements, evaluation criteria, V1 architecture | docs/week2_architecture_decisions.md, week2_requirements.md, week2_evaluation_criteria.md, week2_architecture_v1.png | ✅ |
| W3 | Data pipeline (documents) | notebooks/week3_pipeline.ipynb, docs/week3_data_inventory.md, docs/week3_pipeline_decisions.md, docs/week3_pipeline_diagram.png | 🔄 |