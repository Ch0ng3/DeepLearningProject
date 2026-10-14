
Milestone 3 – Contribute:

    Does fine-tuning hurt performance on the unseen domain, and can weight averaging (WiSE-FT) fix it?
    What happens with only a few training images per class?
    Are the model's confidence scores reliable on the unseen domain?

## Rules from the general guidelines (Option B)

- **No test data for model selection or tuning.** The held-out (test) domain is used only for the final evaluation. Every choice -- checkpoint, learning rate, the WiSE-FT mixing coefficient α, the number of shots, a temperature for calibration -- is made on the training-domain validation split (DomainBed's `out` splits of the training domains), never on the test domain. DomainBed's "oracle" selection is not allowed.
- **At least 3 random seeds** for the main experiment (`--trial_seed` 0, 1, 2, the same splits as Milestone 2), results reported as **mean ± standard deviation**.
- **Error analysis**: which classes and domains fail and why (confusion matrices, examples of misclassified images), and the limitations of the method.
- **At least one clear research question**, answered with evidence.

## Plan

| Question | Experiment | Inputs from Milestone 2 | Selection rule |
|---|---|---|---|
| Does fine-tuning hurt the unseen domain? Can WiSE-FT fix it? | interpolate zero-shot and fine-tuned CLIP weights, θ(α) = (1−α)·θ_zero-shot + α·θ_fine-tuned, for α ∈ {0, 0.1, …, 1} | CLIP zero-shot model; a full-FT CLIP run started from the zero-shot head (see notes); M2 full-FT checkpoints as reference | α chosen on training-domain validation; the curve over α on the test domain is reported only as analysis |
| Few training images per class | linear probe / LoRA / full FT with k ∈ {1, 4, 16, 64} images per class per training domain | Milestone 2 notebooks with a subsampled training set | same training-domain validation rule |
| Are the confidences reliable on the unseen domain? | ECE, NLL, mean confidence vs. accuracy, reliability diagrams; optionally temperature scaling fitted on the training-domain validation split | ECE/NLL/confidence already logged for every Milestone 2 run (`RESULT` lines, error-analysis section of `M2_results.ipynb`) | temperature fitted on the training-domain validation split only |

Notes:
- Weight interpolation only makes sense if both endpoints share the same head: in the WiSE-FT paper the fine-tuned model starts from the zero-shot model (head = class text embeddings). The Milestone 2 full-FT runs start from a **randomly initialized** linear head, so for WiSE-FT this milestone needs a full-FT run whose head is initialized from the zero-shot text embeddings (or from the linear-probe head, LP-FT style). The Milestone 2 checkpoints remain useful as the "plain fine-tuning" reference.
- Record every experiment in an experiment log, as in Milestone 2.
