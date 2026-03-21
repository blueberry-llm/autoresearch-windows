# MoE HEAD_DIM=32 Experiment

**Date:** 2026-03-20
**Branch:** `autoresearch/mar20-moe-hd32`
**Commit:** fecaf45

## Config
- ASPECT_RATIO = 32 (n_embd = 256 = 8 heads × 32 dim)
- HEAD_DIM = 32
- N_EXPERTS = 4, TOP_K = 2
- DEPTH = 8, batch = 4
- 31.5M params

## Results
| Metric | Value |
|---|---|
| **val_bpb** | **1.4495** |
| Steps | 299 |
| VRAM | 3.3 GB |
| Tokens | 156.8M |
| Time | 40 min |
| MFU | 6.7% |

## Outcome: **DISCARD** — worse than AR=32 baseline (1.330)

## Analysis
Reducing HEAD_DIM from 128 to 32 doubles the number of attention heads from 4 to 8. While the model has the same total parameters (31.5M), the smaller head dimension likely reduces the representational capacity per head. The attention mechanism may be less effective at processing complex token interactions with only 32-dimensional query/key projections. The smaller head dim also rounds up the model dimension to 256, same as HEAD_DIM=128 (which also rounds to 256), so the effective model size is identical. The difference is purely in the number of heads (8 vs 4) and per-head dimension (32 vs 128). This suggests 4 heads with 128 dim each is more effective than 8 heads with 32 dim each.

## Key Insight
Head dimension matters more than head count. 4 heads × 128 dim is better than 8 heads × 32 dim for this model size.
