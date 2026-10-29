# Architecture Decisions and Traceability — MyInvois e-Invoice Guide Assistant

 
## Simplest Architecture (Step C1)
User Query → AI Application → AI Capability → Response
 
This simple flow cannot meet FR4, RAI1, RAI2, RAI3, or RAI4, because a language model alone is not grounded in the LHDN documents, cannot cite versioned sources, and has no scope control. The components below are added only for those reasons.
 
## Architecture Needs from Requirements (Step C2)
 
| Requirement ID | What does the requirement need? | Architecture Response |
|---|---|---|
| FR1-FR3 / NFR2 / RAI1 | Answers must come from the official LHDN documents, not general knowledge. | Knowledge source (LHDN documents) and a knowledge access component |
| FR4 / RAI2 | Every answer needs a verifiable document, version, and section reference. | Source tracking: section and version metadata kept with the knowledge, plus source reference labelling |
| RAI3 / NFR4 | Answers must show the version and follow a newly released guideline. | Versioned knowledge source and a document update process |
| FR5 / FR6 / RAI4 | Out-of-scope or unsupported queries must be refused and redirected. | Scope / safety control (input check, "not found" handling, informational notice) |
| RAI5 | Personal identifiers must not be stored or sent to an external model. | Identifier filtering in the scope / safety control; no storage of user inputs |
| NFR1 / NFR3 | Responses must meet latency and cost limits. | A short interaction path, with a limited number of retrieved sections per query |
| O3 evaluation | Accuracy, citations, refusals, and latency must be measurable. | Evaluation point that logs test queries and responses |
 
## V1 Architecture Diagram Specification (Step C3)
Diagram file: `docs/week2_architecture_v1.png`
Colours: green = knowledge and data, blue = AI capability, orange = controls, purple = evaluation, grey = user interface.
 
| Component (label) | Colour | Flow in | Flow out |
|---|---|---|---|
| User (SME owner, accountant, student) | Grey | - | "question" to Application Interface |
| Application Interface (chat) | Grey | "question"; "final answer" | "question" to Scope / Safety Control; "answer" to User |
| Scope / Safety Control (input check, identifier filter, informational notice) | Orange | "question" | "in-scope question" to Knowledge Access; "refusal" to Application Interface |
| Knowledge Access | Green | "in-scope question" | "relevant sections + metadata" to Language Model Service |
| Knowledge Source (LHDN guideline documents with version and date) | Green | "new version" from Document Update Process | "documents" to Knowledge Access |
| Document Update Process | Green | "new guideline file" | "new version" to Knowledge Source |
| Language Model Service | Blue | "question + sections" | "draft answer" to Source Reference / Version Labelling |
| Source Reference / Version Labelling | Orange | "draft answer + metadata" | "answer with citation, version, notice" to Application Interface; "not found" message if sections are insufficient |
| Evaluation Point | Purple | "queries and responses" from Application Interface | "scores and logs" |
 
## Architecture Decision Record (ADR)
 
| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| ADR1: Include knowledge access over the LHDN documents | FR1-FR4, NFR2, RAI1, RAI2: answers must be grounded in official documents and cite them | (a) Language model only; (b) all documents placed in every request; (c) static keyword search only; (d) dynamic knowledge access | Dynamic knowledge access component | More system complexity and a risk of missing relevant sections, but better grounding and traceable citations |
| ADR2: Include an explicit scope / safety control | FR5, FR6, RAI4, RAI5: out-of-scope queries must be refused and identifiers must not reach the model | (a) Instruction to the model only; (b) explicit scope / safety control | Explicit scope / safety control | Possible false refusals and extra processing time (NFR1) |
| ADR3: Keep a versioned knowledge source with an update process | RAI3, NFR4: answers must show the version and follow new guideline releases | (a) One-time load of documents; (b) versioned source with update process | Versioned source with update process | Ongoing maintenance effort and metadata handling, but answers stay current and version-labelled |
 
## Initial Requirements Traceability Matrix
 
| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR4 / RAI2 | Source citation and support | Knowledge Source with metadata; Source Reference / Version Labelling | Citation audit on the 30-question set |
| O1 | RAI1 / NFR2 | Hallucination control; answer correctness | Knowledge Access; Language Model Service | Unsupported-claim and correctness scores on the 30-question set |
| O2 | FR6 / RAI4 | Out-of-scope refusal | Scope / Safety Control | Results on the 20 out-of-scope test queries |
| O2 | RAI3 / NFR4 | Version awareness | Versioned Knowledge Source; Document Update Process | Document-update test with a changed guideline file |
| O3 | NFR1 | Response latency | Short interaction path; limited retrieved sections | p90 latency over the 30-query workload |
| O3 | NFR2 | Baseline comparison | Evaluation Point | Scores of the assistant vs a general-purpose AI assistant |