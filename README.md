# Durban Housing Suitability Analysis

Multi-Criteria Analysis (MCA) for evaluating housing suitability in the Mzinyati Stream Catchment, eThekwini Municipality, South Africa.

## Overview

This project integrates spatial data across hazards, infrastructure, socioeconomic factors, environmental conditions, and community input to identify areas suitable for housing development. Weights are derived using the Analytic Hierarchy Process (AHP) with a fuzzy extension (Buckley's method).

**Notebooks:**

- `Durban_MCA.ipynb` — main analysis pipeline (data loading, factor scoring, AHP weighting, suitability mapping, at-risk building analysis)
- `Field_Map.ipynb` — generates a tablet-ready interactive HTML map from the MCA outputs

## Data Structure

Data files are not included in this repository (they must be obtained separately — see Data Sources below). Organise them as follows:

```
data/
├── reference/
│   ├── Major_Catchments_*.gpkg
│   ├── eThekwini_Municipal_Boundary_*.gpkg
│   ├── Urban_Development_Line_*.gpkg
│   ├── mzinyati_buildings.gpkg
│   └── Street_Address_*.gpkg
├── group_a_hazards/
│   ├── S30E030_FABDEM_V1-2.tif       (DEM — used to derive slope)
│   ├── landslide_susceptibility.tif   (pre-scored 1–5)
│   └── flood_extents.tif              (pre-scored 1–5)
├── group_b_infrastructure/
│   ├── Roads_*.gpkg
│   ├── Fixed_Clinics_*.gpkg
│   ├── Mobile_Clinics_*.gpkg
│   ├── Hospitals_*.gpkg
│   ├── Fire_Stations_*.gpkg
│   ├── water_network.gpkg
│   └── electricity_network.gpkg
├── group_c_socioeconomic/
│   ├── settlements.gpkg
│   ├── investment_nodes.gpkg
│   └── transport_corridors.gpkg
├── group_d_community/
│   └── community_hazard_map.tif       (pre-scored 1–5)
└── group_d_environmental/
    ├── DMOSS_2018_*.gpkg
    ├── Rivers_*.gpkg
    └── wetlands.gpkg
```

## Running the Analysis

### Google Colab (recommended)

1. Open `Durban_MCA.ipynb` via the **Open in Colab** badge at the top of the notebook
2. Upload your data to Google Drive at `MyDrive/Durban/data/`
3. Run all cells in order — Cell 1 installs packages, Cell 2 mounts Drive, Cell 3 onwards runs the analysis

### Local

```bash
git clone https://github.com/kilianodonell-cmd/Durban
cd Durban
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # Mac/Linux
pip install -r requirements.txt
jupyter notebook Durban_MCA.ipynb
```

Place your data in the `data/` folder following the structure above, then run all cells.

## Configuration

All parameters are in **Cell 2 of `Durban_MCA.ipynb`** — this is the only cell you need to edit:

- `DATA_ROOT` / `OUTPUT_ROOT` — paths to your data and output folders
- `TARGET_CRS` — coordinate reference system (default: EPSG:32736, UTM Zone 36S)
- `CELL_SIZE_M` — raster resolution in metres (default: 30 m)
- `RIVER_BUFFER_M` — exclusion buffer around rivers (default: 30 m)
- `FACTORS` — add, remove, or adjust scoring thresholds for each factor
- `AHP_COMPARISONS` — pairwise importance judgements (Saaty scale 1–9)
- `SCENARIOS` — group-level weight combinations for sensitivity testing

## Data Sources

| Layer | Source |
|---|---|
| Major Catchments | eThekwini Municipality / SA National GIS |
| Municipal Boundary | eThekwini Municipality |
| Urban Development Line | eThekwini Municipality |
| Building Footprints | eThekwini Municipality / OpenStreetMap |
| DEM (FABDEM) | [FABDEM v1-2](https://doi.org/10.5523/bris.s5hqmjcdj8yo2ibzi9b4ew3sn) |
| Roads, Clinics, Hospitals, Fire Stations | eThekwini Municipality Open Data |
| DMOSS 2018 | eThekwini Environmental Planning |
| Rivers | eThekwini Municipality |

> Data files are not redistributed here. Contact eThekwini Municipality or the respective sources for access.

## Methodology

1. All vector layers are clipped to the Mzinyati Stream Catchment AOI
2. Each factor is converted to a 30 m raster and scored 1–5 (1 = least suitable, 5 = most suitable)
3. Pairwise comparisons are entered in Cell 2 and converted to AHP weights using Buckley's fuzzy method
4. Weighted linear combination (WLC) is applied within each group, then across groups
5. Constraint layers (rivers, D'MOSS, steep slopes, wetlands) are applied as binary masks
6. Three scenarios (hazard-focused, balanced, infrastructure-focused) test weight sensitivity

## Outputs

| File | Description |
|---|---|
| `outputs/rasters/suitability_*.tif` | Final suitability rasters per scenario (1–5) |
| `outputs/rasters/constraint_mask.tif` | Binary constraint mask |
| `outputs/clipped/*.tif / *.gpkg` | Intermediate clipped and scored layers |
| `outputs/maps/*.png` | Static map exports |
| `outputs/field_map.html` | Interactive map for field use |

## License

MIT — see `LICENSE`

## Citation

O'Donnell, K. (2026). *Durban Housing Suitability Analysis: Multi-Criteria Evaluation for the Mzinyati Stream Catchment*. GitHub. https://github.com/kilianodonell-cmd/Durban

## References

- Saaty, T. L. (1980). *The Analytic Hierarchy Process*. McGraw-Hill.
- Buckley, J. J. (1985). Fuzzy hierarchical analysis. *Fuzzy Sets and Systems*, 17(3), 233–247.
- Chang, D.-Y. (1996). Applications of the extent analysis method on fuzzy AHP. *European Journal of Operational Research*, 95(3), 649–655.
