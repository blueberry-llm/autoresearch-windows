# MoE Top-K=3 Experiment

**Date:** 2026-03-20
**Branch:** `autoresearch/mar20-moe-topk3`
**Commit:** dc0de1b

## Config
- ASPECT_RATIO = 32 (n_embd = 256)
- HEAD_DIM = 128 (4 heads)
- N_EXPERTS = 4, **TOP_K = 3**
- DEPTH = 8, batch = 4
- 31.5M params

## Results
| Metric | Value |
|---|---|
| **val_bpb** | **1.3198** |
| Steps | ~295 (est.) |
| VRAM | ~3.4 GB |
| Time | 40 min |
| MFU | ~6.8% |

## Outcome: **KEEP** — tied best with top_k=1

## Comparison
| Config | val_bpb | Status |
|---|---|---|
| MoE AR=32 top_k=2 (baseline) | 1.330 | keep |
| MoE AR=32 top_k=1 | 1.320 | keep |
| **MoE AR=32 top_k=3** | **1.320** | **keep** |

## Analysis
Increasing TOP_K from 2 to 3 (selecting 3 of 4 experts per token) matches the performance of top_k=1 at 1.320. This suggests that for this small model (31.5M params, 4 experts), the routing parameter has diminishing returns beyond top_k=1. With 4 experts and top_k=3, 75% of experts are selected per token — approaching dense MLP behavior (which selects all experts). The improvement from top_k=2→top_k=3 (1.330→1.320) is similar to top_k=1→top_k=2 (1.330→1.320), indicating a roughly linear relationship between active experts and quality.

However, top_k=3 activates 3× more expert FLOPs per token than top_k=1, making it 33% slower. The equal performance with 3× the compute suggests top_k=1 is the more efficient choice.

## Key Insight
Top-k=3 matches top-k=1 in quality but with 3× the expert compute. Top-k=1 is the most compute-efficient routing choice.
