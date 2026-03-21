# mar21-wd1: MoE weight_decay 0.2 → 0.1

## Hypothesis
Reducing weight_decay from 0.2 to 0.1 reduces regularization. Less L2 penalty could allow the model to fit the training data better within the 40-minute budget.

## Results
- **val_bpb: 1.360533** (DISCARD)
- peak_vram_mb: 3490.2
- num_steps: 321
- training_seconds: 2404.7
- total_tokens_M: 168.3

## Expected
Expected potential improvement from reduced regularization overhead.

## Outcome
**Failure.** val_bpb=1.360533 is significantly worse than parent (1.306817). Less weight decay hurts — the model likely overfits or destabilizes without sufficient regularization. The current weight_decay=0.2 appears well-tuned.
