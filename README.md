# Motor Imagery EEG Decoding — A Build-From-Fundamentals Roadmap

Decoding motor imagery from EEG with classical machine learning. A real
brain–computer interface (BCI) project that reads in two layers:

- **As a project** — a rigorous motor-imagery decoder (CSP+LDA, Riemannian
  pipelines) evaluated per-subject with MOABB.
- **As a learning path** — each step is a self-contained notebook, from
  "what is learning?" to a multi-pipeline benchmark, so you can see not just
  the result but how it was built.

![Python](https://img.shields.io/badge/Python-3.13-blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/status-in%20progress-orange)

## Built With

`scikit-learn` · `MNE` · `MOABB` · `pyRiemann` · `braindecode`

Classical ML for neural signal decoding — the standard stack used in BCI
research for motor-imagery classification.

## Roadmap

The repository is built one class at a time. Each class is self-contained and
connects an ML concept to how it is later used on real EEG.

| #  | Topic                                   | Artifact           | Done |
|----|------------------------------------------|--------------------|------|
| 00 | Repo setup                              | structure + README | ✅   |
| 01 | What is learning? train/test            | notebook           | ⬜   |
| 02 | Cross-validation                        | notebook           | ⬜   |
| 03 | Leakage & group splits                  | notebook           | ⬜   |
| 04 | Features, scaling & Pipeline            | notebook           | ⬜   |
| 05 | Classifiers: LDA & SVM                  | notebook           | ⬜   |
| 06 | Metrics beyond accuracy                 | notebook           | ⬜   |
| 07 | Overfitting & regularization            | notebook           | ⬜   |
| 08 | Real EEG signal (MOABB/MNE)             | notebook           | ✅   |
| 09 | First decoder: CSP+LDA, one subject     | notebook           | ✅   |
| 10 | All subjects & between-subject variance | notebook           | ✅   |
| 11 | Pipeline benchmark (incl. Riemannian)   | notebook           | ✅   |
| 12 | Final packaging                         | README + figures   | ⬜   |

Classes 08-11 (the EEG decoding core) were built first; classes 01-07 (ML
fundamentals on toy data) are being filled in next to complete the learning
path.

## Repo structure

```
motor-imagery-decoding/
├── notebooks/
│   ├── 08_real_eeg_signal.ipynb
│   ├── 09_first_decoder_csp_lda.ipynb
│   ├── 10_all_subjects_variance.ipynb
│   └── 11_pipeline_benchmark.ipynb
├── requirements.txt
├── LICENSE
└── README.md
```

## Run Locally

```bash
git clone https://github.com/<your-username>/motor-imagery-decoding.git
cd motor-imagery-decoding

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

The EEG stack (MNE, MOABB, pyRiemann, braindecode) is introduced from Class 08
onward; earlier classes only need the core scientific Python stack.

## Results so far

CSP+LDA, MDM, and Tangent Space+LDA evaluated on all 9 subjects of
BNCI2014_001 (4-class motor imagery), with `GroupKFold` splitting by
recording session to avoid leakage:

| Pipeline | Mean accuracy | Std across subjects |
|---|---|---|
| CSP+LDA | 0.608 | 0.134 |
| MDM | 0.557 | 0.149 |
| TS+LDA | **0.676** | 0.160 |

(Chance level with 4 classes: 0.25.)

## Lessons Learned

> Filled in as the project progresses — this section is the payoff of the
> two-layer design.

- _Class 09_ — why CSP and LDA must be chained inside a single `Pipeline`:
  fitting CSP once on all the data before splitting leaks test-session
  information into the spatial filters themselves.
- _Class 10_ — reading results at the population level: a single subject
  can't tell you whether a decoder generalises. Mean 0.608, std 0.127 across
  9 subjects — well above chance, but with real between-subject variability
  consistent with known BCI-illiteracy effects.
- _Class 11_ — TS+LDA wins on mean accuracy but is the least consistent
  across subjects; CSP+LDA, despite a lower mean, is the most stable. A
  trade-off between peak performance and reliability.

---

_This README is the project's front door. It is intentionally readable both by
a recruiter scanning for a finished BCI project and by someone curious about how
it was built from first principles._
