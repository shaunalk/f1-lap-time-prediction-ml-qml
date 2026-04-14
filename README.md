# F1 lap time prediction: classical vs quantum ML

**Title:** Predicting Formula 1 Lap Times Using Classical and Quantum Machine Learning: A Comparative Study Using FastF1 Telemetry Data

This repository contains the **implementation** for the project above. The **written thesis or report** presents the research questions, literature, methodology narrative, and interpretation of results; **this codebase** is the executable artefact used to obtain those results (data loading, models, evaluation, figures/tables).

**Repository layout:** All runnable code is self-contained in two Jupyter notebooks under `FullMode/` and `TestMode/` (no separate application entrypoint).

---

## Project metadata

| Field | Value |
|-------|--------|
| Author | Shauna Kearney |
| Institution | University of Limerick |
| Programme | CSIS Final Year Project |
| Academic year | 2025–2026 |

---

## What’s in this folder

| Path | Purpose |
|------|---------|
| `FullMode/NotebookFullMode.ipynb` | Full evaluation run (production-style settings: more data and iterations where configured). Use for **final reported numbers and figures**. |
| `TestMode/NotebookTestMode.ipynb` | Reduced settings for **quick validation** that the pipeline and dependencies work end-to-end. |
| `requirements.txt` | Python package dependencies (minimum versions). |
| `FullMode/Results/`, `TestMode/Results/` | Example output artefacts from a previous run **if present**; regenerate by executing the corresponding notebook if you need a fresh reproducible bundle. |

---

## Environment and reproducibility

- **Python:** 3.8 or newer (3.10+ recommended for current scientific stacks).
- **Install:** `pip install -r requirements.txt` inside a virtual environment.
- **Execution order:** Run the notebook **from top to bottom** on first use so imports, configuration, and helper functions exist before cells that depend on them.
- **Hardware:** `FullMode` is intended for **GPU** runtimes where available (e.g. Colab with GPU); quantum model training uses **simulation** and can be **slow** and memory-heavy compared with classical baselines. `TestMode` is suitable for laptops and shorter runs.
- **Runtime expectations:** A full `FullMode` run may take **a long time** (order of hours), depending on years loaded, sample caps, and quantum iterations—this is normal for simulator-based QNN training, not a sign of failure.
- **Outputs:** Figures, CSV summaries, and PNG/PDF exports are produced as cells execute. Treat any checked-in `Results/` folders as **snapshots** unless your submission policy requires regenerating everything and attaching logs.

---

## Quick start

### Google Colab

1. Upload `FullMode/NotebookFullMode.ipynb` (or `TestMode/NotebookTestMode.ipynb` for a shorter run).
2. Set runtime to **GPU** if offered (recommended for quantum simulation workloads).
3. Run **all cells sequentially** on first execution.

### Local (Jupyter Lab / Notebook)

1. `python -m venv .venv` then activate it.
2. `pip install -r requirements.txt`
3. Open the chosen `.ipynb` and run cells in order.

---

## Dependencies

See **`requirements.txt`**. Core packages include `pandas`, `numpy`, `fastf1`, `scikit-learn`, `qiskit`, `qiskit-machine-learning`, `qiskit-algorithms`, `matplotlib`, `seaborn`, and `scipy`. Optional lines (e.g. `qiskit-aer`, `psutil`, `ydata-profiling`) support specific notebook paths and may be omitted if unused.

---

## Data sources and access

- **FastF1** is used to access Formula 1 timing/telemetry-derived lap data for modelling.
- **Network access** is required for **first-time** session downloads; subsequent runs use on-disk **caching** (typically under a cache directory configured in the notebook environment) and are faster.
- **API limits:** FastF1 relies on upstream services; rate limiting or outages can cause slow loads or skipped sessions—the notebook includes defensive handling where implemented.

This work uses **publicly accessible race data via FastF1**; it is **not** a live strategy or commercial F1 product, and results are **research-grade**, not validated for real-time race operations.

---

## How the notebooks are organized (conceptually)

1. **Configuration** — years, session type, sample limits, PCA dimensions, quantum training iterations, etc.
2. **Data pipeline** — load sessions, clean laps, engineer features, build feature matrices and targets.
3. **Classical models** — Ridge (full features), Histogram Gradient Boosting (full features), Ridge on PCA-reduced features (classical baseline at comparable dimensionality to the quantum input).
4. **Quantum model** — Qiskit quantum neural network on PCA-reduced, scaled inputs suitable for quantum feature encoding.
5. **Evaluation** — grouped cross-validation (to limit leakage across related laps), metrics (MAE, RMSE, R²), timing, and statistical comparisons between classical and quantum fold results.

Section titles and cell indices are defined inside each notebook.

---

## Fair comparison (scope of claims)

- **Same sample caps** where enforced for classical vs quantum comparisons.
- **PCA-reduced classical baseline** to align input dimensionality with the quantum pathway.
- **Grouped cross-validation** (grouped by session/race-like identifiers) to reduce train/test leakage.
- **Same metrics** for all models.

The study reports a **controlled comparison** under computational and coursework constraints. It does **not** assert general quantum advantage for F1 lap-time prediction.

---

## Limitations (summary)

- **Quantum:** Models are trained with **simulators** (and optional backends where configured), not a claim about current hardware quantum advantage.
- **Scope:** Results depend on **data coverage**, **feature choices**, and **hyperparameters** fixed in the notebook; alternative models or tuning could change rankings.
- **Interpretation:** Metrics are reported for **offline** prediction quality and cost; they do not by themselves validate deployment in a real race environment.

---

## Outputs

Generated artefacts typically include model comparison plots, tables (CSV), and per-fold metrics, written to the working directory and/or `FullMode/Results/` or `TestMode/Results/` depending on the notebook’s output workflow. Regenerate by running the relevant notebook after `pip install -r requirements.txt`.

---

## Troubleshooting

- **`NameError` (`config`, `X_classical`, pipeline helpers):** Run the earlier cells that define imports, configuration, and `load_sessions_and_build_dataset`, then rerun the failing cell.
- **FastF1 download or rate limits:** Check network access; wait and retry; use caching after the first successful download.
- **Qiskit import or version errors:** Install packages from `requirements.txt`; align major versions if your environment pins them strictly.

---

## References and acknowledgements

- **FastF1** — Python package for Formula 1 data access and processing: [https://github.com/theOehrly/Fast-F1](https://github.com/theOehrly/Fast-F1) (see repository for citation guidance).
- **Qiskit** — quantum SDK and machine-learning module: [https://qiskit.org/](https://qiskit.org/) and [https://qiskit.org/ecosystem/machine-learning/](https://qiskit.org/ecosystem/machine-learning/).


