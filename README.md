# Broiler PV Solar Fraction — WEA 2026

Supplementary code and parameters for the paper:

> **Solar Fraction Modeling for Rural Broiler Farms via Synthetic Demand, K-Means Clustering, and Harmonic Approximation**  
> Luis Velasquez, Martha P. Camargo M. et al.  


---

## Repository structure

```
broiler-pv-solar-fraction/
├── README.md
├── notebooks/
│   └── Simukation.ipynb   ←notebook
    └── Energy_Generation.ipynb   ←notebook
└── parameters/
    ├── equipment_counts.csv              ← N_h, N_l, N_f, N_p per scenario
    ├── model_parameters.csv              ← all fixed model constants
    └── pv_sizing_results.csv             ← PV layout and SF results
```

---

## Equipment counts per scenario

Derived from floor area and sizing rules (see Section 2.1 of the paper).

| Scenario | N_birds | A_floor [m²] | A_roof [m²] | N_h | N_l | N_f | N_p |
|----------|---------|--------------|-------------|-----|-----|-----|-----|
| micro    | 2,500   | 202.5        | 263.25      | 3   | 3   | 3   | 1   |
| small    | 8,000   | 648.0        | 842.40      | 8   | 9   | 8   | 1   |
| large    | 15,000  | 1,215.0      | 1,579.50    | 15  | 17  | 15  | 2   |

**Sizing rules:**
- `N_h = ceil(N_birds / 1000)` — 1 Master BV 77 heater per brooding circle (~1000 birds)
- `N_l = ceil(40 lux × A_floor / 3000 lm)` — dimensioned for worst-case week-1 illuminance
- `N_f = ceil(N_birds / 1000)` — 1 Vostermans V4E30K fan per 1000 birds
- `N_p = ceil(N_birds / 8000)` — 1 Pearl PEP-07 pump per 8000 birds

---

## Key model parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| Area per 1000 birds | 81 m² | Floor area reference |
| Building height | 2.5 m | Mean height |
| Production cycle | 42 + 5 days | Grow-out + sanitary downtime |
| Heater thermal output | 20 kW_th | Master BV 77 nominal |
| Heater aux. electricity | 0.30 kW | Per unit |
| Heater fuel rate | 2.0 L/h | At full load |
| Fuel LHV | 10 kWh/L | Diesel lower heating value |
| Lamp power | 25 W | Celer avian luminaire |
| Lamp flux | 3000 lm | Per unit |
| Fan power | 100 W | Vostermans V4E30K |
| Fan airflow | 2250 m³/h | At 0 Pa |
| Pump power | 0.50 kW | Pearl PEP-07 |
| Pump schedule | 06:00, 12:00, 18:00 | 20 min per cycle |
| Thermal deadband δ | 0.5 °C | Heating control |
| ΔT_ref | 10 °C | Full-load reference gap |
| Lighting week 1 | 23 h/day, 40 lux | Schedule |
| Lighting after week 1 | 16 h/day, 8 lux | Schedule |

---

## Climatic data

11-year hourly series (2013–2023) from [NASA POWER](https://power.larc.nasa.gov/):
- Variable: `ALLSKY_SFC_SW_DWN` (all-sky surface shortwave downward irradiance)
- Variable: `T2M` (2-m air temperature)
- Location: lat 5.172323, lon −73.632071 (Sabana Centro, Cundinamarca, Colombia)
- Local time: America/Bogota (UTC−5)

---

## PV sizing results

| Scenario | Orientation | Layout | Modules | PV [kWp] | Module area [m²] | SF [%] |
|----------|-------------|--------|---------|----------|-----------------|--------|
| micro    | Portrait    | 8×6    | 48      | 25.92    | 124.05          | 85.46  |
| small    | Landscape   | 27×6   | 162     | 87.48    | 418.67          | 102.53 |
| large    | Portrait    | 20×16  | 320     | 172.80   | 827.00          | 102.53 |

PV module: Eco Green Energy ATLAS 540 W (2279×1134 mm, η = 21.1% STC)  
Tilt: β = 10°, min solar elevation: α = 25°, inter-row clearance: g = 0.30 m

---

## How to run

1. Open `notebooks/Simulation.ipynb` in Google Colab or Jupyter
2. The notebook fetches NASA POWER data via API — internet connection required
3. Run cells sequentially (Bloque 1 → Bloque 18)

**Dependencies:** `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `scipy`, `requests`

---

## Citation

If you use this code or data, please cite

---

## License

MIT License — see `LICENSE` for details.
