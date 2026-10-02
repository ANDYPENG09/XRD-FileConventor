# XRD-FileConventor

**[Open converter](https://andypeng09.github.io/xrd-toolkit/demos/fileconventor/)** · [All XRD tools](https://andypeng09.github.io/xrd-toolkit/)

A standalone, single-file, offline web tool for inter-converting powder X-ray
diffraction (XRD) data between the file formats used by **Rigaku**, **Bruker**
and **MDI Jade**, plus generic numeric text. All processing runs locally in
your browser — no server, no upload, no dependencies.

> **Made by:** PENG &nbsp;|&nbsp; **Built with:** VibeCoding &nbsp;|&nbsp; **License:** MIT

> [!IMPORTANT]
> **Scope.** This release deliberately ships **only the formats the author can validate.**
> The author works with **Rigaku** and **Bruker** diffractometers and analyses data in
> **MDI Jade**, so support is limited to those ecosystems plus generic text.
> Converters for other vendors were written but are **not shipped** — see
> [Why other formats are not included](#why-other-formats-are-not-included).

## Features

- **Zero dependencies** — a single `.html` file. Double-click to open in any modern browser.
- **Fully offline** — your data never leaves the machine.
- **Rigaku `.rasx` input** (SmartLab ZIP container) and **Bruker RAW ver.1 binary output**.
- **Jade MDI ASCII** read and write.
- **Plot preview** with drag-to-zoom and √Intensity view.
- **Wavelength conversion** between anodes (Cu/Co/Fe/Cr/Mo or custom) using the standard formula
  `2θ′ = 2·asin( (λ′/λ)·sin(2θ/2) )`.
- **Data-quality checks** based on the opXRD guidelines (arXiv:2503.05577v2):
  warns on negative angles / fewer than 50 points; rejects all-zero intensity / single unique angle.

## Supported formats

| Vendor / family | Read | Write |
|-----------------|------|-------|
| **Rigaku** | `.rasx` (SmartLab, ZIP) · `.ras` · `.asc` | `.asc` |
| **Bruker** | `.uxd` | `.raw` (ver.1, binary) · `.uxd` |
| **Jade / MDI** | `.mdi` (MDI ASCII) | `.mdi` (MDI ASCII) |
| **Generic** | `.xy` `.xye` `.csv` `.txt` `.dat` `.json` · any two-column numeric text | `XY` `XYE` `CSV` `JSON` · d-spacing table CSV |

> Binary inputs other than `.rasx` (e.g. Bruker `.raw`, `.brml`) cannot be read by this tool;
> export them to a text format from the vendor software first.

## Validation scope

| Status | Formats | Notes |
|--------|---------|-------|
| ✅ **Verified on real instrument files** | Rigaku `.rasx` `.ras` `.asc` · Bruker `.raw` (ver.1) `.uxd` | Round-trip checked, including UTF-16 decoding, attenuation-factor scaling and wavelength propagation for `.rasx` → `.raw` |
| ✅ **Verified by automated round-trip tests** | `.mdi` (Jade) · `.xy` `.xye` `.csv` `.txt` `.json` · d-spacing CSV | Written to spec and read back byte-for-byte; the `.mdi` writer follows the MDI Jade manual but has been exercised on a smaller real-file sample set than the Rigaku/Bruker paths |

**Recommendation:** always open the converted file in your own analysis software and compare the
2θ range, step size, point count and peak positions against the source before using the result for
analysis or publication.

## Why other formats are not included

Readers and writers for **PANalytical `.xrdml`**, **Sietronics `.cpi`**, **Philips `.udf`**,
**GSAS ESD/STD** and **Riet7 `.dat`** were implemented from published open-source specifications,
but the author has no access to those instruments and could not test the output against real
files. Rather than ship converters that might silently produce subtly wrong data, they have been
kept out of this release.

If you have sample files from any of these vendors, please **open an issue** — real-world test
data is the most useful contribution to this project, and the corresponding converter can be
re-enabled and validated quickly.

## Accuracy notes

- **`.rasx` import** correctly handles the UTF-16 LE encoding (BOM `0xFF 0xFE`) of `Profile*.txt`,
  applies the attenuation factor (`y = intensity × attenuation coefficient`), and reads the
  wavelength from `MesurementConditions*.xml` into the output header.
- **Bruker RAW ver.1** is written as a little-endian binary blob (`magic "RAW "`, `float32`
  intensities) whose structure was verified against the open-source **xylib** library
  (`bruker_raw.cpp`, `load_version1`).
- **MDI ASCII** follows the layout documented in the MDI Jade manual: line 1 date + sample name,
  line 2 `start step scanSpeed anode wavelength end nPoints`, then 8 intensities per line with no
  angle column. The 2θ axis is always rebuilt from `start + i·step`, so a malformed
  `end`/`nPoints` field cannot corrupt the data.
- Fixed-step formats (`RAW` / `ASC` / `MDI` / `UXD`) write the average step in the header when the
  source step is non-uniform; a warning is shown in that case.

## Technical notes

- `.rasx` (a ZIP container) is decompressed with the browser-native
  `DecompressionStream('deflate-raw')` — no external library required.
- The whole application is vanilla JavaScript/CSS/HTML; there is nothing to install or build.

## Acknowledgements & references

Format specifications and structural details were adapted from open-source projects and
published documentation:

- **xylib** (LGPL) — reference for the Bruker RAW ver.1 byte layout.
- **GSAS-II** (BSD-3-Clause) — reference for the Rigaku RASX container structure.
- **MDI Jade manual** — reference for the MDI ASCII pattern layout.
- **opXRD** (CC BY 4.0, arXiv:2503.05577v2) — data-quality filtering guidelines.

This project is an **independent re-implementation** written from scratch. It is not affiliated
with, endorsed by, or derived from any commercial converter or instrument-vendor software.

## License

Released under the [MIT License](./LICENSE). Copyright (c) 2026 PENG.
