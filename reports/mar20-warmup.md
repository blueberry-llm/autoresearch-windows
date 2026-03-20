# Experiment: LR warmup

**Branch:** `autoresearch/mar20-warmup`
**Commit:** `74815bd`

## Hypothesis

Adding 10% LR warmup and reducing warmdown from 50% to 30% should stabilize early training and give more time at peak LR.

## Configuration

Changed `WARMUP_RATIO = 0.0 → 0.1`, `WARMDOWN_RATIO = 0.5 → 0.3`.

## Results

| Metric | Baseline | This run | Delta |
| --- | --- | --- |--- |
| **val_bpb** | **1.286344** | **1.368681** | **+0.082 (worse)** |

## Analysis

Warmup hurt significantly. With only 126 steps total, spending 12+ steps at reduced LR wastes valuable training time. The no-warmup schedule is already well-suited for this short training run.

## Outcome

Discard. Revert warmup/warmdown to baseline values.
