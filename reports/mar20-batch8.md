# Experiment: disable autotune, use batch_size=16

**Branch:** `autoresearch/mar20-batch8`
**Commit:** `41a6fb7`

## Hypothesis

The autotune selects batch_size=4 based on highest tok/sec, but larger batches give better gradient estimates. Disabling autotune forces the fallback candidate ordering (32, 16, 8, 4), where 16 is the first viable size.

## Configuration

Set `AUTORESEARCH_DISABLE_AUTOTUNE=1` env var. No code changes to train.py.

| Setting | Baseline | This run |
| --- | --- | --- |
| Batch size | 4 (autotuned) | 16 (fallback) |
| Grad accum steps | 64 | 16 |
| Tokens/step | 8,192 | 32,768 |

## Results

| Metric | Baseline | This run | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.302671** | **1.286344** | **-0.016 (better)** |
| Steps | 129 | 127 | -2 |
| Tokens | 67.6M | 66.6M | -1M |
| Peak VRAM | 3.0 GB | 9.5 GB | +6.5 GB |
| MFU | 10.3% | 10.1% | -0.2% |

## Analysis

The experiment succeeded. Batch size 16 gives better val_bpb (1.286 vs 1.303) with similar token throughput. The larger batch provides less noisy gradient estimates, which translates to more effective optimization despite the same total token budget.

VRAM usage jumped from 3 GB to 9.5 GB, but this is well within the 16 GB limit. MFU is essentially unchanged.

The autotune was wrong to pick batch_size=4 — it optimized for tok/sec but not for val_bpb. This is a fundamental limitation of the current autotune: it measures throughput but not learning quality.

## Outcome

Keep. This should become the new default. To make permanent: adjust the GPU profile candidate ordering or change autotune behavior to prefer larger batches when VRAM allows.
