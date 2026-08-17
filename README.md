# ML-based mapping of post-harvest residual soil nitrate

This repository contains the data inputs and reproducible Python workflow used to map post-harvest residual soil nitrate (NO₃⁻-N) in the **patchCROP diversified cropping experiment** in Brandenburg, Germany.

The analysis combines soil measurements, soil-moisture information, weather variables, Sentinel-2 features, soil/terrain covariates, and nitrogen-management information. Random Forest and XGBoost models are optimized with Optuna and combined in a stacked ensemble. Dynamic predictors are evaluated over antecedent windows of **1, 3, 7, 15, 30, 60, and 90 days** before post-harvest soil sampling.

## Repository structure

```text
.
├── data/
│   ├── patchCROP_Residual_Nitrate.xlsx
│   └── spatial/
│       └── 30patches.zip
├── notebooks/
│   └── RF-XGB-Ens_OPTUNA_Workflow.ipynb
├── .gitignore
├── CITATION.cff
├── LICENSE
├── README.md
└── requirements.txt
```

### Data

`data/patchCROP_Residual_Nitrate.xlsx` is the final analysis dataset used by the notebook. The workbook includes a **README/data-dictionary sheet** describing the variables, temporal windows, units, coding conventions, and known points requiring care when reusing the data.

The main response variable in the analysis is post-harvest topsoil residual nitrate at **0–30 cm depth** (`kgNO3_0_30`). The dataset covers the study period **2020–2024** and integrates observations from 30 experimental patches.

### Spatial boundaries

`data/spatial/30patches.zip` contains the ESRI shapefile components for the 30 patch boundaries used for the spatial train/test visualization and patch-level diagnostic maps.

### Notebook

`notebooks/RF-XGB-Ens_OPTUNA_Workflow.ipynb` contains the complete modelling workflow, including:

- data loading and type cleaning;
- filtering of after-harvest campaigns;
- spatially independent patch hold-out validation;
- Random Forest and XGBoost model fitting;
- Optuna hyperparameter optimization;
- GroupKFold cross-validation grouped by patch;
- Ridge-regression stacked ensemble;
- comparison of antecedent temporal windows;
- RMSE, MAE, and R² evaluation;
- SHAP-based predictor interpretation;
- accumulated local effect / response analyses;
- observed-versus-predicted diagnostics; and
- patch-level spatial mapping.

For repository portability, the notebook uses **relative paths** rather than the local paths used during development. Stored execution outputs were cleared before publication; running the notebook regenerates the analysis outputs.

## Reproducing the analysis

Clone the repository and create a Python environment:

```bash
git clone https://github.com/tawhidhossain13/ML-based-mapping-of-soil-residual-nitrate-.git
cd ML-based-mapping-of-soil-residual-nitrate-
python -m venv .venv
```

Activate the environment and install the dependencies:

```bash
# Windows
.venv\Scripts\activate

# macOS/Linux
source .venv/bin/activate

pip install -r requirements.txt
```

Launch Jupyter and run the notebook from top to bottom:

```bash
jupyter lab notebooks/RF-XGB-Ens_OPTUNA_Workflow.ipynb
```

The notebook automatically resolves the repository root when launched from either the repository root or the `notebooks/` directory. Generated figures, model diagnostics, train/test patch lists, and Excel outputs are written to `outputs/`.

## Expected input paths

The notebook expects the following repository-relative files:

```text
data/patchCROP_Residual_Nitrate.xlsx
data/spatial/30patches.zip
```

If these files are moved or renamed, update the path definitions in **Block B** of the notebook.

## Study context

The workflow was developed for the manuscript:

> **Mapping of residual soil nitrate in a diversified cropping system using multi-source sensing data and stacked ensemble learning**

Authors: Md Tawhid Hossain, Santiago Tamagno, Sonoko D. Bellingrath-Kimura, and Kathrin Grahmann.

The study evaluates how environmental conditions, crop status, soil water dynamics, and nitrogen management explain the spatial variation of post-harvest residual nitrate across a heterogeneous patch-cropping system.

## Reproducibility notes

- The spatial validation split is defined by patch IDs in the notebook to maintain geographic independence between training and test data.
- The notebook evaluates seven antecedent windows: 1, 3, 7, 15, 30, 60, and 90 days before the soil-sampling date.
- Dynamic predictors are therefore measurement-centered antecedent summaries, not fixed crop-phenological intervals.
- `outputs/` is excluded from version control because all files in that folder are reproducible from the supplied inputs and notebook.
- The Excel workbook's README sheet should be consulted before reusing individual variables, especially coded or derived fields.

## Software

The workflow is written in Python and uses pandas, NumPy, scikit-learn, XGBoost, Optuna, SHAP, Matplotlib, GeoPandas, and related geospatial dependencies. See `requirements.txt`.

## Citation

If you use this repository, please cite the associated manuscript once its final bibliographic information is available. Repository citation metadata are also provided in `CITATION.cff`.

## License

See [`LICENSE`](LICENSE) for the repository license.

## Contact

**Md Tawhid Hossain**  
Leibniz Centre for Agricultural Landscape Research (ZALF), Germany  
GitHub: [@tawhidhossain13](https://github.com/tawhidhossain13)
