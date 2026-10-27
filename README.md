# Breaking Camouflage: classifying camouflaged animals in COD10K

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Danish12196/breaking-camouflage-cod10k/blob/main/Week6_AppliedViT_5584978.ipynb)

Danish Ali (5584978) · MAAI901 Deep Learning for Visual Data · University of Wollongong in Dubai · Autumn 2026
Assessment 2: Applied Vision Transformer Report

This repository holds the code and results for the report. It compares four pre-trained model families on 52 camouflaged-animal sub-classes from COD10K-v3 (Fan et al., 2020):

| Family | Pre-training |
|---|---|
| ViT | supervised, ImageNet labels |
| ConvNeXt | supervised CNN, comparable ImageNet data |
| CLIP | 400M image-caption pairs |
| DINOv2 | self-supervised, no labels |

Each family was fine-tuned at Base and Large sizes and at 224 and 448 px, with three seeds per configuration and learning rates chosen on a validation split. The dataset's masks were used to test whether the models rely on the animal or on its background.

## Results

Top-1 accuracy on the 1,949-image test set, mean ± std over three seeds. Random guessing gives 1.9%.

| Model | Base, 224 px | Base, 448 px | Large, 224 px |
|---|---|---|---|
| DINOv2 | 78.1 ± 0.1 | 83.6 ± 0.7 | **84.3 ± 1.2** |
| ConvNeXt | 77.0 ± 0.6 | 82.9 ± 0.8 | 79.5 ± 1.0 |
| ViT | 70.8 ± 0.6 | 78.6 ± 1.0 | 75.1 ± 0.5 |
| CLIP | 70.0 ± 0.6 | 72.3 ± 2.2 | 76.0 ± 1.4 |

Main findings:
- **Pre-training matters more than architecture.** DINOv2 and ConvNeXt are statistically tied at Base 224 px, and both beat the supervised ViT by 6 to 7 points.
- **Resolution helps small animals most.** Training at 448 px adds 5 to 8 points for three of the four families. For animals covering under 2% of the image, the gain is 10.8 to 12.3 points.
- **The models lean on the habitat.** With the animal greyed out, they still score 38 to 59%.

## Running the notebook

1. Open `Week6_AppliedViT_5584978.ipynb` in Colab (use the button above).
2. Runtime → Change runtime type → GPU. An A100 is fastest; a T4 also works.
3. Runtime → Run all.

No setup is needed. The notebook installs its packages and downloads COD10K-v3 automatically from a public Kaggle copy of the official release (about 2.5 GB). To use your own copy instead, drop `COD10K-v3.zip` into Colab's Files panel before running, or set `DATA_SOURCE = "upload"` in the config cell.

Two run modes, set with `RUN_MODE` in the config cell:
- `"full"` reproduces every number in the report. It takes about 3 GPU-hours on an A100.
- `"quick"` runs the whole pipeline with 1 seed and 3 epochs, in roughly an hour on a T4. Its numbers are lower than the report's; use it to check that everything runs.

The outputs saved in the notebook are from the full run on an A100.

## Repository contents

```
Week6_AppliedViT_5584978.ipynb   all experiments, with saved outputs
results/
  ALL_RESULTS.xlsx               every result, one sheet per analysis
  all_runs.csv                   one row per run (zero-shot, probes, sweep, main runs)
  best_lr.json                   backbone learning rate chosen for each family
  dataset_info.json              the 52 classes kept, classes dropped, split sizes
  figures/                       the charts used in the report
  runs/                          per-run metrics (.json) and test-set predictions (.npz)
```

Each `.npz` in `runs/` holds the test logits and labels for one run, in the order of the sorted test-image file names. They are enough to recompute the accuracy, bootstrap and per-class results without retraining.

## Data and references

COD10K-v3 is not included here. It is released by the Media Computing Lab, Nankai University, for non-commercial research use.

- Fan, D.-P., Ji, G.-P., Sun, G., Cheng, M.-M., Shen, J. and Shao, L. (2020) 'Camouflaged object detection', *CVPR*.
- Pre-trained weights come from [timm](https://github.com/huggingface/pytorch-image-models) and [OpenCLIP](https://github.com/mlfoundations/open_clip).# Breaking Camouflage: classifying camouflaged animals in COD10K

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Danish12196/breaking-camouflage-cod10k/blob/main/Week6_AppliedViT_5584978.ipynb)

