# Week 3 Pipeline Decisions — MyInvois e-Invoice Guide Assistant

## 1. Traceability

| Topic 2 requirement | Data need | Source | Pipeline evidence |
|---|---|---|---|
| FR1-FR3 / NFR2 | Guideline content as readable text | S01 (v4.8), S02 (v4.7) | Non-empty `prepared_text_S01.jsonl` and `prepared_text_S02.jsonl`; sample records inspected |
| FR4 / RAI2 | Document name, version, and page on every chunk | S01, S02 | Each record holds source file, SHA-256, version label, and page number |
| RAI3 / NFR4 | Version metadata and a rerunnable pipeline | S01, S02 | `week3_manifest.json`, `pipeline_manifest.json`, and Cell 9 repeatability PASS |

## 2. Selected Sources
- S01: LHDN e-Invoice Guideline, Version 4.8 (30 Aug 2026), 62 pages.
- S02: LHDN e-Invoice Specific Guideline, Version 4.7 (20 Apr 2026), 132 pages.
- Data type: unstructured text (text-based PDF). Pathway A only; other pathways were not run because the project does not use that data.

## 3. Part B Comparison (Step B3)
Same input data, same cleaning (extra spaces collapsed), chunk size 500 characters.

| | Approach A: fixed-size chunks | Approach B: page-aware paragraph chunks |
|---|---|---|
| Description | Whole document text cut every 500 characters | Text kept per page, repeated header and page-number line removed, paragraphs combined up to 500 characters, page number kept |
| Chunks from S01 (62 pages) | 151 (average length 499) | 194 (average length 373) |
| Chunks from S02 (132 pages) | 307 (average length 499) | 382 (average length 382) |
| Page number preserved | No | Yes (all 62 of 62 S01 pages and 132 of 132 S02 pages covered) |
| Boundary evidence | In S02, chunk 3 ends with "9. T" and chunk 4 starts with "RANSACTIONS WHICH...", so the heading word "TRANSACTIONS" is cut in two. | S01 page 30 chunk (record S01-0098) starts and ends at sentence and caption boundaries and contains no repeated header text. S02 page 64 chunk (record S02-0192) starts at list item "iii." with no mid-word cut. |

**Selected approach:** page-aware paragraph chunks of up to 500 characters.

**Linked requirement:** FR4 / RAI2 need a verifiable page reference for every answer. Fixed-size chunking over the whole document cannot provide one, and the evidence above shows it also cuts words in half.

**Benefit:** every chunk keeps its page number, so later answers can cite the document, version, and page. Chunk boundaries fall at paragraph ends instead of mid-word.

**Accepted limitations:**
- It produces about 24-28% more chunks (194 vs 151 for S01, 382 vs 307 for S02) that are shorter on average (373-382 vs 499 characters), because chunks never cross a page boundary.
- A rule that continues over two pages is split, and a list can be split across chunks (the S02 page 64 chunk starts at item "iii.", so items i and ii sit in an earlier chunk).
- The first chunks of both approaches come from the table of contents (dotted lines and page numbers) and are low-value text. This is not filtered in V1.
- Extraction still leaves stray spaces inside some words (for example "e -Invoice" on S01 page 30 and S02 page 64), and flattened tables in S02 are not repaired. Both are carried to Topic 4.
- Paragraph packing is simple string logic, not semantic chunking.

## 4. Cleaning Decisions
- Removed the repeated page header line and the standalone page-number line (they add noise to every chunk). The version is stored as metadata instead.
- Evidence for header removal: without cleaning, the first S01 paragraph-aware chunk begins with "E-INVOICE GUIDELINE (VERSION 4.8) 1", showing that the header and page number enter the chunk text. In the cleaned page-aware chunks (e.g. record S01-0098, page 30), the header text no longer appears.
- Collapsed repeated spaces and tabs.
- Not fixed: stray spaces inside words from extraction (e.g. "e -Invoice" on S01 page 30 and S02 page 64) and flattened tables. Both are recorded as limitations for Topic 4.

