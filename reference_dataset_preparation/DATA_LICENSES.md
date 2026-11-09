# Database Attribution, Reuse, and Redistribution Notice

**Scope:** `reference_dataset_preparation/` within the *RL-Peptide-Property-Benchmarking* repository. **Last reviewed:** 9 November 2026.

This file explains the provenance and **current public distribution status** of the three third-party database reference sets described in the manuscript. It is an attribution and rights notice, **not a new license granted over third-party data**, and does not replace any original database terms.

## Summary

| Source | Peptide class | Files currently publicly distributed here | Rights position and action |
| --- | --- | --- | --- |
| **APD6** | Antimicrobial peptides (AMPs) | No APD6 FASTA files | APD website asserts database copyright; public redistribution permission for our FASTA selections has not been established. Obtain written clarification before restoring. |
| **IEDB** | Human MHC class I HLA-binding peptide records | `IEDB_random_229_seed42.fasta` | IEDB identifies its data under **Creative Commons Attribution 4.0 International (CC BY 4.0)**; retain credit, original source/license link and a statement of modifications, subject to any separately identified third-party restrictions. |
| **CycPeptMPDB** | Cyclic/macrocyclic peptides (mapped representations) | No CycPeptMPDB FASTA file | The database's primary **article** is CC BY 4.0; this alone does not conclusively establish the reuse license for every downloadable CSV/third-party source record. Verify the exact source file and its conditions before redistributing a mapped FASTA. |

## APD6 — Antimicrobial Peptide Database v6

**Official database:** https://aps.unmc.edu/  
**Database terms/background:** https://aps.unmc.edu/about  
**Reference publication:** *APD6: the antimicrobial peptide database is expanded to promote research and development by deploying an unprecedented information pipeline.* Nucleic Acids Research, DOI: https://doi.org/10.1093/nar/gkaf860

Our benchmarking procedure uses the APD6 natural AMP source described in the manuscript, screens for supported canonical residues, eliminates exact duplicates, and selects **229** eligible sequences using `seed=42`.

**Public files intentionally withheld:**

- `APD6_all_eligible_unique.fasta`
- `APD6_random_229_seed42.fasta`

The APD website states that the database is copyright-protected and reserves rights. Although its published article has an open-access license, **the article's license should not be assumed to authorize redistribution of the extracted database records**. Before reposting these FASTA files, request written permission or an explicit applicable data-redistribution term from the database owner. Users may retrieve data from the official source according to its applicable terms.

## IEDB — Immune Epitope Database

**Official database:** https://www.iedb.org/  
**Citation/license information:** https://www.iedb.org/citation_v3.php  
**License:** [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

**Distributed derived file:** `IEDB_random_229_seed42.fasta`

**Attribution statement:**

> This reference subset was derived from the Immune Epitope Database (IEDB) experimental MHC ligand assay export (https://www.iedb.org/). IEDB data are made available under Creative Commons Attribution 4.0 International (https://creativecommons.org/licenses/by/4.0/). Changes made for this research include selecting human MHC class I HLA-A/HLA-B/HLA-C positive experimental records, filtering for candidate direct-binding measurements and linear 8–15-residue peptides, excluding unsupported/modified sequence entries, exact-sequence deduplication, and random sampling of 229 sequences without replacement (seed 42). The resulting subset is not endorsed by or an official curated release of IEDB.

Attribution to IEDB **does not replace** citing the primary publication(s) of the original experimental records where relevant. Consult IEDB's latest policies and any third-party notices before downstream reuse.

## CycPeptMPDB — Cyclic Peptide Membrane Permeability Database

**Reference publication:** Li J. et al. *CycPeptMPDB: A Comprehensive Database of Membrane Permeability of Cyclic Peptides.* Journal of Chemical Information and Modeling 63(7):2240–2250 (2023). DOI: https://doi.org/10.1021/acs.jcim.2c01573  
**Database source cited in the publication:** http://cycpeptmpdb.com/download/

The manuscript's 229 cyclic/macrocyclic reference representations resulted from mapping recognizable D- or N-methylated variants to their standard parent amino-acid identities and then deduplicating the resulting strings. **No random sampling** was used. The mapping discards chemical details such as stereochemistry, N-methylation, connectivity and cyclization topology.

**Public file intentionally withheld:** `CycPeptMPDB_exact_unique.fasta`.

The primary database **article** is published under CC BY 4.0, but we have **not verified that the exact source CSV used by the notebook carries an equivalent express data-redistribution license**. The database compiles records from papers and patents, and publication-level openness does not necessarily resolve rights in all original records or a separately hosted data file. Record the precise CSV URL, retrieval date, any attached terms or license, and the transformation provenance before reconsidering public distribution. The mapping notebook is retained as code, not as a grant of rights to the original dataset.

## Code, data, and manuscript citations

Any `LICENSE` file at the repository root applies **only to material for which the repository authors can grant rights**, subject to that license's terms. It does not automatically relicense third-party databases, their exports, or underlying publications. The repository's benchmarking workflow and this notice may be cited separately from the original database articles; **the original databases still require their own attribution**.

**No warranties or endorsement:** These derived research subsets are provided for sequence-property analysis only; they are not official database distributions, and their inclusion does not imply endorsement by the originating database maintainers.

## Version-history caution

Removing files from the current GitHub directory **does not remove previously committed versions from public Git history, forks, mirrors or caches**. The three withheld FASTA files were previously committed according to the repository history described by its maintainers. If a database owner requires removal of previously exposed data, evaluate repository-history cleanup and GitHub's guidance, recognizing that this cannot revoke copies already made. Keep correspondence and permissions records for future audit.

## Rights-review checklist

- [ ] Obtain an explicit APD6 redistribution decision (or applicable written terms).
- [ ] Identify the **exact** CycPeptMPDB input CSV URL, version, source repository, and license/terms.
- [ ] Preserve an IEDB attribution and modifications statement alongside the distributed FASTA.
- [ ] Check notebook outputs for embedded third-party sequences or original source tables.
- [ ] Update this notice if data distribution or source permissions change.
