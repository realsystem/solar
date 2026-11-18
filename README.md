# solar

General notes and a simple static calculator for sizing small off-grid solar systems (RVs, vans, cabins).

- **[Calculator](index.html):** works out daily loads, how many days the battery lasts without sun, the panel watts you need, a cold-weather check of panel string voltage, and charge controller capacity. It's a single HTML file with no dependencies. Your inputs stay in your browser's local storage.
- **[Sizing guide](docs/sizing-guide.md):** the method, rules of thumb and sources behind the calculator.

## Run it

Open `index.html` in a browser. It's live at **https://overlandn.com/solar/**, deployed by `.github/workflows/deploy.yml` to GitHub Pages on every push to `main`. The repo's Pages source must be set to "GitHub Actions". It's linked as **SolarCalc** in the header of the main site ([realsystem.github.io](https://github.com/realsystem/realsystem.github.io)).

## Privacy

This repository is public. It holds only general information: no personal gear lists, locations or measurements. Settings you enter in the calculator are saved only in your own browser.

## Disclaimer

Estimates only. Measure your real loads, follow your equipment manuals and use proper overcurrent protection.
