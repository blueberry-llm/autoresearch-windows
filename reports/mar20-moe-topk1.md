# MoE Top-K=1 Experiment

**Date:** 2026-03-20
**Branch:** `autoresearch/mar20-moe-topk1`
**Commit:** 43d3e0e

## Config
- ASPECT_RATIO = 32 (n_embd = 256)
- HEAD_DIM = 128 (4 heads)
- N_EXPERTS = 4, **TOP_K = 1**
- DEPTH = 8, batch = 4
- 31.5M params

## Results
| Metric | Value |
|---|---|
| **val_bpb** | **1.3202** |
| Steps | ~291 (est.) |
| VRAM | ~3.3 GB |
| Time | 40 min |
| MFU | ~6.7% |

## Outcome: **KEEP** — tied best with top_k=3, best routing config

## Comparison
| Config | val_bpb | Status |
|---|---|---|
| MoE AR=32 top_k=2 (baseline) | 1.330 | keep |
| **MoE AR=32 top_k=1** | **1.320** | **keep** |
| MoE AR=32 top_k=3 | 1.320 | keep |

## Analysis
Reducing TOP_K from 2 to 1 (selecting only the single best expert per token) improves val_bpb from 1.330 to 1.320 — a 0.010 improvement. This is surprising since top_k=2 allows tokens to leverage two experts, which should provide more representational diversity. However, top_k=1 may benefit from:

1. **Stronger expert specialization**: Each expert receives more tokens on average (1/4 of tokens vs 1/2 with top_k=2), forcing deeper specialization.
2. **Reduced routing overhead**: No weight normalization/renormalization needed.
3. **Less noisy routing**: With top_k=2, the second-best expert selection may introduce noise from the router.

The improvement is modest (~0.8%) but consistent with the "less is more" principle in routing — fewer, more decisive routing decisions may be better than many ambiguous ones.

## Key Insight
Top-k=1 routing outperforms top-k=2 for this model size. Fewer, more decisive expert selections may reduce noise and improve specialization.
