# MoE Shared Expert Experiment

**Date:** 2026-03-20
**Branch:** `autoresearch/mar20-moe-shared1`
**Commit:** 1645919

## Config
- ASPECT_RATIO = 32 (n_embd = 256)
- HEAD_DIM = 128 (4 heads)
- N_EXPERTS = 4, TOP_K = 2
- **N_SHARED_EXPERTS = 1**
- DEPTH = 8, batch = 4
- 32.5M params (+1M for shared expert)

## Results
| Metric | Value |
|---|---|
| **val_bpb** | **1.3325** |
| Steps | 319 |
| VRAM | 3.4 GB |
| Tokens | 167.7M |
| Time | 40 min |
| MFU | 8.7% |

## Outcome: **DISCARD** — worse than top_k=1 (1.320) and top_k=2 baseline (1.330)

## Analysis
Adding 1 shared expert (processed by every token, always active) slightly degrades val_bpb from 1.330 to 1.3325. The shared expert adds ~1M params per layer (256×256×2 weights) but the model also trains 10% more steps (319 vs 290) due to slightly faster per-step time (7.5s vs 8.3s — the shared expert computation doesn't significantly slow things down). However, the extra steps don't compensate for the architectural change.

The shared expert approach, popularized by Mixtral 8×7B and DeepSeekMoE, works well at large scale where:
1. The shared expert captures general knowledge used by all tokens
2. Routed experts specialize in different subsets

At small scale (31.5M params, 4 experts), the overhead of a shared expert may hurt routing quality — the router now distributes tokens across only 4 experts instead of 5 (4 routed + 1 shared), reducing specialization. Additionally, the shared expert may duplicate knowledge that the routed experts already capture, wasting capacity.

## Key Insight
Shared experts don't help at small scale. The routing overhead and reduced expert specialization outweigh the potential benefits of always-active computation. Revisit at larger model sizes.
