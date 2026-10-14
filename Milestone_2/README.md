Milestone 2 – Improve:

    Replace the backbone with DINOv2 and CLIP models.
    Compare three ways of adapting them: a linear classifier on frozen features, LoRA, and full fine-tuning.

## How to run

One Colab notebook per method x backbone (12 runs each: 4 held-out domains x 3 seeds), plus one notebook for the final table. The run notebooks are independent, so different team members can run them in parallel. Everything stays on the Colab VM (no Google Drive): after each run the notebook prints a `RESULT {...}` line, and the saved notebook output is the record.

| Notebook | Runs | Est. time (A100) | Who |
|---|---|---|---|
| `M2_run_lp_dinov2.ipynb` | 12 | ~2.0 h | |
| `M2_run_lp_clip.ipynb` | 12 | ~1.8 h | |
| `M2_run_lora_dinov2.ipynb` | 12 | ~2.2 h | |
| `M2_run_lora_clip.ipynb` | 12 | ~2.0 h | |
| `M2_run_fullft_dinov2.ipynb` | 12 | ~3.4 h | |
| `M2_run_fullft_clip.ipynb` | 12 | ~2.8 h | |
| `M2_results.ipynb` | final table + CLIP zero-shot | ~10 min | |

Times are estimates (+ ~10 min setup per session).

1. Open a run notebook on an A100 runtime, set `SESSION_BUDGET_MIN` (section 4) to how long you can stay, *Run all*. No run is started that cannot finish within the budget.
2. Next session: paste the `RESULT` lines printed so far into `PREVIOUS_RESULTS` (section 4) and *Run all* again: finished runs are skipped.
3. When a notebook has all 12 runs, paste the output of its section 7 into `ALL_RESULTS` in `M2_results.ipynb`.

Model selection is always the training-domain validation set (never the oracle).


