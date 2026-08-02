# SEM + microscopic pore flow (PyVista kit)

Standalone kit for **SEM-informed pore morphology** and a **simplified microscopic flow** visualization in [PyVista](https://pyvista.org/).

The kit is released at: [Methacrylate_Monolith_Simulator_SEM_Microflow_kit_v0.1.0](https://github.com/Hunchback-19A/Methacrylate-Monolith-Simulator-v.donut/releases/tag/sem_microflow_kit_v0.1.0)

This is **not** the full Methacrylate Monolith Simulator desktop app. It is only the 3D pore-flow slice so others can run a demo and see the effect.

> **Status:** Slim extracts (`geometry.py`, `flow_viz.py`), flow/SEM modules, optional C++ sources, and `demo_sem_microflow.py` are in this folder. Add `samples/mesoporous_silica_SEM.tiff` before running the demo (see `samples/ATTRIBUTION.md`).

---

## What you can do

1. Load a grayscale SEM (or SEM-like) image (in .tif or .tiff format) 
2. Map dark pores onto a hollow-cylinder (annular) voxel grid  
3. Compute a simplified steady pressure-driven flow on pore voxels  
4. View the result in an interactive PyVista window  

**Flow directions**

| Mode | Meaning |
|------|--------|
| **Radial** | Drive from the outer wall toward the inner bore |
| **Axial** | Drive from the top toward the bottom |

Zoom out for a dense overview of flow through the bed; zoom in for clearer local paths along pores. Optional Inlet/Outlet markers can be toggled in the viewer when that UI is included.

---

## What this is not

- Not a full CFD solver (no turbulence model, no commercial FEM package)  
- Not a guarantee of experimental permeability or chromatography performance  
- Not your proprietary monolith SEM library — samples here are for trying the pipeline only  

Scientific inspiration for the alternating channel-width / pore-flow idea:

Jungreuthmayer, C., Steppert, P., Sekot, G., Zankel, A., Reingruber, H., Zanghellini, J., & Jungbauer, A. (2015). The 3D pore structure and fluid dynamics simulation of macroporous monoliths: High permeability due to alternating channel width. *Journal of Chromatography A, 1425*, 141–149. https://doi.org/10.1016/j.chroma.2015.11.026

---

## Requirements

- Python 3.10+ recommended (use the version you tested)
- See `requirements.txt` once added; typically:
  - `numpy`
  - `Pillow`
  - `pyvista`
  - `scipy` (if used by your pore/flow path)

**Optional (faster):** C++ extension `micro_flow_native` — see [Optional C++ accelerator](#optional-c-accelerator).

---

## Quick start (after modules are in place)

```bash
cd sem_microflow_kit
python -m venv .venv
# Windows:
.venv\Scripts\activate
pip install -r requirements.txt

python demo_sem_microflow.py --mode radial
# or
python demo_sem_microflow.py --mode axial
```

Use a sample under `samples/`, or pass your own image:

```bash
python demo_sem_microflow.py --sem path\to\your.tif --mode radial
```

Prefer clear **dark pores** on a lighter matrix for the SEM mapper.

---

## Sample images

Do **not** ship internal or confidential lab SEMs.

This kit is set up to use a **public** Wikimedia Commons SEM as the default test file:

- Commons original (JPG): [Mesoporous silica SEM.jpg](https://commons.wikimedia.org/wiki/File:Mesoporous_silica_SEM.jpg) (listed public domain — re-check on Commons before redistributing)
- Kit sample: `Mesoporous_silica_SEM.tiff` — **JPG converted to TIFF** (under `samples/` or the kit root; either capitalization is fine)

Full credit and conversion note: `samples/ATTRIBUTION.md`.

That image is silica nanoparticles, **not** methacrylate monolith morphology; it is only for trying the software pipeline.

---

## Optional C++ accelerator

Source lives under `native/micro_flow/`.

- Without it: demo uses the pure-Python backend (slower, still correct for visualization)  
- With it: pressure / streamline loops can run natively  

Typical Windows build (from kit root, after installing Build Tools + `pybind11`):

```bat
build_micro_flow_native.bat
```

Do not rely on committing a prebuilt `.pyd` — it is tied to Python version and OS.

---

## Repository layout (target)

```text
sem_microflow_kit/
  README.md                 ← this file
  CHECKLIST.md              ← what to copy before publishing
  requirements.txt
  demo_sem_microflow.py
  microscopic_flow.py
  sem_pore_generator.py
  geometry.py / flow_viz.py ← trimmed helpers (names may vary)
  build_micro_flow_native.bat
  native/micro_flow/
  samples/
    ATTRIBUTION.md
    …
```

See **`CHECKLIST.md`** for the full include/exclude list and smoke tests.

---

## Relation to the full simulator

The desktop app wraps this kind of pipeline in a Tk GUI, project files, recipes, and installers. This kit is meant for open sharing of the **PyVista + SEM + optional C++** portion only.

---

## License

- **Your code:** add a LICENSE file when you publish (e.g. MIT).  
- **Sample images:** follow each image’s own license / public-domain dedication and keep attribution.
