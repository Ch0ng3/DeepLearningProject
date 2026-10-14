Milestone 2 – Improve:

    Replace the backbone with DINOv2 and CLIP models.
    Compare three ways of adapting them: a linear classifier on frozen features, LoRA, and full fine-tuning.

## Experimental protocol

The general guidelines ask for controlled improvements: one major change at a time, the same dataset split, a comparable computational budget, and a record of every experiment. This is how Milestone 2 follows them.

**What is shared by every run** (only the backbone and/or the adaptation method change):

| Setting | Value |
|---|---|
| Codebase | DomainBed (pristine clone + the patches in section 2 of each notebook), algorithm ERM |
| Data | PACS from Hugging Face (`flwrlabs/pacs`), leave-one-domain-out, 4 test domains |
| Split | `--holdout_fraction 0.2`, `--trial_seed` 0, 1, 2 (the same 3 splits for every method; seed 0 = the Milestone 1 split) |
| Batch, augmentation | DomainBed defaults: 32 images per training domain, standard DomainBed augmentation |
| Model selection | training-domain validation set (the test domain is never used for selection or tuning) |
| Reporting | test-domain accuracy of the selected checkpoint, mean ± std over the 3 seeds; plus confusion matrix and calibration (ECE, NLL) of the held-out domain for error analysis |

**What changes, one factor at a time:**

| Comparison | Fixed | Varied |
|---|---|---|
| Backbone | adaptation method | ResNet-50 (baseline) → DINOv2 ViT-B/14 → CLIP ViT-B/16 |
| Adaptation | backbone | linear probe → LoRA → full fine-tuning |
| Feature dimension (optional ablation) | DINOv2, linear probe | 5×768 features → CLS token only (`dinov2_cls`) |

**Method-specific settings and computational budget.** Each adaptation method needs its own learning rate, so these differ by design and must be stated in the report:

| Method | Trainable parameters | LR | Steps | Notes |
|---|---|---|---|---|
| ERM ResNet-50 (baseline) | all | 5e-5 | 5001 | Milestone 1 setting (DomainBed defaults, augmix off) |
| Linear probe | head only | 5e-5 (default) | 2000 | same settings as the first linear-probe runs (see the log) |
| LoRA (rank 8, attention qkv + proj) | adapters + head (442,368 + head) | 5e-4 | 1500 | identical adapters on DINOv2 and CLIP |
| Full fine-tuning | all | 1e-5, head ×10 | 1500 | the only method that saves checkpoints (needed for WiSE-FT in Milestone 3) |

The computational budget is reported per method from the measurements (trainable parameters, peak VRAM, seconds per step, minutes per run) in the final table of `M2_results.ipynb`. The ViT methods use fewer steps than the ResNet baseline because the pretrained backbones converge much faster; the "best step" column of the table shows whether the selected checkpoint is close to the last one (which would mean more steps are needed).

## How to run

One notebook for the ResNet-50 baseline and one per method × backbone (12 runs each: 4 held-out domains × 3 seeds), plus one notebook for the final table. The run notebooks are independent, so different team members can run them in parallel. They run on **Colab** or **Kaggle** (detected automatically; on Kaggle *Save & Run All* runs in the background for up to ~12 h).

| Notebook | Runs | Est. time (A100) | Who |
|---|---|---|---|
| `M2_run_erm_resnet50.ipynb` (baseline, 3 seeds) | 12 | ~3.0 h | |
| `M2_run_lp_dinov2.ipynb` | 12 | ~2.0 h | |
| `M2_run_lp_clip.ipynb` | 12 | ~1.8 h | |
| `M2_run_lora_dinov2.ipynb` | 12 | ~2.2 h | |
| `M2_run_lora_clip.ipynb` | 12 | ~2.0 h | |
| `M2_run_fullft_dinov2.ipynb` | 12 | ~3.4 h | |
| `M2_run_fullft_clip.ipynb` | 12 | ~2.8 h | |
| `M2_results.ipynb` | final table, error analysis, CLIP zero-shot | ~10 min | |

