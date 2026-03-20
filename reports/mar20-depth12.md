# Experiment: depth 8 to 12

**Branch:** `autoresearch/mar20-depth12`
**Commit:** `b9c0748`

## Hypothesis

Increasing model depth from 8 to 12 layers should improve val_bpb by giving the model more capacity to learn representations. The baseline uses only 3 GB of 16 GB VRAM, so there is significant headroom for a larger model.

## Configuration

Changed `DEPTH = 8` to `DEPTH = 12` in `train.py`.

| Setting | Baseline | This run |
| --- | --- | --- |
| Depth | 8 | 12 |
| Total params | 75.5M | 185.6M |
| Embed dim | 512 | 768 (12 × 64) |
| Heads | 4 | 6 |

## Results

| Metric | Baseline | This run | Delta |
| --- | --- | --- | --- |
| **val_bpb** | **1.302671** | **1.536281** | **+0.234 (worse)** |
| Steps | 129 | 67 | -48% |
| Tokens | 67.6M | 35.1M | -48% |
| Peak VRAM | 3.0 GB | 4.3 GB | +1.3 GB |
| MFU | 10.3% | 13.4% | +3.1% |

## Analysis

The experiment failed. The larger model is ~48% slower per step, so it sees ~48% fewer tokens in the same 30-minute budget. The extra capacity doesn't compensate for the reduced data exposure. MFU improved (13.4% vs 10.3%) indicating the larger model uses the GPU more efficiently per FLOP, but the total token count is the bottleneck.

**Key insight:** On this hardware/time budget, throughput (tokens seen) matters more than model capacity. The ideal model is the largest one that still maintains high step throughput.

## Outcome

Discard. The baseline 75.5M param model is better for this time budget. Future experiments should focus on improving training efficiency (higher batch size, better optimizer, architecture changes that don't slow down throughput) rather than simply scaling up.
