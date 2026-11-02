# D-InSAR Coherence Analysis (geoskill-dinsar-coherence-analysis)

> Estimate multi-look complex coherence and interferometric phase from registered master/slave complex SLC images, identifying stable scatterers and decorrelated change areas (building changes / landslides / vegetation), and outputting coherence and phase GeoTIFFs plus statistics JSON.

---

## 1. Overview

Estimate the **complex coherence coefficient** and **interferometric phase** from registered master/slave complex single-look images (SLC): - **Complex coherence** γ = |Σ(m·conj(s))| / sqrt(Σ|m|²·Σ|s|²), estimated within a `--looks-r` × `--looks-a` multi-look window. γ∈[0,1]: stable scatterers (buildings, exposed bedrock) show high coherence; changed areas (new construction / demolition, landslide bodies, vegetation) decorrelate and show low coherence. - **Interferometric phase** φ = angle(Σ m·conj(s)), reflecting line-of-sight deformation / topographic phase. Multi-looking is implemented with a boxcar sliding-window mean, where the normalization between numerator and denominator within the same window cancels out.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-dinsar-coherence-analysis.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `coherence.tif` | GeoTIFF (float32) | Complex coherence γ∈[0,1], EPSG:4326 |
| `phase.tif` | GeoTIFF (float32) | Interferometric phase φ∈(-π,π], EPSG:4326 |
| `coherence_statistics.json` | JSON | Mean / quantiles / low-coherence area, etc. |
| `output-manifest.json` | JSON | Run manifest |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