Times are estimates (+ ~10 min setup per session; roughly double on Kaggle's P100/T4).

1. Open a run notebook, choose `BACKUP` (section 1) and set `SESSION_BUDGET_MIN` (section 4) to how long you can stay, then *Run all*. No run is started that cannot finish within the budget.
2. Next session: paste the `RESULT` lines printed so far into `PREVIOUS_RESULTS` (section 4) and *Run all* again: finished runs are skipped.
3. When a notebook has all 12 runs, paste the output of its section 7 into `ALL_RESULTS` in `M2_results.ipynb`, and add a line to the experiment log below.

### Saving results, logs and checkpoints

Cloud sessions disconnect, so results are saved in three ways:

- **Results**: every finished run prints a `RESULT {...}` line in the notebook output (accuracy, selected step, confusion matrix, ECE, NLL, VRAM, time). Keep the notebooks saved with their outputs.
- **Logs**: the DomainBed logs of every run (`results.jsonl`, `out.txt`, `err.txt`) are backed up according to `BACKUP`:
  - `'download'` (default): on Colab, a zip with all logs is downloaded at the end of the session; on Kaggle, they are kept in `/kaggle/working`;
  - `'drive'`: copied to `MyDrive/DeepLearningProject/M2_backup` (the course's recommendation for Colab);
  - `'none'`: no backup.
- **Checkpoints**: full fine-tuning only. They are saved as fp16 (~200 MB per run) and downloaded or copied with the logs, because Milestone 3 (WiSE-FT) needs the fine-tuned weights.

Put the downloaded zips and checkpoints in a shared folder of the team, not in git (they are too large).

## Experiment log

Every experiment of Milestone 2, including the early ones that are no longer part of the final grid. Add one line per notebook when it is finished (results as mean ± std over the 3 seeds, from `M2_results.ipynb`).

| ID | Date | Notebook / setting | Seeds | Avg accuracy (%) | Status / notes |
|---|---|---|---|---|---|
| E0 | Sep 2026 | Milestone 1: ERM ResNet-50, default hparams, 5001 steps | 0 | 84.9 (A 82.5, C 80.9, P 97.8, S 78.3) | done; reference for the 1–3 point reproduction check |
| E1 | Sep 2026 | First linear-probe notebook: DINOv2 (5×768 features), default LR, 2000 steps | 0 | 95.1 | done; superseded by E10 (same settings, 3 seeds) |
| E2 | Sep 2026 | First linear-probe notebook: CLIP ViT-B/16, default LR, 2000 steps | 0 | 96.5 | done; superseded by E11 |
| E3 | | First LoRA/full-FT notebook: LoRA DINOv2, LR 1e-4, 3000 steps | 0 | not recorded (fill in if available) | discarded: in that version LoRA was on attention for DINOv2 but on the MLP for CLIP (not comparable), and the run directories were not persistent |
| E4 | | Same notebook: LoRA CLIP, full FT DINOv2/CLIP | – | – | replaced by the current grid |
| E5 | | `M2_run_erm_resnet50` | 0, 1, 2 | | to run |
| E10 | | `M2_run_lp_dinov2` | 0, 1, 2 | | to run |
| E11 | | `M2_run_lp_clip` | 0, 1, 2 | | to run |
| E12 | | `M2_run_lora_dinov2` | 0, 1, 2 | | to run |
| E13 | | `M2_run_lora_clip` | 0, 1, 2 | | to run |
| E14 | | `M2_run_fullft_dinov2` | 0, 1, 2 | | to run |
| E15 | | `M2_run_fullft_clip` | 0, 1, 2 | | to run |
| E16 | | CLIP zero-shot (`M2_results`, no training) | – | | to run |
| E17 | | optional: `dinov2_cls` ablation (in `M2_run_lp_dinov2`) | 0, 1, 2 | | optional |

## Results

To be filled from `M2_results.ipynb` (final table with mean ± std, error analysis and calibration, CLIP zero-shot).
