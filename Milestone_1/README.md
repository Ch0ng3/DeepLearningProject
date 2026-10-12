# Milestone 1 – Reproduce:

    Clone the DomainBed repository and read the README.
    Do not install the requirements file. Install only the wilds and gdown packages.
    Download only the PACS dataset. The download script has a separate function for each dataset, and downloading all of them is very large. If the Google Drive download fails, use the copy on Hugging Face (flwrlabs/pacs).
    Train the ERM algorithm on PACS four times. Each time, hold out a different domain as the test domain (art, cartoon, photo, sketch).
    For each run, report the accuracy on the held-out domain, using the model chosen by validation on the three training domains. DomainBed's results-collection script does this for you.

Your target: Art 84.7, Cartoon 80.8, Photo 97.2, Sketch 79.3, average 85.5. The paper used a hyperparameter search, so a single run may differ by 1–2 points.

Hint: The current code uses a different backbone by default than the one in the paper. Look for the resnet50_augmix setting and switch it off.

## Results:

ERM on PACS, held-out-domain accuracy with model selection by **training-domain validation** (the model is chosen by validation on the three training domains, as required by the assignment).

| | Art | Cartoon | Photo | Sketch | Average |
|---|---|---|---|---|---|
| **Ours results** | 82.5 | 80.9 | 97.8 | 78.3 | **84.9** |
| Target (paper) | 84.7 | 80.8 | 97.2 | 79.3 | 85.5 |

Each column is the accuracy on that domain when it is held out as the test domain. Our average of **84.9** is within ~0.6 points of the paper's 85.5.