## 5. ETL vs ELT
ETL. Text is cleaned and chunked before it is stored, because the V1 output is a prepared file (JSONL) and the citation metadata (page, version, checksum) must be attached at preparation time. The data is small (two PDFs) and there is no data warehouse to load raw text into.

## 6. Batch vs Streaming
Batch. LHDN publishes guidelines as periodic versioned documents, not as a continuous stream. When a new version appears, the pipeline is rerun on the new file (supports NFR4).

## 7. Output Locations
- Part B prepared outputs: `data/processed/` (`prepared_text_S01.jsonl`, 139,417 bytes, 194 records; `prepared_text_S02.jsonl`, 282,531 bytes, 382 records). Kept in the local workspace and not committed (see section 11).
- Part B lineage manifest: `data/week3_manifest.json` (committed).
- Part C run manifests: `data/pipeline_v1/documents_S01/pipeline_manifest.json` and `data/pipeline_v1/documents_S02/pipeline_manifest.json` (committed). The Part C `prepared_text.jsonl` files are kept locally and not committed.
- Notebook: `notebooks/week3_pipeline.ipynb` (committed with run outputs).

## 8. Stage Contracts and Failure Responses

| Stage | Contract / evidence | Failure response |
|---|---|---|
| Ingest | Both PDFs exist in `data/raw` and are on the approved source list | Stop for missing input |
| Inspect | PDFs readable; text extracted on every page; version string found on the cover | Stop if no text (scanned PDF needs OCR); flag if the version differs from the expected one |
| Prepare | Page-aware chunking produces non-empty chunks with page numbers | Stop and report the transformation failure |
| Validate | Output files exist, are non-empty, and have SHA-256 fingerprints | Stop for missing or empty output |
| Record lineage | Each source links to its exact prepared output in the manifest | Stop and report a missing mapping or metadata |

## 9. Failure Points for the Pipeline Diagram

| Stage | Plausible failure | Response |
|---|---|---|
| Ingest / Inspect | LHDN publishes a new guideline version, or the URL changes, so an old PDF is processed | Compare the cover version against the expected version in the inventory; flag a mismatch and update the inventory before preparing |
| Prepare | Table pages in S02 extract as fragments, so field names or values lose their structure | Spot-check table pages after extraction; record as a known limitation and review in Topic 4 before relying on field-level answers |
| Validate | Chunks are non-empty but contain only table-of-contents dotted lines | Inspect representative chunks manually; filter low-value pages if needed |

## 10. Pipeline Diagram Specification
Diagram file: `docs/week3_pipeline_diagram.png` (expanded DATA component of `week2_architecture_v1.png`).

SOURCE (S01 and S02 PDFs, green) → INGEST (verify files and approved source list, green) → INSPECT (page count, text per page, version string, blue-grey) → PREPARE (remove repeated header, clean spaces, page-aware paragraph chunks, green) → VALIDATE OUTPUT (files exist, non-empty, SHA-256, orange) → RECORD LINEAGE (source-to-output manifest, purple) → prepared JSONL files, which feed Knowledge Access in the architecture.

Show S01 and S02 as two input boxes with separate outputs. Mark the two failure points from section 9 in red with their responses.

## 11. Provenance and Access Caveats
- Document versions in the Part B manifest (S01 v4.8, S02 v4.7) and the access note were entered manually from the PDF covers and are not independently verified. The Part C `pipeline_manifest.json` files still say "Not verified" for source version and access rights, because the lab's Cell 7 sets that text by default.
- Redistribution terms for the LHDN PDFs have not been verified. The raw PDFs and the prepared JSONL files (which contain guideline text) are not committed; they are kept in the local workspace. Manifests and the notebook are committed as non-sensitive evidence.
- A SHA-256 checksum shows the file has not changed. It does not show that the data is correct or that sharing is permitted.

## 12. Reused Trade-offs from Topic 2
- ADR1 (knowledge access over official documents): this pipeline produces the prepared text that knowledge access will use later. Embeddings and indexing are outside Week 3.
- ADR3 (versioned source with update process): version labels and rerunnable preparation support this decision.