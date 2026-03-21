# mar21-auxloss: MoE aux_loss_weight 0.01 → 0.001

## Hypothesis
Reducing aux_loss_weight from 0.01 to 0.001 allows the model to optimize the main loss more aggressively, potentially improving val_bpb. The load balancing constraint is loosened so experts can specialize more.

## Results
- **val_bpb: 1.310250** (NEW BEST!)
- peak_vram_mb: 3490.1
- num_steps: 321
- training_seconds: 2407.0
- total_tokens_M: 168.3

## Expected
Expected a small improvement from reduced aux_loss overhead.

## Outcome
**Success!** val_bpb=1.310250 is slightly better than the parent experiment (1.310673). The reduced load balancing penalty allows the model to route tokens more freely to specialized experts, improving compression.
