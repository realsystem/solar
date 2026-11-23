# solar

- A public off-grid solar sizing calculator (SolarCalc), plus a general sizing guide. It's served at https://overlandn.com/solar/ for RV, truck-camper and overland users.
- It's a single static HTML page: no build step, no dependencies, no backend.

## Commands
- Run locally: `python3 -m http.server 8765`, then open http://localhost:8765/index.html. Opening the file directly (`file://`) also works.
- JS syntax check: `node -e "const h=require('fs').readFileSync('index.html','utf8');new Function(h.split('<script>')[1].split('</script>')[0]);console.log('ok')"`
- Workflow YAML check: `python3 -c "import yaml;yaml.safe_load(open('.github/workflows/deploy.yml'))"`
- Install, lint, typecheck, build and test: none exist. There's no package.json and no test suite.
- Deploy: push to `main`. `.github/workflows/deploy.yml` builds `_site/` and publishes it to GitHub Pages. It can also be run by hand (workflow_dispatch).

## Architecture
- `index.html`: the whole app. Inline `<style>`, markup, and one inline `<script>` IIFE.
  - `PRESETS`: the device list in the "Add device" dropdown.
  - `EXAMPLE`: default state.
  - `FIELDS`: ids of the numeric and select inputs.
  - `calc()`: computes every result and renders `#kpis`, `#stringOut` and `#ctrlOut`.
  - `safeSupportUrl()`: validates the Support button link.
- `docs/sizing-guide.md`: the method, rules of thumb and sources. It's linked from the page via its GitHub URL and is not deployed.
- `.github/workflows/deploy.yml`:
  - "Assemble site" copies only `index.html` and `.nojekyll` into `_site/`.
  - It then fills `<meta name="support-url" content="">` from `SUPPORT_URL`.
  - Then it uploads and deploys `_site/` to Pages.
- Data flow:
  - Inputs and load rows go into `state`, which `calc()` turns into the result tiles.
  - `state` is saved to `localStorage` under the key `offgrid-solar-calc-v1`, and can be exported/imported as JSON.
  - Nothing leaves the browser.
- The site header link lives in another repo: `realsystem/realsystem.github.io`, in `src/layouts/BaseLayout.astro` (`navItems`, `{ href: '/solar', label: 'SolarCalc' }`).

## Conventions
- **Public repo: general content only.** Never add personal gear lists, locations, camera or network setups, measurements, names or emails. Keep user-specific notes outside the repo.
- Keep the page as a single self-contained file. No frameworks, no build tools, no new external scripts. The Google Fonts `<link>` is the only external resource.
- Visual style matches the sibling Tires&Gears tool:
  - dark theme with CSS variables in `:root`, accent `#4eb56a`
  - the `.app-header` and "← Back to main page" link to https://overlandn.com
  - The Support button mirrors the Fuel tool.
- Battery math uses nominal pack voltages: 12.8, 25.6 or 51.2 V for 12, 24 or 48 V systems.
- Escape user text with `esc()` before inserting it into HTML. Wrap `localStorage` calls in try/catch.
- Commits end with the `Co-Authored-By` and `Claude-Session` trailers.
- Work on a feature branch and open a PR to `main`; merge only when the owner asks. This session can't open PRs (no GitHub CLI or API credentials), so share compare links instead.

## Gotchas
- **Deploy publishes only `index.html` and `.nojekyll`.** A new asset file must be added to the `cp` line in "Assemble site", or it won't go live.
- **Keep the support-url meta tag exactly as is:** `<meta name="support-url" content="">`, empty in source. The workflow asserts it exists and fills it.
- `SUPPORT_URL` is a repo secret or variable. Only `https://` links pass; a bare `domain.com/name` gets `https://` added. Without a valid link, the button stays disabled ("Coming soon"). Re-run the deploy after changing it.
- **Pages source must be "GitHub Actions"** (Settings → Pages). Otherwise `deploy-pages` fails with a 404.
- **Changing the shape of `state` breaks saved data.** `load()` returns saved data as is, so a new field would be undefined (NaN in results) for returning users. Merge with `EXAMPLE` or bump the `KEY` version.
- Network-restricted sandboxes can't fetch Google Fonts or overlandn.com. A font error in the console is expected there.

## Definition of done
- The JS syntax check and the workflow YAML check pass.
- A headless-browser load of `index.html` (e.g. Playwright) shows no page errors, renders `#kpis`, and gives correct results for any calc change.
- `index.html` and `docs/` contain no personal data (grep before committing).
- If the deploy or support logic changed, run the "Assemble site" script locally with and without `SUPPORT_URL` and check the meta tag.
