# Experiment: matrix LR 0.06 to 0.08

**Branch:** `autoresearch/mar20-lr08`
**Commit:** `5d93417`

## Results

| Metric | Previous best (LR=0.06) | This run (LR=0.08) | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.280021** | **1.270597** | **-0.009 (better)** |

## Analysis

Continued improvement from higher matrix LR. Muon's orthogonalization handles the larger step sizes well. The trend suggests the optimal LR may be even higher.

## Outcome

Keep. New best val_bpb: 1.270597.
