---
name: geoskill-dinsar-coherence-analysis
description: '从配准主/从复SLC估计多视复相干系数γ与干涉相位，识别稳定散射体与去相关变化区（建筑变化/滑坡/植被），输出相干性与相位GeoTIFF及统计JSON。D-InSAR complex coherence and interferometric phase estimation with multi-looking.'
---

# D-InSAR 相干性分析 | D-InSAR Coherence Analysis

Estimates the **complex coherence** and **interferometric phase** from registered master / slave complex single-look images (SLC):

- **Complex coherence** γ = |Σ(m·conj(s))| / sqrt(Σ|m|²·Σ|s|²), estimated within a `--looks-r` × `--looks-a` multi-looking window. γ∈[0,1]: stable scatterers (buildings, exposed bedrock) have high coherence; changed areas (new construction / demolition, landslides, vegetation) decorrelate and show low coherence.
- **Interferometric phase** φ = angle(Σ m·conj(s)), reflecting line-of-sight deformation / topographic phase.

Multi-looking is implemented with a boxcar sliding-window mean; normalization in the numerator and denominator cancels out within the same window.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy
```

## Usage / 使用方法

### Basic usage (bbox only, master/slave SLC synthesized automatically)

```bash
python geoskill-dinsar-coherence-analysis.py --bbox 116.0 39.0 117.0 40.0 --output-dir ./out
```

### Example 1: synthetic data (offline)

```bash
python geoskill-dinsar-coherence-analysis.py --bbox 116 39 117 40 --synthetic --looks-r 5 --looks-a 1 --output-dir ./syn
```

### Example 2: real master/slave SLC

```bash
python geoskill-dinsar-coherence-analysis.py --input master_slc.tif --slave slave_slc.tif --looks-r 5 --looks-a 2 --output-dir ./real
```

### Example 3: larger multi-looking window (smoother coherence)

```bash
python geoskill-dinsar-coherence-analysis.py --bbox 116 39 117 40 --looks-r 8 --looks-a 4 --output-dir ./smooth --quiet
```

### Example 4: specify polarization and low-coherence threshold

```bash
python geoskill-dinsar-coherence-analysis.py --bbox 121 31 122 32 --polarization vh --coh-threshold 0.4 --output-dir ./sh --quiet
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `coherence.tif` | GeoTIFF (float32) | Complex coherence γ∈[0,1], EPSG:4326 |
| `phase.tif` | GeoTIFF (float32) | Interferometric phase φ∈(-π,π], EPSG:4326 |
| `coherence_statistics.json` | JSON | Mean / quantiles / low-coherence area, etc. |
| `output-manifest.json` | JSON | Run manifest |

## Data Source / 数据源 / Source

- **Real mode**: local registered master / slave complex SLC GeoTIFF (2 bands: real + imaginary parts).
- **Synthetic mode**: locally generated master/slave complex SLC (high correlation in stable areas + decorrelation from injected change patches).

## Privacy / 隐私声明 / Privacy

- Runs fully offline by default; `--synthetic` requires no network.
- All processing is done locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
