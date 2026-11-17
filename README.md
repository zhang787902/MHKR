# MHKR: Information-Theoretic Method Selection for TKG Link Prediction

> **Paper:** *Information-Theoretic Method Selection for Temporal Knowledge Graph Link Prediction: A Training-Free Approach with Provable Optimality*
>
> **Target venue:** IEEE Transactions on Knowledge and Data Engineering (IEEE TKDE)

## Overview

This repository accompanies the paper proposing an information-theoretic framework for temporal knowledge graph (TKG) link prediction method selection. The framework:

1. **Formalizes two prediction tasks** — Plausible Target Identification (PTI) and Temporal Target Prediction (TTP) — with different information requirements
2. **Proves three theorems** establishing method-task matching guarantees with theoretical MRR upper bounds
3. **Introduces MHKR**, a zero-training method achieving MRR 0.44–0.73 on PTI tasks with 0.1s training
4. **Proposes frequency-rule fusion** reaching 96.9% of TLogic's TTP performance

## Key Results

| Task | Dataset | Method | MRR | Training Time |
|------|---------|--------|-----|---------------|
| PTI | GDELT-Weekly | MHKR | 0.731 | 0.1s |
| PTI | ICEWS14 (xERTE) | MHKR | 0.441 | 0.1s |
| PTI | ICEWS18 | MHKR | 0.449 | 0.3s |
| PTI | ICEWS0515 | MHKR | 0.529 | 0.2s |
| TTP | ICEWS14 | MHKR+TLogic (α=0.9) | 0.416 | 0.1s + TLogic |
| TTP | ICEWS14 | TLogic | 0.429 | ~20 min |

## Framework

The core insight is that a TKG dataset's information structure is characterized by two orthogonal components:

- **Entity-pair information** `I(r;(s,o))` — what entity pairs reveal about relations
- **Temporal residual information** `I(r;t|s,o)` — what time adds beyond entity pairs

These partition the total relation entropy `H(r)`, and each favors different methodological approaches:
- High `I(r;(s,o))/H(r)` → frequency-based methods (MHKR) are near-optimal for PTI
- High `I(r;t|s,o)/H(r|s,o)` → temporal modeling (TLogic) is necessary for TTP

## Repository Structure

```
mhkr/
├── mhkr_core.py              # Core MHKR implementation
├── run_experiments.py         # Main experiments (GDELT/ICEWS14)
├── run_baselines.py           # Neural baselines (TransE, TComplEx, RE-Net)
├── run_supplementary.py       # Supplementary (info theory, multi-week, fusion)
├── generate_figures.py        # Figure and table generation
├── results/                   # Experiment result JSON files
├── figures/                   # Generated figures (7 PNG + 3 PDF)
└── README.md
```

## Datasets

- **GDELT-Weekly** — International events, Jan 1–7 2024, CAMEO-coded (Source: [GDELT Project](https://www.gdeltproject.org/))
- **ICEWS14** — Standard TKG benchmark, xERTE split, 7,128 entities (Source: [Harvard Dataverse](https://dataverse.harvard.edu/))
- **ICEWS18** — Larger-scale ICEWS benchmark, 23,033 entities
- **ICEWS0515** — Extended temporal range ICEWS benchmark, 10,488 entities

## Dependencies

- Python 3.8+
- NumPy, SciPy, Pandas
- PyTorch (for neural baselines only)
- Matplotlib, Seaborn (for figure generation)
- TLogic (for fusion experiments): https://github.com/liu-yushan/TLogic

## Reproduction

```bash
# Main experiments
python run_experiments.py

# Neural baselines
python run_baselines.py

# Supplementary (information theory analysis, multi-week, fusion)
python run_supplementary.py

# Generate figures
python generate_figures.py
```

## Information Theory Analysis

| Dataset | H(r) | I(r;(s,o))/H(r) | I(r;t\|s,o)/H(r\|s,o) |
|---------|------|------------------|------------------------|
| GDELT | 4.574 bits | 47.2% | — |
| ICEWS14 | 4.945 bits | 93.4% | 63.4% |
| ICEWS18 | 5.035 bits | 93.3% | 62.6% |
| ICEWS0515 | 4.951 bits | 90.7% | 69.6% |

The dyad memory provides 4.2× more information gain than the type prior layer.

## Citation

```bibtex
@article{mhkr2026,
  title={Information-Theoretic Method Selection for Temporal Knowledge Graph Link Prediction: A Training-Free Approach with Provable Optimality},
  journal={IEEE Transactions on Knowledge and Data Engineering},
  year={2026},
  note={Under review}
}
```

## License

MIT License (code) / Dataset licenses per original sources.
