# FlexiCity – Helsinki Energy Peak Simulator

Built at **Urban Circular Hack Helsinki 2025**

---

## What is this?

Every evening around 7 pm, electricity in Helsinki gets expensive. People come home from work and all at the same time they charge their EVs, cook dinner, run the dishwasher, and heat their homes. This sudden rush pushes up electricity prices and strains the grid.

We used real price data from **Helen**, Helsinki's electricity provider, to show exactly when and why this happens — and built a simulator to show what would happen if people spread out their electricity use more evenly.

Recent research highlights how quickly demand is growing. For example, electricity consumption in Espoo increased 46% between January 2023 and January 2024, and projections suggest that peak power demand in the region could rise by up to 179% between 2020 and 2030—nearly tripling within a decade (Wanne, 2025).

---

## The Problem — The 7 pm Spike

Using Helen's hourly price data, we found that electricity nearly **triples in price** between the middle of the night and the evening peak:

| Time | Price (cents/kWh) |
|------|-------------------|
| 02:00 | 8.52 (cheapest) |
| 17:00 | 18.60 ↑ rising |
| 18:00 | 21.53 ↑ |
| **19:00** | **22.68 — daily peak** |
| 21:00 | 17.09 ↓ falling |

This spike happens because everyone gets home from work at the same time and does the same things at once — plugging in their EV, turning on the oven, switching on heating. If people (or smart devices) could shift some of that load to cheaper hours, everyone saves money and the grid is more stable.
## Features

| Feature | Description |
|---|---|
| 3 scenarios | Baseline, Winter weekday (stronger heating demand), 2030 future (2x EV penetration) |
| 24 flexible loads | City EV fleet, tram depot, hospitals, schools, data centres, residential EVs, and more |
| Before/After charts | Side-by-side 24h demand curves showing naive vs. smart scheduling |
| Real price data | Finnish day-ahead market prices (cents/kWh, hourly, 00–23h) |
| Real CO2 data | Hourly grid carbon intensity (tCO2/MWh), varying by renewable generation |
| Grid vs. Price slider | Shift optimisation priority between flattening the grid and minimising cost |
| Flex participation slider | Simulate partial adoption — what if only 40% of loads participate? |
| 4 optimisation modes | Min peak, Min CO2, Balanced (peak + CO2 + cost), Max peak reduction with positive cost and CO2 gains |
| Live stats panel | Peak before/after, cost saving (EUR/day), CO2 saving (tCO2/day and /year) |
| Editable load list | Add, edit, toggle, or delete any load at runtime |

---

## Quick Start

Requirements: Python 3.8 or higher. Tkinter is included in the Python standard library — no pip installs needed.

```bash
git clone https://github.com/KSou799/Urban-Circular-Hack-Helsinki-2025-Helsinki-Finland.git
cd Urban-Circular-Hack-Helsinki-2025-Helsinki-Finland
python flexicity.py
```

On some Linux distributions tkinter is a separate package:

```bash
sudo apt install python3-tk
```

---

## How the Scheduler Works

**Before (baseline):** Every load is scheduled naively. It starts at the first available hour in its allowed time window and runs for its full duration at full power.

**After (smart scheduling):** For each participating load, the algorithm:

1. Removes the load's power contribution from its naive hours
2. Scores every hour in its allowed window using a weighted combination of grid load at that hour (lower is better) and electricity price at that hour (lower is better)
3. Places variable loads in the individually cheapest or cleanest hours
4. Places fixed-block loads (laundry cycles, defrost cycles) in the cheapest or cleanest continuous block

The Grid/Price slider controls the weighting between the two signals. The Flex participation slider scales how much of each load shifts, simulating partial or phased adoption.

---
## Screenshot

![FlexiCity Screenshot](screenshot.png)

*Top graph:* peak demand  
*Bottom graph:* demand flattened via smart scheduling  

---

## Scenarios

| Scenario | Base Load | CO2 Profile | Notable Changes |
|---|---|---|---|
| Baseline | Typical weekday | Finnish average | Reference case |
| Winter weekday | Strong heating spikes at 05–09h and 16–22h | +25–45% evening intensity | Heat pumps x1.8, EVs x1.4, saunas x1.6 |
| 2030 future | +90% overall, large EV wave 16–21h | -35% average, -55% midday (solar) | EVs x2.2, batteries x1.9, general x1.4 |

---

## Design Decisions and Limitations

- **Units:** Loads are in kW. The base grid load is in MW-scale arbitrary units. The combined demand curve is a relative index suitable for comparison, not an absolute MW figure calibrated to the real Finnish grid.
- **Scheduler:** The optimiser is a greedy weighted heuristic, not a trained model. It is fast, transparent, and sufficient for interactive simulation but does not guarantee a global optimum.
- **Price data:** A single representative day of Finnish day-ahead prices. Live deployment would require Nord Pool API integration.
- **CO2 data:** Synthetic profile based on typical Finnish grid intensity patterns. Live data would come from Fingrid's open API.
- **Inter-asset coordination:** Each load is optimised sequentially, largest first. A production district energy management system would co-optimise all loads simultaneously.

---

## Context

Built in 24 hours at **Urban Circular Hack Helsinki 2025**, a hackathon focused on circular economy solutions for cities.

The Finnish energy context makes demand-response particularly relevant. High electrification rates, a large share of variable renewables, district heating transitioning to heat pumps, and rapid EV adoption combine to create increasingly volatile demand profiles. Shifting even a modest fraction of flexible loads reduces peak infrastructure requirements and unlocks value from cleaner overnight and midday generation.

---

## License

MIT


