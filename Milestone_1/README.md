# Milestone 1 – Reproduce:

    Clone the DomainBed repository and read the README.
    Do not install the requirements file. Install only the wilds and gdown packages.
    Download only the PACS dataset. The download script has a separate function for each dataset,
    and downloading all of them is very large. If the Google Drive download fails, 
    use the copy on Hugging Face (flwrlabs/pacs).
    Train the ERM algorithm on PACS four times. 
    Each time, hold out a different domain as the test domain (art, cartoon, photo, sketch).
    For each run, report the accuracy on the held-out domain, 
    using the model chosen by validation on the three training domains. 
    DomainBed's results-collection script does this for you.

Your target: Art 84.7, Cartoon 80.8, Photo 97.2, Sketch 79.3, average 85.5. The paper used a hyperparameter search, so a single run may differ by 1–2 points.

Hint: The current code uses a different backbone by default than the one in the paper. Look for the resnet50_augmix setting and switch it off.

## Results:

ERM on PACS, held-out-domain accuracy with model selection by **training-domain validation** (the model is chosen by validation on the three training domains, as required by the assignment).

| | Art | Cartoon | Photo | Sketch | Average |
|---|---|---|---|---|---|
| **Ours results** | 82.5 | 80.9 | 97.8 | 78.3 | **84.9** |
| Target (paper) | 84.7 | 80.8 | 97.2 | 79.3 | 85.5 |

Each column is the accuracy on that domain when it is held out as the test domain. Our average of **84.9** is within ~0.6 points of the paper's 85.5.

## How the system works

**Task and protocol.** PACS has 7 classes (dog, elephant, giraffe, guitar, horse, house, person) in 4 domains (art painting, cartoon, photo, sketch). Domain generalization is evaluated *leave-one-domain-out*: the model is trained on 3 domains and tested on the 4th, which it never sees during training or model selection. This is repeated with each domain held out, giving the 4 runs (one per column of the table).

**Data splits.** DomainBed splits every domain into an `in` part (80%) and an `out` part (20%, `--holdout_fraction 0.2`), with a fixed seed (`--trial_seed`). The `in` parts of the 3 training domains are used for training, their `out` parts form the validation set, and the accuracy reported for the held-out domain is measured on its `in` part.

**Model and training (ERM).** Empirical Risk Minimization simply pools the 3 training domains and minimizes the average cross-entropy loss, with no domain-specific term. The network is the torchvision ResNet-50 pretrained on ImageNet (`resnet50_augmix` switched off, batch-norm statistics frozen) followed by a linear classifier. Each training step takes a minibatch of 32 images from every training domain (96 in total) with DomainBed's data augmentation (random resized crop, horizontal flip, color jitter, random grayscale), and updates all weights with Adam (learning rate 5e-5, no weight decay) for 5000 steps.

**Model selection.** Every 300 steps DomainBed evaluates the current model on all splits and writes a checkpoint record to `results.jsonl`. `collect_results` then applies the *training-domain validation* rule: among the checkpoints of a run, it selects the one with the highest mean accuracy on the `out` splits of the 3 training domains, and reports the accuracy of that checkpoint on the held-out domain. The held-out domain is never used to choose anything. The *test-domain validation (oracle)* row selects with the held-out domain itself, so it is not a fair result and is only printed for reference.

**How the paper's numbers were obtained.** The DomainBed paper does not use one configuration. For every algorithm and held-out domain it samples 20 random hyperparameter configurations (learning rate, batch size, weight decay, dropout), repeats the whole search 3 times with different data splits (trial seeds), picks the best configuration of each trial with the same training-domain validation rule, and reports the mean over the 3 trials. We ran a single trial with the default hyperparameters, so a difference of 1–2 points per domain is expected; our average differs by 0.6 points.

## Notes

- **Single run, default hyperparameters.** One run per held-out domain (seed 0) with DomainBed's default ERM hyperparameters (lr 5e-5, batch size 32 per domain, weight decay 0, 5000 steps). The paper reports the best of a random hyperparameter search averaged over 3 trials, which explains the per-domain gaps (Art −2.2, Sketch −1.0). The `+/- 0.0` printed by `collect_results` does not mean zero variance: with a single trial the standard deviation cannot be estimated.
- **Backbone.** `resnet50_augmix` is switched off, so the featurizer is torchvision's ImageNet-pretrained ResNet-50, as in the paper.
- **Data.** The Google Drive download in DomainBed's script failed, so PACS was taken from the Hugging Face copy (`flwrlabs/pacs`), as allowed by the assignment. It has the same 9,991 images (art 2,048, cartoon 2,344, photo 1,670, sketch 3,929). Images were re-encoded as RGB JPEG (quality 95) into DomainBed's folder layout. For this reason we installed `wilds` and `datasets` (for the Hugging Face download) instead of `wilds` and `gdown`.
- **Model selection.** All numbers use the training-domain validation set. The test-domain (oracle) selection, which peeks at the held-out domain, gives 85.3 on average and is not reported as a result.
- **Compute.** Google Colab GPU, ~0.5 s per step, i.e. ~42 min per run and ~2.8 h for the four runs (see `Milestone1.ipynb`).



