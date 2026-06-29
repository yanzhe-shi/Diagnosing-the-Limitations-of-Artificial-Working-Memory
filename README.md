# Diagnosing the Limitations of Artificial Working Memory

Code for the URECA research paper **"Developing Efficient Human-Like Memory
Systems: Diagnosing the Limitations of Artificial Working Memory under Visual
Interference and Out-of-Distribution Inputs."**

This repository contains a single Jupyter/Colab notebook that stress-tests a
pretrained recurrent working-memory model (**LSTM-96**) from the **WorM
benchmark** on the visual **Change Detection (CD)** task. The model is evaluated
**zero-shot** (frozen weights, no retraining) under three families of controlled,
label-preserving manipulations.

---

## Repository contents

| File | Description |
|------|-------------|
| `URECA_Diagnosing-the-Limitations-of-Artificial-Working-Memory .ipynb` | Full evaluation notebook (Stages 0–3): data/model loading, perturbations, evaluation, figures, and result CSVs. |

> Note: the trained model, the WorM dataset, and the generated `paper_outputs/`
> are **not** included (see *Data & model* below). Running the notebook
> regenerates all results, figures, and CSVs.

---

## Experiments

The notebook is organised into stages, each in its own labelled section:

- **Stage 0–1 — Retention-interval interference.** The retention (delay) frames
  are replaced with random static, a checkerboard, or horizontal/vertical
  gratings, against a clean-grey baseline. Tests whether irrelevant visual input
  during the delay disrupts maintenance.
- **Stage 2 — Memory/Probe perturbations (encoding robustness).** With the delay
  kept clean, the Memory and Probe arrays are perturbed by (a) an object hue
  rotation (+60°, red/green → yellow/cyan) or (b) a background recolour
  (grey → steel-blue). Both are deterministic and applied identically to both
  arrays, so the Same/Changed label is preserved by construction.
- **Stage 3 — Contrast sweep (control).** The retention distractor's contrast is
  swept from 0 (clean) to 1 (full static) to test whether the Stage 1 failure is
  driven by distractor saliency.

---

## Key findings (zero-shot, delayed trials, RI > 0)

| Condition | Accuracy | Predicted "Changed" rate |
|-----------|:--------:|:------------------------:|
| Clean baseline | 1.000 | 0.51 |
| Retention interference (static/checkerboard/gratings) | 0.50–0.53 | 0.93–0.97 |
| Stage 2 — object recolour | 0.936 | 0.44 |
| Stage 2 — background recolour | 0.503 | ~1.00 |
| Stage 3 — full contrast (α = 1.0) | 0.503 | 0.97 |

The model is near-perfect on clean trials, collapses under delay interference,
**tolerates novel object colours but fails when only the background changes**,
and shows a graded, contrast-dependent collapse.

---

## How to run

The notebook is written for **Google Colab**.

1. Obtain the **WorM benchmark** code, the pretrained **LSTM_96** checkpoint
   (`model.pt`), and the `wm_bench_data` directory from the WorM authors
   (see *Data & model*). Place them so the notebook finds:
   - `WorM-main/Trained_Models/LSTM/LSTM_96/model.pt`
   - `WorM-main/wm_bench_data/`
2. Open `NEW_URECA_GIT_REPO.ipynb` in Colab and adjust the Google Drive path in
   the first setup cell to point to your `WorM-main` folder.
3. Run the **setup / model-loading cell first**, then run the Stage 0–1, Stage 2,
   and Stage 3 sections in order.
4. Results are written to `paper_outputs/` (CSVs, sample images, plots) and
   zipped for download.

**Dependencies:** Python 3, PyTorch, NumPy, pandas, Matplotlib, tqdm (all
preinstalled in Colab), plus the WorM `src` package. A GPU runtime is
recommended but not required.

---

## Data & model

The model and stimuli come from the **WorM benchmark**:

> A. Sikarwar and M. Zhang, "Decoding the Enigma: Benchmarking Humans and AIs on
> the Many Facets of Working Memory," *NeurIPS 2023, Datasets and Benchmarks
> Track.* arXiv:2307.10768.

These assets are governed by the WorM project's own licence and are not
redistributed here.

---

## Citation

If you use this code, please cite the research paper:

> Shi YanZhe, "Developing Efficient Human-Like Memory Systems: Diagnosing the
> Limitations of Artificial Working Memory under Visual Interference and
> Out-of-Distribution Inputs," NTU URECA Undergraduate Research Programme, 2026.

---

## Acknowledgement

This research was supported by Nanyang Technological University under the URECA
Undergraduate Research Programme.

**Author:** Shi YanZhe · **Supervisor:** Dr. Zhang Mengmi ·
College of Computing and Data Science, Nanyang Technological University
