# Week 1 Project Scope

| Field | Detail |
|---|---|
| **Project Name** | MyInvois e-Invoice Guide Assistant |
| **Domain** | Taxation / Business Compliance (Malaysia) |

## Problem Statement
Malaysian small businesses must follow e-Invoice rules that are spread across several long, versioned LHDN documents and keep changing. Finding which rule applies to a business, and which version is current, is slow and error-prone. A document-grounded AI assistant can answer in seconds, cite the exact guideline version and section, and refuse questions the documents do not support.

## Evidence of the Problem
- Evidence 1: LHDN's own briefing states that the e-Invoice guidelines and FAQs were updated four times between 1 January 2025 and mid-2025, so users must track many changing documents.
  Source / citation: CTOS SME BIZ Day 2025, "Navigating the e-Invoicing" (LHDN speaker slides), https://ctoscredit.com.my/wp-content/uploads/2025/06/CTOS-SME-BIZ-DAY-2025-Navigating-the-e-Invoicing_Dr-Rasyidah-Che-Rosli_LHDN.pdf
- Evidence 2: A new Specific Guideline version (4.7) replaced version 4.6 in April 2026, showing guidance keeps changing.
  Source / citation: VATupdate, 23 April 2026, https://www.vatupdate.com/2026/04/23/malaysia-updates-e-invoicing-framework-specific-guide-v4-7-issued-and-phase-4-relaxation-extended-to-31-december-2027/

## Initial Related Work (at least 3)
- Study / solution 1: Official LHDN e-Invoice Guideline and Specific Guideline documents
  Source / citation: https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf
  Key limitation: Authoritative but long, versioned PDFs; no cross-document question answering.
- Study / solution 2: MyInvois SDK and FAQ microsite
  Source / citation: https://sdk.myinvois.hasil.gov.my/
  Key limitation: Aimed at developers; hard for small business owners to find compliance answers.
- Study / solution 3: Third-party summaries and general-purpose AI assistants
  Source / citation: [TO FILL: one blog or firm summary URL, or a note from your own test of a general AI]
  Key limitation: May be outdated, conflicting, or not grounded in the current official version.

## Engineering Gap / Limitation
No tool gives source-cited, version-aware answers across LHDN's e-Invoice guideline documents. Existing options are long PDFs, developer-focused pages, or ungrounded summaries that can be outdated.

## Initial Project Objectives
- O1: To design a document-grounded AI assistant for Malaysian e-Invoice questions.
- O2: To develop the assistant with retrieval, citation of document version and section, and guardrails that refuse unsupported or out-of-scope questions.
- O3: To evaluate the assistant on answer accuracy, groundedness, latency, and safety.

## Target Users
Small business owners, accountants, and students learning about Malaysian e-Invoicing.

## Data Sources (at least 2)
- Source 1: LHDN e-Invoice Guideline (Version 4.7) — URL: https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf
- Source 2: LHDN e-Invoice Specific Guideline — URL: https://www.hasil.gov.my/wp-content/uploads/irbm-e-invoice-specific-guideline.pdf
- Source 3 (if any): LHDN e-Invoice FAQ — URL: [TO FILL] / MyInvois SDK — https://sdk.myinvois.hasil.gov.my/

## Similar to EduAdvisor
Both use a document-grounded Q&A design (retrieval over official public documents with source citation) for a clearly defined user group.

## Different from EduAdvisor
Different domain (tax compliance, not education). The documents are versioned and change often, so the assistant must track and show the document version and date. Answers also involve thresholds and phase dates that must be exact.

## Responsible AI Risks (at least 2)
- Risk 1 — Hallucination: the model invents a threshold, date, or field name → a business wrongly believes it is exempt or compliant.
- Risk 2 — Outdated information: an old guideline version is used → advice no longer matches current rules.
- Risk 3 — Over-reliance: answers are treated as official tax advice → business skips checking with LHDN or a tax professional. Mitigation: informational-only notice and a link to the official source.