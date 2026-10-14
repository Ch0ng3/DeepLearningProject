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

## Notes

- **Single run, default hyperparameters.** One run per held-out domain (seed 0) with DomainBed's default ERM hyperparameters (lr 5e-5, batch size 32 per domain, weight decay 0, 5000 steps). The paper reports the best of a random hyperparameter search averaged over 3 trials, which explains the per-domain gaps (Art −2.2, Sketch −1.0). The `+/- 0.0` printed by `collect_results` does not mean zero variance: with a single trial the standard deviation cannot be estimated.
- **Backbone.** `resnet50_augmix` is switched off, so the featurizer is torchvision's ImageNet-pretrained ResNet-50, as in the paper.
- **Data.** The Google Drive download in DomainBed's script failed, so PACS was taken from the Hugging Face copy (`flwrlabs/pacs`), as allowed by the assignment. It has the same 9,991 images (art 2,048, cartoon 2,344, photo 1,670, sketch 3,929). Images were re-encoded as RGB JPEG (quality 95) into DomainBed's folder layout. For this reason we installed `wilds` and `datasets` (for the Hugging Face download) instead of `wilds` and `gdown`.
- **Model selection.** All numbers use the training-domain validation set. The test-domain (oracle) selection, which peeks at the held-out domain, gives 85.3 on average and is not reported as a result.
- **Compute.** Google Colab GPU, ~0.5 s per step, i.e. ~42 min per run and ~2.8 h for the four runs (see `Milestone1.ipynb`).



