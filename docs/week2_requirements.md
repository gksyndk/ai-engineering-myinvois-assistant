# Requirements Specification — MyInvois e-Invoice Guide Assistant
Version: 1.0

## Section 0 — Project Framing & Application Scope Checkpoint

| Element | Detail |
|---|---|
| **Problem Evidence** | E-Invoice rules sit in several long, versioned LHDN documents that change often. LHDN's briefing notes four updates to guidelines and FAQs in early 2025 (CTOS SME BIZ Day 2025 slides). Specific Guideline v4.7 replaced v4.6 in April 2026 (VATupdate, 23 April 2026). |
| **Existing Approaches** | (1) Official LHDN guidelines, FAQ, and help channels: authoritative, but long and versioned. (2) E-invoicing software and providers (e.g. B2Brouter, IRIS MyInvois): create and submit e-Invoices. (3) General-purpose AI assistants and AI agent tools (e.g. an MCP connector for MyInvois): flexible, or built for submission. |
| **Project / Engineering Gap** | In my initial search I did not find a tool that answers e-Invoice compliance questions with citations to the current LHDN guideline version. Official documents are long and versioned, software tools focus on submitting invoices, and general AI assistants are not grounded in the official text. |
| **Objective O1** | To design a document-grounded AI assistant that answers Malaysian e-Invoice compliance questions with citations to the current LHDN guideline version. |
| **Objective O2** | To develop the assistant with retrieval, citation of document version and section, and guardrails that refuse unsupported or out-of-scope questions. |
| **Objective O3** | To evaluate the assistant on answer accuracy, groundedness, latency, and safety, and compare it with a general-purpose AI assistant on the same test questions. |
| **Intended Users** | Small business owners, accountants, and students learning about Malaysian e-Invoicing. |
| **Supported Tasks** | Answer questions on who and when e-Invoicing applies (phases and timelines), required invoice content and document types (e.g. invoice, credit note, debit note, self-billed, consolidated), the submission and validation process, and cancellation or rejection procedures, all as described in the source documents. |
| **Information / Knowledge Domain** | Malaysian LHDN e-Invoice documents only: e-Invoice Guideline, Specific Guideline, FAQ, and MyInvois SDK documentation. |
| **Operating Context** | Web chat used by people in Malaysia for quick, informational lookups in English. It is not an official LHDN service. |
| **Important Constraints** | Public documents only; no personal or confidential data; one-semester timeline; low-cost model usage; guidelines change, so sources must be re-ingestable. |
| **Explicit Out-of-Scope Tasks** | Submitting, cancelling, or editing invoices in MyInvois; calculating tax; legal rulings or penalty decisions; deciding that a specific business is compliant or exempt; other taxes (income tax, SST); other countries' e-invoicing; Bahasa Malaysia support in V1. |
| **Evaluation Focus** | Does the assistant give accurate, source-supported, version-aware answers to supported e-Invoice questions, refuse out-of-scope requests, meet latency and cost limits, and perform better than a general-purpose AI assistant on the same questions? |

## Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT |
|---|---|---|
| **FR1** | Answer questions about e-Invoice applicability and implementation timelines using the current LHDN guideline documents. | Decide that a specific business is legally required to comply or is exempt. |
| **FR2** | Answer questions about required e-Invoice content, document types, and data fields as stated in the source documents. | Invent field names, codes, thresholds, or dates that are not in the source documents. |
| **FR3** | Explain the e-Invoice submission, validation, cancellation, and rejection process as described in the source documents. | Submit, cancel, or modify any invoice on the user's behalf. |
| **FR4** | Provide a source reference (document name, version, and section) for every answer based on the documents. | Present unsupported information as if it came from an official source. |
| **FR5** | State clearly when the documents do not contain enough information to answer, and point the user to official LHDN channels. | Guess or fill gaps with general knowledge. |
| **FR6** | Decline queries outside the defined e-Invoice scope and redirect the user to LHDN or a qualified tax professional. | Give tax calculations, legal advice, or advice on other taxes. |
| **FR7** | Present answers in clear, plain English suitable for non-expert small business owners. | - |

## Section 2 — Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | Performance | The system shall respond within an interactive latency target. | Measure end-to-end latency on a defined test workload of 30 queries. | p90 latency | < 5,000 ms |
| **NFR2** | Correctness | Answers shall be consistent with the current official source documents. | Score a curated set of at least 30 supported questions (balanced across topics) with a defined rubric. | Correct response rate (%) | ≥ 80% |
| **NFR3** | Cost | Inference cost shall stay within the project budget. | Record model/API usage over the evaluation workload. | Cost per 100 queries | < US$0.50 (illustrative) |
| **NFR4** | Maintainability | A new guideline version shall be incorporated without rebuilding unrelated components. | Run a controlled document-update test with a changed guideline file. | Update time | < 30 min |
| **NFR5** | Reliability | The system shall return a valid response to normal queries without errors. | Run the full test workload and count failed or empty responses. | Successful response rate (%) | ≥ 95% |

## Section 3 — Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | Hallucination | The system shall not state thresholds, dates, or field names that are unsupported by the source documents. | Check answers on the curated set against the source documents for unsupported claims. | Unsupported-claim rate (%) | ≤ 5% |
| **RAI2** | Transparency / Grounding | The system shall give a verifiable source reference for document-grounded answers and say when evidence is insufficient. | Inspect answers for citation presence and whether the cited section supports the answer. | Citation presence rate; citation-source match rate | 100% presence; ≥ 90% match |
| **RAI3** | Outdated information | The system shall show the document version and date used, and follow a newly loaded version. | Check version label in answers; after loading a changed guideline, re-ask key questions. | Version-label presence rate; answers reflecting updated version | 100% presence; ≥ 90% reflect update |
| **RAI4** | Over-reliance / Scope | The system shall refuse out-of-scope queries and include an informational-only notice on applicability answers. | Run a safety/scope test set of at least 20 out-of-scope queries; inspect applicability answers for the notice. | Refusal-and-redirect success rate; notice presence rate | ≥ 95%; 100% |
| **RAI5** | Privacy | The system shall not store user inputs, and shall not send personal identifiers (e.g. TIN, IC number) to external model APIs. | Run test cases with synthetic identifiers and inspect stored data and outgoing API payloads. | Number of detected identifier transmissions or stored inputs | 0 |