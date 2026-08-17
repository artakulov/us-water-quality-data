# U.S. Water Quality & Home Safety Data

> Open dataset from [ZipCheckup](https://zipcheckup.com) | Artifact materialized 2026-08-17 | Source vintage varies by record and is not aggregated

## Coverage

- **42,679** ZIP-code records in the exported dataset
- **23,795** records with mapped water-system data
- **66** federal and state data sources (EPA SDWIS, ECHO, LCR, PFAS, FEMA, Census ACS, USGS, CDC, DOE, and more)
- **567K+** pages on zipcheckup.com

## Files

| File | Description |
|------|-------------|
| `zipcheckup-water-quality.csv.gz` | Main dataset, one row per ZIP code |
| `zipcheckup-water-quality.json.gz` | Same data in JSON format |
| `zipcheckup-metadata.json` | Field descriptions, data sources, methodology |
| `CHANGELOG.md` | Versioned artifact update log |

## Data Fields

Key fields per ZIP code:
- **Home Safety Score 4.0.0:** published only when all four required inputs are present; four inputs are grouped into three equally weighted domains
- **Violations:** total, health-based, monitoring/reporting
- **Lead:** typed measurement evidence where available
- **Copper:** action-level exceedance flag only; no concentration is published because the source does not provide a verified unit
- **PFAS:** detection status, levels where available
- **Radon:** EPA zone classification
- **Historical NFIP Claims:** claim records are not current FEMA property-map coverage
- **Demographics:** population served, median home value, housing age

## Available On

External mirrors have independent versions and may lag this local artifact. Verify the exact remote version and file hashes before use.

- **npm:** `npm install us-water-quality-data` ([package](https://www.npmjs.com/package/us-water-quality-data))
- **PyPI:** `pip install us-water-quality-data` ([package](https://pypi.org/project/us-water-quality-data/))
- **HuggingFace:** [datasets/artakulov/us-water-quality-data](https://huggingface.co/datasets/artakulov/us-water-quality-data)
- **Kaggle:** [artakulov/us-water-quality-data](https://www.kaggle.com/datasets/artakulov/us-water-quality-data)
- **GitHub:** [artakulov/us-water-quality-data](https://github.com/artakulov/us-water-quality-data)
- **API:** [zipcheckup.com/api/](https://zipcheckup.com/api/)

## License

CC BY 4.0, free to use with attribution to [ZipCheckup](https://zipcheckup.com).

## Citation

```
ZipCheckup. U.S. Water Quality & Home Safety Data.
Artifact materialized 2026-08-17. Source vintage varies by record and is not aggregated.
https://zipcheckup.com | https://github.com/artakulov/us-water-quality-data
```
