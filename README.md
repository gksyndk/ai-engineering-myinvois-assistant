# MyInvois e-Invoice Guide Assistant

> Answers Malaysian e-Invoice questions for small businesses, accountants, and students, grounded in official LHDN documents.

## Problem Statement
E-Invoice rules are spread across long, versioned LHDN documents that keep changing. This assistant gives source-cited, version-aware answers using retrieval over the official documents.

## Evidence of the Problem
- LHDN's briefing notes four updates to the guidelines and FAQs in the first half of 2025 (CTOS SME BIZ Day 2025 slides).
- Specific Guideline v4.7 replaced v4.6 in April 2026 (VATupdate, 23 April 2026).

## Initial Related Work / Existing Approaches
- Official LHDN guideline PDFs — authoritative, but long and versioned.
- MyInvois SDK and FAQ site — developer-focused.
- Third-party summaries and general AI assistants — may be outdated or ungrounded.

## Engineering Gap / Limitation
No tool gives source-cited, version-aware answers across LHDN's e-Invoice documents.

## Project Objectives
- O1: To design a document-grounded AI assistant for Malaysian e-Invoice questions.
- O2: To develop it with retrieval, version-aware citation, and guardrails.
- O3: To evaluate accuracy, groundedness, latency, and safety.

## Target Users
Small business owners, accountants, and students learning about Malaysian e-Invoicing.

## Data Sources
- LHDN e-Invoice Guideline v4.7 — https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf
- LHDN e-Invoice Specific Guideline — https://www.hasil.gov.my/wp-content/uploads/irbm-e-invoice-specific-guideline.pdf
- MyInvois SDK — https://sdk.myinvois.hasil.gov.my/

## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |