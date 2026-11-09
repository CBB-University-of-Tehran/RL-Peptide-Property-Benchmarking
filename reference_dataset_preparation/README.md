# Reference Dataset Preparation

This directory documents the preparation of **three task-matched reference subsets**, each consisting of **229 amino-acid sequence representations**, for the physicochemical benchmarking analysis accompanying *Reinforcement Learning in Peptide Design: A Systematic Review and Original Property-Based Benchmarking Analysis of Generated Sequences*.

> **Data-distribution notice (9 November 2026):** To avoid redistributing database-derived sequence collections before their reuse conditions are clarified, the APD6 and CycPeptMPDB reference FASTA files are **not currently distributed** in this repository. The IEDB-derived reference FASTA and the two preparation notebooks are retained. The absence of the APD6 and CycPeptMPDB FASTA files does **not** mean those datasets were excluded from the manuscript's analysis. See [DATA_LICENSES.md](DATA_LICENSES.md) for rights, attribution, and provenance details.

## Peptide categories and data sources

| Benchmark category | Source | Selection and interpretation | Public sequence file in this folder |
| --- | --- | --- | --- |
| Antimicrobial peptides (AMPs) | [APD6](https://aps.unmc.edu/) natural AMP collection (APD2024a, as described in the manuscript) | Valid standard amino-acid sequences were screened, exact duplicates removed, and 229 eligible unique sequences sampled without replacement using seed 42. | **Not distributed pending rights clarification** |
| MHC class I-binding peptides | [IEDB](https://www.iedb.org/) experimental **MHC ligand assay export** | Positive human HLA-A/HLA-B/HLA-C, linear 8–15-residue peptides, screened for direct-binding assay relevance; unsupported/modified sequences and exact duplicates excluded, then 229 sampled without replacement using seed 42. | [`IEDB_random_229_seed42.fasta`](IEDB_random_229_seed42.fasta) |
| Cyclic/macrocyclic peptides | [CycPeptMPDB](http://cycpeptmpdb.com/) | Recognizable standard, D-configured and N-methylated monomers mapped to parent canonical amino-acid identities; unsupported monomers excluded; exact-string deduplication yielded **229 mapped representations**. **No random sampling** was applied. | **Not distributed pending rights/provenance clarification** |

These are **selected, task-related database reference subsets**, not statistically representative samples of every peptide in their respective categories and not necessarily natural-peptide populations.

## Currently maintained files

| File | Purpose |
| --- | --- |
| [`EDB_MHCI_HLA_DirectBinding_Filtering_RandomSampling_229_Seed42.ipynb`](EDB_MHCI_HLA_DirectBinding_Filtering_RandomSampling_229_Seed42.ipynb) | IEDB experimental MHC-I HLA record filtering, deduplication, and seeded reference sampling. The original filename begins `EDB` rather than `IEDB`. |
| [`IEDB_random_229_seed42.fasta`](IEDB_random_229_seed42.fasta) | The 229 selected IEDB-derived MHC-I reference sequences used for benchmarking. |
| [`cycpeptmpdb_map_unique_sequences.ipynb`](cycpeptmpdb_map_unique_sequences.ipynb) | CycPeptMPDB monomer-to-parent-residue mapping and exact-string deduplication code; does not include the resulting FASTA here. |
| [`README.md`](README.md) | Directory purpose, methodology, file inventory, and reproducibility qualifications. |
| [`DATA_LICENSES.md`](DATA_LICENSES.md) | Source attribution, redistribution status, and rights caveats for each database. |

## Reference files used in the analysis but not currently hosted here

- `APD6_all_eligible_unique.fasta` — filtered, exact-unique APD6 AMP sampling frame.
- `APD6_random_229_seed42.fasta` — the selected 229 APD6 AMP reference sequences.
- `CycPeptMPDB_exact_unique.fasta` — the 229 deduplicated mapped cyclic/macrocyclic sequence representations.

These filenames are documented to preserve correspondence with the manuscript and benchmarking inputs. Their inclusion here is **documentation only**, not a download offer.

## Selection and chemical caveats

- APD6 and IEDB were sampled **without replacement** using a fixed seed (`42`) and a deterministically ordered eligible sequence frame, as specified in the manuscript.
- The CycPeptMPDB subset contains all 229 retained **mapped strings** after specified filtering/deduplication, not a random sample of 229.
- The cyclic sequence mapping **does not retain stereochemistry, N-methylation, cyclization topology, termini, or cross-link chemistry**. It supports sequence-composition comparisons, not a chemically complete reconstruction of cyclic molecules.
- In the primary cyclic benchmarking analysis, only **sequence length, GRAVY, and aliphatic index** are used for distributional composite comparisons. Nominal molecular weight, sequence-based pI, and net charge are exploratory and should not be interpreted as topology-aware chemical quantities.
- No biological activity, efficacy, or superiority of a generation model can be inferred from similarity to the selected reference distributions alone.

## Reproducibility and data access

The retained notebooks document the transformation workflows; they do not necessarily bundle the original downloaded database exports. Reproducing the full reference-set construction may require access to the original database exports, source-specific records, notebook dependencies, extraction dates, and any locally held provenance metadata. The APD6 and CycPeptMPDB derived FASTA inputs are not presently provided here. Consequently, **end-to-end reproduction from this public folder alone is not currently guaranteed**. Researchers should obtain source data from the original providers under their applicable terms.

For the broader benchmarking pipeline, see the [repository root](../) and its main notebook. Any input instructions there should be read together with this availability notice.

## Data attribution and reuse

The analysis cites the originating databases in the manuscript. In addition, downstream users must follow the providers' current license, attribution, and redistribution terms. In particular, a license for this repository's original code **does not automatically apply** to third-party database content. See [DATA_LICENSES.md](DATA_LICENSES.md).

## Relevant links

- [APD6 database](https://aps.unmc.edu/) and [APD6 paper](https://doi.org/10.1093/nar/gkaf860)
- [IEDB database](https://www.iedb.org/) and [IEDB citation information](https://www.iedb.org/citation_v3.php)
- [CycPeptMPDB paper](https://doi.org/10.1021/acs.jcim.2c01573)
