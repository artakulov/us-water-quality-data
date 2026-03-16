# US Water Quality Data by ZIP Code

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Updated Daily](https://img.shields.io/badge/updated-daily-brightgreen.svg)]()
[![ZIP Codes](https://img.shields.io/badge/ZIP%20codes-3%2C500%2B-blue.svg)]()

Structured water quality data for 3,500+ U.S. ZIP codes, derived from the EPA Safe Drinking Water Information System (SDWIS). Each record includes violation history, lead and copper sampling results, radon zone classification, water source type, and a composite Home Safety Score. The dataset is designed for researchers, developers, journalists, and data scientists who need machine-readable, ZIP-level water quality data without building their own EPA data pipeline.

---

## Quick Start

### Download

| Format | File | Size |
|--------|------|------|
| CSV | [zipcheckup-water-quality.csv](https://zipcheckup.com/data/open/zipcheckup-water-quality.csv) | ~1 MB |
| JSON (array) | [zipcheckup-water-quality.json](https://zipcheckup.com/data/open/zipcheckup-water-quality.json) | ~2 MB |
| Metadata | [zipcheckup-metadata.json](https://zipcheckup.com/data/open/zipcheckup-metadata.json) | < 5 KB |

### curl examples

```bash
# Download CSV
curl -O https://zipcheckup.com/data/open/zipcheckup-water-quality.csv

# Download JSON
curl -O https://zipcheckup.com/data/open/zipcheckup-water-quality.json

# Dataset metadata
curl https://zipcheckup.com/data/open/zipcheckup-metadata.json
```

---

## Data Format

### CSV / JSON fields

| Field | Type | Description |
|-------|------|-------------|
| `zip` | string | 5-digit U.S. ZIP code |
| `city` | string | City name |
| `state` | string | 2-letter state abbreviation |
| `latitude` | float | ZIP centroid latitude |
| `longitude` | float | ZIP centroid longitude |
| `home_safety_score` | integer | Composite score 0–100 (null if insufficient data) |
| `home_safety_grade` | string | Letter grade: A / B / C / D / F |
| `total_violations` | integer | Total violations in the past 5 years |
| `health_violations` | integer | Health-based violations in the past 5 years |
| `unresolved_violations` | integer | Currently unresolved violations |
| `contaminant_count` | integer | Number of distinct health-based contaminants detected |
| `health_contaminant_names` | string | Semicolon-separated contaminant names |
| `lead_level_mg_l` | float | 90th percentile lead level in mg/L (null if no data) |
| `copper_level_mg_l` | float | 90th percentile copper level in mg/L (null if no data) |
| `radon_zone` | integer | EPA radon zone: 1 = highest risk, 3 = lowest risk |
| `water_source` | string | `SW` = Surface Water, `GW` = Groundwater |
| `system_name` | string | Primary water system name |
| `pwsid` | string | EPA Public Water System ID |
| `population` | integer | Population served by the primary system |

### Sample record (JSON)

```json
{
  "zip": "10001",
  "city": "New York",
  "state": "NY",
  "system_name": "NEW YORK CITY SYSTEM",
  "pwsid": "NY7003493",
  "population": 8271000,
  "water_source": "SW",
  "total_violations": 7,
  "health_violations": 0,
  "unresolved_violations": 0,
  "lead_level_mg_l": 0.01,
  "copper_level_mg_l": null,
  "radon_zone": 1,
  "home_safety_score": 36,
  "home_safety_grade": "F",
  "latitude": 40.7484,
  "longitude": -73.9967,
  "contaminant_count": 1,
  "health_contaminant_names": "Lead"
}
```

---

## Coverage

- **ZIP codes:** 3,500+ (updated daily as new data is processed)
- **States:** All 50 U.S. states + D.C.
- **Violation window:** Past 5 years (rolling)
- **Lead/copper data:** Where EPA LCR sampling records are available
- **Radon data:** County-level EPA radon zones (all covered ZIPs)
- **Update frequency:** Daily — data is re-fetched from EPA SDWIS each night

Coverage prioritizes ZIP codes by population. High-population areas have full data; rural and low-population ZIPs are added continuously.

---

## API Access

Single-ZIP JSON records are available via the ZipCheckup public API:

```bash
# Get data for a specific ZIP code
curl https://api.zipcheckup.com/v1/zip/90210

# Score only (lightweight)
curl https://api.zipcheckup.com/v1/zip/90210/score

# Rankings — best and worst ZIPs
curl "https://api.zipcheckup.com/v1/rankings?limit=10&order=asc"
```

Full API documentation: [api.zipcheckup.com/v1/](https://api.zipcheckup.com/v1/)

The per-ZIP API response includes additional fields not present in the flat dataset: full violation history, multiple water systems serving the ZIP, and recent violation details.

---

## Use Cases

- **Real estate applications** — surface water quality risk alongside walk scores and school ratings
- **Health research** — correlate ZIP-level water quality with health outcomes, demographics, or income data
- **Investigative journalism** — identify ZIP codes with persistent unresolved violations or elevated lead levels
- **Environmental advocacy** — map water quality disparities across income or racial demographics
- **Home buying tools** — give prospective buyers a water quality signal before closing
- **Public health dashboards** — embed ZipCheckup data alongside other environmental indicators

---

## Data Source & Methodology

**Primary source:** [EPA Safe Drinking Water Information System (SDWIS)](https://www.epa.gov/enviro/sdwis-overview)

The pipeline queries the EPA SDWIS public API to retrieve water system violations and contaminant data for each state. ZIP codes are matched to water systems via EPA GEOGRAPHIC_AREA records and the `zipcodes` npm package. Lead and copper levels come from EPA Lead and Copper Rule (LCR) sampling results. Radon zone data is sourced from EPA county-level radon zone maps.

**Home Safety Score** is a composite 0–100 score that penalizes health-based violations, unresolved violations, lead exceedances, and contaminant count. A grade of A–F is assigned from the score. Full methodology: [zipcheckup.com/about/home-safety-score/](https://zipcheckup.com/about/home-safety-score/)

**Limitations:**
- ZIP-to-water-system matching is approximate; some ZIPs are served by multiple systems
- Lead/copper levels reflect 90th percentile tap sampling, not point-of-use measurements
- Radon is county-level, not measured at the address level
- Data lags EPA SDWIS by up to 24 hours

---

## Citation

**BibTeX:**

```bibtex
@dataset{zipcheckup_water_quality_2026,
  author    = {Akulov, Artem},
  title     = {{US Water Quality Data by ZIP Code}},
  year      = {2026},
  publisher = {ZipCheckup},
  url       = {https://github.com/artakulov/us-water-quality-data},
  note      = {Data sourced from EPA SDWIS. Updated daily.},
  license   = {CC BY 4.0}
}
```

**Plain text:**

> Akulov, A. (2026). *US Water Quality Data by ZIP Code* [Dataset]. ZipCheckup. https://github.com/artakulov/us-water-quality-data. Data sourced from EPA Safe Drinking Water Information System (SDWIS). License: CC BY 4.0.

---

## License

Data is released under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt the data for any purpose, including commercial use, provided you give appropriate credit:

> Data by [ZipCheckup.com](https://zipcheckup.com) — sourced from EPA SDWIS. License: CC BY 4.0.

---

## Related

- **Live site:** [zipcheckup.com](https://zipcheckup.com) — free water quality reports by ZIP code
- **Methodology:** [zipcheckup.com/about/home-safety-score/](https://zipcheckup.com/about/home-safety-score/)
- **Open data page:** [zipcheckup.com/data/](https://zipcheckup.com/data/)
- **Contaminants guide:** [zipcheckup.com/contaminants/](https://zipcheckup.com/contaminants/)

---

Maintained by [Artem Akulov](https://github.com/artakulov) — [ZipCheckup.com](https://zipcheckup.com)
