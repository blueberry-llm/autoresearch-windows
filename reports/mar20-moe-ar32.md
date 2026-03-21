# Experiment: MoE ASPECT_RATIO=32

**Branch:** `autoresearch/mar20-moe-ar32`
**Commit:** `522eb95`

## Configuration

MoE with 4 experts, top-2, ASPECT_RATIO=32 (n_embd=256), batch=4, 40-minute budget.

## Results

| Metric | AR=64 (batch=4) | AR=32 (batch=4) | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.529510** | **1.330348** | **-0.199 (much better!)** |
| Params | 75.5M | 31.5M | -44M |
| Steps | 214 | 329 | +115 |
| Peak VRAM | 4.6 GB | 3.3 GB | -1.3 GB |

## Analysis

Dramatic improvement. The smaller model (31.5M vs 75.5M) trains 54% more steps (329 vs 214) in the same time budget. Each MoE expert is smaller (256d vs 512d), making each forward pass much faster. The total compute per step is roughly 4x less, allowing 1.5x more steps.

This approaches the dense baseline val_bpb (1.271) — within 0.06. The remaining gap is likely due to MoE routing overhead and smaller expert capacity.

## Outcome

**Keep as baseline.** This config (4 experts, top-2, AR=32) is the reference. It has been surpassed by **top_k=1** (1.320) and **top_k=3** (1.320). Updated best MoE config is **top_k=1** at 1.320 — now the daily branch default.
