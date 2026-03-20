# Baseline: ultrafineweb on RTX 4060 Ti 16GB

**Branch:** `autoresearch/mar20`
**Commit:** `6b4e29f`

## Hypothesis

Establish the baseline val_bpb on the ultrafineweb dataset with the default model configuration on RTX 4060 Ti 16GB (Ada, CC 8.9).

## Configuration

| Setting | Value |
| --- | --- |
| Dataset | ultrafineweb (CrowdMind/ultrafineweb_dolma_shuffled) |
| Model depth | 8 layers |
| Heads | 4 (head_dim=128) |
| Embed dim | 512 |
| Total params | 75.5M (33.6M value embeddings, 25.2M transformer, 8.4M wte, 8.4M lm_head) |
| Sequence length | 2048 |
| Vocab size | 16,384 |
| Window pattern | SSSL (short/short/short/long) |
| Optimizer | Muon (matrix params) + AdamW (embeddings/scalars) |
| AMP dtype | bfloat16 |
| Attention | SDPA |
| Autotune result | batch_size=4, checkpointing=on |

## Results

| Metric | Value |
| --- | --- |
| **val_bpb** | **1.302671** |
| Training steps | 129 |
| Total tokens | 67.6M |
| Training time | 1812.9s (~30.2 min) |
| Total time | 2162.0s (~36.0 min) |
| Peak VRAM | 3033.1 MB (2.96 GB) |
| MFU | 10.29% |
| Throughput | ~34,120 tok/sec |

## Notes

- Loss decreased from ~9.7 to ~3.94 over 129 steps. Still converging when the 30-minute budget ended.
- VRAM headroom is significant (~13 GB free), leaving room for larger models.
- MFU at 10.3% is modest — typical for small models on consumer GPUs with SDPA.
- Batch size was autotune-selected as 4 (smallest viable), suggesting the value embeddings (33.6M params) are the main memory consumer.
- The model enters epoch 3 by the end of training, so it's seen the data ~2.5 times.

## Outcome

Baseline established. Future experiments will attempt to beat val_bpb 1.302671.
