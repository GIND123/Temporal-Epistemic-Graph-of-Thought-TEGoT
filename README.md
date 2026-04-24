# Temporal-Epistemic Graph of Thought (TEGoT)

Clinical graph reasoning with decomposed uncertainty for ICU mortality prediction.

## Overview
TEGoT combines:
- A heterogeneous temporal patient graph from MIMIC-IV structured ICU events
- Clinical note embeddings from BioMistral-7B (LoRA fine-tuned on MIMIC-IV-Note)
- A variational reasoning graph that models structural uncertainty
- Factored Monte Carlo inference to separate epistemic, structural, and aleatoric uncertainty

The goal is not only strong mortality prediction performance, but also better calibrated probabilities and uncertainty signals that help detect likely model errors.

## Correct Hugging Face Model
Use this BioMistral checkpoint location:
- https://huggingface.co/GOVINDFROM/BioMistral-7B-MIMIC-Notes

## Key Results (from current architecture report)
- TEGoT ensemble AUROC: 0.931 (95% CI: 0.918 to 0.950)
- TEGoT ensemble AUPRC: 0.655 (95% CI: 0.623 to 0.687)
- Brier score: 0.078
- Outperforms strong baselines including Logistic Regression, MLP, and GATv2 deep ensembles
- Structural uncertainty is a strong error signal (error-detection AUROC up to 0.724 after residualization)

## Repository Layout
- `Full_Graph_Implementation.ipynb`: End-to-end graph and model workflow
- `biomistral.ipynb`: BioMistral integration and experimentation notebook
- `Bundle/`: Processed cohort metadata and serialized train/val/test bundles
- `Plots/`: Exported figures for performance and uncertainty analysis

## Data Sources
- MIMIC-IV v3.1 (structured clinical events)
- MIMIC-IV-Note v2.2 (clinical notes)

This project assumes approved MIMIC access and local data preparation consistent with PhysioNet usage terms.

## Method Summary
1. Build a heterogeneous temporal graph per ICU stay (labs, vitals, medications, procedures, I/O, microbiology, diagnoses, patient node).
2. Encode notes with BioMistral and fuse note context with graph features via cross-attention and gating.
3. Run variational reasoning over latent graph structure using stochastic edges.
4. Predict mortality and decompose uncertainty via dropout x graph sampling.
5. Apply post-hoc temperature scaling for calibration.

## Sample Graphs

<img width="3590" height="2773" alt="image" src="https://github.com/user-attachments/assets/9c34730f-8490-494f-9e48-b2bac2b55188" />

<img width="3590" height="2773" alt="image" src="https://github.com/user-attachments/assets/27504442-6ad6-42fd-8cab-08d8a8f812ba" />


## Quick Start
1. Clone this repository.
2. Install Python dependencies used in the notebooks.
3. Open and run `Full_Graph_Implementation.ipynb` for the main pipeline.
4. Use `biomistral.ipynb` for BioMistral-specific runs.

## Reproducibility Notes
- Reported experiments use patient-level split separation.
- Bootstrap confidence intervals are computed with 1,000 resamples.


## Citation
If you use this repository or ideas from TEGoT, please cite the TEGoT project and associated manuscript once finalized.
