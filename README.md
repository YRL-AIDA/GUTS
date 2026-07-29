# <div align="center"> Ground-truth-Unified-Tabular-Set (GUTS) </div>

**Ground-truth-Unified-Tabular-Set (GUTS)**: A novel benchmark for table header recognition.

## Version

1.0

## Preliminaries

Table header recognition is the task of locating the cells of a table that carry
its structural metadata — column headers, projected (row) headers, and other
label cells — as opposed to the cells that carry data values. Correct header
identification is a prerequisite for higher-level tasks such as table
understanding, question answering over tables, and table-to-database conversion,
because it determines how each data value should be interpreted and addressed.

A central observation behind GUTS is that "the header of a table" is not a
single objective ground truth. The same table can be annotated differently
depending on the *purpose* for which the headers are being identified. GUTS
therefore provides, for the RealHitBench portion of the data, three parallel
annotation viewpoints of the *same* tables:

- **expert** — headers as identified by an independent expert (a data analyst),
  reflecting a general, human-analyst reading of the table.
- **measurement** — headers identified from a measurement perspective: cells are
  treated as headers insofar as they define the dimensions and units along which
  the numeric values are measured.
- **table_normalization** — headers identified from the perspective of relational
  normalization, i.e. the labels needed to convert the table into first and
  second normal form (1NF/2NF).

The PubTables-1M portion is annotated from a single **structure** viewpoint,
inherited from its original cell-topology ground truth.

