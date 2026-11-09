# RL-Peptide-Property-Benchmarking

**Reproducible, property-based characterization of publicly reported peptide sequences generated or optimized using reinforcement learning (RL) and related reward-guided approaches.**

This repository accompanies the manuscript **“Reinforcement Learning in Peptide Design: A Systematic Review and Original Property-Based Benchmarking Analysis of Generated Sequences”** by Fereshteh Noroozi Tiyoula, Marzieh Gholami, Shohreh Ariaeenejad, and Kaveh Kavousi (manuscript under revision; publication metadata to be updated).

The analysis characterizes **available, publicly reported/generated peptide sequences**. It is **not** a controlled, head-to-head performance benchmark of generative models, and distributional proximity to a reference set is **not** evidence of superior bioactivity, efficacy, developability, or model performance.

## Scope

The benchmarking examines six sequence-derived physicochemical descriptors:

1. Sequence length
2. Nominal molecular weight (MW)
3. Grand average of hydropathicity (GRAVY)
4. Isoelectric point (pI)
5. Net charge at pH 7.0
6. Aliphatic index

Reference-based comparisons are organized by biological/design task: **antimicrobial peptides (AMPs)**, **MHC class I-binding peptides**, and **cyclic/macrocyclic peptides**. Generated sequences that cannot be assigned to these groups using reviewed sequence metadata or model-category mappings may be treated as **Mixed**; no corresponding Mixed reference dataset is supplied here.

## Reference dataset preparation

The [`reference_dataset_preparation/`](reference_dataset_preparation/) directory documents the preparation of three **selected, task-relevant database reference subsets**, each containing **229 amino-acid sequence representations**. These reference sets are not claimed to be representative random samples of their entire source databases.

| File | Category | Description |
| --- | --- | --- |
| [`APD6_all_eligible_unique.fasta`](reference_dataset_preparation/APD6_all_eligible_unique.fasta) | **AMP** | Eligible unique sequences retained from the APD6 natural AMP source collection after canonical-residue and exact-duplicate screening; this is the eligible pool, **not** the final 229-peptide subset. |
| [`APD6_random_229_seed42.fasta`](reference_dataset_preparation/APD6_random_229_seed42.fasta) | **AMP** | Final reference set: 229 eligible unique antimicrobial peptides randomly sampled without replacement (seed 42). |
| [`IEDB_random_229_seed42.fasta`](reference_dataset_preparation/IEDB_random_229_seed42.fasta) | **MHC class I / HLA** | Final reference set: 229 unique 8–15-residue peptide sequences from filtered positive experimental MHC-I ligand records; sampled with seed 42. |
| [`EDB_MHCI_HLA_DirectBinding_Filtering_RandomSampling_229_Seed42.ipynb`](reference_dataset_preparation/EDB_MHCI_HLA_DirectBinding_Filtering_RandomSampling_229_Seed42.ipynb) | **MHC class I / HLA** | Notebook for IEDB record filtering, sequence cleaning/deduplication, and selection. The filename retains `EDB` as uploaded. |
| [`CycPeptMPDB_exact_unique.fasta`](reference_dataset_preparation/CycPeptMPDB_exact_unique.fasta) | **Cyclic/macrocyclic** | Final set of 229 unique mapped canonical-amino-acid sequence representations; **no random subsampling** was applied. |
| [`cycpeptmpdb_map_unique_sequences.ipynb`](reference_dataset_preparation/cycpeptmpdb_map_unique_sequences.ipynb) | **Cyclic/macrocyclic** | Notebook for mapping annotated monomers to parent canonical amino-acid identities and exact-string deduplication. |

### 1. APD6: antimicrobial peptide reference

- **Source:** Antimicrobial Peptide Database, version 6 (APD6), using the public **APD2024a** natural AMP FASTA collection (3,306 entries in the source collection).
- **Selection:** Filter to sequences with supported standard amino-acid symbols, remove exact duplicate sequences, order the eligible frame deterministically, then sample **229 unique sequences without replacement, random seed 42**.
- **Files:** `APD6_all_eligible_unique.fasta` preserves the eligible pool; `APD6_random_229_seed42.fasta` is the benchmark input.
- **Interpretation:** A selected reference set of database-annotated natural AMPs; not an assay-matched or exhaustive AMP population.

