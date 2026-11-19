# Privacy Leakage in Machine Learning: Membership Inference Attacks, Defenses, and Disparate Vulnerability

Course project: **Trustworthy Artificial Intelligence** | Trust pillar: **Privacy** | PyTorch + Opacus (differential privacy)

**Everything is in one notebook:** [`membership_inference_project_final.ipynb`](membership_inference_project_final.ipynb).
It is already executed, so all outputs, tables and figures can be read directly on GitHub without running anything.

## What the project does

A model behaves slightly differently on data it was trained on. A *membership inference attack* exploits this to tell whether a specific record was in the training set. On CIFAR-10 the notebook:

| RQ | Question | Section |
|---|---|---|
| RQ1 | How well can a black-box attacker separate members from non-members? | 5, 6 |
| RQ2 | Is overfitting the cause of the leakage? | 7 |
| RQ3 | Which defenses give the best privacy-utility trade-off (incl. DP-SGD)? | 8 |
| RQ4 (novel) | Is privacy risk unequally distributed across classes and hard samples? | 9 |

Headline numbers (undefended model): loss-threshold attack AUC about 0.83; shadow-model attack AUC about 0.86 with roughly 3x the true-positive rate at 1% false-positive rate. DP-SGD (epsilon = 8) brings the attack to about chance (AUC about 0.52). Full results: Section 10 of the notebook.

## Easiest way to run it (Google Colab, free GPU)

1. Click: **[Open in Colab](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO/blob/main/membership_inference_project_final.ipynb)**
2. `Runtime` > `Change runtime type` > **T4 GPU**
3. `Runtime` > **Run all**

(The notebook's first code cell installs Opacus; PyTorch and the other libraries are already present in Colab.)

## Run locally

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate

# 1) PyTorch first
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu126   # NVIDIA GPU
# pip install torch torchvision                                                     # CPU only

# 2) everything else
pip install -r requirements.txt

jupyter lab membership_inference_project_final.ipynb
```

Then choose **Run All**.

## Things to know before running

- **Runtime.** The first run trains many models. Expect tens of minutes on a GPU (author used an RTX 4070). A CPU works but is much slower.
- **Caching.** Trained models and results are saved to `artifacts/` and reloaded on later runs, so re-running takes only a few minutes. Changing `CONFIG` (dataset size, epochs, seed) changes the cache names automatically, so stale models are never reused.
- **Data.** CIFAR-10 (~170 MB) is downloaded automatically into `data/` on the first run.
- **Reproducibility.** Seeds are fixed, but GPU arithmetic is not perfectly deterministic: numbers may differ in the third decimal place; the conclusions do not change.
- **Optional cells** (Section 8.6) are off by default; set `RUN_OPTIONAL = True` in the config cell to enable them.

## Repository contents

```
.
├── membership_inference_project_final.ipynb   # the full project (executed, with outputs)
├── requirements.txt
├── README.md
└── .gitignore
```