Providing several viewpoints makes it possible to study the *ambiguity* of the
task itself. As the inter-annotator agreement statistics below show, the three
viewpoints agree strongly on some header types (e.g. column headers) and only
weakly on others (e.g. metadata cells), which quantifies how much the notion of
a "header" depends on the downstream goal. Formal definitions of each viewpoint
and of the labelled cell attributes (`is_column_header`,
`is_projected_row_header`, `is_metadata`) are given in the accompanying
publication (see [References](#references)).

## Dataset statistics

- Unique tables: **1000** (500 PubTables-1M + 500 RealHitBench); table
  annotations: **2000** (RealHitBench is annotated from 3 viewpoints).
- Total cells: **1,840,196**; non-empty: **1,840,196**; header: **28,836**;
  spanning (merged): **16,636**.
  *(The "total cells" figure aggregates both sources across all annotations;
  the per-source `cells_total` for RealHitBench below counts only its three
  viewpoints and therefore differs from this total by the PubTables-1M cell
  count.)*
- Grid cells (rows × cols): **2,384,559**, of which empty (not listed in the
  annotation): **467,674** (19.61%).
- Mean cell value length: **9.09 chars** (2.01 tokens).

### Summary by source

| source       | tables | rows_mean | cols_mean | cells_total | cells_mean | mean_cell_len | numeric_% | alpha_% | alphanum_% | empty_grid_% | header_cells_% | span_cells_% | multilevel_header_% | header_depth_mean |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| PubTables-1M | 500  | 18.0  | 6.7  | 45201   | 90.4   | 9.74 | 60.1 | 24.2 | 11.8 | 5.8  | 8.2 | 7.1 | 69.2 | 1.82 |
| RealHitBench | 1500 | 112.6 | 14.5 | 1794995 | 1196.7 | 8.87 | 81.4 | 11.7 | 5.1  | 19.9 | 1.4 | 0.7 | 66.3 | 3.15 |

### By annotation viewpoint

| source       | viewpoint            | tables | rows_mean | cols_mean | cells_total | cells_mean | mean_cell_len | numeric_% | alpha_% | alphanum_% | empty_grid_% | header_cells_% | span_cells_% | multilevel_header_% | header_depth_mean |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| PubTables-1M | structure            | 500 | 18.0  | 6.7  | 45201  | 90.4   | 9.74 | 60.1 | 24.2 | 11.8 | 5.8  | 8.2 | 7.1 | 69.2 | 1.82 |
| RealHitBench | expert               | 500 | 113.6 | 14.6 | 599023 | 1198.0 | 9.13 | 81.4 | 11.6 | 5.1  | 20.9 | 1.8 | 0.8 | 74.4 | 3.16 |
| RealHitBench | measurement          | 500 | 110.7 | 14.4 | 597291 | 1194.6 | 8.36 | 81.5 | 11.6 | 5.0  | 17.9 | 1.7 | 0.7 | 78.4 | 4.44 |
| RealHitBench | table_normalization  | 500 | 113.6 | 14.6 | 598681 | 1197.4 | 9.12 | 81.2 | 11.9 | 5.1  | 21.0 | 0.7 | 0.8 | 46.2 | 1.86 |

### By domain (unique tables; RealHitBench uses the *expert* annotation)

| source       | domain       | tables | rows_mean | cols_mean | cells_total | numeric_% | multilevel_header_% |
|---|---|---|---|---|---|---|---|
| PubTables-1M | biomedical   | 500 | 18.0  | 6.7  | 45201  | 60.1 | 69.2  |
| RealHitBench | economy      | 131 | 124.1 | 14.6 | 225705 | 80.8 | 63.4  |
| RealHitBench | society      | 85  | 116.1 | 17.8 | 60063  | 66.4 | 72.9  |
| RealHitBench | science      | 79  | 98.0  | 12.3 | 32611  | 87.0 | 91.1  |
| RealHitBench | environment  | 45  | 67.2  | 12.9 | 29370  | 69.3 | 62.2  |
| RealHitBench | education    | 36  | 225.2 | 21.2 | 147367 | 88.1 | 61.1  |
| RealHitBench | employment   | 31  | 124.2 | 10.1 | 30800  | 83.0 | 93.5  |
| RealHitBench | health       | 24  | 70.9  | 13.1 | 17011  | 87.2 | 70.8  |
| RealHitBench | welfare      | 15  | 64.7  | 8.7  | 5933   | 80.2 | 100.0 |
| RealHitBench | sport        | 13  | 35.1  | 15.0 | 4501   | 71.4 | 76.9  |
| RealHitBench | food         | 12  | 139.6 | 13.2 | 14358  | 87.1 | 100.0 |
| RealHitBench | religion     | 10  | 51.0  | 17.8 | 5151   | 80.0 | 80.0  |
| RealHitBench | transport    | 8   | 94.5  | 18.0 | 10416  | 74.4 | 100.0 |
| RealHitBench | energy       | 6   | 160.3 | 14.0 | 12473  | 94.6 | 50.0  |
| RealHitBench | geography    | 4   | 200.5 | 9.8  | 2838   | 67.2 | 50.0  |
| RealHitBench | productivity | 1   | 95.0  | 10.0 | 426    | 65.7 | 100.0 |

### Cell types

| type               | count   | %     |
|---|---|---|
| numeric            | 1468306 | 79.79 |
| numeric_composite  | 19342   | 1.05  |
| alphabetic         | 220693  | 11.99 |
| alphanumeric       | 96527   | 5.25  |
| symbolic           | 35328   | 1.92  |
| empty              | 0       | 0.0   |

### Distributions (over all annotations)

| metric         | mean  | std     | min                | p25                   | median               | p75                 | p95                 | max                |
|---|---|---|---|---|---|---|---|---|
| n_rows         | 88.94 | 375.65  | 3.0                | 16.0                  | 30.0                 | 63.0                | 304.0               | 6783.0             |
| n_cols         | 12.59 | 16.86   | 2.0                | 6.0                   | 9.0                  | 15.0                | 28.0                | 363.0              |
| n_cells        | 920.1 | 6105.83 | 9.0                | 83.75                 | 210.0                | 513.0               | 2656.0              | 122080.0           |
| header_depth   | 2.82  | 4.88    | 0.0                | 1.0                   | 2.0                  | 3.0                 | 7.0                 | 127.0              |
| grid_gap_ratio | 0.18  | 0.2     | 0.0                | 0.015625              | 0.1129349046015713   | 0.2833333333333333  | 0.6042792248864355  | 0.8769676884838442 |
| mean_cell_len  | 9.09  | 6.93    | 1.0732117138742199 | 5.483870967741935     | 7.520074696545285    | 10.54945054945055   | 17.536941176470584  | 88.50420168067227  |
| span_ratio     | 0.05  | 0.07    | 0.0                | 0.0031847133757961785 | 0.022988505747126436 | 0.06060606060606061 | 0.15648347701149404 | 0.9711711711711711 |
| empty_ratio    | 0.0   | 0.0     | 0.0                | 0.0                   | 0.0                  | 0.0                 | 0.0                 | 0.0                |

### Inter-annotator agreement between viewpoints

**`is_column_header`** — 594,605 cells compared over 500 shared tables;
Fleiss' κ = **0.603**; fully matching tables: 58 (11.6%).

| pair                                | agreement | Cohen κ | F1    | Jaccard |
|---|---|---|---|---|
| expert vs measurement               | 0.987     | 0.731   | 0.737 | 0.584   |
| expert vs table_normalization       | 0.984     | 0.558   | 0.565 | 0.393   |
| measurement vs table_normalization  | 0.983     | 0.469   | 0.477 | 0.313   |

**`is_projected_row_header`** — 594,605 cells compared over 500 shared tables;
Fleiss' κ = **0.303**; fully matching tables: 35 (7.0%).

| pair                                | agreement | Cohen κ | F1    | Jaccard |
|---|---|---|---|---|
| expert vs measurement               | 0.93      | 0.61    | 0.648 | 0.48    |
| expert vs table_normalization       | 0.906     | 0.069   | 0.077 | 0.04    |
| measurement vs table_normalization  | 0.901     | 0.044   | 0.051 | 0.026   |

**`is_metadata`** — 594,605 cells compared over 500 shared tables;
Fleiss' κ = **0.073**; fully matching tables: 146 (29.2%).

| pair                                | agreement | Cohen κ | F1    | Jaccard |
|---|---|---|---|---|
| expert vs measurement               | 0.936     | 0.036   | 0.048 | 0.024   |
| expert vs table_normalization       | 0.996     | 0.678   | 0.68  | 0.515   |
| measurement vs table_normalization  | 0.937     | 0.033   | 0.041 | 0.021   |

### Vocabulary

- Unique cell values: **269,070**; unique headers: **6,144**; token vocabulary:
  **140,466**.
- Mean header: 14.64 chars / 2.46 tokens.
- Cells containing dates: 5.42%; percentages: 0.26%; units of measurement in
  parentheses: 0.78%; footnotes: 0.28%.

### Companion resources

```json
{
  "rdf": {
    "n_files": 500,
    "extensions": {
      ".ttl": 500
    },
    "size_mb": 2.14
  },
  "visual_schemes": {
    "n_files": 500,
    "extensions": {
      ".jpg": 500
    },
    "size_mb": 82.76
  },
  "generated_normalized_tables": {
    "n_files": 517,
    "extensions": {
      ".xlsx": 517
    },
    "size_mb": 5.67
  },
  "example": {
    "n_files": 4,
    "extensions": {
      ".xlsx": 2,
      ".jpg": 2
    },
    "size_mb": 0.23
  },
  "html": {
    "PubTables-1M": 500,
    "expert": 500,
    "measurement": 500,
    "table_normalization": 500
  },
  "rdf_schema": {
    "n_schemas": 500,
    "total_classes": 782,
    "total_properties": 4358,
    "mean_classes_per_schema": 1.56,
    "mean_properties_per_schema": 8.72,
    "approx_statements": 26136
  }
}
```

## GUTS in the context of existing corpora

GUTS is built on top of two established corpora — **PubTables-1M** and
**RealHitBench** — and re-annotates a balanced 1,000-table sample of them for the
header-recognition task from multiple viewpoints. The table below situates GUTS
among established table-structure-recognition and table-understanding corpora.

| Dataset | Year | Source | Formats | # Domain | # Table | Annotation |
|---|---|---|---|---|---|---|
| Fang et al. | 2012 | CiteSeerX (scientific PDFs) | PDF | 1 | 255 | Manual + ML (header/data) |
| Seth & Nagy | 2013 | Statistical websites | CSV (from HTML) | 1 | 200 | Manual (critical cells) |
| Adelfio & Samet | 2013 | Various websites | HTML, spreadsheets | Many | 16,048 | CRF (schema + rows) |
| TEXUS | 2019 | Financial reports | PDF → XML/CSV | 1 | 101 | Expert (evaluation) |
| TabbyXL | 2019 | Government statistics | XLSX, CSV | 1 | 200 | Rule-based + GT |
| Paramonov et al. | 2021 | Government statistics | XLSX | 1 | 200 | Rule-based |
| TOMATE | 2021 | HTML repositories (incl. Wikipedia) | HTML | Many | 3,009 | Unsupervised (heuristics) |
| TabularNet | 2021 | Spreadsheets | spreadsheets | 1 | 3,410; 6,988 | Manual (5 roles, regions) |
| PubTables-1M | 2022 | PubMed Central | PDF, HTML | 1 | 947,642 | Automatic (XML) |
| RealHitBench | 2025 | Open-data platforms | LaTeX, HTML, PNG | 24 | 708 | Manual (QA pairs) |
| **GUTS (ours)** | **2026** | **PubTables-1M, RealHitBench** | **JSON, HTML** | **16** | **1,000** | **Manual (3 viewpoints, 2k)** |

> Source corpora:
> [PubTables-1M](https://github.com/microsoft/table-transformer) ·
> [RealHitBench](https://github.com/cspzyy/RealHiTBench)

## Directory Structure

- `sources` contains a labeled dataset from three viewpoints on header
  identification:
  - `expert` contains labeled tables made by an independent expert (a data
    analyst);
  - `measurement` contains labeled tables in terms of measurement;
  - `table_normalization` contains labeled tables in terms of table
    normalization (converting tables to the first and second normal form):
    - `example` contains one example of manual table labeling from the viewpoint
      of table normalization;
    - `generated_normalized_tables` contains fragments of normalized tables
      generated by LLMs (Qwen3.6-Plus and Qwen3.7-Plus) as examples;
    - `rdf` contains RDF data schemas in the RDF/XML format;
    - `visual_schemes` contains visual data schemas;
    - `xlsx` contains labeled tables with marked headers.

## Usage

GUTS is intended primarily as an evaluation benchmark for **table header
recognition**, with a focus on comparing methods (including LLM-based ones)
across different, well-defined annotation viewpoints. Typical uses include:

- **Header-recognition evaluation.** Given the raw cells of a table (in JSON or
  HTML), predict the header cells and compare against the ground truth using
  precision, recall, and F1 at the cell level. Because RealHitBench tables carry
  three parallel annotations, methods can be scored against each viewpoint
  separately or against their agreement.
- **Benchmarking LLM-based table understanding.** The JSON/HTML table
  representations can be sent to an LLM endpoint, and the model's predicted
  header coordinates (e.g. in `row col | text` form) can be scored against the
  ground truth. GUTS was designed to support exactly this kind of prompt-based
  evaluation across models, prompts, and input formats.
- **Studying task ambiguity.** The inter-annotator agreement statistics let
  researchers quantify how much "the header" depends on the annotation purpose,
  and to test whether a model can be steered toward a particular viewpoint.
- **Structural-complexity analysis.** The rich per-table statistics
  (multi-level headers, spanning cells, header depth, empty-grid ratio) allow
  slicing evaluation by structural difficulty and by domain.
- **Auxiliary resources.** The companion RDF schemas, visual schemes, and
  LLM-generated normalized tables support work on table-to-schema mapping,
  table normalization, and table-to-RDF conversion.

The benchmark is *not* intended for training large models from scratch: at 1,000
unique tables it is an evaluation set, and the header cells should be treated as
reference labels rather than as a large-scale training corpus.

## Authors

- [Ilia I. Okhotin](mailto:ilia.ohotin@yandex.ru)
- [Nikita O. Dorodnykh](mailto:tualatin32@mail.ru)
- [Aleksandr Yu. Yurin](mailto:j80@yandex.ru)

## References

*Publication forthcoming — a citation to the accompanying paper will be added
here upon publication.*

This benchmark is derived from two existing corpora, which should also be cited
when using GUTS:

- B. Smock, R. Pesala, R. Abraham. *PubTables-1M: Towards Comprehensive Table
  Extraction From Unstructured Documents.* CVPR, 2022.
- P. Wu, Y. Yang, G. Zhu, et al. *RealHiTBench: A Comprehensive Realistic
  Hierarchical Table Benchmark for Evaluating LLM-Based Table Analysis.*
  Findings of ACL, 2025.
