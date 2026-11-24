# updated MLST

[![build](https://github.com/Guilai-Jiang/updated-MLST/actions/workflows/build-conda.yml/badge.svg)](https://github.com/Guilai-Jiang/updated-MLST/actions/workflows/build-conda.yml)
[![license](https://img.shields.io/badge/license-GPL--3.0--or--later-blue.svg)](LICENSE)

Updated core-genome MLST (cgMLST) and HierCC assignment for bacteria: allele
calling for a configurable core-genome scheme, md5 allele fingerprints,
reference-profile matching and hierarchical cluster assignment.

## Requirements

Python ≥ 3.9 and, on `PATH`:

| Tool | Version |
|---|---|
| `makeblastdb`, `blastn` | ≥ 2.14 |
| `diamond` | ≥ 2.2 |

## Install

```bash
conda create -n updated-mlst -c conda-forge -c bioconda \
  python=3.10 blast diamond numpy pandas numba biopython click
conda activate updated-mlst
pip install .
```

## Database

The database is distributed as a release asset:

```bash
curl -LO https://github.com/Guilai-Jiang/updated-MLST/releases/download/db-v1.0/updated-mlst-db-1.0.tar.gz
mkdir -p ~/updated-mlst-db && tar xzf updated-mlst-db-1.0.tar.gz -C ~/updated-mlst-db
export UPDATED_MLST_DB=~/updated-mlst-db
```

It is located in this order: `--db` → `$UPDATED_MLST_DB` →
`$CONDA_PREFIX/share/updated-mlst/db` → `./db`.

| File | Content |
|---|---|
| `core_scheme.cgMLST` | core gene identifiers |
| `MLSTdb0.1.fasta` | reference allele for every core gene |
| `new_profile_md5.csv` | md5 allele profiles of the reference genomes |
| `new_HierCC.HierCC` | HierCC levels for the same genomes |

The first run writes `new_profile_md5.csv.pkl` (about 230 MB); later runs start
in ~2 s instead of ~20 s.

## Usage

```bash
updated-mlst -q genome.fna.gz -p sample -o results -n 8
```

| Option | Meaning |
|---|---|
| `-q, --query` | input genome: fasta/fastq, optionally gzipped |
| `-p, --prefix` | output file prefix |
| `-o, --outdir` | output directory |
| `-n, --n_thread` | threads (default 8) |
| `--db` | database directory |

## Output

| File | Content |
|---|---|
| `*.alleles.profile` | md5 allele profile, one column per locus (GrapeTree compatible) |
| `*.summary.txt` | reference genome, allelic distance, HC6, HC650 |

## License

GPL-3.0-or-later. See [LICENSE](LICENSE) and [COPYING](COPYING). Database terms
are described in [docs/DATABASE-LICENSES.md](docs/DATABASE-LICENSES.md).

## Contact

Questions, bug reports and suggestions are very welcome. Please open an
[issue](https://github.com/Guilai-Jiang/updated-MLST/issues) or write to
[gljiang@alu.suda.edu.cn](mailto:gljiang@alu.suda.edu.cn).