APD6 source: <https://aps.unmc.edu/downloads>.

### 2. IEDB: MHC class I / human HLA reference

- **Source:** Experimental **MHC Ligand** database records exported from the **Immune Epitope Database (IEDB)**, not the IEDB binding-prediction Analysis Resource.
- **Selection:** Linear peptides; **MHC class I**, human **HLA-A, HLA-B, or HLA-C** restrictions; positive experimental outcomes; screening of assay method and response annotations to identify candidate **direct-binding** records; sequence length **8–15** residues; removal of records with unsupported residue symbols or documented chemical modifications; exact-sequence deduplication; deterministic random selection of **229 unique sequences (seed 42)**.
- **Provenance:** The corresponding notebook describes the selection workflow. Retain the original IEDB export and an audit table of epitope identifiers, HLA restrictions, assay methods, and response annotations to enable independent reconstruction.
- **Important qualification:** A positive filtered MHC-ligand/direct-binding record should not be conflated with experimentally measured T-cell immunogenicity, nor is every such assay equivalent to a uniform affinity measurement.

IEDB source: <https://www.iedb.org/>.

### 3. CycPeptMPDB: cyclic/macrocyclic peptide reference

- **Source:** A publicly available **CycPeptMPDB-derived CSV** containing monomer-level cyclic-peptide annotations (document the precise CSV location, version, and retrieval date in the provenance record).
- **Mapping:** Canonical residues and recognizable D-amino-acid or N-methylated variants are mapped to the corresponding **parent standard amino-acid identity** (e.g., L, dL, meL, and Me_dL become `L`). Entries with monomers that cannot be mapped unambiguously are excluded.
- **Deduplication:** Exact duplicates of the resulting **simplified amino-acid strings** are removed, giving **229 distinct mapped representations**. There is **no random downsampling** for this group.
- **Important chemical limitation:** These strings do **not** retain stereochemistry, N-methylation, cyclization topology, covalent connectivity, or all terminal/side-chain modifications. Distinct cyclic molecules may collapse to the same mapped string. These are therefore **sequence-composition references**, **not** complete molecular structures or a chemically validated natural-product sample.

Original database: <http://cycpeptmpdb.com/>.

## Reproducing the reference-set preparation

1. Obtain the corresponding original source exports from their listed providers, subject to each provider's terms of use. Keep an unmodified copy **outside** this repository unless redistribution permission has been verified.
2. Run the two supplied notebooks in [`reference_dataset_preparation/`](reference_dataset_preparation/) using the documented inputs, or reproduce the APD6 filtering and seeded sampling from the original natural-AMP FASTA source.
3. Verify the resulting selection against the checked-in FASTA files. Check **229 records and 229 unique mapped sequence strings** in each final FASTA, supported amino-acid alphabet, and relevant category/length restrictions.
4. Run the main benchmarking notebook in this repository using the three final FASTA files as task-matched reference inputs, together with the generated-sequence inputs described in the manuscript and supplement.

**Reproducibility note:** The final FASTA files are provided for direct use. Complete independent reconstruction also requires identifying the **exact source release/export and retrieval date**, recording any input checksums, and retaining applicable filtering/audit metadata. Notebook availability alone does not establish provenance for an external input that is not included.

### Quick validation (optional)

If Biopython is installed, validate the three final reference FASTA files:

```python
from pathlib import Path
from Bio import SeqIO

folder = Path("reference_dataset_preparation")
files = [
    "APD6_random_229_seed42.fasta",
    "IEDB_random_229_seed42.fasta",
    "CycPeptMPDB_exact_unique.fasta",
]
canonical = set("ACDEFGHIKLMNPQRSTVWY")
for filename in files:
    records = list(SeqIO.parse(folder / filename, "fasta"))
    sequences = [str(r.seq).upper() for r in records]
    assert len(sequences) == 229, (filename, len(sequences))
    assert len(set(sequences)) == 229, filename
    assert all(s and set(s) <= canonical for s in sequences), filename
    if filename.startswith("IEDB_"):
        assert all(8 <= len(s) <= 15 for s in sequences), filename
    print(f"{filename}: {len(sequences)} valid unique sequences")
```

The validator checks final FASTA integrity; it **does not** validate the upstream database retrieval, experimental assay annotations, or mapping decisions.

