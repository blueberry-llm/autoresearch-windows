# Experiment: matrix LR 0.04 to 0.06

**Branch:** `autoresearch/mar20-lr06`
**Commit:** `77c572e`

## Hypothesis

Muon's Newton-Schulz orthogonalization normalizes gradient updates, so it can often tolerate higher learning rates than AdamW. Increasing matrix LR from 0.04 to 0.06 (50% higher) should speed up convergence.

## Configuration

Changed `MATRIX_LR = 0.04 → 0.06`.

## Results

| Metric | Best (batch=16) | This run | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.286344** | **1.280021** | **-0.006 (better)** |
| Steps | 127 | 127 | same |
| Tokens | 66.6M | 66.6M | same |

## Analysis

The higher matrix LR gives a modest but consistent improvement. The orthogonalization in Muon keeps the update direction stable even at higher LR, allowing faster convergence within the fixed time budget.

## Outcome

Keep. New best val_bpb: 1.280021.
