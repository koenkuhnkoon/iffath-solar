# Iffath Poultry Farm — Solar Decision Guide

## What this project is

A single-page decision guide and interactive calculator (`index.html`) helping the owner of Iffath Poultry Farm in Faisalabad, Pakistan decide whether to install a 100 kW solar system. It is built for a non-technical audience — the farm owner and his business partner — and is deployed on GitHub Pages.

The owner will iterate on this document as he gathers more information (e.g. confirms FESCO net metering status, gets battery replacement quotes). Updates should keep the same structure and tone: clear, honest about uncertainty, and transparent about sources.

**Live site:** https://pakistan-solar.vercel.app  
**Repo:** https://github.com/koenkuhnkoon/iffath-solar  
**Push:** use `GH_TOKEN=$(gh auth token)` and push to `https://x-access-token:${GH_TOKEN}@github.com/koenkuhnkoon/iffath-solar.git`  
**Deploy:** `npx vercel@latest --prod` from the project directory (Vercel account: iffath)

---

## Farm context

- **Location:** Faisalabad, Punjab, Pakistan. Feeder: New Khanuana 101204 (FESCO)
- **Business:** Two poultry control sheds, 50/50 partnership. Land and buildings owned by the owner (not the partner).
- **Rent structure:** Owner charges partner ₨250,000/month per shed = ₨500,000/month total (₨60 lakh/year)
- **Financing deal:** Partner finances solar installation; owner repays by forgoing rent on one shed (₨250,000/month) until the debt is cleared. After payoff, owner owns the solar asset and can renegotiate rent upward.
- **Generator:** 60 kW diesel generator for load-shedding backup

---

## Actual data (do not change without a new source)

These numbers come from the real FESCO electricity bill (Dec 2024–Nov 2025) and owner's notes. They are hard-coded as constants at the top of the `<script>` block.

| Constant | Value | Source |
|---|---|---|
| `ANNUAL_KWH` | 93,960 kWh | Actual FESCO bill |
| `ANNUAL_BILL` | ₨4,884,882 | Actual FESCO bill |
| `EFFECTIVE_RATE` | ~₨52/kWh | Derived: ANNUAL_BILL / ANNUAL_KWH |
| `COST_ONGRID` | ₨10,000,000 | Installer quote (1 crore) |
| `COST_HYBRID_NB` | ₨11,700,000 | Installer quote (1.17 crore) |
| `COST_HYBRID_B` | ₨20,000,000 | Installer quote (2 crore) |
| `RENT_FOREGONE` | ₨250,000/month | Owner's notes |
| `DIESEL_GENERATOR` | 7,000 L/yr | Owner's notes — generator backup (solar-saveable) |
| `DIESEL_BROODER` | 6,000 L/yr | Owner's notes — brooder heaters, winter nights (NOT solar-saveable) |
| `SOLAR_KW` | 100 kWp | Installer quote |
| `PEAK_SUN_HOURS` | 5.26 hrs/day | NASA POWER via ProfileSolar.com |
| `SYSTEM_EFFICIENCY` | 0.80 | Industry standard |
| `SOILING_FACTOR` | 0.90 | Faisalabad dusty climate estimate |

Monthly bills hard-coded in the `BILLS` array (used only for the display table, not calculations).

---

## Prices to update regularly

These change and should be refreshed when the owner revisits:

- **HSD diesel price:** Check https://www.ogra.org.pk/notified-petroleum-prices — updated twice monthly by OGRA. Currently ₨520.35/litre (April 3, 2026 hike; previous was ₨335.86).
- **NEPRA export rate:** Currently ₨11.30/unit (new net billing, post Feb 9 2026). Grandfathered net metering is ₨27/unit. Source: NEPRA SRO 251(I)/2026.
- **FESCO tariff:** If the farm's electricity bills change significantly, update `ANNUAL_BILL` and `ANNUAL_KWH`, which will recalculate `EFFECTIVE_RATE` automatically.

---

## The three options

| Option | Cost | Payback (rent method) | Key risk |
|---|---|---|---|
| 1 — On-grid 100 kW | ₨1.00 crore | 40 months | Generator–inverter software integration reliability |
| 2 — Hybrid, no battery | ₨1.17 crore | 47 months | ❌ Cloudy day + grid outage = fans stop = bird deaths |
| 3 — Hybrid + battery | ₨2.00 crore | 80 months | Battery replacement cost unknown; ask installer |

Option 2 is not recommended — the biological risk for a poultry operation is documented in peer-reviewed literature (Cambridge / World's Poultry Science Journal).

---

## Open questions (owner must answer)

These are the 8 items in the checklist section. As answers come in, update the guide:

1. **Net metering status** — does the farm have a grandfathered FESCO agreement at ₨27/unit? If yes, update `i-export-rate` default to 27.
2. **Load shedding hours and timing** — check ccms.pitc.com.pk for feeder "New Khanuana". Update `i-diesel-red-ongrid` default based on actual daytime outage hours.
3. **Diesel purpose** — ANSWERED: 7,000 L/yr for generators (solar-saveable, calculator default), 6,000 L/yr for brooder heaters (winter nights, not solar-saveable, excluded from calculator). Do not change these unless owner provides new data.
4. **Roof area and structural capacity** — can both sheds hold a 100 kW system (~500–600 m²)? Note any structural reinforcement cost.
5. **Battery replacement quote** — update `i-battery-replacement` default once installer provides a figure.
6. **Generator–inverter software** — vendor name, annual support cost, warranty impact.
7. **GST on imported panels** — 18% GST is proposed but not yet law. Update cost constants if enacted.
8. **Peak simultaneous load** — bills show max MDI 64 kW from grid; confirm total peak including generator-supplied load.

---

## Source quality standards

Every claim in the document carries a badge. When adding new content, use:
- `<span class="src">OGRA</span>` / `<span class="src">NEPRA</span>` — confirmed official government source
- `<span class="src src-bill">actual bill</span>` — from the FESCO bill image
- `<span class="src src-est">estimated</span>` — reasonable assumption, not verified
- `<span class="src src-warn">ask installer</span>` — must be quoted before relying on the number

Do not remove source badges or downgrade certainty without a good reason. The transparency is intentional.

**Credible sources used in this document:**
- OGRA: ogra.org.pk/notified-petroleum-prices
- NEPRA SRO 251(I)/2026: nepra.org.pk (primary legal text)
- IEEFA April 2025 report: ieefa.org (policy and grid analysis, ★★★★★)
- NASA POWER via ProfileSolar.com: satellite solar irradiation data (★★★)
- Cambridge / World's Poultry Science Journal: poultry heat stress (★★★★)
- Acta Mechanica Malaysia 2022: small-scale poultry solar feasibility — cited but contextualised (★★★)

---

## How to update the calculator

All inputs and constants are in the `<script>` block near the bottom of `index.html`. The key ones are labelled with comments. The `calculate()` function runs on every input change and updates all displayed results automatically. To change a default slider value, update both the `value=` attribute on the `<input>` element and the displayed span.

When adding new inputs, follow the existing pattern: `<input type="range">` with `oninput="syncVal(this,'v-id');calculate()"`.

---

## Tone and style

- Address the owner directly ("your farm", "you")
- Be honest about uncertainty — never present an estimate as a fact
- The document is bilingual-ready (English only for now; Urdu toggle is a possible future enhancement)
- Playful but professional: the rooster/sun motifs in the header are intentional; keep section content factual and precise
- No dark patterns: the "what we don't know" checklist is a first-class section, not a footnote
