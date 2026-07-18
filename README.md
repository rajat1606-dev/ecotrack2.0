# EcoTrack Pro — Carbon Footprint Calculator

A single-page, client-side web application that estimates a user's annual CO₂ emissions across six lifestyle categories and generates a personalised, actionable reduction plan. Built as a static, dependency-free front-end — no backend, no build step, no login.

**Live demo:** open `index.html` in any modern browser.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | Semantic HTML5 (single file) |
| Styling | Vanilla CSS3 — custom properties (`:root` variables), CSS Grid & Flexbox, keyframe animations |
| Logic | Vanilla JavaScript (ES6+), no frameworks |
| Fonts | Google Fonts — `DM Serif Display`, `DM Sans` (CDN) |
| Icons | Font Awesome 6.4.0 (CDN) |
| Persistence | Browser `localStorage` (Save / Load) |
| Build tooling | None required — zero-install, ships as one `.html` file |

There is currently one file in the repo: `index.html` (~1,985 lines / ~77 KB), containing all markup, styles, and script inline.

---

## Architecture

The app is a **multi-step wizard** driving a single calculation engine, structured as:

```
index.html
├── <style>            → Design system (CSS variables, components, responsive rules, print styles)
├── <body>
│   ├── .topnav         → Logo + branding
│   ├── .hero            → Landing copy + global benchmark stats (world avg / US avg / Paris target)
│   ├── .qbar             → Quick actions: Save, Load, Share, Eco Tip, Reset
│   ├── .step-track        → 6-step progress indicator (clickable, stateful)
│   ├── .card #calcCard     → Form wizard container
│   │    ├── #sec-0 Location & Household
│   │    ├── #sec-1 Transportation
│   │    ├── #sec-2 Home (energy)
│   │    ├── #sec-3 Diet
│   │    ├── #sec-4 Lifestyle
│   │    └── #sec-5 Digital footprint
│   └── #resultsCard       → Results view (hidden until calculation runs)
│        ├── .result-hero        → Big emissions number + verdict badge
│        ├── .result-scores      → Score cards (summary metrics)
│        ├── .breakdown-list     → Per-category emissions bars
│        ├── .rec-grid           → Personalised recommendation cards (difficulty-tagged)
│        ├── .timeline           → Projected reduction timeline
│        ├── .comparison-grid    → Comparison vs. national/global/Paris-target averages
│        └── .result-actions     → Export/share actions
└── <script>            → State management, validation, emission calculations, DOM rendering
```

### Design system
All colors, spacing, radii, and transitions are defined once as CSS custom properties in `:root` (e.g. `--green`, `--surface`, `--radius`, `--transition`), making the dark, eco-themed UI easy to re-skin. Layout is fully responsive via CSS Grid with breakpoints collapsing multi-column grids to single-column below `600px`. A dedicated `@media print` block strips chrome (nav, buttons, step tracker) for clean PDF/print export of results.

### Wizard flow
- Each step is a `.form-section` toggled via an `active` class; only one is visible at a time (`display:none` otherwise), with a `slideUp` CSS animation on entry.
- `.step-track` mirrors progress with numbered, clickable nodes (`data-step` attributes) that update `active` / `done` states as the user moves forward/back.
- Inputs use native form controls (`<select>`, `<input type="number">`, `<input type="range">`) plus custom-styled components:
  - **Toggle cards** (`.toggle-card[data-val]`) for single-choice selections like vehicle type, each carrying its own emission factor in `data-val` (e.g. Petrol `0.27 kg/km`, Diesel `0.21`, Hybrid `0.13`, Electric `0.07`, Motorbike `0.15`, No Car `0`).
  - **Sliders** (`input[type=range]`) for continuous values (income level, daily driving distance) with live-updating badges and tick labels.
  - **Checkbox cards** (`.check-item`) for multi-select lifestyle factors.

### Calculation engine
On submission, JavaScript reads all form state and computes a weighted annual CO₂ total (in tonnes) across the six categories, using region-aware factors (selected country influences grid/fuel emission intensity). The results view then:
- Renders a headline emissions figure with a **verdict** (`verdict-good` / `verdict-ok` / `verdict-high`) based on thresholds.
- Builds a **category breakdown** with animated proportional bars (`.bk-bar`, width transitions).
- Generates **recommendations** tagged by impact/difficulty (`diff-easy` / `diff-med` / `diff-hard`).
- Plots a **reduction timeline** with projected savings per action.
- Shows **comparison cards** against world average (4.8t), a reference national average (16t), and the Paris Agreement 2050 target (2t).

### State & persistence
- **Save/Load**: serializes current form state to `localStorage` so users can resume later.
- **Share**: generates a shareable summary/link of results.
- **Reset**: clears all inputs back to defaults.
- **Toast** component (`.toast`) gives non-blocking feedback for these actions (e.g. "Saved successfully").

---

## Getting Started

No installation or build step is required.

```bash
git clone https://github.com/rajat1606-dev/ecotrack2.0.git
cd ecotrack2.0
```

Then simply open `index.html` in a browser, or serve it locally:

```bash
# Python
python3 -m http.server 8000

# Node
npx serve .
```

Visit `http://localhost:8000`.

---

## Project Structure

```
ecotrack2.0/
└── index.html   # Entire application: markup, styles, and logic
```

> Currently a single-file app by design (no bundler/build pipeline). See Roadmap below for planned modularisation.

---

## Roadmap / Ideas for Contribution
- [ ] Split inline CSS/JS into separate `styles.css` / `app.js` files for maintainability
- [ ] Move emission factors into a config/JSON data file for easier regional updates
- [ ] Add unit tests for the calculation engine
- [ ] Add a PDF export using the existing print stylesheet
- [ ] Expand country/region emission-factor coverage
- [ ] Add dark/light theme toggle (currently dark-only)

---

## Contributing

Issues and PRs are welcome. Since this is currently a single-file project, please keep the HTML/CSS/JS organised under clear section comments (`<!-- ━━━ SECTION: ... ━━━ -->`) matching the existing convention until modularisation lands.

---

## License

No license file is currently present in the repository — add one (MIT recommended for an open personal project) if you intend for others to reuse the code.
