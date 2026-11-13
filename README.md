<div align="center">
  <h1>From Benchmark to Decision</h1>
  <h3>Reproducibility, Stability, and Robust Suite Selection for Post-Quantum TLS 1.3</h3>
  <p><strong>Independent replication · Ranking stability · Uncertainty-aware cryptographic migration</strong></p>
  <p>
    <a href="#associated-publication"><img alt="Accepted paper" src="https://img.shields.io/badge/Research-Accepted%20Paper-167347?style=flat-square"></a>
    <a href="#study-overview"><img alt="TLS 1.3" src="https://img.shields.io/badge/Protocol-TLS%201.3-2855A6?style=flat-square"></a>
    <a href="#experimental-design"><img alt="Three independent runs" src="https://img.shields.io/badge/Independent%20runs-3-6A4C93?style=flat-square"></a>
    <a href="#results-at-a-glance"><img alt="Approximately 4.72 million handshakes" src="https://img.shields.io/badge/Handshakes-%E2%89%884.72%20million-48566A?style=flat-square"></a>
    <a href="#repository-contents"><img alt="Six CSVs and manifest" src="https://img.shields.io/badge/Dataset-6%20CSVs%20%2B%20manifest-555555?style=flat-square"></a>
  </p>
  <p><em>When benchmark rankings are nearly identical, are the decisions based on them equally reliable?</em></p>
</div>Study overview
Post-quantum TLS studies often rank algorithms using results from a single measurement campaign, even when performance differences are smaller than normal system variability. This study independently executes the public Docker artifact of Sosnowski et al. three times on a Windows 11/WSL2 platform to determine which conclusions are sufficiently stable for cryptographic migration decisions. Three runs cover 23 key-establishment and 24 authentication configurations (22 and 23 adjacent pairs), totaling approximately 4.72 million handshakes. Rankings are highly repeatable, with pairwise Spearman correlations of 0.9983–1.0000 for key establishment and 0.9991–0.9998 for authentication. Median coefficients of variation are 2.94% and 3.69%, respectively. Despite this consistency, complete rankings can overstate certainty. Run ranges overlap for 12 of 22 adjacent key-establishment pairs and 8 of 23 adjacent authentication pairs. Dilithium5 and Falcon512, for example, differ by only 3 μs in mean latency, while their ordering changes or ties across runs. To address this limitation, this study distinguishes observed-range separation from unresolved near-ties and introduces a replication-aware selector. It applies security, hybridization, latency, byte-volume, and segment constraints using worst-observed latency and a variability penalty. The results confirm broad family-level trends: Kyber provides low latency with higher communication overhead, high-category hybrids are influenced mainly by their classical component, and Dilithium and Falcon exhibit a latency–bandwidth trade-off. Migration policy should therefore rely on stable performance regions rather than a single total ranking.
[!IMPORTANT]
Central finding: Near-perfect agreement between overall rankings does not guarantee that adjacent configurations can be distinguished reliably. Migration decisions should account for observed variability and unresolved near-ties.

Results at a glance
Measurement	Key establishment	Authentication
Configurations evaluated	23	24
Adjacent ranked pairs	22	23
Pairwise Spearman rank correlation, across runs	0.9983–1.0000	0.9991–0.9998
Median coefficient of variation	2.94%	3.69%
Adjacent pairs with overlapping run ranges	12 / 22 (54.5%)	8 / 23 (34.8%)


<p align="center"><strong>3 independent runs</strong> &nbsp;·&nbsp; <strong>47 configurations</strong> &nbsp;·&nbsp; <strong>≈ 4.72 million TLS handshakes</strong></p>The correlation and variability figures describe strong aggregate repeatability. The overlap counts show why local comparisons still require caution: a high global correlation does not by itself resolve every neighboring rank.
Research contribution
This work extends a public post-quantum TLS benchmark from measuring performance to assessing the stability of performance-based decisions.
1. Independent repetition. Execute the publicly available, containerized TLS measurement artifact in three campaigns on a Windows 11/WSL2 host.
2. Replication-aware interpretation. Evaluate both global ranking consistency and the observed run-to-run variability of individual configurations.
3. Near-tie identification. Distinguish adjacent configurations whose observed latency ranges are separated from those whose ranges overlap.
4. Constraint-aware selection. Consider security level, hybridization, latency, transmitted byte volume, and deployment segment together, rather than relying on latency rank alone.
5. Conservative decision criteria. Account for worst-observed latency and a variability penalty when identifying viable migration choices.
From measurements to decisions
```mermaid
flowchart LR
    A[Public TLS 1.3 Docker artifact] --> B[Three independent runs]
    B --> C[Latency datasets]
    C --> D[Ranking stability and variability]
    D --> E[Observed-range overlap analysis]
    E --> F[Constraint-aware suite selection]
    F --> G[Stable migration options]
```
This workflow summarizes the study's analytical approach; it is not a claim that all analysis scripts or deployment tooling are included in this repository.
Principal findings
Finding	Implication for migration
Rankings are highly reproducible. Spearman correlations are approximately 1 across both experiment groups.	Broad comparative trends are repeatable on the tested platform.
Some adjacent comparisons remain unresolved. Run ranges overlap for 12/22 key-establishment pairs and 8/23 authentication pairs.	A complete ordered list can suggest more precision than the measurements support.
Dilithium5 and Falcon512 form a close comparison. Their mean latencies differ by only 3 μs, with ordering changes or ties across runs.	Very small apparent advantages should not automatically drive a migration choice.
Kyber exhibits low latency but higher communication overhead.	Consider handshake byte volume alongside execution time.
High-category hybrid performance depends strongly on the classical component.	Evaluate the actual hybrid configuration, not only the post-quantum constituent.
Dilithium and Falcon exhibit a latency–bandwidth trade-off.	Select according to deployment requirements rather than a universal winner.


