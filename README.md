# Border Spillover Risk in Armed Conflict

This project develops a geospatial risk-screening framework for identifying frontier zones exposed to armed spillover. It uses the Global Terrorism Index 2026 border-proximity trend as a starting point, and it's built on ACLED event data.

The main case is jihadist violence in West Africa, with a focus on Sahel-to-coastal spillover. A comparative case applies the same logic to hybrid armed violence on the Colombia-Venezuela border.

## Research Question

How do armed groups use borderlands to expand operational reach, and which indicators can help identify frontier zones at risk of spillover?

## Project Outputs

[See notebook on nbviwer](https://nbviewer.org/github/lisianealves/borderland-spillover-risk-framework/blob/main/notebooks/border_spillover_analysis.ipynb)

| Output | Description |
|:--|:--|
| Border Militancy Exposure Index | Identifies border dyads where militant violence is already concentrated, contributing to Coastal Spillover Risk Index v2 |
| Coastal Spillover Risk Index v2 | Ranks Sahel-to-coastal border segments by observed spillover indicators |
| Hybrid Border Violence Exposure Index | Comparative Colombia-Venezuela stress test by frontier zone |
| Static maps | Border bands, risk-ranked border segments, operational geography, and comparative border exposure |
| Methodology | Reproducible actor filters, border-distance logic, index formulas, and limitations |

## Main Finding

The West Africa results suggest that borderland risk is not evenly distributed across coastal exposure areas. The strongest spillover signal appears along the Benin-Niger frontier, followed by Burkina Faso-Togo and Benin-Burkina Faso. These segments combine recent violence, border proximity, coastal-side activity, and operational-geography indicators such as proximity to protected areas.

The Colombia-Venezuela comparison shows that a similar borderland framework can be useful outside jihadist violence, but the interpretation changes. There, the relevant phenomenon is hybrid armed governance: insurgency, criminal economies, armed groups, and state fragmentation operating through frontier zones.

## Coastal Spillover Risk Index v2

| Border dyad | Score | Risk level | Evidence confidence | Recent events | Within 25 km of protected area | Within 10 km of major road |
|:--|--:|:--|:--|--:|--:|--:|
| Benin - Niger | 0.847 | Severe | High | 108 | 99.1% | 99.1% |
| Burkina Faso - Togo | 0.544 | High | High | 273 | 28.8% | 97.9% |
| Benin - Burkina Faso | 0.423 | Moderate | High | 143 | 58.9% | 93.4% |
| Benin - Nigeria | 0.389 | Moderate | Low | 20 | 35.2% | 59.3% |
| Burkina Faso - Ivory Coast | 0.206 | Low | Medium | 83 | 2.4% | 84.2% |
| Burkina Faso - Ghana | 0.118 | Low | Medium | 78 | 11.7% | 94.8% |

## Colombia-Venezuela Comparative Index

| Frontier zone | Score | Risk level | Evidence confidence | Total events | Recent events | Actor families |
|:--|--:|:--|:--|--:|--:|--:|
| Colombia - Norte de Santander | 0.758 | Severe | High | 802 | 153 | 6 |
| Colombia - Arauca | 0.620 | High | High | 549 | 138 | 4 |
| Colombia - Boyaca | 0.444 | Moderate | Low | 10 | 4 | 1 |
| Colombia - La Guajira | 0.364 | Moderate | High | 57 | 30 | 5 |
| Colombia - Cesar | 0.321 | Moderate | High | 93 | 27 | 5 |
| Venezuela - Tachira | 0.317 | Moderate | Low | 105 | 3 | 7 |

## Data

Raw data is not included in this repository.

Required local inputs:

| File | Source |
|:--|:--|
| `data/raw/acled_west_africa_2018_2025.csv` | ACLED export for West Africa countries |
| `data/raw/acled_colombia_venezuela_2018_2025.csv` | ACLED export for Colombia and Venezuela |
| `data/raw/ne_10m_admin_0_countries/` | Natural Earth Admin 0 Countries |
| `data/raw/osm_geofabrik/*.gpkg.zip` | Geofabrik OpenStreetMap GeoPackage extracts |

Sources:

- [ACLED](https://acleddata.com/data/)
- [Global Terrorism Index 2026](https://www.economicsandpeace.org/report/global-terrorism-index-2026/)
- [Natural Earth Admin 0 Countries](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-0-countries/)
- [Geofabrik OpenStreetMap extracts](https://download.geofabrik.de/)

## Methods

The full methodology is documented in [docs/methodology.md](docs/methodology.md).

Short version:

1. Download broad ACLED event exports without actor filtering in the interface.
2. Apply reproducible actor-family filters in Python.
3. Build international land-border segments from Natural Earth country boundaries.
4. Calculate event distance to the nearest relevant border.
5. Add operational-geography proxies from OpenStreetMap: major roads and protected areas.
6. Normalize indicators with min-max scaling.
7. Build risk-screening scores by border dyad or frontier zone.
8. Map and compare the resulting exposure patterns.

## Reproduce

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the pipeline:

```bash
python3 scripts/build_acled_jihadist_events.py
python3 scripts/add_border_distance.py
python3 scripts/add_operational_geography_west_africa.py
python3 scripts/build_border_spillover_index_v1.py
python3 scripts/build_coastal_spillover_risk_index_v1.py
python3 scripts/build_coastal_spillover_risk_index_v2.py
python3 scripts/build_acled_colombia_venezuela_events.py
python3 scripts/add_colombia_venezuela_border_distance.py
python3 scripts/build_hybrid_border_violence_index_v1.py
python3 scripts/make_maps.py
```

Optional:

```bash
python3 scripts/download_geofabrik_gpkg.py --set west_africa_core
```

## Interpretation Notes

This is a screening tool, not a forecast. A high score means observed indicators align with a spillover risk logic, it does not estimate the probability of a future attack.

The Colombia-Venezuela case is not treated as terrorism. It is included to test whether the same borderland exposure logic helps interpret a different armed ecosystem shaped by insurgency, organized crime, illicit economies, and fragmented state control.
