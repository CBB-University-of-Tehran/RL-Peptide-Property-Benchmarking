# Reproducibility and data availability

This document describes how to inspect the saved results and how to rerun the property-based peptide benchmarking analysis associated with **Reinforcement Learning in Peptide Design: A Systematic Review and Original Property-Based Benchmarking Analysis of Generated Sequences**.

> **Status:** The published repository snapshot supports inspection of the code, the generated-candidate FASTA, one reference FASTA, and the saved HTML report. **A complete independent rerun from repository files alone is not currently possible**, because two of the three reference FASTA files are not distributed here. The supplied HTML is an archived result; this document does not claim that a clean-environment rerun has been independently verified.

## 1. Repository entry points

| Path | Role |
| --- | --- |
| [`RL-Peptide-Property-Benchmarking.ipynb`](RL-Peptide-Property-Benchmarking.ipynb) | Analysis notebook; **filename is intentionally unchanged** |
| [`Property-based benchmarking.fasta`](Property-based%20benchmarking.fasta) | 148 publicly reported, selected generated peptide candidates |
| [`peptide_benchmarking_report.html`](peptide_benchmarking_report.html) | Saved benchmarking report and figures |
| [`reference_dataset_preparation/IEDB_random_229_seed42.fasta`](reference_dataset_preparation/IEDB_random_229_seed42.fasta) | Distributed MHC reference subset (229 sequences) |
| [`reference_dataset_preparation/README.md`](reference_dataset_preparation/README.md) | Reference-subset preparation documentation |
| [`reference_dataset_preparation/DATA_LICENSES.md`](reference_dataset_preparation/DATA_LICENSES.md) | Third-party data provenance and reuse considerations |
| [`requirements.txt`](requirements.txt) | Direct Python dependencies; not a full environment lock |
| [`README.md`](README.md) | Project overview, methodology, interpretation, and data-access summary |

The generated peptide sets are distributed as **77 AMP, 39 cyclic/macrocyclic, 4 MHC-related, and 28 Mixed** candidate sequences. Three selected task-related reference sets of **229** sequence representations each were used when the saved report was produced. The Mixed category has no shared reference subset.

## 2. Required inputs and access limitations

Run the notebook's interactive upload cell with **four FASTA files, in order**:

1. `Property-based benchmarking.fasta` — **provided** in the repository.
2. `APD6_random_229_seed42.fasta` — **not publicly distributed** in this repository.
3. `IEDB_random_229_seed42.fasta` — **provided** under `reference_dataset_preparation/`.
4. `CycPeptMPDB_exact_unique.fasta` — **not publicly distributed** in this repository.

The APD6 reference was sampled from eligible, unique APD6 records using seed 42; the IEDB reference was selected from screened human MHC class I ligand records and sampled using seed 42. The cyclic reference comprises 229 exact-unique parent-amino-acid strings after monomer mapping and filtering, **without random subsampling**. Its simplified sequences do not retain cyclization topology, stereochemistry, N-methylation, covalent cross-links, or terminal modifications.

The preparation notebooks do not contain every original source export and intermediate needed to reconstruct all three reference sets exactly. Obtain source records and the two missing reference inputs through arrangements permitted by the respective data providers. **Do not assume that the repository's software license covers third-party database records.** See the reference preparation README and data-rights notes linked above.

Using newly sampled reference sets—even with the same sample size—can change numeric results. Exact reproduction of the archived tables requires the original input sequences and compatible analysis code and dependencies.

## 3. Software environment

The intended runtime is **Google Colab with Python 3**, because the notebook uses Colab-specific file upload interfaces. Its setup cell installs **Biopython 1.86** together with `pandas`, `numpy`, `scipy`, `matplotlib`, and `seaborn`. Some visualization code may also rely on `Pillow` (`PIL`). Install the requirements when setting up a compatible environment:

```bash
python -m pip install -r requirements.txt
```

The repository's dependency list is **not an environment lockfile**. Except for Biopython 1.86 as fixed in the notebook and manuscript, package versions, the Python patch version, and the exact Colab image have not been independently recorded here. Consequently, a future execution can produce environment-dependent differences. Do not present a clean rerun as verified until it has actually been completed and checked.

## 4. Running the analysis (when all four inputs are available)

