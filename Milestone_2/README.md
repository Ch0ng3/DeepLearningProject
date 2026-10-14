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
| Split | `--holdout_fraction 0.2`, `--trial_seed 0` / `--seed 0`: the same split for every method and the same as Milestone 1 |
| Batch, augmentation | DomainBed defaults: 32 images per training domain, standard DomainBed augmentation |
| Model selection | training-domain validation set (the test domain is never used for selection or tuning) |
| Reporting | test-domain accuracy of the selected checkpoint for each held-out domain and their average; plus confusion matrix and calibration (ECE, NLL) of the held-out domain for the analysis of the results |

**What changes, one factor at a time:**

| Comparison | Fixed | Varied |
|---|---|---|
| Backbone | adaptation method | ResNet-50 (Milestone 1 baseline) → DINOv2 ViT-B/14 → CLIP ViT-B/16 |
| Adaptation | backbone | linear probe → LoRA → full fine-tuning |
| Feature dimension (optional ablation) | DINOv2, linear probe | 5×768 features → CLS token only (`dinov2_cls`) |

**Method-specific settings and computational budget.** Each adaptation method needs its own learning rate, so these differ by design and must be stated in the report:

| Method | Trainable parameters | LR | Steps | Notes |
|---|---|---|---|---|
| ERM ResNet-50 (baseline = Milestone 1 run) | all | 5e-5 | 5001 | DomainBed defaults, augmix off |
| Linear probe | head only | 5e-5 (default) | 2000 | same settings as the first linear-probe runs (see the log) |
| LoRA (rank 8, attention qkv + proj) | adapters + head (442,368 + head) | 5e-4 | 1500 | identical adapters on DINOv2 and CLIP |
| Full fine-tuning | all | 1e-5, head ×10 | 1500 | |

The computational budget is reported per method from the measurements (trainable parameters, peak VRAM, seconds per step, minutes per run) in the final table of `M2_results.ipynb`. The ViT methods use fewer steps than the ResNet baseline because the pretrained backbones converge much faster; the "best step" column of the table shows whether the selected checkpoint is close to the last one (which would mean more steps are needed).

## How to run

One notebook per method × backbone (4 runs each: one per held-out domain, seed 0), plus one notebook for the final table. The baseline is the Milestone 1 run. The run notebooks are independent, so different team members can run them in parallel. They run on **Google Colab**.

| Notebook | Runs | Est. time (A100) | Who |
|---|---|---|---|
| `M2_run_lp_dinov2.ipynb` | 4 | ~40 min | |
| `M2_run_lp_clip.ipynb` | 4 | ~35 min | |
| `M2_run_lora_dinov2.ipynb` | 4 | ~45 min | |
| `M2_run_lora_clip.ipynb` | 4 | ~40 min | |
| `M2_run_fullft_dinov2.ipynb` | 4 | ~70 min | |
| `M2_run_fullft_clip.ipynb` | 4 | ~55 min | |
| `M2_results.ipynb` | final table, error analysis, CLIP zero-shot | ~10 min | |

Times are estimates (+ ~10 min setup per session). In total about 5 hours of GPU time.

1. Open a run notebook, set `SESSION_BUDGET_MIN` (section 4) to how long you can stay, then *Run all* (section 1 asks for Google Drive access). No run is started that cannot finish within the budget.
2. Next session: *Run all* again: the runs already saved on Drive are skipped.
3. When all notebooks are done, run `M2_results.ipynb` (it reads every run from Drive) and add the results to the experiment log below.

### Saving results and logs

Every finished run is saved immediately to **Google Drive** (`MyDrive/DeepLearningProject/M2_backup/<run>/`): `result.json` (the numbers of the run) and the DomainBed logs (`results.jsonl`, `out.txt`, `err.txt`). Section 1 asks once for Drive access.

- In a later session the runs already on Drive are skipped automatically: just *Run all* again.
- `M2_results.ipynb` reads all runs from Drive (runs saved on the Drive of another team member can be pasted as `RESULT` lines).
- Model weights are not saved (not needed for Milestone 2); set `SAVE_CHECKPOINTS = True` in section 4 to save them (fp16, ~200 MB per run).

## Experiment log

Every experiment of Milestone 2, including the early ones that are no longer part of the final grid. Add one line per notebook when it is finished (average accuracy over the 4 held-out domains, from `M2_results.ipynb`).

| ID | Date | Notebook / setting | Seeds | Avg accuracy (%) | Status / notes |
|---|---|---|---|---|---|
| E0 | Sep 2026 | Milestone 1: ERM ResNet-50, default hparams, 5001 steps | 0 | 84.9 (A 82.5, C 80.9, P 97.8, S 78.3) | done; reference for the 1–3 point reproduction check |
| E1 | Sep 2026 | First linear-probe notebook: DINOv2 (5×768 features), default LR, 2000 steps | 0 | 95.1 | done; E10 repeats it in the final pipeline (same settings) |
| E2 | Sep 2026 | First linear-probe notebook: CLIP ViT-B/16, default LR, 2000 steps | 0 | 96.5 | done; E11 repeats it in the final pipeline (same settings) |
| E3 | | First LoRA/full-FT notebook: LoRA DINOv2, LR 1e-4, 3000 steps | 0 | not recorded (fill in if available) | discarded: in that version LoRA was on attention for DINOv2 but on the MLP for CLIP (not comparable), and the run directories were not persistent |
| E4 | | Same notebook: LoRA CLIP, full FT DINOv2/CLIP | – | – | replaced by the current grid |
| E10 | 14 Oct 2026 | `M2_run_lp_dinov2` | 0 | 94.5 (A 97.8, C 92.4, P 99.9, S 87.7) | done; ECE 2.0%; ~5.5 min/run, 1.4 GB; matches `collect_results`; −0.6 vs E1 (run-to-run variation) |
| E11 | 14 Oct 2026 | `M2_run_lp_clip` | 0 | 96.7 (A 96.8, C 98.5, P 99.4, S 92.1) | done; ECE 21.0% (under-confident); ~5 min/run, 1.3 GB; matches `collect_results`; +0.2 vs E2 |
| E12 | 14 Oct 2026 | `M2_run_lora_dinov2` | 0 | 94.5 (A 96.2, C 95.6, P 100.0, S 86.4) | done; same average as the linear probe (C +3.2, A −1.6, S −1.3); ECE 3.4% but over-confident on sketch (ECE 9.0%); ~7.8 min/run, 10.6 GB; best step 300–1200 |
| E13 | | `M2_run_lora_clip` | 0 | | to run |
| E14 | | `M2_run_fullft_dinov2` | 0 | | to run |
| E15 | | `M2_run_fullft_clip` | 0 | | to run |
| E16 | | CLIP zero-shot (`M2_results`, no training) | – | | to run |
| E17 | | optional: `dinov2_cls` ablation (in `M2_run_lp_dinov2`) | 0 | | optional |

## Results

To be filled from `M2_results.ipynb` (final table, error analysis and calibration, CLIP zero-shot).
