# Experiment: MoE baseline (batch=16, AR=64)

**Branch:** `autoresearch/mar20`
**Commit:** `c53eb3f`

## Configuration

MoE with 4 experts, top-2 routing, batch_size=16 (autotuned), ASPECT_RATIO=64, expanded dataset (59K train rows), 40-minute budget.

## Results

| Metric | Value |
| --- | --- |
| **val_bpb** | **1.598648** |
| Steps | 187 |
| Tokens | 98.0M |
| Peak VRAM | 15.3 GB |
| MFU | 11.5% |
| Params | 75.5M |

## Analysis

MoE baseline with batch=16. VRAM nearly maxed. The model sees data ~4x through training but MoE routing overhead and small expert capacity (512d) limit learning.

## Outcome

Discard. Lower batch sizes should give more steps and better results.
