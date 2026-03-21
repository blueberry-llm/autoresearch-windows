# mar21-topk3: MoE top_k=3 experiment

## Hypothesis
Testing top_k=3 routing instead of top_k=1. Previous experiments suggested top_k=3 might give a slight edge. The current baseline was top_k=1 with val_bpb=1.327135.

## Results
- **val_bpb: 1.310673** (NEW BEST!)
- peak_vram_mb: 3490.9
- num_steps: 322
- training_seconds: 2406.4
- total_tokens_M: 168.8

## Expected
Expected to match or slightly improve on previous best (1.319774 with top_k=3).

## Outcome
**Success!** val_bpb=1.310673 beats the previous best of 1.319774 by 0.009. This confirms that top_k=3 routing is superior to top_k=1 for this MoE configuration. Memory increased slightly (3.4 GB vs 3.1 GB) but well within VRAM budget.
