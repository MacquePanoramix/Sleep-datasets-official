# Project instructions for Codex

This project analyzes sleep datasets for a CreaTe Hybrid Worlds installation.

Use dyslexia-friendly markdown explanations in notebooks.

Important folders:
- Notebooks/ contains Jupyter notebooks.
- data/ contains datasets.
- Graphs/ contains saved graph outputs.

When running notebooks, use:

python -m jupyter nbconvert --to notebook --execute Notebooks/<notebook_name>.ipynb --inplace

If a dataset download fails, first check whether the dataset already exists in data/.

Do not delete existing notebooks.

Prefer simple, interpretable models:
- baseline mean model
- linear regression
- random forest or gradient boosting only as comparison

For every model, report:
- rows used
- baseline performance
- model performance
- coefficients or feature importance
- final decision: use / do not use
- formula for installation if usable