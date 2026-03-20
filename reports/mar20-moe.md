# Experiment: MoE with 4 experts, top-2 routing

**Branch:** `autoresearch/mar20-moe`
**Commit:** `7488474`

## Results

| Metric | Best (dense) | This run (MoE) | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.270597** | **1.713739** | **+0.443 (much worse)** |
| Steps | 127 | 134 | +7 |
| Tokens | 66.6M | 70.3M | +3.7M |
| Peak VRAM | 9.5 GB | 15.4 GB | +5.9 GB |

## Analysis

MoE failed badly. Root causes:
1. Expert capacity too small: expert_dim=n_embd=512 vs dense MLP hidden=2048 (4x smaller)
2. Router overhead wastes capacity without enough data to specialize
3. VRAM nearly maxed (15.4/16 GB) from storing 4 expert weight matrices
4. 70M tokens insufficient for expert specialization

## Outcome

Discard. MoE is not viable at this scale. Revert to dense MLP.
