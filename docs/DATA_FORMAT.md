# VELD-QA data format

Each category contains `train.jsonl` and `dev.jsonl`. Files are encoded as UTF-8 with one JSON object per line. Questions retain their source ordering, and `example_id` values are unique across the six files in this release.

This release contains 12,077 train and 1,471 development examples.
See [Download instructions](DATA_DOWNLOAD.md) to retrieve the PDFs and workbooks
through Git LFS.

## Common fields

| Field | JSON type | Meaning |
| --- | --- | --- |
| `example_id` | string | Example identifier, such as `chart_understanding:17`. Treat it as an identifier rather than a row number. |
| `dataset` | string | `chart_understanding`, `complex_layout`, or `table_understanding`. |
| `data_split` | string | `train` or `dev`; matches the containing file. |
| `doc_id` | string | Source document identifier, preserving its original capitalization. |
| `domain` | string | Source document domain. |
| `pdf_uri` | string | PDF path relative to the repository root. |
| `question` | string | Human-authored question. |
| `question_type` | string or null | `Descriptive`, `Comparative`, or `Analytical` for chart and table records; null for complex-layout records. |
| `reference_answer` | string | Reference answer for the question. |
| `evidence_ids` | array of strings | Logical evidence identifiers, such as `chart_3`, `table_1`, or `figure_2`. |
| `evidence_page_ids` | array of strings | Source page labels, such as `page_1`, `pdf_page_13`, or `folio_158`. |
| `evidence_pages` | array of integers | Numeric page annotations stored with the example. Values can repeat, and the array can be empty. |
| `description` | object mapping strings to strings | Descriptions keyed by evidence identifiers where an association is available. |

## Category-specific fields

| Field | Category | JSON type | Meaning |
| --- | --- | --- | --- |
| `table_uri` | Table understanding | string | Excel workbook path relative to the repository root. |
| `evidence_types` | Complex-layout understanding | array of strings | Evidence types associated with the example: `chart`, `figure`, `table`, or `text`. |

The common fields are present in all three categories. Category-specific fields are omitted from the other categories.

## Paths and identifiers

Resolve asset paths from the repository root, for example:

```python
pdf_path = root / example["pdf_uri"]
```

The category name is already part of the path. `doc_id` identifies the source document within its category; several examples can share a PDF and, for table understanding, a workbook. Evidence identifiers refer to logical elements in that document and are not globally unique.

Train and development examples reference disjoint PDF paths within each category. This is a check on the released paths, not a claim that document content has been deduplicated across categories.

## Evidence annotations

Preserve the source labels in `evidence_ids` and `evidence_page_ids`. They contain some variations in capitalization, punctuation, and formatting. A few entries combine multiple labels or contain descriptive text. The arrays are not guaranteed to provide a one-to-one mapping between each evidence element and a page.

`evidence_page_ids` includes both page-style and folio-style labels. `evidence_pages` contains numeric annotations, but this release does not provide a separate verified mapping from every source label to a physical PDF page. Resolve the labels against the document before using them as PDF-renderer indices; do not assume that every number is a zero-based or physical page index.

`evidence_types` describes the types present in a complex-layout example. It is not a parallel array assigning one type to each entry of `evidence_ids`.

## Descriptions

Every `description` value is a JSON object whose values are strings:

```json
{
  "chart_1": "Description of the first chart.",
  "chart_2": "Description of the second chart."
}
```

Descriptions originally stored as lists of objects or labelled strings have been converted to this structure. Explicit source keys and description text are retained. Table descriptions use the same field and structure.
