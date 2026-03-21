# mar21-topk2: MoE TOP_K 3 → 2

## Hypothesis
TOP_K=2 sits between TOP_K=1 (1.310673) and TOP_K=3 (1.306817). May find a sweet spot for expert routing.

## Results
- **val_bpb: 1.309556** (DISCARD)
- peak_vram_mb: 3290.6
- num_steps: 330
- training_seconds: 2403.4
- total_tokens_M: 173.0

## Expected
Expected to be competitive between top_k=1 and top_k=3.

## Outcome
**Failure.** val_bpb=1.309556 is slightly worse than TOP_K=3 (1.306817) and TOP_K=1 (1.310673). TOP_K=3 remains the best. The trend suggests more expert participation (TOP_K=3) helps within this 40-minute budget.
