# Data Inventory — Week 3

Pathway: A (PDF/TXT). Raw PDFs are kept in the Colab workspace and are not committed to GitHub (redistribution terms not yet verified).

## Requirement-to-Data Mapping (Step A1)

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| FR1-FR3: answer applicability, required fields, and process questions | Current e-Invoice rules, document types, data fields, and submission process | S01 LHDN e-Invoice Guideline; S02 LHDN e-Invoice Specific Guideline | Official and public (hasil.gov.my), so authoritative. Long, versioned PDFs; S02 contains tables that extract as fragments. |
| FR4 / RAI2: cite document, version, and section | Document name, version, and page identity for every chunk | S01 and S02 (cover page version, page numbers), plus filename and SHA-256 | Printed page numbers match PDF page order on sampled pages (1, 32, 67), so page-level citation is feasible. |
| RAI3 / NFR4: version awareness and easy updates | Version and publication date of each source; a rerunnable pipeline | Cover page text of S01 and S02; pipeline manifest checksums | Versions are read from the PDF cover, not independently verified. |

## Data Inventory (Step A4)

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | IRBM-e-Invoice-Guideline.pdf — https://www.hasil.gov.my/wp-content/uploads/IRBM-e-Invoice-Guideline.pdf | Unstructured text (PDF); main knowledge source for FR1-FR4, RAI2, RAI3 | Version 4.8, published 30 Aug 2026 / Inland Revenue Board of Malaysia (LHDN) | Public on hasil.gov.my; redistribution terms not verified | 62 pages, 75,704 characters, 0 pages with almost no text. Sampled pages 1 and 32 read cleanly. Header "E-INVOICE GUIDELINE (VERSION 4.8)" and page number repeat on every page. Occasional stray spaces inside words (e.g. "utili ses"). |
| S02 | irbm-e-invoice-specific-guideline.pdf — https://www.hasil.gov.my/wp-content/uploads/irbm-e-invoice-specific-guideline.pdf | Unstructured text (PDF); specific rules and worked examples for FR1-FR3 | Version 4.7, published 20 Apr 2026 / Inland Revenue Board of Malaysia (LHDN) | Public on hasil.gov.my; redistribution terms not verified | 132 pages, 154,193 characters, 0 pages with almost no text. Sampled pages 1 and 67 readable. Header "E-INVOICE SPECIFIC GUIDELINE (VERSION 4.7)" repeats on every page. Tables extract as short fragments (e.g. "Number", "included by", "Additional Remarks"). Stray spaces inside some words (e.g. "C ee Sdn Bhd"). |

## Checkpoint A
- FR1-FR3 need the content of both guidelines.
- FR4 / RAI2 need document, version, and page identity on each chunk.
- RAI3 / NFR4 need version and date metadata and a rerunnable pipeline.

## Known Limitations (carried to Topic 4)
- Table content in S02 is partly flattened, so field-level questions (FR2) need review.
- Text extraction inserts stray spaces inside some words, which may affect exact-match checks.
- Table-of-contents pages contain dotted lines and will produce low-value text unless filtered.
- Versions are read from the PDF cover and not independently verified; rights to redistribute are not verified.
- S01 changed from v4.7 to v4.8 during this project, which supports the need for version-aware citation (RAI3).