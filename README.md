# XRD-FileConventor

A standalone, single-file, offline web tool for inter-converting powder X-ray
diffraction (XRD) data between common file formats. All processing runs
locally in your browser — no server, no upload, no dependencies.

> **Made by:** PENG &nbsp;|&nbsp; **Built with:** VibeCoding &nbsp;|&nbsp; **License:** MIT

## Features

- **Zero dependencies** — a single `.html` file. Double-click to open in any modern browser.
- **Fully offline** — your data never leaves the machine.
- **10+ text formats** inter-convertible, plus **Rigaku `.rasx` input** and **Bruker RAW ver.1 binary output**.
- **Plot preview** with drag-to-zoom and √Intensity view.
- **Wavelength conversion** between anodes (Cu/Co/Fe/Cr/Mo or custom) using the standard formula
  `2θ′ = 2·asin( (λ′/λ)·sin(2θ/2) )`.
- **Data-quality checks** based on the opXRD guidelines (arXiv:2503.05577v2):
  warns on negative angles / fewer than 50 points; rejects all-zero intensity / single unique angle.

## Supported formats

| Direction | Formats |
|-----------|---------|
| **Input** | `.xy` `.xye` `.csv` `.txt` `.dat` (Riet7) `.cpi` (Sietronics) `.udf` (Philips) `.uxd` (Bruker) `.asc` (Rigaku) `.ras` (Rigaku) `.rasx` (Rigaku SmartLab, ZIP) `.xrdml` (PANalytical) `.gsas`/`.gsa` (GSAS) `.mdi` (Jade) `.json` and generic two-column text |
| **Output** | `XY` `XYE` `CSV` `Riet7 DAT` `Sietronics CPI` `Philips UDF` `Bruker UXD` `Rigaku ASC` `GSAS ESD` `PANalytical XRDML` `Bruker RAW ver.1` `JSON` `d-spacing CSV` |

> Binary/compressed inputs other than `.rasx` (e.g. `.raw`, `.brml`) are not read by the web
> version; convert them with a desktop tool such as PowDLL first.

## Accuracy notes

- **`.rasx` import** correctly handles the UTF-16 LE encoding (BOM `0xFF 0xFE`) of `Profile*.txt`,
  applies the attenuation factor (`y = intensity × attenuation coefficient`), and reads the
  wavelength from `MesurementConditions*.xml` into the output header.
- **Bruker RAW ver.1** is written as a little-endian binary blob (`magic "RAW "`, `float32`
  intensities) whose structure was verified against the open-source **xylib** library
  (`bruker_raw.cpp`, `load_version1`).
- Fixed-step formats (DAT/CPI/UDF/ASC/GSAS/XRDML/RAW) write the average step in the header when
  the source step is non-uniform.

## Technical notes

- `.rasx` (a ZIP container) is decompressed with the browser-native
  `DecompressionStream('deflate-raw')` — no external library required.
- The whole application is vanilla JavaScript/CSS/HTML; there is nothing to install or build.

## Acknowledgements & references

Format specifications and structural details were adapted from open-source projects:

- **xylib** (LGPL) — reference for the Bruker RAW ver.1 byte layout.
- **GSAS-II** (BSD-3-Clause) — reference for PANalytical XRDML and related structures.
- **opXRD** (CC BY 4.0, arXiv:2503.05577v2) — data-quality filtering guidelines.

This project is an **independent re-implementation** written from scratch. It is not affiliated
with, endorsed by, or derived from the original PowDLL desktop application (Nikos Kourkoumelis)
or any other converter named above.

## License

Released under the [MIT License](./LICENSE). Copyright (c) 2026 PENG.
