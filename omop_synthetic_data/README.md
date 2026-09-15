# Synthetic OMOP Demonstration Dataset

## Overview

This dataset is a synthetic, OMOP-compatible dataset generated for the UNCAN demonstration workflow. It is intended to support technical demonstrations of cross-border oncology data sharing, data ingestion, interoperability, user-interface development, API integration, and exploratory non-clinical analytics.

The release contains 1,000 synthetic patients, matching the number of patients in the source cohort. It preserves the OMOP CSV directory and file layout used by the demonstration environment, including partitioned files where applicable.

## Generation workflow

The dataset was generated through the following workflow:

1. A real OMOP CDM export was transformed into a patient-level feature table.
2. Direct identifiers and technical relational identifiers were excluded from the modelling table, including patient source identifiers, event primary keys, visit identifiers, provider identifiers, care-site identifiers, location identifiers, and original dates.
3. Clinical information was represented as aggregate patient-level features, including demographics, event counts, selected OMOP concepts, treatment/procedure durations, measurement summaries, and relative timing features.
4. Alia Santé CTGAN model was trained on this feature table and generated a synthetic patient-level dataset.
5. The CTGAN output was post-processed to enforce valid feature types and basic consistency rules, including non-negative counts and durations, valid categorical values, bounded ages, and coherent measurement summaries.
6. A synthetic OMOP CSV tree was reconstructed from the generated patient-level data. New synthetic identifiers and synthetic pseudo-dates were created during reconstruction; no source identifiers or original dates were reused.

## Included domains

The release follows the OMOP directory structure supplied for the demonstration and includes the following domains:

- `person`
- `visit_occurrence`
- `condition_occurrence`
- `procedure_occurrence`
- `drug_era`
- `dose_era`
- `measurement`
- `episode`
- `episode_event`
- `death`

Each CSV retains the column names and column order of its corresponding input template. Where the source contained several CSV partitions for a domain, the same partitioned file layout is retained.

## Quality assessment

The synthetic dataset was evaluated through Alia Santé's quality report using statistical fidelity, structural, correlation, privacy, and anonymisation-oriented metrics.

| Metric | Score |
|---|---:|
| Overall quality score | 93.85 / 100 |
| Fidelity score | 0.929 |
| Structural score | 0.853 |
| Distribution score | 0.977 |
| Correlation score | 0.957 |
| Privacy score | 0.987 |
| Anonymisation score | 0.974 |
| Similarity score | 1.000 |

These results indicate strong preservation of the evaluated patient-level statistical properties while maintaining a high privacy-oriented score.

## Intended use

This dataset is suitable for:

- Demonstrating an end-to-end OMOP data-sharing workflow.
- Testing OMOP CSV ingestion and schema compatibility.
- Testing platform interfaces, APIs, access workflows, and data catalogues.
- Developing and testing SQL queries, ETL components, dashboards, and interoperability services.
- Training and demonstration activities using a synthetic oncology-oriented OMOP dataset.
- Exploratory and descriptive analyses on the explicitly represented domains.

## Important limitations

This is a synthetic demonstration dataset and must not be used for clinical decision-making, patient-level inference, or regulatory evidence generation.

The source modelling representation was patient-level and aggregate. Consequently, original event sequences, original timestamps, and original event-to-visit relationships are not preserved. Dates in the release are synthetic pseudo-dates generated during reconstruction.

## Privacy and data handling

No source patient IDs, source UUIDs, original event IDs, provider IDs, care-site IDs, location IDs, or original calendar dates are included in this release.

This dataset remains subject to the project governance framework. Users must not attempt to re-identify individuals or link this dataset to external patient-level data.

## Version

- Generator: Alia Santé CTGAN
- Synthetic cohort size: 1,000 patients
- Data model: OMOP CDM-compatible CSV export
- Intended scope: technical demonstration and platform development
