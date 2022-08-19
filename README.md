# cookiecutter-data-science [my personal edition]

An opinionated (but hopefully still welcoming and accessible) take on popular [cookiecutter-data-science](https://github.com/drivendata/cookiecutter-data-science) project template.

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
- Add `/metadata` folder
  - Inspired by AWS Athena cli api, which along with SQL query results can download query metadata in JSON (things like query text, date of execution, etc.). I generally implement something similar when i'm fetching datasets via SQL in python or creating modeling artefacts.
  - => incites good reproducibility practices
- More elaborate `/notebook` directory structure 
  - `/examples` for very stable / reproducible /"gold standard" code (especially good for quick onboarding)
  - `/sandbox` for personal directories or any kind of contained disorder
  - `/reports` for what actually is "report materials", i.e. code generated figures or other artefacts derieved from notebook workflow in the form of HTML or .ipynb; ideally you should leverage some experiment management framework instead for the sake of reproducibility; 
  - => enables multiple good practices
- Different naming convention for notebooks
  - `<the creator's initials>-<description>-<version>-<more specific description>-<version>`
    => allows for better sorting of fresh works related to single topic
    => allows to version work that is done based off some other work (like superstructure)
  - for better sorting of fresh works related to single topic
- Modified `/src` directory structure 
  - to properly accomodate `poetry` `src`-based workflow (`poetry` has some rules here)
  - to make module structure more reasonable for use as python modules
  - the underscore "_" reflects that it's private code of the submodule and the file can be named whatever, or it can be multiple files with "_"; regardless, `__init__.py` should expose module as `data`, not as `_data`; this convention is inspired by `sklearn` repository structure.
- Add `/artefacts` directory (replaces `/models` directory)
  - a place for everything serialized (including models)
- Removed `/reports` directory
  - ideally reports should be experiment management frameworks (to ensure reproduciiblity)
  - can leverage `/notebooks/reports` for notebook-produced report materials
  - `Powerpoint` reports, if they are needed, better go into `/references`
 

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
│   ├── raw            <- The original, immutable data dump.
│   └── **meta**       <-

├── conf <- configs
    └── **base** <- reference example/standard configs
    └── **local** <- custom/personal configs
├── **cli** <- cli scripts
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── **artefacts**      <- Serialized fitted models/pipelines/python objects/etc
│
├── *experiments*        <- Jupyter notebooks / scripts of experiments. Naming convention is: 
│   |                     <the creator's initials>-<description>-<version>-<more specific description>-<version>
│   ├── **examples**   <- "Golden" notebooks/scripts for reference (highly reproducible examples of pipelines)
│   ├── *reports*    <- Clean notebooks for showcase / htmls / plots
│   └── **sandbox**    <- For personal dirty experiments (any work-in-progress mess should be contained here preferably)
|       ├── <creator_1>  <- A directory for each user
|       └── <creator_2>
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
│
├── pyproject.toml   <- The requirements file for flexible developing in the environment,
│                       generated manually or with `poetry init`
├── poetry.lock      <- The requirements file for hard repdorucibility of the environment
│
├── src                <- Source code for use in this project.
|   └── <repo name> <- Demanded by `poetry` in `src` mode
│       ├── __init__.py    <- Makes src a Python module
│       │
│       ├── data           <- Submodule for data generation & processing
            ├── __init__.py    <- Makes the directory a Python module
            └── _data.py   <- Submodule code. 
        ├── analytics <- code for datavis/summaries/reporting
            ├── __init__.py    <- Makes the directory a Python module
            └── _analytics.py <- Submodule code
        ├── modeling <- code for modeling (e.g. custom implementations)
            ├── __init__.py    <- Makes the directory a Python module
            └── _modeling.py <- Submodule code
        ├── utils <- small utilities like metric computation functions
            ├── __init__.py    <- Makes the directory a Python module
            └── _utils.py <- Submodule code
        ├── pipelines <- WIP; should be a pipeline catalogue of sorts (see Kedro); perhaps should live near the /conf directory instead/
            ├── __init__.py    <- Makes the directory a Python module
            └── _pipelines.py <- Submodule code
        └── experimental <- anything not stable / tested enough to be promoted to into dedicated submodules
            ├── _data.py
            ├── _analytics.py
            ├── _modeling.py
            ├── _utils.py
            └── _pipelines.py <- Submodule code
            
│   
│
└── tests            <- folder with unit tests
    ├── __init__.py
    ├── test_<repo name>.py <- the most basic test - importing the project as a module (es created by `poetry` automatically)
```
