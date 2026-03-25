# US Water Quality Data by ZIP Code

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Updated Daily](https://img.shields.io/badge/updated-daily-brightgreen.svg)]()
[![ZIP Codes](https://img.shields.io/badge/ZIP%20codes-41K%2B-blue.svg)]()
[![CCR Systems](https://img.shields.io/badge/CCR%20systems-1%2C021-blue.svg)]()

Structured water quality data for 41,000+ U.S. ZIP codes, derived from 20 federal and state sources including EPA SDWIS, EPA ECHO, UCMR5, Consumer Confidence Reports (CCR), state MCL databases, CDC blood lead, FEMA flood claims, Census housing age, and USGS groundwater. Each record includes violation history, lead and copper sampling results, PFAS detections, radon zone classification, flood risk, and a composite Home Safety Score. The dataset is designed for researchers, developers, journalists, and data scientists who need machine-readable, ZIP-level water quality data without building their own EPA data pipeline. Also available on-chain via API3 Airnode oracle.

---

## Datasets

| File | Records | Size | Description |
|------|---------|------|-------------|
| [zipcheckup-water-quality.csv](zipcheckup-water-quality.csv) | 41,344 ZIPs | ~3.5 MB | Main dataset — violations, lead/copper, scores |
| [zipcheckup-water-quality.json](zipcheckup-water-quality.json) | 41,344 ZIPs | ~21 MB | Same as CSV in JSON array format |
| [ccr-enriched.csv](ccr-enriched.csv) | 10,011 ZIPs | ~1.5 MB | Consumer Confidence Report data, 1,021 systems |
| [state-mcl-crossref.json](state-mcl-crossref.json) | 201 comparisons | ~146 KB | State vs. federal MCL crossref, 22 violations |
| [anomalies.json](anomalies.json) | 11,908 anomalies | ~5.3 MB | Pattern anomalies across 9 types |
| [l3-metrics.csv](l3-metrics.csv) | 39,422 ZIPs | ~1.5 MB | L3 composite risk metrics |
| [zipcheckup-metadata.json](zipcheckup-metadata.json) | — | ~3 KB | Dataset summary and field documentation |

---

## Quick Start

### Download

```bash
# Main dataset (CSV)
curl -O https://zipcheckup.com/data/open/zipcheckup-water-quality.csv

# Main dataset (JSON)
curl -O https://zipcheckup.com/data/open/zipcheckup-water-quality.json

# CCR data
curl -O https://zipcheckup.com/data/open/ccr-enriched.csv

# Dataset metadata
curl https://zipcheckup.com/data/open/zipcheckup-metadata.json
```

---

## Main Dataset Fields

### zipcheckup-water-quality.csv / .json

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
| `ccr_contaminant_count` | integer | Contaminants in Consumer Confidence Report (null if no CCR) |
| `ccr_violation_count` | integer | Violations in Consumer Confidence Report |
| `enforcement_action_count` | integer | Total EPA/state enforcement actions |
| `enforcement_health_violations` | integer | Health-based enforcement violations |
| `has_active_issues` | boolean | Currently active enforcement or violation issues |
| `boil_water_advisories` | integer | Number of boil water advisories issued |

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

## Supplementary Datasets

### ccr-enriched.csv — Consumer Confidence Reports

CCR data parsed from 1,021 water system Consumer Confidence Reports, covering 10,011 ZIP codes.

| Field | Description |
|-------|-------------|
| `zip` | ZIP code |
| `ccr_available` | Whether a CCR was found for this ZIP's system |
| `ccr_year` | Report year |
| `ccr_system_name` | Water system name from CCR |
| `ccr_pwsid` | EPA Public Water System ID |
| `ccr_contaminant_count` | Number of contaminants in the CCR |
| `ccr_violations` | Violations reported in the CCR |
| `ccr_source_type` | Water source description from CCR |
| `ccr_lead_90th_ppb` | 90th percentile lead level in ppb |
| `ccr_copper_90th_ppb` | 90th percentile copper level in ppb |
| `ccr_top_contaminants` | Top contaminants with levels and units (* = violation) |

### state-mcl-crossref.json — State vs. Federal MCL Comparison

Cross-reference of measured contaminant levels against state-specific Maximum Contaminant Limits (MCLs) that are stricter than federal EPA limits. Covers CA, NJ, MA, NH, VT, CT, HI, and NM.

- **199 systems** with CCR data analyzed
- **201 comparisons** across contaminants and states
- **22 systems** fail their state MCL while passing the federal limit

Key fields per comparison: `pwsid`, `system_name`, `state`, `contaminant`, `measured`, `federal_mcl`, `state_mcl`, `unit`, `passes_federal`, `fails_state`, `zip_codes`, `notes`.

Notable findings:
- California enforces secondary manganese standard as a primary MCL — several large utilities exceed it
- New Jersey has a 1,4-dioxane MCL recommendation (0.33 ppb) not yet formally adopted
- Massachusetts and Vermont use composite PFAS standards (sum of multiple compounds)

### anomalies.json — Pattern Anomalies

Statistical and pattern anomalies detected across 29,218 scanned ZIP codes. 11,908 total anomalies across 9 types.

| Anomaly Type | Count | Description |
|-------------|-------|-------------|
| `enforcement-spike` | 5,662 | Sudden increase in enforcement actions |
| `score-contradiction` | 3,051 | Score inconsistent with underlying data signals |
| `pfas-cluster` | 1,207 | Adjacent ZIPs with PFAS exceedances suggesting shared source |
| `island-of-safety` | 544 | Clean ZIP surrounded by high-risk neighbors |
| `silent-danger` | 667 | High risk indicators but no official violations |
| `wealth-paradox` | 633 | High-income area with poor water quality |
| `school-zone-risk` | 133 | Elevated risk near schools |
| `double-burden` | 10 | Low income + poor water quality |
| `infrastructure-mismatch` | 1 | Infrastructure age inconsistent with compliance record |

Each anomaly includes: `type`, `zip`, `severity` (1–10), `headline`, `description`, `sources`, `interestingness` score.

### l3-metrics.csv — Composite Risk Metrics

Derived L3 metrics for 39,422 ZIP codes combining EPA data, FEMA flood data, Census income data, and BLS utility rates.

| Field | Description |
|-------|-------------|
| `zip` | ZIP code |
| `lead_exposure_probability` | Estimated lead exposure probability 0–100 |
| `lead_risk_label` | Low / Moderate / High / Critical |
| `maintenance_debt_usd` | Estimated deferred infrastructure maintenance cost ($) |
| `compliance_risk_score` | Likelihood of future violations 0–100 |
| `compliance_risk_label` | Low / Moderate / High / Critical |
| `energy_burden_pct` | Estimated % of household income spent on energy |
| `energy_burden_label` | Low / Moderate / High / Severe |
| `flood_annual_cost_usd` | Expected annual flood damage cost ($) |

---

## Coverage

- **ZIP codes (main):** 41,344 (updated daily as new data is processed)
- **ZIP codes (L3 metrics):** 39,422
- **ZIP codes (CCR enriched):** 10,011
- **States:** All 50 U.S. states + D.C.
- **Water systems (CCR):** 1,021 parsed reports
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
- **Investigative journalism** — identify ZIP codes with persistent unresolved violations or elevated lead levels, PFAS clusters, or silent dangers
- **Environmental advocacy** — map water quality disparities across income or racial demographics
- **Home buying tools** — give prospective buyers a water quality signal before closing
- **Public health dashboards** — embed ZipCheckup data alongside other environmental indicators
- **Regulatory research** — compare state MCL enforcement against federal standards

---

## Data Sources & Methodology

**Primary sources:**
- [EPA Safe Drinking Water Information System (SDWIS)](https://www.epa.gov/enviro/sdwis-overview)
- [EPA ECHO enforcement database](https://echo.epa.gov/)
- Consumer Confidence Reports (CCR) — utility-submitted annual reports
- State MCL databases: CA, NJ, MA, NH, VT, CT, HI, NM

**ZIP-to-system matching:** EPA GEOGRAPHIC_AREA records and the `zipcodes` npm package. Some ZIPs are served by multiple systems; the record reflects the primary system by population served.

**Home Safety Score** is a composite 0–100 score that penalizes health-based violations, unresolved violations, lead exceedances, and contaminant count. A grade of A–F is assigned from the score. Full methodology: [zipcheckup.com/about/home-safety-score/](https://zipcheckup.com/about/home-safety-score/)

**CCR pipeline:** Consumer Confidence Reports are fetched from EPA's CCR website, parsed for contaminant tables, cross-referenced against SDWIS PWSID records, and matched to ZIP codes via the GEOGRAPHIC_AREA table.

**Anomaly detection:** Rule-based + threshold detection over the full SDWIS + ECHO + PFAS + Census dataset. Each anomaly is scored for severity (1–10) and interestingness.

**L3 metrics:** Derived from SDWIS + FEMA National Flood Insurance Program data + Census ACS income data + BLS regional utility rate data.

**Limitations:**
- ZIP-to-water-system matching is approximate; some ZIPs are served by multiple systems
- Lead/copper levels reflect 90th percentile tap sampling, not point-of-use measurements
- Radon is county-level, not measured at the address level
- CCR data reflects utility-reported levels, which may lag actual conditions
- State MCL crossref covers only states with confirmed stricter-than-federal limits
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
  note      = {Data sourced from EPA SDWIS, EPA ECHO, Consumer Confidence Reports, and state MCL databases. Updated daily.},
  license   = {CC BY 4.0}
}
```

**Plain text:**

> Akulov, A. (2026). *US Water Quality Data by ZIP Code* [Dataset]. ZipCheckup. https://github.com/artakulov/us-water-quality-data. Data sourced from EPA Safe Drinking Water Information System (SDWIS), EPA ECHO, Consumer Confidence Reports, and state MCL databases. License: CC BY 4.0.

---

## License

Data is released under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt the data for any purpose, including commercial use, provided you give appropriate credit:

> Data by [ZipCheckup.com](https://zipcheckup.com) — sourced from EPA SDWIS, EPA ECHO, and Consumer Confidence Reports. License: CC BY 4.0.

---

## Related

- **Live site:** [zipcheckup.com](https://zipcheckup.com) — free water quality reports for 41,000+ ZIP codes
- **Developer Hub:** [zipcheckup.com/developers/](https://zipcheckup.com/developers/) — API docs, embed widget, Airnode oracle
- **State Deep Dives:** [zipcheckup.com/states/](https://zipcheckup.com/states/) — 51 in-depth state reports
- **Methodology:** [zipcheckup.com/about/home-safety-score/](https://zipcheckup.com/about/home-safety-score/)
- **Open data page:** [zipcheckup.com/data/](https://zipcheckup.com/data/)
- **Blockchain oracle:** API3 Airnode on Ethereum Sepolia — smart contracts can query water quality scores on-chain
- **npm CLI:** [`zipcheckup`](https://www.npmjs.com/package/zipcheckup) — check any ZIP from the terminal

---

Maintained by [Artem Akulov](https://github.com/artakulov) — [ZipCheckup.com](https://zipcheckup.com)
