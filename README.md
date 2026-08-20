# Modelling Cycling Crash Severity and Spatial Inequalities in the UK Using Machine Learning and GIS

Analysis notebook for a dissertation project modelling cycling crash severity and spatial
inequalities across UK regions (West Midlands, Great London, Great Manchester), using machine learning
(LightGBM / Random Forest with SHAP explainability) and GIS-based exposure normalisation.

## Contents

- `notebooks/analysis_FINAL_1.ipynb` — main analysis notebook:
  - **Cell A** — exposure-normalised crash rates (Table 5.1, Figures 5.4–5.6)
  - **Cell B** — severity classification modelling (Tables 5.2–5.4, Figures 5.7–5.12)

## Setup

```bash
pip install -r requirements.txt
```

Then open `notebooks/analysis_FINAL_1.ipynb` in Jupyter. The notebook expects the input
data files to be present in the working directory — update the file paths at the top of
each cell to point to your local copies.

## Data

Raw datasets (DfT road casualty statistics, ONS/IMD indices, GIS rasters and shapefiles)
are not included in this repository due to size and licensing considerations. See the
notebook for the expected file names and sources.

## Results

Small summary tables produced by the notebook are included under `results/`:

- `table_5_1_exposure_normalised_rates.csv` — Table 5.1
- `tables_5_2_to_5_4_model_performance.csv` — Tables 5.2–5.4
- `birmingham_exposure_normalised_summary.csv`, `london_exposure_normalised_summary.csv`,
  `manchester_exposure_normalised_summary.csv` — per-city exposure-normalised rate summaries

Figures referenced in the tables above are rendered inline in the notebook itself.

## License

MIT — see [LICENSE](LICENSE).
