# Experiment: matrix LR 0.08 to 0.10

**Branch:** `autoresearch/mar20-lr10`
**Commit:** `9c7021c`

## Results

| Metric | Best (LR=0.08) | This run (LR=0.10) | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.270597** | **1.279071** | **+0.008 (worse)** |

## Analysis

Past the sweet spot. LR 0.10 is too aggressive — the model overshoots good minima. Optimal matrix LR is ~0.08.

## Outcome

Discard. Keep MATRIX_LR=0.08 as baseline going forward.
