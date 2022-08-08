# cookiecutter-data-science [my personal edition]

An opinionated take on popular [cookiecutter-data-science](https://github.com/drivendata/cookiecutter-data-science) project template.

Main differences:
- Ditch the notions of script-based workflow in favor of modular pipeline-based workflow (akin to [kedro](https://github.com/kedro-org/kedro), or anything DAG, like Airflow). This implies that:
  - we leverage python modules instead of python scripts where appropriate
  - we design pipelines (and preferably record them in some pipeline catalog, like proposed by [kedro](https://github.com/kedro-org/kedro)) as a series of python callables instead of shell executables
  - => easy to develop/run pipelines in different environments (notebook, script, Airflow pipeline, etc.) 
- Project dependency mgmt via [poetry](https://github.com/python-poetry/poetry)
  - environment mgmt easy like with `conda`
    - but runs on `pip` packages
      - => better ecosystem support (in general you break `conda` env by installing anything via `pip`)
  - => powereful dependency specification via semantic versioning in `pyproject.toml`
  - => exact environment reproducibility via `poetry.lock` (akin to [standard dependency mgmt in Julia](https://docs.julialang.org/en/v1/stdlib/Pkg/))
- Add `metadata` folder
  - Inspired by AWS Athena cli api, which along with SQL query results can download query metadata in JSON (things like query text, date of execution, etc.). I generally implement something similar when i'm fetching datasets via SQL in python or creating modeling artefacts.
  - => incites good reproducibility practices
- More elaborate `notebook` directory structure 
  - `examples` for very stable / reproducible /"gold standard" code (especially good for quick onboarding)
  - `sandbox` for personal directories or any kind of contained disorder
  - => enables multiple good practices

---
The directory structure of your new project looks like this:
```

├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
├── src                <- Source code for use in this project.
│   ├── __init__.py    <- Makes src a Python module
│   │
│   ├── data           <- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io
```
