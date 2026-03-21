# MoE 8 Experts Experiment

**Date:** 2026-03-20
**Branch:** `autoresearch/mar20-moe-8experts`
**Commit:** 39cfc3e

## Config
- ASPECT_RATIO = 32 (n_embd = 256)
- HEAD_DIM = 128 (4 heads)
- **N_EXPERTS = 8**, TOP_K = 2
- DEPTH = 8, batch = 4
- 35.7M params (+4.2M vs 4-expert baseline)

## Results
| Metric | Value |
|---|---|
| **val_bpb** | **1.3457** |
| Steps | 305 |
| VRAM | 3.4 GB |
| Tokens | 159.9M |
| Time | 40 min |
| MFU | 8.7% |

## Outcome: **DISCARD** — worse than 4-expert baseline (1.330)

## Comparison
| Config | val_bpb | Params | Steps |
|---|---|---|---|
| MoE AR=32, 4 experts, top-2 | 1.330 | 31.5M | 329 |
| **MoE AR=32, 8 experts, top-2** | **1.346** | **35.7M** | **305** |

## Analysis
Doubling experts from 4 to 8 increases parameters by 4.2M but degrades val_bpb by 0.016 (1.330→1.345). The model also trains fewer steps (305 vs 329) due to increased compute per step. At this model size (31-36M params), 8 experts may be excessive — each expert has only ~5.3K tokens per step (262K total / 8 / 6 layers ≈ 5.5K), which is insufficient for meaningful specialization. With 4 experts, each gets ~10K tokens per step, still small but twice as much.

The balance loss (computed as n_experts × Σ(frac_tokens_per_expert)²) increases with more experts, encouraging more uniform routing. However, the auxiliary loss gradient may destabilize training with many experts, especially when each expert receives few tokens.

## Key Insight
More experts hurt at small scale. 4 experts is the sweet spot for this model size — enough tokens per expert for partial specialization without excessive routing overhead.