## Interpretation of the benchmarking

- The input peptide sets are **publicly reported or otherwise publicly available candidates**, with unequal and sometimes very small sample sizes. They need not represent complete model-output distributions.
- Comparisons with literature-defined windows and comparisons with reference distributions answer **different questions**; neither independently measures biological activity or the incremental contribution of RL.
- Distributional comparisons may include Mann–Whitney U, Kolmogorov–Smirnov, normalized Wasserstein distance, reference-percentile coverage, Benjamini–Hochberg false discovery rate correction, bootstrapped uncertainty intervals, and descriptor/scaling sensitivity analyses, as documented in the main analysis and manuscript.
- For **primary cyclic/macrocyclic reference comparisons**, use **length, GRAVY, and aliphatic index** only. Nominal sequence-derived MW, pI, and charge assume simplified linear-peptide chemistry and are **not** topology-aware comparisons; any results using them should be identified as exploratory sensitivity analyses.
- Statistical non-significance is **not** proof of distributional equivalence. Similarity to one of these selected reference subsets is **not** a measure of novelty relative to complete training databases.

## Data provenance and attribution

The database records in this repository are **third-party-derived data**, not newly created raw data. Please cite the appropriate original source even if you use a derived FASTA file, and also cite the accompanying manuscript or repository where relevant.

**APD6:** Wang G. et al. *APD6: the antimicrobial peptide database is expanded to promote research and development by deploying an unprecedented information pipeline*. **Nucleic Acids Research** (2026 issue; published online 2025). <https://doi.org/10.1093/nar/gkaf860>. Source download and terms: <https://aps.unmc.edu/downloads>.

**IEDB:** *The Immune Epitope Database (IEDB): 2024 update*. **Nucleic Acids Research**. <https://doi.org/10.1093/nar/gkae1092>. Official citation information: <https://www.iedb.org/citation_v3.php>.

**CycPeptMPDB:** Li J., Yanagisawa K., Sugita M., Fujie T., Ohue M., Akiyama Y. *CycPeptMPDB: A Comprehensive Database of Membrane Permeability of Cyclic Peptides*. **Journal of Chemical Information and Modeling** 63 (2023), 2240–2250. <https://doi.org/10.1021/acs.jcim.2c01573>.

## Licensing and redistribution

**Source-database terms and repository-code licensing are separate.** Cite the original database papers; citation alone does **not** automatically establish permission to redistribute a downloaded dataset. In particular:

- APD6 provides downloadable research data but explicitly asserts copyright over its database. Check its current terms and request permission where necessary before republishing a substantial derived dataset.
- IEDB publishes citation/reuse information; check the current data-specific terms and retain attribution.
- The CycPeptMPDB research article is open access under CC BY 4.0, but the exact redistribution terms for the underlying database export or any intermediate third-party CSV should be checked separately. Record the intermediate CSV's original source and license.

**No open-source license is asserted by this README.** The repository maintainers should choose an explicit `LICENSE` for **original code and documentation** only after confirming institutional/author rights; any such license should explicitly **exclude third-party database-derived FASTA data unless those rights are documented**. A separate `DATA_LICENSES.md` or `NOTICE` file is recommended to track upstream rights, provenance, and attribution per source.

## Citation of this repository

If using the analysis pipeline or prepared reference subsets, cite the original database papers above **and** the associated manuscript when bibliographic information becomes available. For reproducibility, report the repository URL, the commit SHA used, the release/version (if available), and the date accessed:

<https://github.com/CBB-University-of-Tehran/RL-Peptide-Property-Benchmarking>

## Maintainer checklist before final publication

- [ ] Record the exact APD6 source file/retrieval date and eligible-pool count after filtering.
- [ ] Identify and archive the precise IEDB export and CycPeptMPDB-derived CSV provenance; include input checksum and field/filter audit where legally shareable.
- [ ] Confirm the notebooks generate byte-equivalent or sequence-equivalent FASTAs using the specified seed and order.
- [ ] Add a code-specific `LICENSE` and a third-party-data notice **after checking redistribution rights**.
- [ ] Add a tagged GitHub release (and optionally a Zenodo DOI) matched to the submitted/revised manuscript.
- [ ] Ensure references to file names, model names, analysis notebooks, manuscript figures, and supplementary tables match the final repository revision.
