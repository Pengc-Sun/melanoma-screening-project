# Experiment 2 — Multi-Modal Fusion Model

**Date:** 2026.6.15–2026.6.21

This directory contains the formal validation evidence for the multi-modal model combining ResNet50 image features with age, sex and anatomical-site metadata.

## Included Evidence

- `metrics_summary.csv`: formal validation metrics
- `history.csv`: epoch-level training history
- `predictions.csv`: validation predictions with local directory paths removed
- `report_figures/`: training curves, confusion matrix, ROC curve, PR curve, probability distribution and threshold analysis

The trained `model.pth` checkpoint and source dataset are intentionally excluded.