1. Open `RL-Peptide-Property-Benchmarking.ipynb` in Google Colab.
2. Execute the setup cell and proceed sequentially through **all** subsequent analysis, diagnostic, sensitivity, and visualization cells.
3. Upload the four FASTA inputs in the order given in Section 2 when prompted.
4. Check the sequence-integrity and reference-size audits before interpreting the output; the expected reference-set size is 229 unique canonical strings per reference category.
5. Run the HTML export cell; the output is named **`peptide_benchmarking_report.html`**.
6. Compare the resulting report with the archived HTML report in the repository. Retain the execution environment, input-file hashes, and generated log/report as provenance if preparing a validated future release.

**No rerun is needed merely to browse the committed HTML report.** However, viewing an archived output is not equivalent to independently reproducing it.

## 5. Methodological invariants to verify

The following settings characterize the archived analysis; they should not be altered silently when trying to reproduce its results:

- **Descriptors:** length, nominal molecular weight, GRAVY, pI, net charge at pH 7, and aliphatic index; pI is estimated via a bisection search over pH 0–14 based on Biopython's sequence-charge model.
- **Primary cyclic comparison:** **length, GRAVY, and aliphatic index only**. Six- and five-descriptor cyclic comparisons are exploratory sequence-based sensitivity analyses and are not topology-corrected molecular comparisons.
- **Distributional tests:** Mann–Whitney U and Kolmogorov–Smirnov comparisons are exploratory and require at least three generated sequences. Benjamini–Hochberg adjustments are computed separately by test type and peptide category; use **adjusted q-values** for the reported significance interpretations.
- **Distance metric:** per-descriptor Wasserstein distance is normalized by the reference **population** standard deviation (`ddof=0`). Composite distance is the unweighted arithmetic mean of the applicable per-descriptor distances, using their **unrounded numeric values**; round for presentation only.
- **Uncertainty:** 2,000 peptide-level bootstrap iterations with seed 42, independently resampling generated and reference peptides. Small or closely related selected candidate sets limit the interpretation of confidence intervals; resampling cannot correct candidate-selection bias.
- **Similarity:** Smith–Waterman local alignments have a minimum of 70% query coverage and five aligned residues; local identity does not establish novelty, absence of memorization, or global sequence similarity.

Selected checks against the archived report and manuscript:

| Category | Model | Candidate N | Primary composite distance | 95% bootstrap CI |
| --- | --- | ---: | ---: | --- |
| AMP | DIFF-AMP | 6 | 0.685 | 0.568–1.133 |
| AMP | MOQA | 4 | 0.774 | 0.745–0.937 |
| AMP | HMAMP | 10 | 0.551 | 0.483–0.782 |
| Cyclic | CYC_BUILDER | 9 | 2.387 | 1.850–3.034 |
| Cyclic | HIGHPLAY | 30 | 3.318 | 2.959–3.798 |

These are **descriptive, reference-dependent distances**, not competitive model rankings or biological-performance measurements. Differences in reference composition, peptide topology, and candidate selection may change their interpretation.

## 6. Known presentation caveats in the archived notebook

The notebook/report snapshot may retain presentation labels from earlier development, including an unadjusted `Significant_Diff (p<0.05)` flag alongside BH-adjusted results, wording implying `100%-compliance` for a reference baseline, and an exploratory-inference status assigned to Mixed-category models despite the lack of a shared Mixed reference set.

For interpretation:

- Rely on the **BH-adjusted q-values** when discussing multiplicity-corrected significance; a raw p-value flag is not an adjusted result.
- A reference distribution has **zero self-distance**, but its own P5–P95 interval does **not** imply 100% coverage.
- **No reference-based hypothesis testing or composite reference distance was conducted for Mixed** in the reported design.

These are documentation and presentation caveats; this file does **not** assert that the underlying notebook or the archived HTML has been regenerated to remove them. The primary composite distances above were calculated from unrounded component distances in the validated analysis.

## 7. Minimum provenance for a validated future release

Before claiming a fully independently reproducible release, record (a) legally accessible exact reference inputs and their SHA-256 hashes, (b) a fully pinned environment or lockfile, (c) a complete clean-run execution record, (d) verification of all outputs against the archived report, and (e) a stable version tag identifying the precise source code, data-access documentation, and saved outputs.

Until then, the accurate claim is: **analysis code, selected generated sequences, selected saved results, and partial reference-preparation materials are publicly inspectable; complete independent regeneration from repository assets alone remains limited by missing source/reference inputs and an unlocked environment.**
