# Quick Setup

## Google Colab (easiest)

1. Click the **Open in Colab** button in `Durban_MCA.ipynb`
2. Upload your data to Google Drive at `MyDrive/Durban/data/` following the folder structure in README.md
3. Run cells top to bottom — packages install automatically in Cell 1

## Local (Windows / Mac / Linux)

**1. Clone the repo**
```bash
git clone https://github.com/kilianodonell-cmd/Durban
cd Durban
```

**2. Create a virtual environment**
```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# Mac / Linux
python3 -m venv .venv
source .venv/bin/activate
```

**3. Install packages**
```bash
pip install -r requirements.txt
```

**4. Add your data**

Place data files in the `data/` folder following the structure in README.md.

**5. Edit paths in Cell 2**

Open `Durban_MCA.ipynb` and update `DATA_ROOT` and `OUTPUT_ROOT` at the top of Cell 2 to point to your local `data/` and `outputs/` folders.

**6. Run**
```bash
jupyter notebook Durban_MCA.ipynb
```

Run all cells in order. Outputs are saved to `outputs/`.

## Troubleshooting

| Problem | Fix |
|---|---|
| `ModuleNotFoundError` | Run `pip install -r requirements.txt` |
| `FileNotFoundError: data/...` | Check your data folder structure matches README.md |
| Slow raster processing | Increase `CELL_SIZE_M` in Cell 2 (e.g. 60 m instead of 30 m) |
| Wrong coordinate system | Check `TARGET_CRS` in Cell 2 matches your data |
