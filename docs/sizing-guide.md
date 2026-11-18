# Small off-grid solar: sizing guide

This guide is for small off-grid systems: vans, truck campers, travel trailers and cabins. All figures are rules of thumb, so measure your own loads before buying anything. Prices and market notes are as of November 2026.

## 1. Size for the worst month

1. **Load audit.** For each device, multiply watts by hours per day, then add them up to get Wh/day. Measure with a battery monitor or a plug-in watt meter. Guessing is the most common mistake.
2. **Worst-month peak sun hours (PSH).** Look up every month for your site in [PVGIS](https://re.jrc.ec.europa.eu/pvg_tools/en/) or [NREL PVWatts](https://pvwatts.nrel.gov/), and use the worst month you'll actually camp in.
3. **Array:** `W_array = Wh_day / (PSH_worst × 0.75)`
4. **Battery:** `Wh_battery = Wh_day × days_of_autonomy / (usable_fraction × 0.95)`. LiFePO4 has a usable fraction of about 0.8. Plan for 2–3 days of autonomy, or 2 days plus a generator.
5. **Inverter:** add up the continuous watts of everything that runs at the same time, plus headroom for motor start surges.

Location changes the array size more than any other choice. Here is the same 2 kWh/day load in December (PSH = peak sun hours):

| Location | Worst-month PSH | Panels needed |
|---|---|---|
| Phoenix, AZ | 4.75 | ~560 W |
| Denver, CO | 3.71 | ~720 W |
| Seattle, WA | 1.32 | ~2,020 W |

The PSH values come from NREL PVWatts v8 at a 20° tilt.

## 2. Batteries

| | AGM lead-acid | LiFePO4 |
|---|---|---|
| Cycle life | 400–800 | 3,000–6,000 |
| Usable depth of discharge | ~50% | 80–100% |
| Round-trip efficiency | 80–85% | 92–98% |
| Weight | ~30 kg/kWh | ~7 kg/kWh |

- LiFePO4 costs 2–3× more upfront but about 5× less per kWh cycled.
- **Never charge LiFePO4 below 0 °C** unless the battery self-heats. Charging it cold causes lithium plating, which is permanent damage. Discharging works down to about −20 °C.

## 3. Charge controllers

- **MPPT beats PWM** for anything above a few hundred watts, especially in cold or low light, where the gain is 10–40%.
- **Check the string voltage in the cold.** A panel's open-circuit voltage (Voc) rises as it gets colder. Use `Voc_cold = Voc_STC × (1 + coef × (T_min − 25))`, where coef is about −0.0028/°C for typical panels; your panel's datasheet gives the exact value. The result must stay below the controller's maximum PV voltage, with some margin. Exceeding it can destroy the controller.
  - Example: four 12 V-class panels (~22 V Voc each) total ~88 V at 25 °C and ~98 V at −15 °C. That's fine on a 150 V controller and too close on a 100 V one.
- **Controller output:** at 12 V, a controller can use about its amp rating × ~13 V of panels. A 40 A controller handles ~520 W and a 70 A controller ~1,000 W. Above that, the controller clips (wastes the extra) at midday.

## 4. System voltage and protection

| System voltage | Practical inverter ceiling | Typical use |
|---|---|---|
| 12 V | ~1.5–2 kW | Van, truck camper, trailer |
| 24 V | ~2–4 kW | Small cabin, large RV |
| 48 V | 4 kW+ | Cabin, tiny home |

- **Main fuse:** put it on the battery positive, sized at ~1.25 × continuous current. Its interrupt rating should be at least the battery's Ah × 50. Class T fuses work for any bank; MRBF fuses for 12 V batteries up to 200 Ah. MEGA fuses are too weak to sit directly on a LiFePO4 battery.
- **Parallel banks:** fuse each battery separately.
- **NEC Article 706** applies to battery storage over 1 kWh.

## 5. RV specifics

- **Charging while driving:** use a DC-DC charger (20–50 A). A trailer's 7-pin connector gives only a trickle charge (~3–8 A). For full-rate charging, run a dedicated heavy cable from the truck battery.
- **Small roofs** (truck campers, vans): combine roof panels with portable panels.
- **Air conditioning:** a 13,500 BTU rooftop unit needs a soft starter. Even then, 400 Ah of LiFePO4 gives only about 2–3 hours of cooling, so plan on a generator for AC.
- **Fridge type is the biggest lever.** A residential 120 V fridge uses about 80–120 Ah/day, plus inverter idle. A 12 V compressor fridge uses ~30–45 Ah/day. A propane fridge uses almost nothing from the battery.

## 6. Always-on loads add up

A device that runs 24 hours a day costs `watts × 24 / 12.8` Ah per day at 12 V.

| Device | Typical draw | Ah/day at 12 V |
|---|---|---|
| Inverter idle (large all-in-one) | 30–80 W | 56–150 |
| Inverter idle (small, e.g. 500 VA) | ~6 W | ~11 |
| Laptop used as a server | ~30 W | ~56 |
| Starlink Mini | 25–40 W average, ~15 W idle | ~47–75 |
| Intel N100 mini PC (Linux) | ~6 W idle, 7–10 W light load | ~11–19 |
| Wi-Fi router | 8–15 W | ~15–28 |
| PoE camera | 2–5 W (IR on at night), up to ~12 W | ~4–9 each |
| Raspberry Pi 5 | ~3–8 W | ~6–15 |

- **Power these from 12 V DC, not through the inverter.** The inverter adds its idle draw plus ~10% conversion loss.
- **A small, dedicated low-power box beats a repurposed laptop.** For example, a camera recorder (NVR) on an N100 mini PC running Frigate with OpenVINO uses about a third of a laptop's power.

## 7. US market notes (Nov 2026)

- Section 232 sets a minimum price for imported panels ($0.38/W plus a 15% duty) starting Dec 4, 2026.
- The 30% residential solar tax credit (25D) expired on Dec 31, 2025.

## Sources

- [SurgePV: off-grid sizing guide](https://www.surgepv.com/blog/off-grid-solar-system-sizing-calculator-guide)
- [Voltflow: lithium vs lead-acid](https://www.voltflow.net/blog/lithium-battery-vs-lead-acid-solar-2026)
- [Victron: PWM or MPPT](https://www.victronenergy.com/blog/2014/07/21/which-solar-charge-controller-pwm-or-mppt/)
- [Victron SmartSolar 150/70 specs](https://www.victronenergy.com/media/pg/Manual_SmartSolar_MPPT_150-70_up_to_250-100_VE.Can/en/technical-specifications.html)
- [Renogy Rover 40A specs](https://www.renogy.com/collections/all-products/products/rover-li-40-amp-mppt-solar-charge-controller)
- [Tiny Build Electrics: LiFePO4 fuse selection](https://tinybuildelectrics.com/?p=12797)
- [Mike Holt: NEC 706.1](https://www.mikeholt.com/files/PDF/20_SOLAR_706.1.pdf)
- [GreenWatt: peak sun hours by state](https://www.thegreenwatt.com/average-peak-sun-hours-by-state/)
- [RMI: batteries and backup generators](https://rmi.org/?p=19350)
- [Solar Power World: winter prep](https://www.solarpowerworldonline.com/2015/09/getting-your-off-grid-solar-system-ready-for-winter/)
- [SolarReviews: 2026 tariffs](https://www.solarreviews.com/blog/new-tariffs-to-hike-us-solar-panel-costs)
- [Starlink Mini spec sheet](https://www.networkinnovations.com/hubfs/docs/product/starlink/NI-Starlink-Mini.pdf)
- [Reolink PoE power consumption](https://support.reolink.com/hc/en-us/articles/900000593323)
- [MiniPCLab: mini PC power data](https://minipclab.com/blog/mini-pc-power-consumption-guide)
- [Frigate recommended hardware](https://docs.frigate.video/frigate/hardware)
- [Micro-Air: batteries for RV AC](https://www.microair.net/blogs/news/how-many-batteries-do-you-need-to-run-an-rv-ac-overnight)
