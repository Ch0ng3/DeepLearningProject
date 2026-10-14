# DeepLearningProject - Project 5

## Team Members

| Name | Neptun Code |
|---|---|
| Márton Tamás Magyar | `TG2UTM` |
| Daniel Brody | `IASULA` |
| Marco Buonfantino | `FMDKOH` |

## Project Level and Status

**Target level: Option B – Advanced Project** (Milestones 1, 2 and 3) – *to be confirmed by the team.*

| Part | Folder | Status |
|---|---|---|
| Milestone 1 – Reproduce | `Milestone_1/` | done: 84.9 vs. 85.5 in the paper (seed 0) |
| Milestone 2 – Improve | `Milestone_2/` | notebooks ready, runs to do; protocol and experiment log in its README |
| Milestone 3 – Research Contribution | `Milestone_3/` | after Milestone 2 |
| Final report (IEEE 2-column, max 6 pages) | `Report/` | skeleton ready |

How Milestone 2 follows the general guidelines below: one factor changed at a time (backbone or adaptation method), the same data split as Milestone 1 for every run, the computational budget of every method measured and reported, every experiment recorded in the experiment log (`Milestone_2/README.md`); results, logs and fine-tuned checkpoints saved after every run; Colab's pre-installed PyTorch with only the missing packages installed; Kaggle supported as the alternative platform. Model selection always uses the training-domain validation set.

# Specifications
A classifier trained on photos should also recognise the same objects in paintings, cartoons and sketches, but most models cannot. You will compare backbones pretrained in different ways (supervised, self-supervised DINOv2, and language-supervised CLIP) using DomainBed. The dataset is PACS: 7 object classes in 4 styles, or "domains".

Milestone 1 – Reproduce:

    Clone the DomainBed repository and read the README.
    Do not install the requirements file. Install only the wilds and gdown packages.
    Download only the PACS dataset. The download script has a separate function for each dataset, and downloading all of them is very large. If the Google Drive download fails, use the copy on Hugging Face (flwrlabs/pacs).
    Train the ERM algorithm on PACS four times. Each time, hold out a different domain as the test domain (art, cartoon, photo, sketch).
    For each run, report the accuracy on the held-out domain, using the model chosen by validation on the three training domains. DomainBed's results-collection script does this for you.

Your target: Art 84.7, Cartoon 80.8, Photo 97.2, Sketch 79.3, average 85.5. The paper used a hyperparameter search, so a single run may differ by 1–2 points.

Hint: The current code uses a different backbone by default than the one in the paper. Look for the resnet50_augmix setting and switch it off.

Milestone 2 – Improve:

    Replace the backbone with DINOv2 and CLIP models.
    Compare three ways of adapting them: a linear classifier on frozen features, LoRA, and full fine-tuning.

Milestone 3 – Contribute:

    Does fine-tuning hurt performance on the unseen domain, and can weight averaging (WiSE-FT) fix it?
    What happens with only a few training images per class?
    Are the model's confidence scores reliable on the unseen domain?

Watch out: DomainBed's "oracle" selection method uses test-domain data, so it is not a fair result.

Related materials:

    Code: https://github.com/facebookresearch/DomainBed
    Code: https://github.com/facebookresearch/dinov2 and https://github.com/mlfoundations/open_clip
    Data – PACS (fallback copy): https://huggingface.co/datasets/flwrlabs/pacs
    Paper – DomainBed: https://arxiv.org/abs/2007.01434
    Paper – PACS: https://arxiv.org/abs/1710.03077
    Paper – DINOv2: https://arxiv.org/abs/2304.07193
    Paper – CLIP: https://arxiv.org/abs/2103.00020
    Paper – WiSE-FT: https://arxiv.org/abs/2109.01903

# General Project Guidelines

## Computing Resources

All projects are designed to run using free computing resources, mainly Google Colab and Kaggle.

Google Colab provides free GPU access, although GPU availability and GPU type may vary. If a GPU is not available in Colab, you can use Kaggle Notebooks as an alternative.

Kaggle normally provides a weekly GPU quota, with individual GPU sessions of up to approximately 12 hours.

**Important:** Cloud sessions can disconnect or reset. Therefore, always save your model checkpoints, results, logs, and important files regularly. If you use Google Colab, saving them to Google Drive is strongly recommended.

## Environment and Compatibility

Research repositories are sometimes several years old and may depend on older versions of Python, PyTorch, CUDA, or other libraries.

Colab updates its Python and pre-installed libraries regularly. Therefore, an older repository may not run immediately with its original `requirements.txt`.

If this happens:

- First try using Colab's pre-installed PyTorch.
- Install only the additional packages required by the repository.
- Avoid replacing the entire Colab environment unless necessary.
- Check the instructions provided for your selected project, as known compatibility issues will be mentioned there.

Troubleshooting and adapting an existing research repository is also part of the learning process.

## Milestone 1 – Reproduce

Run the published implementation and try to reproduce the main results reported in the paper or project description.

A small difference due to random initialization, hardware, library versions, or training conditions is normal. A difference of approximately 1–3 percentage points may be considered normal for many experiments.

If your result is significantly different, investigate the reason and explain it clearly in your report.

The objective is not only to run the code, but also to understand how the system works and how the reported results were obtained.

## Milestone 2 – Improve

After reproducing the baseline, make controlled improvements to the system.

Examples may include:

- modifying the model architecture,
- changing features or preprocessing,
- improving training strategies,
- testing different loss functions,
- adding augmentation,
- or investigating another clearly motivated improvement.

Make one major change at a time whenever possible so that you can understand its effect.

Keep the same dataset split and use a comparable computational budget.

Record every experiment and its results.

## Milestone 3 – Research Contribution

For the advanced version of the project, you should go beyond reproducing and improving the baseline and investigate a small research question.

You should:

- Avoid using the test set for model selection or hyperparameter tuning.
- Use the test set only for the final evaluation.
- Run the main experiment with at least 3 random seeds.
- Report the main results as mean ± standard deviation.
- Analyse the model's errors and limitations.
- Investigate and answer at least one clear research question.

## Project Levels

**Option A – Basic Project**

Teams completing Option A are expected to:

- successfully complete Milestone 1 – Reproduce,
- complete a meaningful Milestone 2 – Improve,
- analyse and discuss their results,
- and submit the final project report.

**Option B – Advanced Project**

Teams completing Option B are expected to complete:

- Milestone 1,
- Milestone 2,
- and Milestone 3 – Research Contribution.

## Final Report

- It must be written using the IEEE 2-column format and not exceed 6 pages.
- Outstanding projects with strong and well-supported results may be further developed into a workshop or conference submission.

