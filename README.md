## PPP-Tiger

Precise Point Positioning (PPP) analysis and visualization using a self-contained set of GNSS utilities under `funcs/`, driven by the main Jupyter Notebook `ppp-tiger-main.ipynb`. The notebook demonstrates a full workflow: RINEX decoding, satellite visibility analysis (skyplot, elevation), PPP computation with precise products, result visualization, and export to RTKLib-compatible `.pos`.

### Features
- **RINEX support**: Navigation/Observation decoding via `funcs.rinex`.
- **Ephemeris and precise products**: Broadcast ephemeris and IGS precise orbits/clocks/biases via `funcs.ephemeris` and `funcs.peph`.
- **PPP engine**: GPS-only PPP demo (static or kinematic) with troposphere/ionosphere options via `funcs.ppp` or `funcs.pppmod`.
- **Visualization**: Skyplot, elevation time series, ENU error plots, histograms, CDFs.
- **Export**: RTKLib-compatible `.pos` output (e.g., for RTKPlot).

### Repository Structure
- `ppp-tiger-main.ipynb` — Main notebook that runs the entire workflow end-to-end.
- `funcs/` — Core modules:
  - `gnss.py` — GNSS constants, time/coord utilities, data structures (`Eph`, `Geph`, `Nav`, etc.).
  - `rinex.py` — RINEX decoder for NAV/OBS/CLK.
  - `ephemeris.py` — Satellite position/clock from broadcast and precise sources.
  - `peph.py` — Precise ephemeris/clock handling (SP3/CLK), antenna models (ANTEX), OSB (BIA).
  - `ppp.py` / `pppmod.py` — PPP processing components.
  - `plot.py` — Utility plotting (skyplot, elevation).
  - `mlambda.py`, `typer.py`, `rawnav.py` — Supporting algorithms and types.
- `data/` — Place your RINEX and auxiliary product files here (see below).
- `gps_only_ppp.pos` — Example RTKLib `.pos` output created by the notebook.
- `gps_only_ppp.log` — Example processing log created by the notebook.

### Requirements
- Python 3.9+ recommended
- JupyterLab or Jupyter Notebook
- Python packages:
  - `numpy`
  - `matplotlib`
  - `cartopy` (for map projections in skyplot/ground track; optional if you skip those plots)
  - `scipy` (recommended)
  - `ipykernel` (for Jupyter)

Quick install (pip):
```bash
python -m pip install --upgrade pip
pip install numpy matplotlib cartopy scipy ipykernel jupyter
```

Cartopy note: On Windows, you may need additional system packages (PROJ/GEOS). If installation is difficult, you can comment out the cartopy-based ground track cell(s) in the notebook and still run the PPP workflow.

### Quick Start
1. Clone the repository and open a Python environment with the requirements installed.
2. Put required RINEX and precise product files under `data/` (see above).
3. Launch Jupyter and open the notebook:
   ```bash
   jupyter lab
   # or
   jupyter notebook
   ```
4. Run `ppp-tiger-main.ipynb` from top to bottom.
   - The notebook will:
     - Decode RINEX NAV/OBS
     - Load precise products (SP3/CLK/OSB/ANTEX)
     - Initialize GPS-only PPP
     - Process epochs and compute ENU errors
     - Plot visibility and solution-quality figures
     - Export RTKLib `.pos` as `gps_only_ppp.pos`

### Output Artifacts
- `gps_only_ppp.pos` — RTKLib-compatible trajectory file (ECEF positions per epoch, quality flags).
- `gps_only_ppp.log` — Optional log with diagnostic messages (depends on logging level).
- Figures — Skyplot, elevation plots, ENU time series, scatter plots, histograms, CDFs (rendered in the notebook; you can save them manually if needed).

### Typical Workflow Notes
- The demo uses **GPS-only** signals for clarity. You can extend to other constellations by adjusting signal selection in the notebook (see `rSigRnx(...)` and `rnx.setSignals`).
- For cartopy-based plots, ensure `cartopy` and its binary dependencies are installed; otherwise, comment those cells.
- If using different product providers (e.g., CODE, GFZ, JPL), update file names/paths accordingly.

### Troubleshooting
- If imports like `from funcs....` fail, ensure you run the notebook from the repository root so that `funcs` is on `PYTHONPATH`.
- If cartopy installation is problematic, run without map-based plots or install prebuilt binaries for your platform.
- If `.pos` fields appear empty or quality flags are unexpected, verify that the precise products, antenna models, and signals match your RINEX data and epoch span.

### Acknowledgements
- International GNSS Service (IGS) for precise orbit, clock, bias, and antenna products.
- The GNSS community and open data providers enabling reproducible PPP research and development.
