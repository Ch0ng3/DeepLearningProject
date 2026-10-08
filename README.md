# DeepLearningProject
Project 5
Márton Tamás Magyar    TG2UTM
Daniel Brody           IASULA
Marco Buonfantino      FMDKOH
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

