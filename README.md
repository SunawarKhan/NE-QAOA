[README.md](https://github.com/user-attachments/files/30392109/README.md)
# NE-QAOA: Noise-Aware Multi-Objective Evolutionary Optimization of QAOA for EV Charging

Runnable research package accompanying the paper **NE_QAOA_Research_Paper.docx**.

## Contents
- `NE_QAOA_EV_Charging.ipynb` — main notebook (already executed, all outputs + figures embedded). PennyLane-based.
- `NE_QAOA_Research_Paper.docx` — full paper (equations, figures, in-text citations, references).
- `src/` — Python modules the notebook imports:
  - `acn_dataset.py` — ACN-Data-schema dataset (synthetic surrogate + real-token hook `load_real_acn`) and TOU tariff.
  - `qaoa_core.py` — QUBO/Ising build, QAOA circuit (X and XY mixers), ideal + NISQ-noise QNodes, brute-force reference.
  - `optimize.py` — 4-objective NSGA-II evaluator (quality, feasibility, noise, hardware) + all baselines.
  - `run_experiment.py` — full pipeline driver + figure generators.
  - `make_static_figures.py` — methodology / architecture / flowchart / circuit figures.
  - `build_notebook.py`, `render_equations.py`, `build_docx.py` — assembly scripts.
- `figures/` — all PNG figures (incl. `eq/` equation renders).
- `outputs/` — `results.json`, `pareto.npz`, `acn_sessions.csv.gz`.

## Reproduce
```bash
pip install pennylane pymoo matplotlib pandas numpy nbformat nbconvert
# to re-run the notebook, keep src/ modules importable (run from a dir where they're on the path)
cp src/*.py .
jupyter nbconvert --to notebook --execute --inplace NE_QAOA_EV_Charging.ipynb
```

## Using the REAL ACN-Data
The experiments use a schema-faithful surrogate because the live API is token-gated.
For genuine data, get a token at https://ev.caltech.edu, `pip install acnportal`, and call
`load_real_acn(api_token=...)` in `src/acn_dataset.py` instead of the synthesizer.

## Key result (6-qubit instance)
NE-QAOA approx ratio **0.925**, feasibility 0.493 (best combined among quantum methods);
**0.928 ± 0.005** vs COBYLA 0.760 ± 0.195, Wilcoxon **p = 0.009**. Exact optimum energy 5.58.
