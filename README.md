# MNIST Digit Classification: Error Analysis & Data Augmentation

## Overview

A CNN model trained on MNIST, reaching 97.8% test accuracy. Beyond its baseline accuracy, this project also investigates which digits the model confuses and tests whether data augmentation can reduce them.

## Baseline Model & Accuracy

- Model is a Keras Sequential CNN with 3 Conv2D layers (32, 64, 64 filters), MaxPooling2D, Flatten, and Dense layers.
- Trained on 60,000 MNIST handwritten digit images and tested against 10,000.
- Baseline test accuracy: 97.8%.

## Error Analysis

1. The model misclassified an average of 98 out of 10,000 test images across five baseline runs.
2. The most common confusions were 5s misread as 8s, 5s misread as 9s, and 4s misread as 6s.
3. This likely happens because the model learns one narrow, fixed representation of each digit rather than the range of ways it can be written by hand — so digits that share similar strokes or curves (like 5/8 or 4/6) get mixed up when handwriting varies from that fixed pattern.

## Fix: Data Augmentation

To address these confusions, two augmentation layers were added directly into the model, applied only during training: `RandomRotation(0.03)` (~10° of rotation) and `RandomTranslation(0.07, 0.07)` (~2px of shift in each direction). Rather than training on the same 60,000 images every epoch, the model now sees a slightly different rotation and position of each digit each time. The goal was not to make individual images clearer, but to prevent the model from relying on one fixed appearance for each digit — closer to how real handwriting naturally varies. The test set was left untouched throughout, so evaluation remained a fair, unmodified comparison.

## Results & Verdict

Five baseline (non-augmented) runs and five augmented runs were each trained and evaluated independently, to account for the normal run-to-run variation in retraining a neural network from scratch.

- **Baseline misclassified counts:** 83, 105, 94, 107, 103 (average ≈ 98)
- **Augmented misclassified counts:** 91, 82, 97, 74, 88 (average ≈ 86)

The augmented runs averaged fewer errors, and one run (74) fell below the entire baseline range. However, the two ranges still overlap substantially (83–107 vs. 74–97), so with only five runs per condition, this isn't yet strong enough evidence to claim a definitive improvement.

**Verdict:** Data augmentation shows a promising trend toward reducing misclassifications, but the result is not conclusive at this sample size. A natural next step would be more trials to tighten the comparison.