[!NOTE]
An overlap of observed run ranges is a descriptive indication that the ordering is not resolved by these runs. It is not a formal equivalence test or a statistical confidence interval.

Experimental design
Element	Description
Baseline artifact	Public containerized measurement framework accompanying Sosnowski et al., The Performance of Post-Quantum TLS 1.3 (2023)
Replication platform	Windows 11 with Windows Subsystem for Linux 2 (WSL2)
Measurement campaigns	Three independently executed runs
Experiment groups	Key establishment (all-kem) and authentication (all-sig)
Coverage	23 key-establishment and 24 authentication configurations
Scale	Approximately 4.72 million TLS handshakes across the three runs
Core analyses	Rank agreement, variation, observed-range overlap, and replication-aware selection


Interpretation of stability
The study deliberately separates two questions:
- Global stability: Are overall configuration rankings similar across independent runs?
- Local resolution: Are the performance differences between neighboring configurations sufficiently separated in the observed measurements to support a definite preference?
These are not equivalent. The proposed selector uses measured latency conservatively, alongside security and communication constraints, so that uncertainty can affect the final decision rather than being concealed by a total ranking.
Repository contents
This repository shares the six latency CSV files from the three measurement campaigns, together with their manifest.
Post-Quantum-TLS-1.3/
├── README.md
├── CSV_MANIFEST.txt
├── run1_all-kem_latencies.csv
├── run1_all-sig_latencies.csv
├── run2_all-kem_latencies.csv
├── run2_all-sig_latencies.csv
├── run3_all-kem_latencies.csv
└── run3_all-sig_latencies.csv
File	Description
[`CSV_MANIFEST.txt`](CSV_MANIFEST.txt)	Dataset manifest and inventory
[`run1_all-kem_latencies.csv`](run1_all-kem_latencies.csv)	Key-establishment latencies — run 1
[`run1_all-sig_latencies.csv`](run1_all-sig_latencies.csv)	Authentication latencies — run 1
[`run2_all-kem_latencies.csv`](run2_all-kem_latencies.csv)	Key-establishment latencies — run 2
[`run2_all-sig_latencies.csv`](run2_all-sig_latencies.csv)	Authentication latencies — run 2
[`run3_all-kem_latencies.csv`](run3_all-kem_latencies.csv)	Key-establishment latencies — run 3
[`run3_all-sig_latencies.csv`](run3_all-sig_latencies.csv)	Authentication latencies — run 3


Working with the data
1. Consult [`CSV_MANIFEST.txt`](CSV_MANIFEST.txt) for the published dataset inventory and metadata.
2. Treat the run1, run2, and run3 files as separate measurement campaigns; retain run identities during analysis.
3. Analyze all-kem and all-sig as distinct experiment groups, and verify configuration alignment before comparing runs.
4. Report both aggregate rank consistency and variability or overlap for comparisons that affect a selection decision.
[!CAUTION]
This repository is a measurement-data release, not a complete, one-command replication package. It does not claim to include the original Docker environment, experiment scripts, or the full analysis/selector implementation. For the upstream benchmark implementation, follow the links below.

Reproducibility and scope
These measurements support independent examination of the study's reported latency observations. Three runs on one host environment help characterize within-platform repeatability; they do not establish performance stability across every CPU, network, virtualization layer, TLS stack, or production environment.

The selection framework is intended to inform migration planning, not to replace cryptographic security review, interoperability testing, operational validation, or deployment-specific performance measurements. Algorithm names reflect the configurations investigated in the study; readers should verify current standards and implementation support before deployment.
Associated publication
From Benchmark to Decision: Reproducibility, Stability, and Robust Suite Selection for Post-Quantum TLS 1.3

Status: Accepted conference paper.
Publication link / DOI: To be added when available.

This repository contains supporting measurement files for that study. If you use the datasets in published work, please cite the associated paper once its complete bibliographic record is available.
Upstream benchmark and attribution
The measurement framework replicated in this research was developed and published by Sosnowski et al.:
M. Sosnowski, F. Wiedner, E. Hauser, L. Steger, D. Schoinianakis, S. Gallenmüller, and G. Carle, “The Performance of Post-Quantum TLS 1.3,” Companion of the 19th International Conference on Emerging Networking EXperiments and Technologies (CoNEXT ’23), 2023. https://doi.org/10.1145/3624354.3630585

- Original artifact and evaluation code: tumi8/pqs-tls-measurements
- Original artifact documentation: The Performance of Post-Quantum TLS 1.3 — Additional Material
This repository presents an independent replication and subsequent stability/selection analysis. It is not the official repository of Sosnowski et al. and does not imply their endorsement.
<p align="center"><strong>Reproducible measurements are the starting point. Reliable migration decisions require understanding what the measurements can—and cannot—distinguish.</strong></p>
