# LatentForge: Diversity-Aware Image Generation Using a Variational Autoencoder

## Overview

LatentForge is a generative AI project that investigates how KL-divergence weighting affects image reconstruction and latent-space regularization in a Variational Autoencoder (VAE).

The project uses the Fashion-MNIST dataset to compare fixed-beta training, conventional KL warm-up, and a validation-guided adaptive-beta strategy.

## Research Question

How does a validation-guided KL-divergence weighting strategy affect the trade-off between reconstruction quality, generated-image diversity, and latent-space regularity compared with fixed-weight VAE and conventional KL warm-up baselines?

## Objectives

- Implement a convolutional Variational Autoencoder.
- Compare different KL-divergence weighting strategies.
- Evaluate image reconstruction loss and KL divergence.
- Analyze basic latent-space statistics.
- Explore diversity among generated images.
- Investigate whether adaptive beta weighting improves the balance between reconstruction and regularization.

## Dataset

**Fashion-MNIST** is a dataset containing 70,000 grayscale images of clothing and footwear across 10 categories.

| Dataset split | Number of images |
|---|---:|
| Training | 54,000 |
| Validation | 6,000 |
| Test | 10,000 |

The training dataset was divided into training and validation subsets using random seed 42. Images were converted to 28 × 28 grayscale tensors with pixel values between 0 and 1.

## Model Architecture

LatentForge uses a convolutional Variational Autoencoder with a 16-dimensional latent space.

- **Encoder:** Extracts features from input images and estimates the parameters of the latent distribution.
- **Latent representation:** Encodes learned image features in a probabilistic latent space.
- **Decoder:** Reconstructs images from latent representations and generates samples from latent vectors.

The model's training objective combines binary cross-entropy reconstruction loss with KL divergence.

The beta parameter (β) controls the contribution of KL divergence to the training objective.

## Experimental Configurations

Six experimental configurations were compared.

| Experiment | Strategy |
|---|---|
| Baseline VAE | Fixed β = 1.0 |
| Fixed Beta 0.5 | Fixed β = 0.5 |
| Fixed Beta 2.0 | Fixed β = 2.0 |
| Fixed Beta 4.0 | Fixed β = 4.0 |
| KL Warm-up | Gradually increases β during early training |
| Adaptive Beta | Uses warm-up followed by validation-guided beta adjustment |

## Training Configuration

| Parameter | Value |
|---|---|
| Model | Convolutional VAE |
| Latent dimension | 16 |
| Optimizer | Adam |
| Learning rate | 0.001 |
| Batch size | 128 |
| Nominal training duration | 30 epochs |
| Dataset | Fashion-MNIST |
| Data split random seed | 42 |

## Results

The following table summarizes the reconciled validation results from the available checkpoints.

| Experiment | Reconstruction Loss | KL Divergence |
|---|---:|---:|
| Baseline | 221.2267 | 13.3148 |
| Fixed β = 0.5 | **216.9110** | 20.0060 |
| Fixed β = 2.0 | 225.1748 | 9.6284 |
| Fixed β = 4.0 | 231.1169 | **6.6013** |
| KL Warm-up | 217.0877 | 16.2400 |
| Adaptive Beta | 217.2489 | 15.8316 |

### Key Observations

- Fixed β = 0.5 achieved the lowest reconstruction loss among the compared checkpoints.
- Fixed β = 4.0 achieved the lowest KL divergence.
- The results demonstrate a trade-off between reconstruction and latent-space regularization.
- The adaptive-beta model produced competitive reconstruction results but did not demonstrate a clear advantage over the fixed-beta alternatives.
- The recorded adaptive-beta schedule remained at β = 1.0 after warm-up.

**Note:** Total validation losses use different beta weights and should not be treated as a directly comparable standalone ranking across all configurations.

## Generated-Image Diversity

The project explored generated-image variation using mean pairwise pixel distance.

The existing measurements are exploratory. Pixel-space distance is a limited proxy for diversity and does not reliably measure meaningful differences in clothing styles or semantic content.

The current results do not establish that any one beta strategy produces superior semantic diversity.

## Technology Stack

- Python
- PyTorch
- Torchvision
- NumPy
- Pandas
- Matplotlib
- Fashion-MNIST
- Jupyter Notebook
- Google Colab

## Project Structure

The following is a suggested repository structure. Include only files that actually exist in your project.

```text
LatentForge/
├── README.md
├── requirements.txt
├── notebooks/
│   └── LatentForge.ipynb
├── results/
│   ├── final_validation_comparison_reconciled.csv
│   └── final_latent_space_comparison.csv
├── figures/
│   ├── reconstruction_comparison.png
│   ├── generated_samples_comparison.png
│   ├── kl_comparison.png
│   └── diversity_comparison.png
└── report/
    └── LatentForge_Research_Report.pdf
```

## How to Run

The experiments were developed in Google Colab.

1. Open the project notebook in Google Colab.
2. Install the required dependencies.
3. Load the Fashion-MNIST dataset.
4. Run the notebook cells in order to train or evaluate the models.
5. Review the generated figures and saved evaluation results.

The notebook should be tested from a fresh runtime before claiming that the entire workflow is reproducible.

## Limitations

- Comparable test-set evaluations have not been verified for all six experiments.
- The warm-up snapshot's epoch attribution is inferred from matching validation metrics.
- The available diversity measurements are exploratory and were not independently reproduced after the runtime reset.
- The experiments do not include multiple independent training seeds or statistical significance tests.
- The adaptive-beta strategy did not demonstrate a clear improvement over the fixed-beta baselines.
- Basic latent-space statistics do not establish complete prior matching or semantic diversity.

## Future Work

- Evaluate all models using a consistent held-out test protocol.
- Repeat experiments with multiple random seeds.
- Introduce perceptual or feature-space diversity metrics.
- Investigate the adaptive-beta adjustment criteria.
- Record beta changes and validation behavior throughout training.
- Document dependency versions and improve reproducibility.

## Conclusion

LatentForge provides an experimental comparison of different KL-divergence weighting strategies for a convolutional Variational Autoencoder on Fashion-MNIST.

The results show that changing beta affects the balance between reconstruction loss and KL divergence. However, the current experiments do not establish that validation-guided adaptive weighting outperforms fixed-beta alternatives.

The project provides a foundation for further investigation into VAE training strategies, latent-space regularization, and generative image evaluation.

## Project Information

- **Project Name:** LatentForge
- **Category:** Generative AI, Deep Learning, Variational Autoencoders
- **Dataset:** Fashion-MNIST