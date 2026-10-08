Milestone 1 – Reproduce:

    Clone the DomainBed repository and read the README.
    Do not install the requirements file. Install only the wilds and gdown packages.
    Download only the PACS dataset. The download script has a separate function for each dataset, and downloading all of them is very large. If the Google Drive download fails, use the copy on Hugging Face (flwrlabs/pacs).
    Train the ERM algorithm on PACS four times. Each time, hold out a different domain as the test domain (art, cartoon, photo, sketch).
    For each run, report the accuracy on the held-out domain, using the model chosen by validation on the three training domains. DomainBed's results-collection script does this for you.

Your target: Art 84.7, Cartoon 80.8, Photo 97.2, Sketch 79.3, average 85.5. The paper used a hyperparameter search, so a single run may differ by 1–2 points.

Hint: The current code uses a different backbone by default than the one in the paper. Look for the resnet50_augmix setting and switch it off.
