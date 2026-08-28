# Dataset Changelog

## 2026-08-28

**Coverage:** 42,679 ZIP codes | 66 federal/state data sources | 567K+ pages

Source vintage varies by record and is not aggregated. Source-specific cadences vary. Artifact materialization is not a source observation date.

Entries before the D218 integrity release are quarantined in Git history. They include superseded counts, absence-as-zero headlines, and an invalid copper-unit claim. The legacy ccr-enriched.csv companion file was retired on 2026-08-19 because ccr_copper_90th_ppb mixed mg/L and ppm source values under a ppb label, understating some values by 1000x. Use copper_action_level_exceedance in the current dataset; no unverified copper concentration is published. Do not reuse the retired artifact as current evidence.
