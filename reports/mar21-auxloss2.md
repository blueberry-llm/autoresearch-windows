# mar21-auxloss2: MoE aux_loss_weight 0.001 → 0.0001

## Hypothesis
Further reducing aux_loss_weight from 0.001 to 0.0001 allows even more freedom for expert specialization. The load balancing penalty becomes negligible.

## Results
- **val_bpb: 1.306817** (NEW BEST!)
- peak_vram_mb: 3489.2
- num_steps: 322
- training_seconds: 2403.4
- total_tokens_M: 168.8

## Expected
Expected continued improvement from reduced aux_loss overhead.

## Outcome
**Success!** val_bpb=1.306817 is better than parent (1.310250). The trend of reducing aux_loss_weight continues to improve results. The model can now route tokens almost entirely based on the main loss.