Danish Ali (5584978) · MAAI901 Deep Learning for Visual Data · University of Wollongong in Dubai · Autumn 2026
Assessment 2: Applied Vision Transformer Report

This repository holds the code and results for the report. It compares four pre-trained model families on 52 camouflaged-animal sub-classes from COD10K-v3 (Fan et al., 2020):

| Family | Pre-training |
|---|---|
| ViT | supervised, ImageNet labels |
| ConvNeXt | supervised CNN, comparable ImageNet data |
| CLIP | 400M image-caption pairs |
| DINOv2 | self-supervised, no labels |

Each family was fine-tuned at Base and Large sizes and at 224 and 448 px, with three seeds per configuration and learning rates chosen on a validation split. The dataset's masks were used to test whether the models rely on the animal or on its background.

## Results

Top-1 accuracy on the 1,949-image test set, mean ± std over three seeds. Random guessing gives 1.9%.

| Model | Base, 224 px | Base, 448 px | Large, 224 px |
|---|---|---|---|
| DINOv2 | 78.1 ± 0.1 | 83.6 ± 0.7 | **84.3 ± 1.2** |
| ConvNeXt | 77.0 ± 0.6 | 82.9 ± 0.8 | 79.5 ± 1.0 |
| ViT | 70.8 ± 0.6 | 78.6 ± 1.0 | 75.1 ± 0.5 |
| CLIP | 70.0 ± 0.6 | 72.3 ± 2.2 | 76.0 ± 1.4 |

Main findings:
- **Pre-training matters more than architecture.** DINOv2 and ConvNeXt are statistically tied at Base 224 px, and both beat the supervised ViT by 6 to 7 points.
- **Resolution helps small animals most.** Training at 448 px adds 5 to 8 points for three of the four families. For animals covering under 2% of the image, the gain is 10.8 to 12.3 points.
- **The models lean on the habitat.** With the animal greyed out, they still score 38 to 59%.

## Running the notebook

1. Open `Week6_AppliedViT_5584978.ipynb` in Colab (use the button above).
2. Runtime → Change runtime type → GPU. An A100 is fastest; a T4 also works.
3. Runtime → Run all.

No setup is needed. The notebook installs its packages and downloads COD10K-v3 automatically from a public Kaggle copy of the official release (about 2.5 GB). To use your own copy instead, drop `COD10K-v3.zip` into Colab's Files panel before running, or set `DATA_SOURCE = "upload"` in the config cell.

Two run modes, set with `RUN_MODE` in the config cell:
- `"full"` reproduces every number in the report. It takes about 3 GPU-hours on an A100.
- `"quick"` runs the whole pipeline with 1 seed and 3 epochs, in roughly an hour on a T4. Its numbers are lower than the report's; use it to check that everything runs.

The outputs saved in the notebook are from the full run on an A100.

## Repository contents

```
Week6_AppliedViT_5584978.ipynb   all experiments, with saved outputs
results/
  ALL_RESULTS.xlsx               every result, one sheet per analysis
  all_runs.csv                   one row per run (zero-shot, probes, sweep, main runs)
  best_lr.json                   backbone learning rate chosen for each family
  dataset_info.json              the 52 classes kept, classes dropped, split sizes
  figures/                       the charts used in the report
  runs/                          per-run metrics (.json) and test-set predictions (.npz)
```

Each `.npz` in `runs/` holds the test logits and labels for one run, in the order of the sorted test-image file names. They are enough to recompute the accuracy, bootstrap and per-class results without retraining.

## Data and references

COD10K-v3 is not included here. It is released by the Media Computing Lab, Nankai University, for non-commercial research use.

- Fan, D.-P., Ji, G.-P., Sun, G., Cheng, M.-M., Shen, J. and Shao, L. (2020) 'Camouflaged object detection', *CVPR*.
- Pre-trained weights come from [timm](https://github.com/huggingface/pytorch-image-models) and [OpenCLIP](https://github.com/mlfoundations/open_clip).
