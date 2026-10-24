# Week 1 Project Scope

| Field | Detail |
|---|---|
| **Project Name** | MyInvois e-Invoice Guide Assistant |
| **Domain** | Taxation / Business Compliance (Malaysia) |

## Problem Statement
Malaysian small businesses must follow e-Invoice rules that are spread across several long, versioned LHDN documents that keep changing. Existing software helps businesses create and submit e-Invoices, but does not explain which rules apply to them, and general AI assistants are not grounded in the current official guideline. A document-grounded AI assistant can answer compliance questions in seconds, cite the exact guideline version and section, and refuse questions the documents do not support.

## Evidence of the Problem
- Evidence 1: LHDN's own briefing states that the e-Invoice guidelines and FAQs were updated four times between 1 January 2025 and mid-2025, so users must track many changing documents.
  Source / citation: CTOS SME BIZ Day 2025, "Navigating the e-Invoicing" (LHDN speaker slides), https://ctoscredit.com.my/wp-content/uploads/2025/06/CTOS-SME-BIZ-DAY-2025-Navigating-the-e-Invoicing_Dr-Rasyidah-Che-Rosli_LHDN.pdf
- Evidence 2: A new Specific Guideline version (4.7) replaced version 4.6 in April 2026, showing guidance keeps changing.
  Source / citation: VATupdate, 23 April 2026, https://www.vatupdate.com/2026/04/23/malaysia-updates-e-invoicing-framework-specific-guide-v4-7-issued-and-phase-4-relaxation-extended-to-31-december-2027/

## Initial Related Work (at least 3)
- Study / solution 1: Official LHDN e-Invoice guidelines, FAQ, and help channels (help desk and live chat)
  Source / citation: LHDN, "e-Invoice Guideline (Version 4.7)", https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf ; help desk and live chat listed in NST, 2025, https://www.nst.com.my/amp/news/nation/2025/06/1226501/lhdn-e-invoice-implementation-revenue-below-rm5mil-deferred-jan-1-2026
  Key limitation: Authoritative, but the documents are long and versioned, and live help depends on staff availability.
- Study / solution 2: E-invoicing software and service providers (e.g. B2Brouter, IRIS MyInvois)
  Source / citation: B2Brouter, https://www.b2brouter.net/de/international/malaysia/ ; IRIS Business, https://www.irisbusiness.com/wp-content/uploads/2024/08/Intimation_GoLive_IRISMyeinvois_01Aug2024_Signed.pdf
  Key limitation: They create and submit e-Invoices; they do not answer which compliance rule applies to a given business.
- Study / solution 3: General-purpose AI assistants and AI agent tools (e.g. an MCP connector for MyInvois)
  Source / citation: [TO FILL: your own test of a general AI assistant, with date and tool name] ; MyInvois MCP connector, https://skillsplayground.com/connectors/app-wishpool-malaysia-invoice/
  Key limitation: General assistants are not grounded in the current official guideline version, and the connector is built for submitting invoices, not explaining rules.

## Engineering Gap / Limitation
In my initial search I did not find a tool that answers e-Invoice compliance questions with citations to the current LHDN guideline version. Official documents are long and versioned, software tools focus on submitting invoices, and general AI assistants are not grounded in the official text.

## Initial Project Objectives
- O1: To design a document-grounded AI assistant that answers Malaysian e-Invoice compliance questions with citations to the current LHDN guideline version.
- O2: To develop the assistant with retrieval, citation of document version and section, and guardrails that refuse unsupported or out-of-scope questions.
- O3: To evaluate the assistant on answer accuracy, groundedness, latency, and safety, and compare its answers with a general-purpose AI assistant on the same test questions.

## Target Users
Small business owners, accountants, and students learning about Malaysian e-Invoicing.

## Data Sources (at least 2)
- Source 1: LHDN e-Invoice Guideline (Version 4.7) — URL: https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf
- Source 2: LHDN e-Invoice Specific Guideline — URL: https://www.hasil.gov.my/wp-content/uploads/irbm-e-invoice-specific-guideline.pdf
- Source 3 (if any): LHDN e-Invoice FAQ — URL: https://sdk.myinvois.hasil.gov.my/faq/ / MyInvois SDK — https://sdk.myinvois.hasil.gov.my/

## Similar to EduAdvisor
Both use a document-grounded Q&A design (retrieval over official public documents with source citation) for a clearly defined user group.

## Different from EduAdvisor
Different domain (tax compliance, not education). The documents are versioned and change often, so the assistant must track and show the document version and date. Answers involve exact thresholds and phase dates. Unlike the software tools in the related work, it explains rules and does not submit invoices.

## Responsible AI Risks (at least 2)
- Risk 1 — Hallucination: the model invents a threshold, date, or field name → a business wrongly believes it is exempt or compliant.
- Risk 2 — Outdated information: an old guideline version is used → advice no longer matches current rules.
- Risk 3 — Over-reliance: answers are treated as official tax advice → business skips checking with LHDN or a tax professional. Mitigation: informational-only notice and a link to the official source.
- Risk 4 — Scope creep: users ask the assistant to submit or fill in invoices, which it does not do → confusion or wrong actions. Mitigation: refuse and point to the MyInvois portal.