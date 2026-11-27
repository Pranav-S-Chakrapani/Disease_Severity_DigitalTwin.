# Disease Severity Digital Twin

An image-based Digital Twin for crop disease detection **and severity estimation** — not just classification. Each new leaf image updates a structured health-state record: disease label, estimated severity (% leaf area affected), and model confidence.

Built on three public datasets: **PlantVillage** (classification baseline), **PlantDoc** (field-condition domain-shift testing), and **PlantSeg** (pixel-level lesion segmentation for severity).

## Status: Days 1–3 complete

| Day | Task | Result |
|---|---|---|
| 1 | Dataset verification, train/val/test splits, EDA | 162,912 PlantVillage images verified (38 classes), 29 PlantDoc field classes verified, PlantSeg (7,711 images) downloaded |
| 2 | Classifier fine-tuning (PlantVillage + PlantDoc) | **95.53%** in-domain accuracy (PlantVillage test set) vs. **21.19%** zero-shot cross-domain accuracy (PlantDoc) — a **74.3 percentage point domain gap**, confirming the lab-to-field generalization problem this project targets |
| 3 | Severity segmentation (SegFormer, fine-tuned on PlantSeg) | **IoU 0.5914**, **Dice 0.7433**, **Pixel Accuracy 0.8953** on held-out lesion segmentation (binarized: lesion vs. background) |

## What's in this repo

- `Disease_Severity_DigitalTwin.ipynb` — full working notebook: dataset download/verification, EDA, classifier fine-tuning, cross-domain evaluation, segmentation fine-tuning, severity computation
- `day2_results.json` / `day3_results.json` — raw metrics
- `class_distribution.png` — PlantVillage class balance (EDA)
- `confusion_matrix_plantvillage.png` — classifier confusion matrix, in-domain test set
- `segmentation_result.png` — ground-truth vs. predicted lesion mask, with computed severity %

## Why the domain-gap result matters

A classifier trained only on lab-background images performs well on more lab images (95.53%) but drops sharply on real field photos (21.19%). This is the core research gap this project addresses: combining classification, lesion segmentation, and a structured, updatable Digital Twin state — evaluated explicitly for robustness across domains, rather than reporting one more in-domain accuracy number.

## Next (Day 4+)

- Digital Twin state schema + update mechanism (structured health record per plant)
- Lightweight demo app (Streamlit/Gradio): image in → disease label + severity % + confidence out
- Ablations: classification-only vs. combined pipeline; single-dataset vs. combined-dataset training
