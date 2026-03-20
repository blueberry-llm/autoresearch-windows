# Experiment: MoE batch size comparison

## Batch=8

**Branch:** `autoresearch/mar20-moe-batch8`
**Commit:** `54f5679`

| Metric | batch=16 | batch=8 | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.598648** | **1.540797** | **-0.058 (better)** |
| Steps | 187 | 210 | +23 |
| Peak VRAM | 15.3 GB | 8.2 GB | -7.1 GB |

## Batch=4

**Branch:** `autoresearch/mar20-moe-batch4`
**Commit:** `7b1909a`

| Metric | batch=8 | batch=4 | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.540797** | **1.529510** | **-0.011 (better)** |
| Steps | 210 | 214 | +4 |
| Peak VRAM | 8.2 GB | 4.6 GB | -3.6 GB |

## Analysis

Smaller batches consistently improve val_bpb for MoE. More optimizer steps within the same time budget. VRAM usage drops dramatically (15.3 → 4.6 GB going from batch=16 to batch=4).

Note: batch=6 was tested but rejected — `2^19 / (6 * 2048) = 42.67` is not an integer. Only powers of 2 are valid batch sizes with current TOTAL_BATCH_SIZE.

## Outcome

batch=4 is the best. Used for subsequent experiments.
