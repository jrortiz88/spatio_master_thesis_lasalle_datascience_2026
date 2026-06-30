# Spatio

**Spatio** is a fully reproducible data science framework for neighborhood segmentation and residential suitability assessment in New York City. It integrates ACS census indicators, NYC Open Data, OpenStreetMap amenities, crime proxies, and Zillow market signals into a unified neighborhood‑level analytical dataset.

The core analytical method is **Agglomerative Clustering (cosine distance, average linkage)** applied in PCA‑reduced feature space, producing three interpretable residential typologies. Suitability is evaluated through **block‑weighted composite indicators** (Income‑oriented, Growth‑oriented, and Hybrid), built over standardized and direction‑adjusted variables with sensitivity analysis.

A portable Random Forest classifier and Ridge regression models are included to support **cross‑city scalability**, validated externally on Boston.

This repository accompanies academic thesis work. It supports structured comparison and decision‑support analysis — it does **not** forecast investment returns or guarantee real‑estate performance.



##  Repository structure


```
.
├── notebooks_clean/        17 notebooks, numbered in pipeline order
└── data/                   not included in this repo — see "Getting the data" below
```

All notebooks resolve data paths relative to their own location (`DATA_DIR = Path("../data").resolve()`), so `data/` must sit as a sibling of `notebooks_clean/` at the repo root.

`data/models/portability/` is not pre-created — it is generated automatically the first time `11_regression.ipynb` runs.

## Getting the data

The `data/` directory is ~650MB and contains files exceeding GitHub’s 100MB limit, so it is **not** committed to the repository.  
Instead, it is distributed as a ZIP file attached to the latest **GitHub Release**

### Download instructions

1. Download `spatio_data_v1.zip` from the latest Release:  
   https://github.com/jrortiz88/spatio_master_thesis_lasalle_datascience_2026/releases/latest

2. Extract the ZIP at the repository root so your structure becomes:
`./data/` next to `./notebooks_clean/`.

The archive ships pre-populated with every raw, intermediate, and processed file the notebooks read, so the pipeline can be run end-to-end without regenerating anything. Every save call in every notebook is intentionally commented out, so running the notebooks will not overwrite or add files to your local copy.

## Setup

Requires Python 3.10+.

```bash
pip install -r requirements.txt
```

`geopandas`, `shapely`, and `osmnx` depend on GDAL/GEOS/PROJ system libraries. On Windows in particular, installing via conda is usually easier than pip:

```bash
conda install -c conda-forge geopandas shapely osmnx scikit-learn matplotlib seaborn shap minisom
```

### Census API key

`1_extract_acs_2024.ipynb` calls the Census Bureau ACS5 API and requires an API key. Sign up for a free key at https://api.census.gov/data/key_signup.html and set it in the notebook before running.

### Internet access

Notebooks 1 (Census ACS), 5 (NYC Open Data crime + tract geometry), and 6 (NYC Open Data restaurants/schools/facilities + OpenStreetMap via OSMnx) make live API calls regardless of the vendored `data/` snapshot. These require internet access and may be slow or rate-limited; results can drift slightly from the thesis if the upstream sources have since been updated.

## How to run

Open `notebooks_clean/` as your working directory before launching Jupyter — every notebook's relative paths assume this.

```bash
cd notebooks_clean
jupyter lab
```

Recommended execution order:

```
nb1 → nb2 → nb3 → nb4 → nb5 → nb6 → nb7 → nb8 → nb9 → nb10 → nb11 (NYC main pipeline)
nb12 → nb13 → nb14   (Boston branch — independent of the main NYC pipeline)
nb15                 (Zillow branch — depends on nb9's crosswalk outputs)
nb16                 (dashboard export — depends on nb7, nb8, nb9, nb10, nb15)
nb17                 (NYC listing demo — standalone, only needs the Kaggle CSV)



nb14 is analysis-only and produce no saved files — skip them if you only want the data artifacts.

## License

No license is currently specified for this repository. Default copyright applies — all rights reserved.
