[README.md](https://github.com/user-attachments/files/34823758/README.md)
# Quartic Fibonacci Julia set: numerical verification

Verification package accompanying **Zero area of the canonical quartic Fibonacci Julia set**.

The package contains the code, certificate data, and replay instructions for the computer-assisted part of the proof. It verifies the finite numerical hypotheses of the manuscript's zero-area criterion for the canonical Fibonacci generalized polynomial-like map of critical degree four. The analytic arguments and the identification with the canonical map are given in the manuscript.

## Download the complete verification package

Use the [version v1.0.0 release page](https://github.com/RunzeZhangMATH/quartic-fibonacci-verification/releases/tag/v1.0.0), or download these assets directly:

- [quartic_fibonacci_verification_2026-11-12.zip](https://github.com/RunzeZhangMATH/quartic-fibonacci-verification/releases/download/v1.0.0/quartic_fibonacci_verification_2026-11-12.zip) — complete verification package, including numerical records.
- [release_checksums.json](https://github.com/RunzeZhangMATH/quartic-fibonacci-verification/releases/download/v1.0.0/release_checksums.json) — file sizes and SHA-256 checksums.

**Choose the explicitly named verification ZIP above.** GitHub also displays **Source code (zip)** and **Source code (tar.gz)**. Those are automatic snapshots of the repository files at the release tag. They do not include the separately uploaded release assets and therefore do not provide the complete verification package.

The verification ZIP has size **376,580,138 bytes** (approximately 376.6 MB, or 359.1 MiB) and SHA-256:

```text
972906a2200181ff5ac7a8bb3fb97c7cea46cfb364bfe92e6c2b0c29fa8dc4f8
```

On Linux, check the downloaded file with:

```bash
sha256sum quartic_fibonacci_verification_2026-11-12.zip
```

On Windows, use PowerShell:

```powershell
Get-FileHash .\quartic_fibonacci_verification_2026-11-12.zip -Algorithm SHA256
```

The computed hash must match the value above; letter case is immaterial. A matching checksum establishes file integrity, not the numerical validity of the certificate.

## Requirements

Run the verification in a Linux environment. On Windows, use a Linux environment such as Ubuntu under WSL.

Required software:

- Python 3, with NumPy and mpmath;
- a C++17 compiler with OpenMP support;
- the usual Linux process environment and sufficient disk space for several gigabytes of intermediate output.

For example, on Ubuntu:

```bash
sudo apt-get update
sudo apt-get install -y python3 python3-venv python3-pip g++ unzip
python3 -m venv ~/quartic-fibonacci-venv
source ~/quartic-fibonacci-venv/bin/activate
python3 -m pip install numpy mpmath
```

The proof programs do not require Matplotlib or SciPy.

Run Python with assertions enabled: do not use `-O`, `-OO`, or an enabled `PYTHONOPTIMIZE` setting. The driver rejects an optimized interpreter. The C++ verification builds require `-frounding-math -ffp-contract=off`; do not enable fast-math. Use the builds invoked by the replay driver, which also performs the required arithmetic checks. The independent kernel calculation requires `long double` with at least 64 significand bits.

The driver raises the stack allowance to 128 MiB and rejects a smaller hard limit.

## Run the verification

Extract the verification ZIP and enter the extracted directory containing `reproduce.py`. No second numerical archive is needed.

To inspect the complete command sequence without executing it:

```bash
python3 reproduce.py all --list
```

For a quick check of the packaged inputs:

```bash
python3 reproduce.py integrity
```

For the complete ordered numerical replay:

```bash
python3 reproduce.py all --threads 4
```

The full run can take hours and generate several gigabytes of intermediate files. The driver stops at the first failure and records subprocess commands, exit codes, dated logs, and a machine-readable summary under `release_runs/`.

Individual phases can also be run separately:

| Command | Purpose |
| --- | --- |
| `python3 reproduce.py integrity` | Authenticate the recorded input files by their sizes and hashes. |
| `python3 reproduce.py local` | Demonstrate the finite Newton search, repeat the independent analytic validation, and check the identification contours and basin bounds. |
| `python3 reproduce.py geometry` | Recompute the domain enclosures, geometric pruning, physical isolators, and slit endpoints. |
| `python3 reproduce.py orbits --threads 4` | Replay the landing computations, check accepted terminal witnesses and the cover chain, and re-certify the final loss union. |
| `python3 reproduce.py kernel` | Recompute the production and independent distortion-kernel bounds. |
| `python3 reproduce.py accounting` | Check the exact landing and loss areas, disjoint unions, coverage, kernel weights, and final rational inequality. |

Passing an individual phase establishes that phase only. In particular, the integrity check and a recount of stored areas do not replace fresh orbit replay.

## Expected certificate

The manuscript applies its uniform contraction criterion with indices `(n, m0, t) = (3, 2, 5)`. The numerical bounds used in the proof are:

| Quantity | Certified value |
| --- | --- |
| Upper bound `A_3^+` for the normalized landing area | `23788665 / 536870912` |
| Lower bound `gamma^-` for the distortion-weighted loss kernel | `6481603931109663 / 144115188075855872` |
| Contraction factor `theta = A_3^+ / gamma^-` | `2128573712302080 / 2160534643703221 < 1` |
| Area of the final certified loss union | `5543321 / 4194304` |

The full loss replay must leave zero unresolved area. The kernel verification must cover all required rows and certify a lower bound at least as strong as the stated `gamma^-`. A different rigorous implementation may produce a stronger bound without reproducing every final floating-point bit.

Together with the analytic arguments in the manuscript, the inequality `theta < 1` yields the uniform recurrence `a_(k+2) <= theta * a_k` for `k >= 5`, and hence zero area of the filled Julia set and Julia set.

## Verification scope and records

Both the landing and loss constructions combine inverse Koebe estimates with direct whole-cell propagation. Landing refinement replaces previously retained cells. Loss accounting takes exact unions, removes overlap within each batch and against preceding batches, and counts each accepted region once.

The package includes the frozen production inputs, independent verification programs, and historical review records. `RELEASE_MANIFEST.json` inventories release inputs. Historical results under `review_records/` are distinct from new outputs under `release_runs/`.

The local Taylor, geometry, and kernel checks include separate implementations. The landing and full loss replay drivers reuse the production physical-orbit engine; they provide fresh trajectory and set checks, rather than an independent implementation of that engine. Analytic continuation, the disjointness arguments, all-depth Fibonacci combinatorics, and canonical identification are proved in the manuscript.

See the manuscript's section **Reproducibility and scope of the finite verification** for the detailed verification contract and the distinction between recorded review results, the checks rerun for the manuscript revision, and the complete replay recipe.

## Citation and version

This release accompanies the verification package dated **12 November 2026**. To identify the exact package used in the manuscript, cite **version v1.0.0** and its release page:

https://github.com/RunzeZhangMATH/quartic-fibonacci-verification/releases/tag/v1.0.0

Retain the archive SHA-256 above when comparing downloads or reproducing the calculation.
