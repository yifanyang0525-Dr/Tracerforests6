# TracerForests6

A lightweight, heuristic pipeline for IPv6 router alias resolution using traceroute path similarity, multi-vantage probing, and ML-based validation. <!-- :contentReference[oaicite:0]{index=0} -->

> **Open-source notice: The code and associated materials will be released after the paper passes peer review / upon acceptance.**

## Overview

* **Problem:** IPv6 alias resolution is critical for router-level topology and inventorying, but the IPv6 address space makes IPv4-style “exhaustive interface collection + aliasing” impractical at scale. <!-- :contentReference[oaicite:1]{index=1} -->
* **Why it matters:** Correctly mapping multiple IPv6 interface addresses to the same router supports topology reconstruction and downstream network analysis tasks. <!-- :contentReference[oaicite:2]{index=2} -->
* **Core idea:** For two IPv6 addresses on the same router, single-prober traceroute paths tend to have the **same hop count** and be **highly similar**, differing at **one hop** or a **small number of isolated hops**—these divergences are mined as candidate alias pairs. <!-- :contentReference[oaicite:3]{index=3} -->
* **Validation + expansion:** Candidate pairs are validated by a **Random Forest classifier** trained on multidimensional path features, then used as new seeds in an **iterative, multi-vantage expansion** until convergence. <!-- :contentReference[oaicite:4]{index=4} -->

## Key Features

* **Seeded discovery:** Starts from a set of high-confidence **initial alias pairs** and iteratively expands the alias inventory. <!-- :contentReference[oaicite:5]{index=5} -->
* **Single-VP candidate mining:** Generates candidates from **equal-length** traceroute paths by identifying **unique** or **locally isolated** divergent hops. <!-- :contentReference[oaicite:6]{index=6} -->
* **Multi-VP cascade:** Uses geographically distributed **vantage points (VPs)**; validated pairs are aggregated and redistributed as seeds for subsequent probing rounds. <!-- :contentReference[oaicite:7]{index=7} -->
* **Alias-relation propagation:** Aggregates newly validated pairs to derive additional implied alias relations, forming auxiliary seeds for the next frontier. <!-- :contentReference[oaicite:8]{index=8} -->
* **Feature-driven validation:** Random Forest validation over easily collectable features derived from traceroute paths, RTT, hop-limit behavior, and address characteristics. <!-- :contentReference[oaicite:9]{index=9} -->
* **Convergence control (ε):** Iterations stop when the **growth ratio** falls below a threshold ε (stabilization criterion). <!-- :contentReference[oaicite:10]{index=10} -->

## Method Summary

1. **Input seeds:** Provide initial, high-confidence alias pairs. <!-- :contentReference[oaicite:11]{index=11} -->
2. **Probe paths:** From each VP, run traceroute to both addresses of each seed pair. <!-- :contentReference[oaicite:12]{index=12} -->
3. **Mine candidates:** For equal-length paths, extract candidate alias hops from unique/isolated divergences. <!-- :contentReference[oaicite:13]{index=13} -->
4. **Extract features + classify:** Compute feature vectors and validate candidates using the pre-trained Random Forest classifier. <!-- :contentReference[oaicite:14]{index=14} -->
5. **Aggregate + propagate:** Merge validated pairs, apply alias-relation propagation, and distribute auxiliary seeds for the next iteration. <!-- :contentReference[oaicite:15]{index=15} -->
6. **Stop on convergence:** Repeat until the growth ratio is below ε. <!-- :contentReference[oaicite:16]{index=16} -->

## Quickstart

### Installation

```bash
# TODO: Replace with the exact installation steps and dependencies
# provided in the released code (e.g., requirements.txt / Dockerfile).

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt  # TODO
```

### Run

```bash
# TODO: Replace "tracerforests6" with the actual entrypoint and flags.

python -m tracerforests6 \
  --seeds <initial_alias_pairs> \
  --vps <vantage_points_config> \
  --epsilon <epsilon> \
  --out <discovered_alias_pairs>
```

* `--seeds`: initial alias pairs (the starting set). <!-- :contentReference[oaicite:17]{index=17} -->
* `--vps`: configuration for geographically distributed probers / vantage points. <!-- :contentReference[oaicite:18]{index=18} -->
* `--epsilon`: convergence threshold ε for the stabilization criterion. <!-- :contentReference[oaicite:19]{index=19} -->
* `--out`: output list of validated alias pairs.

## Paper Results

* **Reported in the paper:** Alias-pair identification accuracy of **96.85%** on the combined ground-truth dataset. <!-- :contentReference[oaicite:20]{index=20} -->
* **Reported in the paper:** Overall expansion of confirmed alias pairs by **1.78×** over the initial seed set (e.g., total 68,766 validated alias pairs vs. 38,538 initial). <!-- :contentReference[oaicite:21]{index=21} -->
* **Reported in the paper:** In a controlled comparison, TracerForests6 discovered **6,216** alias pairs vs. **882** for TBT (≈ **7×** more). <!-- :contentReference[oaicite:22]{index=22} -->



## License & Responsible Use

* **License: TBD after peer review**

Please use responsibly:

* Minimize measurement load (smallest feasible packets; strict limits on probe frequency and volume) to avoid congestion or device degradation. <!-- :contentReference[oaicite:24]{index=24} -->
* Follow active-measurement best practices and ensure probing is legal/compliant; avoid anomalous packets that could harm routers or intermediates. <!-- :contentReference[oaicite:25]{index=25} -->
* Treat derived alias/topology data as potentially sensitive; apply anonymization before public release of sensitive artifacts. <!-- :contentReference[oaicite:26]{index=26} -->

