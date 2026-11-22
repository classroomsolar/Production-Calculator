# Production_Calculator_prompt
## PV Site, Resource, Plant Sizing & Energy Yield Calculator

| Item | Detail |
|---|---|
| Calculator | **Production_Calculator** |
| Status | Existing calculator — continue and complete on the website |
| Website page | `/pv-site-yield` (site & weather, geometry & shading, tracker & bifacial, plant sizing, energy yield & losses) |
| Master Prompt V5.0 scope | Group 1 — Solar Resource and PV System Design (R001–R024) + Group 16 — Energy Yield, Losses and Plant Performance (R379–R400) |
| Project types | P1 utility fixed-tilt, P2 utility single-axis tracker, P3 commercial carport, P4 commercial/industrial rooftop, P5 residential rooftop — applicability recorded per requirement (REQUIRED / CONDITIONAL / NOT APPLICABLE / UNSUPPORTED, Section 19) |
| Revision | Final — 14 November 2026 |

**This is the only .md file for Production_Calculator.** It holds only what is specific to this calculator. Everything common to all calculators — owner and language rules, the development process (execution cycle, phases, engineering and testing rules, working method, records), report and output presentation, the module completion checklist, website acceptance and the common tool list — is in **Master Prompt V5.0** (`00_MASTER_PROMPT_V5.0.txt`) and is not repeated here. Give the AI both files. If they disagree, the Master Prompt wins.

Every other .md file of this repository (including `00_INDEX.md` and the original prompt file) is copied word for word in the **Appendix** at the end, so nothing is lost when those files are deleted. Sections 1–11 are the working file; the appendix is reference only.

## 1. What this calculator does

Solar resource and site assessment (GHI, DNI, DHI, POA), solar position and sun path, module operating temperature, horizon / near / far / row-to-row shading, GCR, tilt and azimuth analysis, fixed-tilt and single-axis tracker configuration with backtracking, bifacial gain and albedo, available area and capacity, DC/AC ratio, PV and inverter block sizing (Group 1, R001–R024) — and design-stage energy yield: annual, monthly and hourly energy, PR, every loss stage from module temperature to the POI, clipping, availability, degradation, bifacial and tracker gain, curtailment, auxiliary load, net export and comparison of alternatives (Group 16, R379–R400). Plant sizing and energy yield are two separate engines. Results are modelled design values, never measured performance; P50/P90 only when statistically justified (Section 38).

**Scope boundary:** Never build, anywhere in this calculator: generator sources or sizing, HV / EHV equipment or studies (above the 35 kV class), substation engineering, field testing / measurement / commissioning, structural / civil / mechanical calculations, and any financial or cost calculation (equipment prices, CAPEX, OPEX, LCOE, tariffs). BESS is deferred (Master Prompt Section 18): do not build it now, but keep data models ready for it. Everything is built on the public website — no spreadsheets, no VBA, no desktop tools.

## 2. Requirements of this calculator

### 2.1 Functional requirements from Master Prompt V5.0

Every requirement below must end up implemented and verified, or marked NOT APPLICABLE / NOT SUPPORTED with a written reason. "Current state" is the starting point found in the earlier work; confirm it in Phase 0.

**Group 1 — Solar Resource and PV System Design (R001–R024, 24 active)**

| ID | Requirement (Master Prompt V5.0, verbatim) | Current state |
|---|---|---|
| R001 | Solar resource assessment using supplied or approved resource datasets. | Built earlier — verify |
| R002 | Global Horizontal Irradiance (GHI) processing. | Built earlier — verify |
| R003 | Direct Normal Irradiance (DNI) processing. | Built earlier — verify |
| R004 | Diffuse Horizontal Irradiance (DHI) processing. | Built earlier — verify |
| R005 | Plane of Array (POA) irradiance calculation. | Built earlier — verify |
| R006 | Solar azimuth calculation. | Built earlier — verify |
| R007 | Solar elevation and altitude calculation. | Built earlier — verify |
| R008 | Sun-path and solar-position modelling. | Partly built — complete |
| R009 | Module operating-temperature estimation. | Built earlier — verify |
| R010 | Temperature correction of PV electrical parameters. | Built earlier — verify |
| R011 | Horizon shading assessment. | Built earlier — verify |
| R012 | Near-object shading assessment. | Partly built — complete |
| R013 | Far-object shading assessment. | Partly built — complete |
| R014 | Row-to-row shading assessment. | Partly built — complete |
| R015 | Ground Coverage Ratio (GCR) calculation. | Partly built — complete |
| R016 | Tilt-angle selection and analysis. | Partly built — complete |
| R017 | Module orientation and azimuth analysis. | Partly built — complete |
| R018 | Fixed-tilt system configuration. | Built earlier — verify |
| R019 | Tracker rotation and operating-angle modelling. | Built earlier — verify |
| R020 | Tracker backtracking calculation. | Built earlier — verify |
| R021 | Bifacial gain and albedo modelling. | Built earlier — verify |
| R022 | Available installation area and capacity estimation. | Not built — build new |
| R023 | DC/AC ratio calculation and evaluation. | Built earlier — verify |
| R024 | PV block, inverter block and total plant capacity configuration and comparison. | Partly built — complete |

**Group 16 — Energy Yield, Losses and Plant Performance (R379–R400, 22 active)**

| ID | Requirement (Master Prompt V5.0, verbatim) | Current state |
|---|---|---|
| R379 | Annual energy-yield calculation. | Built earlier — verify |
| R380 | Monthly energy-yield calculation. | Partly built — complete |
| R381 | Hourly energy-yield modelling. | Built earlier — verify |
| R382 | Performance Ratio (PR) calculation. | Not built — build new |
| R383 | Module-temperature loss calculation. | Partly built — complete |
| R384 | Soiling-loss modelling. | Not built — build new |
| R385 | Shading-loss modelling. | Partly built — complete |
| R386 | Module-mismatch loss modelling. | Built earlier — verify |
| R387 | DC wiring-loss calculation. | Built earlier — verify |
| R388 | AC wiring-loss calculation. | Built earlier — verify |
| R389 | Transformer-loss calculation. | Partly built — complete |
| R390 | Inverter-efficiency and inverter-loss modelling. | Built earlier — verify |
| R391 | Inverter clipping calculation. | Built earlier — verify |
| R392 | Assumed plant-availability modelling. | Not built — build new |
| R393 | Module and system degradation modelling. | Not built — build new |
| R394 | Bifacial energy-gain modelling. | Built earlier — verify |
| R395 | Tracker energy-gain modelling. | Built earlier — verify |
| R396 | Curtailment-loss modelling. | Partly built — complete |
| R397 | Grid-unavailability-loss modelling. | Not built — build new |
| R398 | Auxiliary-load calculation. | Not built — build new |
| R399 | Net-energy-export calculation. | Partly built — complete |
| R400 | Comparison of alternative system configurations and modelled performance. | Partly built — complete |

### 2.2 Additional requirements from this calculator's earlier documents

These came from the earlier README / prompt / requirement documents and are kept because they match Master Prompt V5.0.

| # | Requirement | Matches V5.0 |
|---|---|---|
| D01 | Weather/time series: TMY, historical, EPW/CSV, 8760/8784, timestamps, leap year, UTC/local | R001–R004 |
| D02 | Solar position and irradiance: zenith, azimuth, POA, IAM, spectral, Perez sky diffuse | R005–R008 (IAM/spectral feed R379–R381) |
| D03 | Array orientation and tracker: tilt, azimuth, pitch, slope, axis, backtracking, limits, stow, GCR | R014–R020 |
| D04 | Shading: horizon, obstacles, 3-D scene; electrical substring/bypass mismatch separate | R011–R014, R039, R385–R386 |
| D05 | Bifacial: bifaciality, albedo, row geometry, torque tube, rear mismatch | R021, R394 |
| D06 | Inverter: DC limits, AC nameplate, CEC/Sandia efficiency, PQ capability, clipping | R390–R391 here; R083, R087 → Inverter_LV_AC |
| D07 | PV thermal model (Faiman/SAPM), soiling, snow, spectral, degradation | R009, R383–R384, R393 |
| D08 | Electrical yield losses DC→POI: DC ohmic, mismatch, inverter, AC cable, transformer, auxiliaries, availability, curtailment | R386–R392, R396–R399 (cable resistances from DC/AC cable calculators) |
| D09 | Production reporting: hourly, monthly, annual, kWh/kWp, PR, CF, loss diagrams | R379–R382 |
| D10 | Module layout planning, string allocation, field segments, cable routes | R022 (area/capacity) in scope; automatic site layout needs approval (Group 17 boundary) |
| D11 | P50/P75/P90/P95/P99 uncertainty | Section 38 (only when statistically justified) |
| D12 | Run real pvlib and PySAM/SSC 8,760-hour benchmark with identical inputs | Sections 38, 67 |
| D13 | Live public-URL Playwright desktop/mobile acceptance | Section 95 |
| D14 | One PV list (39,400 primary records); one inverter list (5,739) (from the earlier Engineering_Master_Database documents) | Section 44 — Production's 26,000-record catalogue is reconciled into it |

### 2.3 Engineering method and verification requirements (Master Prompt Part 7)

Apply Master Prompt Section 38 in full, plus:

- Weather provenance and quality checks; time-zone and timestamp alignment (UTC calendar) — a one-hour shift silently corrupts every annual result.
- Solar position and transposition against an independent implementation (e.g. pvlib run in a separate script) with the **same** inputs.
- The pending **pvlib ModelChain + PySAM/SSC 8,760-hour matched benchmark** (scripts in `04_EXTERNAL_VERIFICATION_KIT/`): same weather, same module/inverter parameters, same loss conventions; report hourly RMSE/MAE/bias, annual kWh and % difference. Different defaults must not be compared as if equivalent.
- Tracker backtracking angle against published equations; bifacial view-factor against a documented reference.
- Loss waterfall closes to the net energy (energy balance check).

### 2.4 Shared engines and connections to other calculators (Sections 23, 45, 49)

- **Owns:** weather/resource service, solar geometry and POA, shading engine, tracker and bifacial engines, plant-sizing engine (R022–R024), energy-yield engine (Group 16).
- **Uses:** module and inverter records from the Master Database; string configuration from PV_String_MPPT; DC and AC cable losses from DC_Cable_Connector and Inverter_LV_AC (same resistance data — never a separate copy); transformer losses from Transformer_POI; export limits and curtailment from LV_Interconnection_Metering.
- **Gives:** POA, module temperature and site temperature extremes to PV_String_MPPT; plant/block configuration to the network model; yield and loss waterfall to Documentation_Schedules_QA.
- **Moves out:** string/MPPT logic, electrical screens, POI screen, embedded cable-thermal copy, embedded load-flow/fault studies — each to its own calculator.

| Direction | Data | From / to |
|---|---|---|
| In | Module, inverter records (with DB revision) | Master Engineering Database |
| In | String count, MPPT allocation, DC/AC ratio inputs | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector) |
| In | DC and AC cable losses | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector) / the LV AC calculators (Inverter_LV_AC, LV_Interconnection_Metering) cable engines |
| In | Transformer no-load and load losses | the MV calculators (MV_Electrical_Design, Transformer_POI) |
| In | Export limit, curtailment rules | the LV AC calculators (Inverter_LV_AC, LV_Interconnection_Metering) |
| Out | POA, module temperature, hourly irradiance | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector) (temperature-corrected Voc/Vmp, R026–R027 use the same site temperature basis) |
| Out | Plant capacity / block configuration (R024) | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector), the LV AC calculators (Inverter_LV_AC, LV_Interconnection_Metering), the MV calculators (MV_Electrical_Design, Transformer_POI), Fault_Current_Studio network model |
| Out | Annual / monthly / hourly yield, loss waterfall | Documentation_Schedules_QA_Calculator reports |

Every connection is a typed, versioned contract; a changed upstream result marks this calculator's dependent results STALE.

## 3. Work already done

**Status:** live at `https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/` (Wix site ID `ec984f80-a998-4e29-a38c-ca6b85dac4f4`). The live site had 253 assets (revision `d9b85e4e42e7059f7a92ef081496ccf68a79511b`); the last verified local copy had 240 assets. **Take the current live code as the starting point** and never overwrite it with an older copy.

**Functions already built (verify each):** weather import (PVGIS, EPW, CSV; 8,760/8,784 h, UTC calendar); solar position and POA (isotropic, Hay–Davies, Reindl transposition; Perez not yet); horizon and 3-D obstruction screening; fixed-tilt geometry; single-axis tracker geometry with rotated-axis azimuth fix and backtracking; bifacial view-factor model (fixed and tracker); substring/bypass mismatch model; De Soto single-diode module model (280 independent SciPy operating points); CEC/Sandia inverter efficiency model with 2,343 fitted inverters (opt-in hourly coupling that refuses out-of-envelope configurations); hourly MPPT-current and transformer-kVA rejection gates; DC topology for combined feeder vs individual string loops; 8,760-hour per-stage energy reconciliation; P50/P75/P90 screening; 26,000-record PV module catalogue with a full qualification register; POI three-phase current/power vectors (100 independent checks).

**Known data state:** of 26,000 module records, 24,705 have a blank source field, 69 lack valid STC values, 39 have >7 % Vmp×Imp/Pmp mismatch, 19 duplicate maker/model names; zero records are qualified for construction use (a review flag, not proof of bad data). A former CSV export truncated at 250 rows — fixed; keep as a regression case. Demo defaults (5,000 kW PV, 12,470 V bus, 25° tilt, synthetic weather/horizon) are examples, never site data.

## 4. Functions still to be added

- [ ] R002–R005 Perez (and other) transposition models with coefficient sources, IAM and spectral correction.
- [ ] R011–R014 verified near-object and row-to-row shading engine; documented far-object/horizon method; shading on bifacial rear side.
- [ ] R015 GCR calculation; R016 tilt selection and analysis; R017 orientation/azimuth analysis.
- [ ] R019–R020 tracker rotation limits, axis tilt, cross-axis slope and slope-aware backtracking.
- [ ] R022 available installation area and capacity estimation; R024 PV/inverter block and plant capacity configuration and comparison (separate engine from yield).
- [ ] R382 Performance Ratio; R384 soiling loss; R392 availability; R393 module and system degradation; R397 grid unavailability; R398 auxiliary load.
- [ ] R383, R385–R391, R396, R399 complete DC→POI loss chain with energy-balance closure; inverter loss from a parameterised efficiency model; DC/AC cable losses from DC_Cable_Connector and Inverter_LV_AC; transformer losses from Transformer_POI; curtailment and export from LV_Interconnection_Metering.
- [ ] R400 comparison of alternative configurations and modelled performance.
- [ ] Long-term resource uncertainty for P50/P90 with documented statistics.

**Requirements with no implementation yet (build new):**

- [ ] R022 Available installation area and capacity estimation
- [ ] R382 Performance Ratio (PR) calculation
- [ ] R384 Soiling-loss modelling
- [ ] R392 Assumed plant-availability modelling
- [ ] R393 Module and system degradation modelling
- [ ] R397 Grid-unavailability-loss modelling
- [ ] R398 Auxiliary-load calculation

## 5. Earlier work still to be completed

- [ ] Export the live 253-asset (or newer) code, compare it with the 240-asset copy, put both in Git, inventory every module.
- [ ] Move string/MPPT logic to PV_String_MPPT_Calculator; electrical/NEC screens to DC_Protection_Combiner, Inverter_LV_AC and LV_Interconnection_Metering; POI/PPC to LV_Interconnection_Metering and Transformer_POI; the embedded cable-thermal copy to Cable_Thermal_Calculator; the radial load-flow / fault-magnitude studies to Fault_Current_Studio — results must match after each move.
- [ ] Remove from the website any module that earlier versions contained outside V5.0 scope.
- [ ] Reconcile the 26,000-record PV catalogue and the CEC inverter library into the Master Database (all IDs kept); resolve provenance of blank-source records from manufacturer datasheets where obtainable; keep unresolved records flagged.
- [ ] Security review: no private or licence-restricted source code served publicly.

**Partly built — complete them:**

- [ ] R008 Sun-path and solar-position modelling
- [ ] R012 Near-object shading assessment
- [ ] R013 Far-object shading assessment
- [ ] R014 Row-to-row shading assessment
- [ ] R015 Ground Coverage Ratio (GCR) calculation
- [ ] R016 Tilt-angle selection and analysis
- [ ] R017 Module orientation and azimuth analysis
- [ ] R024 PV block, inverter block and total plant capacity configuration and comparison
- [ ] R380 Monthly energy-yield calculation
- [ ] R383 Module-temperature loss calculation
- [ ] R385 Shading-loss modelling
- [ ] R389 Transformer-loss calculation
- [ ] R396 Curtailment-loss modelling
- [ ] R399 Net-energy-export calculation
- [ ] R400 Comparison of alternative system configurations and modelled performance

**Built earlier — rebuild/carry over to the website and prove with tests:**

- [ ] R001 Solar resource assessment using supplied or approved resource datasets
- [ ] R002 Global Horizontal Irradiance (GHI) processing
- [ ] R003 Direct Normal Irradiance (DNI) processing
- [ ] R004 Diffuse Horizontal Irradiance (DHI) processing
- [ ] R005 Plane of Array (POA) irradiance calculation
- [ ] R006 Solar azimuth calculation
- [ ] R007 Solar elevation and altitude calculation
- [ ] R009 Module operating-temperature estimation
- [ ] R010 Temperature correction of PV electrical parameters
- [ ] R011 Horizon shading assessment
- [ ] R018 Fixed-tilt system configuration
- [ ] R019 Tracker rotation and operating-angle modelling
- [ ] R020 Tracker backtracking calculation
- [ ] R021 Bifacial gain and albedo modelling
- [ ] R023 DC/AC ratio calculation and evaluation
- [ ] R379 Annual energy-yield calculation
- [ ] R381 Hourly energy-yield modelling
- [ ] R386 Module-mismatch loss modelling
- [ ] R387 DC wiring-loss calculation
- [ ] R388 AC wiring-loss calculation
- [ ] R390 Inverter-efficiency and inverter-loss modelling
- [ ] R391 Inverter clipping calculation
- [ ] R394 Bifacial energy-gain modelling
- [ ] R395 Tracker energy-gain modelling

## 6. Checks and audits still to be done

- [ ] Real pvlib ModelChain and PySAM/SSC (PVWatts/SAM) runs with the same 8,760-hour weather, module/inverter parameters and loss conventions; report hourly RMSE/MAE/bias, annual kWh and % difference per loss stage.
- [ ] Solar position and transposition against an independent implementation; leap-year, time-zone, polar-site and EPW edge cases.
- [ ] Tracker backtracking angles against published equations / pvlib on flat and sloped terrain; bifacial view factors against a documented reference.
- [ ] Live public-URL Playwright tests on desktop and mobile: inputs, numerical results, module loading, 26,000-record browsing, reports, CSV exports, network errors.
- [ ] Engineering and software audit of every engine; challenge campaign of 200 cases.
- [ ] Plus the standard audits and research register of Master Prompt Sections 11 and 72 (engineering, software, data, integration, UI/UX, security, requirements coverage).

## 7. Phases for this calculator
Phase names, "done when" rules and the process are in Master Prompt Section 62 and Section 7. What each phase means here:

| Phase | Work for this calculator |
|---|---|
| 0 Inventory (read-only) | Export and reconcile the live code; module inventory; map R001–R024 and R379–R400. |
| 1 Engine | Repair/confirm resource, geometry/POA, shading, tracker, bifacial, plant-sizing and yield engines as separate engines with contracts. |
| 2 Functional verification | Inputs, units, defaults, overrides, sequence, equations, intermediate values, invalid inputs, lookups, warnings, repeatability |
| 3 Challenge and independent validation | Matched pvlib/PySAM benchmark, hand calculations for solar position and POA, golden yield cases for P1–P5, challenge campaign. |
| 4 Customer workflow | Which inputs are visible, auto-filled from the database, inherited from other calculators, optional or advanced |
| 5 UX/UI | Site and resource dashboard, sun path, shading views, tracker/bifacial views, monthly/hourly charts, loss waterfall. |
| 6 Integration | Connect to PV_String_MPPT (strings), cable calculators and Transformer_POI (losses), LV_Interconnection_Metering (export limits), Documentation_Schedules_QA (reports). |
| 7 Release | Staging and public-URL tests, monitoring, backup/restore, rollback |

### 7.1 Calculator-specific rules carried over from the earlier documents
- One integrated Solar Resource / PV Production / Electrical platform; browser only; shared project inputs and contracts
- Temporary browser-local cases expiring within 24 h; no accounts or permanent storage without approval → **Decided: private project workspaces with accounts and server-side authorisation (Sections 79, 81); browser storage only as a non-authoritative draft cache (Section 44).**
- Never invent equipment specs or approvals; conservative assumptions labelled
- Units SI and imperial incl. ft/in; explicit internal conversions
- Match functions of PVsyst, HelioScope, SAM, PVWatts with original methods; never claim equivalence without evidence
- Engineering Source Register with page coverage; NEC 2023 only partly read
- Track every requirement with the Section 57 lifecycle (NOT IMPLEMENTED … ENGINEERING VALIDATED → DEPLOYED → APPROVED) instead of the earlier status words
- Public thermal-engine JS on the solar site may expose private source; review before public serving
- ChatGPT / Google AI Studio may assist input completion; never replace numerical kernels

## 8. Final result — what "finished" looks like

- Separate, verified engines for resource, solar geometry/POA, shading, tracker/backtracking, bifacial, plant sizing and energy yield, all running on the website.
- Hourly, monthly and annual yield with a closed DC→POI loss waterfall, PR, every Group 16 loss, net export and alternative comparison.
- Documented, matched benchmark against pvlib and PySAM/SAM with error metrics; no claim of equivalence beyond that evidence.
- Every requirement in section 2 implemented, or marked NOT APPLICABLE / NOT SUPPORTED with a written reason, with evidence — plus the module completion checklist, website acceptance, reports/exports and final delivery report of Master Prompt Sections 57, 77–78, 87, 94 and 95.

## 9. Tools specific to this calculator
The common tool list is in Master Prompt Section 8.

| Purpose | Tool |
|---|---|
| PV modelling reference runs | pvlib-python, NREL PySAM / SAM SSC, PVWatts API |
| Weather data | PVGIS API, NSRDB / TMY / EPW sources |
| Feature benchmarking only | PVsyst, HelioScope, SAM documentation |

Benchmark tools are used to compare features and results only — never copy their code, data or methods without checking them (Section 12).

## 10. Kept from the earlier QA reports in this repository
From `02_RECENT_QA_AND_REPORTS/` (232-, 237- and 240-asset reports, 25–26 Oct 2026). Only facts not already in sections 1–9.

**Release history on the live Wix site** (`ec984f80-a998-4e29-a38c-ca6b85dac4f4`):
| Date | Revision | Serving assets | What changed |
|---|---|---|---|
| 25 Oct 2026 | `84a9e4ffdbafb03cc0474e9e9d82ee8df5e98809` | 232 | `grid-poi-screen.js`, `grid-poi-ui.js` (balanced 3-phase POI snapshot); `bess-dispatch.js`, `bess-ui.js` (24-value UTC load profile, grid-import limit); `main.js`, `index.html`; backups in `release-backups/pre-grid-poi-bess-demand-20261025/`; 0 deletions |
| 26 Oct 2026 | `fe4b08869861d49f35cb540d7a4aa2f90dd98037` | 237 | P50/P90 finance scenario sensitivities (research feature); 3 files replaced, 1 new module; 39 rollback manifests |
| 26 Oct 2026 | `1daff091e0192dd988a986888d6c191c1934b420` | 240 | `fullQualificationRegister`: complete 26,000-row, 20-column module register CSV (the old export stopped at 250 rows) |

Later the live site showed 253 assets (revision `d9b85e4e…`, section 3). The ZIPs of these releases were **reconstructed**, not downloaded from Wix — never deploy an older ZIP without a fresh revision check. Wix `headless.files.patch` is not atomic: avoid two writers at once.

**Scope warning for Phase 0:** the live site contains BESS dispatch, POI/PPC screens, a bundled v3.6.2 thermal engine and P50/P90 → finance sensitivities. Under section 1 (no financial calculations, BESS deferred) and section 5 these are to be moved out or removed — results must match after each move.

**Test evidence recorded (all synthetic or local, not public-URL acceptance):** 107 engineering regression assertions; 30,036 POI/BESS numeric checks (incl. 10,000 seeded random cases, 2024 leap year); 100 independent Python POI phasor checks; 78,840 independent 8,760-hour stage checks (arithmetic only, not pvlib/PySAM equivalence); 280 SciPy De Soto points (peak relative error < 2e-6); pinned pvlib 0.16.1 PVWatts inverter 2,004 tests and shaded-fraction / projected-zenith 2,500 tests; 766 finance assertions; 68 → 90 local Chromium desktop/mobile checks; 13 full-catalogue assertions; 14/14 download checks with a 7-record fixture (the full 26,000-record browser fixture stalled — not a pass); 129 JS files parse; bundled thermal engine 537 internal checks.

**Catalogue facts:** canonical 26,000-row JSON SHA-256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`. Historical SAM identity check: 8,202 exact STC matches, 236 within 2 %, 58 larger differences, 12 ambiguous, 17,492 with no exact SAM identity. Local physics screen: 108 records flagged (`equipment_local_physics_anomalies.csv`); review queue of 70 (`equipment_reference_exception_queue.csv`); 1,159 records with a non-external source reference. CEC list inclusion is not OEM approval.

**Blocked in the earlier environment:** pip install of pvlib / PySAM failed (no package index); Chromium could not open the public Wix URL (`ERR_BLOCKED_BY_ADMINISTRATOR`). Needed for real validation: actual site weather and plant topology, current OEM datasheets, utility interconnection data, an environment with pvlib and PySAM installed, a browser that can reach the public URL.

## 11. Status on 22 Nov 2026
- Website: live (earlier work). Repository: commit `d904f78`, 15 Nov 2026.
- Phases 0–7 of section 7: **none started.** Next task: Phase 0 (read-only inventory and Phase 0 report).

---

# APPENDIX — all other .md files of this repository (word for word)

The documents below are copied word for word from the repository. Only heading levels were moved down so they fit inside this file.


---

## A1. Release test report, 232 assets (25 Oct 2026)


*Source file: `02_RECENT_QA_AND_REPORTS/232_Asset_Release_Test_Report_2026-10-25.md`*

### Solar PV Engineering Platform — guarded continuation release

**Existing Wix site:** https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/  
**Site ID:** `ec984f80-a998-4e29-a38c-ca6b85dac4f4`  
**Authenticated published site revision:** `84a9e4ffdbafb03cc0474e9e9d82ee8df5e98809`  
**Published file-set:** 232 serving assets; 233 in reconstructed developer ZIP including `wix.config.json`.  
**Publication ID:** `cb6947f9-98d6-4c70-b1a8-f08b46ae96ad`.

#### Changes now published to the SAME site

- New `grid-poi-screen.js` and `grid-poi-ui.js`: user-entered balanced three-phase bidirectional POI and separate PV feeder power/current/transformer rating snapshot. Independent import and export real-power caps; explicit net kvar, kVA and voltage at *each* conductor location. POI cap is not used to erase upstream branch current. No false claim to operating PPC/protection.
- `bess-dispatch.js` and `bess-ui.js`: optional 24-value daily UTC load profile, optional grid-import limit with rejection where demand cannot be physically supplied, peak grid import and load values. Original constant-load result maintained with omitted optional fields. AC bus is not POI.
- `main.js` and `index.html`: register POI screen and attach it to the existing engineering tab; include POI output in integrated Excel/print reporting; update the stale catalogue SHA-256 label to the actual 26,000-row `master_pv_modules.json` value. No master database edits.
- Added four exact old source files and one pre-release manifest in `release-backups/pre-grid-poi-bess-demand-20261025/`. All modified/deployed source files were read back through authenticated Wix helpers and all six FNV checks matched the tested local byte content. There were **zero deletions**. Wix `headless.files.patch` is **not atomic** against concurrent authors.

#### Independently executed checks

| Evidence | Executed | Result | Limit |
|---|---:|---|---|
| Previous engineering regression (all inherited suites) | 107 assertions | All pass | Synthetic/known fixtures |
| New POI numeric scenarios and BESS annual calendar/cap invariants | 30,036 assertions | All pass | Includes seeded 10,000-case randomized scenarios |
| Independent Python POI phasor/power comparisons | 100 records | All pass | Does not approve wiring/ratings |
| Independent Python detailed PV 8760-hour stage-reconciliation values | 78,840 stage checks | All pass | Checks arithmetic, **not** full pvlib/PySAM equivalence |
| Chromium integrated module checks, mobile and desktop | 68 checks | All pass | Locally loaded real JS modules; not public URL browser |
| Active and rollback JS syntax | 129 source files | All parse | Syntax only |
| Retained catalogue selection grouping regressions | 13 | All pass | No manufacturer datasheet authentication |
| Original bundled v3.6.2 thermal engine internal full QA | 537 checks in 10 suites | All pass | IEC-oriented internal tests, not external validation or construction approval |

PV hourly synthetic example: 2025 (8,760 hours); modeled exported energy 29,799.3617549 kWh, night/grid import 189.8 kWh, constrained by a **hypothetical** 12 kW POI export cap. This is an accounting fixture rather than a specific site estimate.

#### Remaining limits (not asserted complete)

- Complete model-to-model annual validation against installed `pvlib` and SSC/PySAM did not run; neither package is installed and the package index is unavailable. Pinned original pvlib function-level reference checks from earlier releases remain in the evidence ZIP. NLR/NREL's official SAM PySAM documentation makes clear that its SSC modules require matching default/model settings for meaningful comparisons.
- Original 26,000 rows remain preserved (SHA-256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`), but tens of thousands of records require independent manufacturer/document revision linkage and prior 58 STC discrepancies have not all been requalified. The CEC itself disclaims independent verification of manufacturer-reported properties.
- POI, MV equipment, NEC cable thermal, protection relay, BESS PCS/EMS, actual PPC dynamic response and finance Pxx continue to be research/preliminary screens; no protective relay setting, listing or construction certification was obtained.
- Network APIs and live Wix-hosted browser interactions were not fully acceptance-tested. Authenticated Wix deployment + file readback and local Chromium UI are distinct evidence.

#### Recovery and file provenance

`Solar_PV_Engineering_Verified_Current_232_Assets_2026-10-25.zip` is **a reconstructed complete-source ZIP**, made from the prior exact published 225-file source archive and six authenticated modified/added files; it is NOT a new byte-for-byte ZIP downloaded from the Wix site. The generated `wix.config.json` must NOT be pushed as a serving asset. The on-site recovery manifest corresponds to the authenticated previous revision and original source fingerprints. Private complete backups should live in Library or private repository, not solely under publicly served `release-backups/` paths. **Never deploy an older ZIP to the site without a fresh revision check.**

Official reference URLs: https://www.se.com/us/en/faqs/FA338225/ (balanced three-phase current); https://www.energy.ca.gov/programs-and-topics/programs/solar-equipment-lists (CEC caveat); https://sam.nlr.gov/software-development-kit-sdk/pysam.html (PySAM/SSC).


---

## A2. Five-stage validation report, 237 assets (26 Oct 2026)


*Source file: `02_RECENT_QA_AND_REPORTS/237_Asset_Stage1-5_Validation_Report_2026-10-26.md`*

### Solar PV Engineering Platform — five-stage validation continuation

Date: 2026-10-26. **Existing site retained:** https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/

**Published release**: authenticated Wix revision `fe4b08869861d49f35cb540d7a4aa2f90dd98037`; 237 serving files; 39 rollback manifests. Wix upload ID `ed1407b0-b543-4427-b3f3-1ff0fcf83bbc`. Three active files replaced, one new active module, three on-site source backups and one rollback manifest added. No deletion, no change to 26,000-row canonical module JSON. Four active changed/added JS files were independently read back with matching version fingerprints. Wix patch is non-atomic; avoid concurrent writers.

#### Stage 1 — solar production physics and reference checks

- Existing energy regression: **107 checks pass**.
- Independent Python/Scipy: **280** synthetic De Soto operating points, independently solved I–V roots and maximum power via brentq and bounded optimizer. Peak relative error less than **2e-6** on Voc/Isc/Vmp/Imp/Pmp; maximum absolute differences documented in `single_diode_scipy_comparison.json`.
- Pinned original pvlib 0.16.1 PVWatts inverter function: **2,004** tests, 0 kW maximum difference in these test vectors.
- Pinned original pvlib 0.16.1 shaded_fraction1d / projected_solar_zenith_angle functions: **2,500** tests; maximum absolute shaded-fraction error about 2.3e-15; projection error about 5.0e-14 degrees.
- Synthetic **8,760-hour** full PV stage simulation reconciled **78,840** independent Python stage-power checks from DC module through POI including AC/DC ohmic and transformer losses.
- **Not achieved:** complete pip-installed pvlib ModelChain plus SSC/PySAM annual resource/tracker/bifacial validation. Both packages failed installation in this environment. Pinned selected reference functions cannot substitute for this. No actual project weather and production benchmark supplied.

#### Stage 2 — equipment catalogue

- Canonical 26,000 PV records and all record IDs preserved, catalogue SHA-256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`.
- Original historical SAM identity classification: 8,202 exact STC five-field matches, 236 near-matches within 2%, 58 larger differences, 12 ambiguous identities, and 17,492 without exact reference identities.
- Local electrical screening flagged **108 records with at least one STC/value issue** (includes 106 with nameplate product deviation over a 7% triage threshold, and rows with nonpositive fields). A flag does not establish incorrect OEM data; see file-level CSV.
- `equipment_reference_exception_queue.csv` prioritises **70** historical SAM differences/ambiguities; `equipment_local_physics_anomalies.csv` tracks the 108 independent local physics flags. Manufacturer revision/datasheet verification is not complete. No records automatically overwritten.
- CEC authoritative list: https://www.energy.ca.gov/programs-and-topics/programs/solar-equipment-lists. Inclusion there is **not** a blanket OEM engineering approval; verify listing per utility/AHJ.

#### Stage 3 — electrical, BESS, grid and POI

- **30,036** independently reproduced 3-phase POI/BESS numerical checks, with 2024 leap-year and regular-year dispatch cases.
- Existing conductor, source current, three-phase apparent power, thermal and tracker screening regression tests remain green.
- 5 MW export cap does not establish import current/transformer limits or upstream PV branch rating; failures are explicitly flagged.
- Physical grid and PCS control, protection coordination, fault studies, conductor termination/OCPD selection, thermal environment, inverter/BMS approval require original project data and cannot be certified.

#### Stage 4 — P50/P90/finance

- **New published research feature:** explicit user-confirmed same-project Pxx annual yield to nominal pre-tax P50/P90 finance **scenario sensitivities**, four scenario rows (P50, independent P90, correlated P90, high-correlation stress).
- **766 dedicated finance numerical assertions pass.** Invalid/nonphysical percentiles are never substituted with zero. Changing annual yield, correlation, finance, or source data invalidates the comparison before it appears in a download or integrated report.
- This **does not** produce a P90 NPV or financeable probability distribution. Tax, debt, incentives, tariff/BESS dispatch and measured covariance remain outside this research model.

#### Stage 5 — website release and acceptance

- **90/90** local Chromium desktop/mobile integration checks passed, including the new linkage, explicit source attestation, fresh/stale browser report handling, CSV/JSON controls, and existing electrical/BESS/browser interactions. Zero uncaught script errors in checked synthetic scenarios.
- Authentic Wix revision, asset count, upload response, pre-release source fingerprints, four published source fingerprints and on-site rollback presence verified.
- Direct Chromium navigation to public Wix host returns **ERR_BLOCKED_BY_ADMINISTRATOR** in this environment. Hence direct hosted browser acceptance and external PVGIS/API integration are **not verified**. Source-readback is not equivalent to live user acceptance.

#### Release artefacts and integrity

- `Solar_PV_Engineering_Current_237_Asset_Release_2026-10-26.zip` has 238 archive entries including generated `wix.config.json` and 237 actual site assets. It is **reconstructed** from the authenticated previous 232-asset release plus verified changed files, and includes rollback paths. It is not a freshly downloaded Wix ZIP.
- `Solar_PV_Engineering_Five_Stage_QA_2026-10-26.zip` contains runnable tests, independent vector results, equipment exception list, existing regression and browser results, and pinned reference context.
- `Solar_PV_Engineering_237_Asset_Release_Integrity_2026-10-26.json` contains counts, hashes, source mutation ledger, and scope limitations.

**Release outcome:** Incremental production software improvement completed, validated and published on the existing site. **Full independent annual-model equivalence, original manufacturer certification, site-specific electrical approval and public browser acceptance remain unproven** and must not be represented as done.


---

## A3. Final-stage engineering exceptions, 240 assets (26 Oct 2026)


*Source file: `02_RECENT_QA_AND_REPORTS/240_Asset_Final_Stage_Engineering_Exceptions.md`*

### Solar PV Engineering Platform — work executed on 26 October 2026

#### Site and release proof

- Original site: https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/
- Wix site ID: `ec984f80-a998-4e29-a38c-ca6b85dac4f4`
- Authenticated current revision: `1daff091e0192dd988a986888d6c191c1934b420`; **240 serving assets**.
- Guarded Wix publication ID: `7541f3c1-c132-4b1a-b362-0346b7c67a38`.
- Exactly 2 active catalogue source files updated, 2 previous originals + 1 manifest added to site backups; **0 deletions**.
- Both published active source files passed Wix authenticated readback FNV checks. FNV is a consistency check, not cryptographic proof. Sources in local archive include SHA256 checksums.
- All **26,000 canonical manufacturer-module source records and their unique IDs were retained**. Original JSON SHA256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`.
- Backup archive is **reconstructed from authenticated prior 237-asset snapshot** and verified patch/backup bytes, not a fresh complete Wix ZIP download. Never blind-overwrite newer live revisions with the backup.

#### New defect fixed in this work

Before the release, the catalogue UI inspected 26,000 records but exported only **250 selected anomaly examples**, potentially leaving users with an incomplete review register. Added `fullQualificationRegister` to produce a **20-column, all-26,000-record, read-only** manufacturer traceability and qualification register, including every source ID, electrical fields, STC consistency flags, source classification, external reference status and recommended review priority. The UI now exports this complete CSV instead of the first 250.

#### Verified re-runs in this work

- **107/107** original engineering regression assertions passed.
- **90/90** embedded browser module desktop/mobile integration checks passed, with no unchecked code changes to the older engines.
- **13** new full-catalogue assertions passed against the complete 26,000-record canonical catalogue.
- **14/14** desktop/mobile real download UI checks passed with a 7-record browser fixture. Complete 26,000-row register data and identity were separately validated with Node and Python. The large 26,000-record browser fixture stalled; **do not describe that as a passing full-size browser acceptance**.
- **280** independently solved SciPy single-diode operating points passed.
- **766/766** Pxx-to-finance research sensitivity assertions passed.
- **100/100** independent Python checks matched three-phase POI current/power vectors.

#### Manufacturer data accountability

- Full source register produced: **26,000 rows, 26,000 unique IDs**.
- **24,705** records have a blank source field and **1,159** have a non-external source reference (categories are disjoint based on the current source checker).
- **69** source records lack valid complete STC numeric/relationship conditions; **39** valid-STC records have >7% `Vmp×Imp/Pmp` discrepancies; no OEM source records were changed or erased.
- **Zero** original rows are automatically qualified for detailed PV engineering and construction; that is intentional, not evidence that all modules are defective. An external CEC/SAM match alone does not verify current OEM certification, source revision, exact SDE model or site-specific electrical limits.
- The existing reference queue separately lists 58 substantial historical SAM STC discrepancies plus 12 ambiguous model identities. Several thousand records have no exact SAM identity. The new full register is a review aid, not an OEM certification.

#### Five-stage state

1. **PV numerical modelling:** Selected pvlib 0.16.1 source kernels, 280 independent SciPy SDM points, and 8,760-hour per-stage energy reconciliation were checked. Full installed pvlib ModelChain, PySAM SSC/PVWatts annual equivalence **could not run** because the Python environment cannot retrieve packages (DNS/approved index unavailable), and no authenticated site-level solar weather/plant benchmark was supplied. Do not claim annual yield equivalence.
2. **Equipment:** Full source-level register completed, exported, validated and added to original website. Most records still require original OEM specification evidence; CEC self-reported entries are not automatically certified by this software. CEC reference: https://www.energy.ca.gov/programs-and-topics/programs/solar-equipment-lists
3. **Electrical/BESS:** Existing controlled three-phase POI and BESS research models passed engineering regression and independent checks. Actual feeder, PCS/BMS, utility protection and AHJ approval cannot be established without exact equipment and grid studies.
4. **P50/P90/finance:** Existing explicitly linked screening sensitivity passed 766 checks. It does not model joint financial uncertainty, full tariffs, tax/debt or a bankable probability distribution.
5. **Website:** Code published/read-back verified at Wix with backups. Local integrated Chromium workflow passed 90 checks. The public URL and local HTTP localhost navigation are blocked by this environment's Chromium administrator policy; hence **live public-host browser and external weather API acceptance are unverified**.

#### What the external qualifications require

Actual site weather and plant topology; original current-revision module/inverter/PCS/transformer/OCPD datasheets and tests; utility interconnection/control/short-circuit ratings and protection requirements; project load/temperature/soil/tray layout where applicable; economics/tariff/financing input; a runnable environment with actual pvlib and NREL-PySAM/SSC installed; a browser allowed to visit the public Wix URL. Without these, no credible 100% engineering or bankability certification is possible.

**Result: verified production update and full review register delivered. External and project-specific qualifications are open and explicitly labelled, not quietly marked complete.**


---

## A4. Calculator prompt files — index (00_INDEX.md, same in all five calculator repos)


*Source file: `00_INDEX.md`*

### Calculator prompt files — index

One file per calculator. Each file is the only document that calculator needs; give it to the AI together with Master Prompt V5.0 (`00_MASTER_PROMPT_V5.0.txt`). Every file has the same sections:

0 About this file · 1 What the calculator does · 2 Requirements (V5.0 requirements verbatim + requirements from the earlier documents + method/verification + connections) · 3 Work already done · 4 Functions still to be added · 5 Earlier work still to be completed · 6 Checks and audits still to be done · 7 Process to follow · 8 Final result · 9 Plugins and tools · 10 First task

**Existing calculators**

| File | Calculator | V5.0 group |
|---|---|---|
| Fault_Current_Studio_prompt.md | Fault current, protection, arc flash, load flow, harmonics | 14 |
| Production_Calculator_prompt.md | Site, resource, shading, tracker, bifacial, plant sizing, energy yield | 1, 16 |
| Cable_Thermal_Calculator_prompt.md | Trench, underground cable, duct bank thermal | 8 |
| Grounding_Calculator_prompt.md | Grounding and bonding | 9 |
| Cable_Tray_Calculator_prompt.md | Conduit, raceway, cable tray, routing, cable pulling | 7 |
| Vault_Calculator_prompt.md | Boxes, enclosures, handholes, vaults | 6 |

**New calculators**

| File | Calculator | V5.0 group |
|---|---|---|
| PV_String_MPPT_Calculator_prompt.md | PV module, string and MPPT design | 2 |
| DC_Protection_Combiner_Calculator_prompt.md | DC protection, fuses, combiner/junction boxes | 3 |
| DC_Cable_Connector_Calculator_prompt.md | DC cables and connectors | 4 |
| Inverter_LV_AC_Calculator_prompt.md | Inverter and LV AC design | 5 |
| Lightning_Surge_Protection_Calculator_prompt.md | Lightning and surge protection | 10 |
| LV_Interconnection_Metering_Calculator_prompt.md | LV interconnection, export control, metering | 11 |
| MV_Electrical_Design_Calculator_prompt.md | Medium-voltage design | 12 |
| Transformer_POI_Calculator_prompt.md | Transformers and POI | 13 |
| Interconnection_Architecture_Calculator_prompt.md | Interconnection architectures | 15 |
| Documentation_Schedules_QA_Calculator_prompt.md | Schedules, BOM, reports, design QA | 17 |

**Shared (used by every calculator)**

| File | Role |
|---|---|
| Engineering_Master_Database_prompt.md | The single equipment database |
| Platform_Website_prompt.md | The website itself: pages, projects, network model, shared engines, security, deployment |

All 428 Master Prompt requirement IDs are covered, each in exactly one file.

**Suggested order:** Engineering_Master_Database → Platform_Website → Fault_Current_Studio → Production_Calculator → Grounding, Cable_Thermal, Cable_Tray, Vault → PV_String_MPPT → DC_Protection_Combiner → DC_Cable_Connector → Inverter_LV_AC → Transformer_POI → MV_Electrical_Design → LV_Interconnection_Metering → Lightning_Surge_Protection → Interconnection_Architecture → Documentation_Schedules_QA.


---

## A5. Original prompt file (as it was before this merge)


*Sections 1–11 above contain all of its calculator-specific content; the common parts are in Master Prompt V5.0. Kept here only so nothing is lost word for word.*


*Source file: `Production_Calculator_prompt.md`*

### Production_Calculator_prompt
#### PV Site, Resource, Plant Sizing & Energy Yield Calculator

| Item | Detail |
|---|---|
| Calculator | **Production_Calculator** |
| Status | Existing calculator — continue and complete on the website |
| Website page | `/pv-site-yield` (site & weather, geometry & shading, tracker & bifacial, plant sizing, energy yield & losses) |
| Master Prompt V5.0 scope | Group 1 — Solar Resource and PV System Design (R001–R024) + Group 16 — Energy Yield, Losses and Plant Performance (R379–R400) |
| Project types | P1 utility fixed-tilt, P2 utility single-axis tracker, P3 commercial carport, P4 commercial/industrial rooftop, P5 residential rooftop — applicability recorded per requirement (REQUIRED / CONDITIONAL / NOT APPLICABLE / UNSUPPORTED, Section 19) |
| Revision | Final — 14 November 2026 |

#### 0. About this file

- This is the **only** project document for this calculator. It replaces every earlier README, prompt, handoff and requirement document; everything still needed from them is written here.
- Governing specification: **Master Prompt V5.0** (Integrated Electrical PV Design Engineering Platform, 9 November 2026). Give the AI both this file and the Master Prompt at the start of every session. If the two ever disagree, the Master Prompt wins.
- The calculator is part of **one integrated public website** (Master Prompt Sections 2, 43, 80): its own route/page, shared project data, shared equipment database, shared reports.
- Reply to the owner in **Hinglish** (simple Hindi–English). Write code, reports and documents in English.
- The owner is an electrical / PV design engineer, not a software developer. The AI does the research, design, coding, testing, debugging and documentation itself (Master Prompt Sections 1–4).


#### 1. What this calculator does

Solar resource and site assessment (GHI, DNI, DHI, POA), solar position and sun path, module operating temperature, horizon / near / far / row-to-row shading, GCR, tilt and azimuth analysis, fixed-tilt and single-axis tracker configuration with backtracking, bifacial gain and albedo, available area and capacity, DC/AC ratio, PV and inverter block sizing (Group 1, R001–R024) — and design-stage energy yield: annual, monthly and hourly energy, PR, every loss stage from module temperature to the POI, clipping, availability, degradation, bifacial and tracker gain, curtailment, auxiliary load, net export and comparison of alternatives (Group 16, R379–R400). Plant sizing and energy yield are two separate engines. Results are modelled design values, never measured performance; P50/P90 only when statistically justified (Section 38).

**Scope boundary:** Never build, anywhere in this calculator: generator sources or sizing, HV / EHV equipment or studies (above the 35 kV class), substation engineering, field testing / measurement / commissioning, structural / civil / mechanical calculations, and any financial or cost calculation (equipment prices, CAPEX, OPEX, LCOE, tariffs). BESS is deferred (Master Prompt Section 18): do not build it now, but keep data models ready for it. Everything is built on the public website — no spreadsheets, no VBA, no desktop tools.


#### 2. Requirements of this calculator


##### 2.1 Functional requirements from Master Prompt V5.0

Every requirement below must end up implemented and verified, or marked NOT APPLICABLE / NOT SUPPORTED with a written reason. "Current state" is the starting point found in the earlier work; confirm it in Phase 0.


**Group 1 — Solar Resource and PV System Design (R001–R024, 24 active)**

| ID | Requirement (Master Prompt V5.0, verbatim) | Current state |
|---|---|---|
| R001 | Solar resource assessment using supplied or approved resource datasets. | Built earlier — verify |
| R002 | Global Horizontal Irradiance (GHI) processing. | Built earlier — verify |
| R003 | Direct Normal Irradiance (DNI) processing. | Built earlier — verify |
| R004 | Diffuse Horizontal Irradiance (DHI) processing. | Built earlier — verify |
| R005 | Plane of Array (POA) irradiance calculation. | Built earlier — verify |
| R006 | Solar azimuth calculation. | Built earlier — verify |
| R007 | Solar elevation and altitude calculation. | Built earlier — verify |
| R008 | Sun-path and solar-position modelling. | Partly built — complete |
| R009 | Module operating-temperature estimation. | Built earlier — verify |
| R010 | Temperature correction of PV electrical parameters. | Built earlier — verify |
| R011 | Horizon shading assessment. | Built earlier — verify |
| R012 | Near-object shading assessment. | Partly built — complete |
| R013 | Far-object shading assessment. | Partly built — complete |
| R014 | Row-to-row shading assessment. | Partly built — complete |
| R015 | Ground Coverage Ratio (GCR) calculation. | Partly built — complete |
| R016 | Tilt-angle selection and analysis. | Partly built — complete |
| R017 | Module orientation and azimuth analysis. | Partly built — complete |
| R018 | Fixed-tilt system configuration. | Built earlier — verify |
| R019 | Tracker rotation and operating-angle modelling. | Built earlier — verify |
| R020 | Tracker backtracking calculation. | Built earlier — verify |
| R021 | Bifacial gain and albedo modelling. | Built earlier — verify |
| R022 | Available installation area and capacity estimation. | Not built — build new |
| R023 | DC/AC ratio calculation and evaluation. | Built earlier — verify |
| R024 | PV block, inverter block and total plant capacity configuration and comparison. | Partly built — complete |

**Group 16 — Energy Yield, Losses and Plant Performance (R379–R400, 22 active)**

| ID | Requirement (Master Prompt V5.0, verbatim) | Current state |
|---|---|---|
| R379 | Annual energy-yield calculation. | Built earlier — verify |
| R380 | Monthly energy-yield calculation. | Partly built — complete |
| R381 | Hourly energy-yield modelling. | Built earlier — verify |
| R382 | Performance Ratio (PR) calculation. | Not built — build new |
| R383 | Module-temperature loss calculation. | Partly built — complete |
| R384 | Soiling-loss modelling. | Not built — build new |
| R385 | Shading-loss modelling. | Partly built — complete |
| R386 | Module-mismatch loss modelling. | Built earlier — verify |
| R387 | DC wiring-loss calculation. | Built earlier — verify |
| R388 | AC wiring-loss calculation. | Built earlier — verify |
| R389 | Transformer-loss calculation. | Partly built — complete |
| R390 | Inverter-efficiency and inverter-loss modelling. | Built earlier — verify |
| R391 | Inverter clipping calculation. | Built earlier — verify |
| R392 | Assumed plant-availability modelling. | Not built — build new |
| R393 | Module and system degradation modelling. | Not built — build new |
| R394 | Bifacial energy-gain modelling. | Built earlier — verify |
| R395 | Tracker energy-gain modelling. | Built earlier — verify |
| R396 | Curtailment-loss modelling. | Partly built — complete |
| R397 | Grid-unavailability-loss modelling. | Not built — build new |
| R398 | Auxiliary-load calculation. | Not built — build new |
| R399 | Net-energy-export calculation. | Partly built — complete |
| R400 | Comparison of alternative system configurations and modelled performance. | Partly built — complete |

##### 2.2 Additional requirements from this calculator's earlier documents

These came from the earlier README / prompt / requirement documents and are kept because they match Master Prompt V5.0.

| # | Requirement | Matches V5.0 |
|---|---|---|
| D01 | Weather/time series: TMY, historical, EPW/CSV, 8760/8784, timestamps, leap year, UTC/local | R001–R004 |
| D02 | Solar position and irradiance: zenith, azimuth, POA, IAM, spectral, Perez sky diffuse | R005–R008 (IAM/spectral feed R379–R381) |
| D03 | Array orientation and tracker: tilt, azimuth, pitch, slope, axis, backtracking, limits, stow, GCR | R014–R020 |
| D04 | Shading: horizon, obstacles, 3-D scene; electrical substring/bypass mismatch separate | R011–R014, R039, R385–R386 |
| D05 | Bifacial: bifaciality, albedo, row geometry, torque tube, rear mismatch | R021, R394 |
| D06 | Inverter: DC limits, AC nameplate, CEC/Sandia efficiency, PQ capability, clipping | R390–R391 here; R083, R087 → Inverter_LV_AC |
| D07 | PV thermal model (Faiman/SAPM), soiling, snow, spectral, degradation | R009, R383–R384, R393 |
| D08 | Electrical yield losses DC→POI: DC ohmic, mismatch, inverter, AC cable, transformer, auxiliaries, availability, curtailment | R386–R392, R396–R399 (cable resistances from DC/AC cable calculators) |
| D09 | Production reporting: hourly, monthly, annual, kWh/kWp, PR, CF, loss diagrams | R379–R382 |
| D10 | Module layout planning, string allocation, field segments, cable routes | R022 (area/capacity) in scope; automatic site layout needs approval (Group 17 boundary) |
| D11 | P50/P75/P90/P95/P99 uncertainty | Section 38 (only when statistically justified) |
| D12 | Run real pvlib and PySAM/SSC 8,760-hour benchmark with identical inputs | Sections 38, 67 |
| D13 | Live public-URL Playwright desktop/mobile acceptance | Section 95 |
| D14 | One PV list (39,400 primary records); one inverter list (5,739) (from the earlier Engineering_Master_Database documents) | Section 44 — Production's 26,000-record catalogue is reconciled into it |

##### 2.3 Engineering method and verification requirements (Master Prompt Part 7)

Apply Master Prompt Section 38 in full, plus:

- Weather provenance and quality checks; time-zone and timestamp alignment (UTC calendar) — a one-hour shift silently corrupts every annual result.
- Solar position and transposition against an independent implementation (e.g. pvlib run in a separate script) with the **same** inputs.
- The pending **pvlib ModelChain + PySAM/SSC 8,760-hour matched benchmark** (scripts in `04_EXTERNAL_VERIFICATION_KIT/`): same weather, same module/inverter parameters, same loss conventions; report hourly RMSE/MAE/bias, annual kWh and % difference. Different defaults must not be compared as if equivalent.
- Tracker backtracking angle against published equations; bifacial view-factor against a documented reference.
- Loss waterfall closes to the net energy (energy balance check).


##### 2.4 Shared engines and connections to other calculators (Sections 23, 45, 49)

- **Owns:** weather/resource service, solar geometry and POA, shading engine, tracker and bifacial engines, plant-sizing engine (R022–R024), energy-yield engine (Group 16).
- **Uses:** module and inverter records from the Master Database; string configuration from PV_String_MPPT; DC and AC cable losses from DC_Cable_Connector and Inverter_LV_AC (same resistance data — never a separate copy); transformer losses from Transformer_POI; export limits and curtailment from LV_Interconnection_Metering.
- **Gives:** POA, module temperature and site temperature extremes to PV_String_MPPT; plant/block configuration to the network model; yield and loss waterfall to Documentation_Schedules_QA.
- **Moves out:** string/MPPT logic, electrical screens, POI screen, embedded cable-thermal copy, embedded load-flow/fault studies — each to its own calculator.

| Direction | Data | From / to |
|---|---|---|
| In | Module, inverter records (with DB revision) | Master Engineering Database |
| In | String count, MPPT allocation, DC/AC ratio inputs | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector) |
| In | DC and AC cable losses | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector) / the LV AC calculators (Inverter_LV_AC, LV_Interconnection_Metering) cable engines |
| In | Transformer no-load and load losses | the MV calculators (MV_Electrical_Design, Transformer_POI) |
| In | Export limit, curtailment rules | the LV AC calculators (Inverter_LV_AC, LV_Interconnection_Metering) |
| Out | POA, module temperature, hourly irradiance | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector) (temperature-corrected Voc/Vmp, R026–R027 use the same site temperature basis) |
| Out | Plant capacity / block configuration (R024) | the DC calculators (PV_String_MPPT, DC_Protection_Combiner, DC_Cable_Connector), the LV AC calculators (Inverter_LV_AC, LV_Interconnection_Metering), the MV calculators (MV_Electrical_Design, Transformer_POI), Fault_Current_Studio network model |
| Out | Annual / monthly / hourly yield, loss waterfall | Documentation_Schedules_QA_Calculator reports |

Every connection is a typed, versioned contract; a changed upstream result marks this calculator's dependent results STALE.


#### 3. Work already done

**Status:** live at `https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/` (Wix site ID `ec984f80-a998-4e29-a38c-ca6b85dac4f4`). The live site had 253 assets (revision `d9b85e4e42e7059f7a92ef081496ccf68a79511b`); the last verified local copy had 240 assets. **Take the current live code as the starting point** and never overwrite it with an older copy.

**Functions already built (verify each):** weather import (PVGIS, EPW, CSV; 8,760/8,784 h, UTC calendar); solar position and POA (isotropic, Hay–Davies, Reindl transposition; Perez not yet); horizon and 3-D obstruction screening; fixed-tilt geometry; single-axis tracker geometry with rotated-axis azimuth fix and backtracking; bifacial view-factor model (fixed and tracker); substring/bypass mismatch model; De Soto single-diode module model (280 independent SciPy operating points); CEC/Sandia inverter efficiency model with 2,343 fitted inverters (opt-in hourly coupling that refuses out-of-envelope configurations); hourly MPPT-current and transformer-kVA rejection gates; DC topology for combined feeder vs individual string loops; 8,760-hour per-stage energy reconciliation; P50/P75/P90 screening; 26,000-record PV module catalogue with a full qualification register; POI three-phase current/power vectors (100 independent checks).

**Known data state:** of 26,000 module records, 24,705 have a blank source field, 69 lack valid STC values, 39 have >7 % Vmp×Imp/Pmp mismatch, 19 duplicate maker/model names; zero records are qualified for construction use (a review flag, not proof of bad data). A former CSV export truncated at 250 rows — fixed; keep as a regression case. Demo defaults (5,000 kW PV, 12,470 V bus, 25° tilt, synthetic weather/horizon) are examples, never site data.


#### 4. Functions still to be added

- [ ] R002–R005 Perez (and other) transposition models with coefficient sources, IAM and spectral correction.
- [ ] R011–R014 verified near-object and row-to-row shading engine; documented far-object/horizon method; shading on bifacial rear side.
- [ ] R015 GCR calculation; R016 tilt selection and analysis; R017 orientation/azimuth analysis.
- [ ] R019–R020 tracker rotation limits, axis tilt, cross-axis slope and slope-aware backtracking.
- [ ] R022 available installation area and capacity estimation; R024 PV/inverter block and plant capacity configuration and comparison (separate engine from yield).
- [ ] R382 Performance Ratio; R384 soiling loss; R392 availability; R393 module and system degradation; R397 grid unavailability; R398 auxiliary load.
- [ ] R383, R385–R391, R396, R399 complete DC→POI loss chain with energy-balance closure; inverter loss from a parameterised efficiency model; DC/AC cable losses from DC_Cable_Connector and Inverter_LV_AC; transformer losses from Transformer_POI; curtailment and export from LV_Interconnection_Metering.
- [ ] R400 comparison of alternative configurations and modelled performance.
- [ ] Long-term resource uncertainty for P50/P90 with documented statistics.

**Requirements with no implementation yet (build new):**

- [ ] R022 Available installation area and capacity estimation
- [ ] R382 Performance Ratio (PR) calculation
- [ ] R384 Soiling-loss modelling
- [ ] R392 Assumed plant-availability modelling
- [ ] R393 Module and system degradation modelling
- [ ] R397 Grid-unavailability-loss modelling
- [ ] R398 Auxiliary-load calculation

#### 5. Earlier work still to be completed

- [ ] Export the live 253-asset (or newer) code, compare it with the 240-asset copy, put both in Git, inventory every module.
- [ ] Move string/MPPT logic to PV_String_MPPT_Calculator; electrical/NEC screens to DC_Protection_Combiner, Inverter_LV_AC and LV_Interconnection_Metering; POI/PPC to LV_Interconnection_Metering and Transformer_POI; the embedded cable-thermal copy to Cable_Thermal_Calculator; the radial load-flow / fault-magnitude studies to Fault_Current_Studio — results must match after each move.
- [ ] Remove from the website any module that earlier versions contained outside V5.0 scope.
- [ ] Reconcile the 26,000-record PV catalogue and the CEC inverter library into the Master Database (all IDs kept); resolve provenance of blank-source records from manufacturer datasheets where obtainable; keep unresolved records flagged.
- [ ] Security review: no private or licence-restricted source code served publicly.

**Partly built — complete them:**

- [ ] R008 Sun-path and solar-position modelling
- [ ] R012 Near-object shading assessment
- [ ] R013 Far-object shading assessment
- [ ] R014 Row-to-row shading assessment
- [ ] R015 Ground Coverage Ratio (GCR) calculation
- [ ] R016 Tilt-angle selection and analysis
- [ ] R017 Module orientation and azimuth analysis
- [ ] R024 PV block, inverter block and total plant capacity configuration and comparison
- [ ] R380 Monthly energy-yield calculation
- [ ] R383 Module-temperature loss calculation
- [ ] R385 Shading-loss modelling
- [ ] R389 Transformer-loss calculation
- [ ] R396 Curtailment-loss modelling
- [ ] R399 Net-energy-export calculation
- [ ] R400 Comparison of alternative system configurations and modelled performance

**Built earlier — rebuild/carry over to the website and prove with tests:**

- [ ] R001 Solar resource assessment using supplied or approved resource datasets
- [ ] R002 Global Horizontal Irradiance (GHI) processing
- [ ] R003 Direct Normal Irradiance (DNI) processing
- [ ] R004 Diffuse Horizontal Irradiance (DHI) processing
- [ ] R005 Plane of Array (POA) irradiance calculation
- [ ] R006 Solar azimuth calculation
- [ ] R007 Solar elevation and altitude calculation
- [ ] R009 Module operating-temperature estimation
- [ ] R010 Temperature correction of PV electrical parameters
- [ ] R011 Horizon shading assessment
- [ ] R018 Fixed-tilt system configuration
- [ ] R019 Tracker rotation and operating-angle modelling
- [ ] R020 Tracker backtracking calculation
- [ ] R021 Bifacial gain and albedo modelling
- [ ] R023 DC/AC ratio calculation and evaluation
- [ ] R379 Annual energy-yield calculation
- [ ] R381 Hourly energy-yield modelling
- [ ] R386 Module-mismatch loss modelling
- [ ] R387 DC wiring-loss calculation
- [ ] R388 AC wiring-loss calculation
- [ ] R390 Inverter-efficiency and inverter-loss modelling
- [ ] R391 Inverter clipping calculation
- [ ] R394 Bifacial energy-gain modelling
- [ ] R395 Tracker energy-gain modelling

#### 6. Checks and audits still to be done

- [ ] Real pvlib ModelChain and PySAM/SSC (PVWatts/SAM) runs with the same 8,760-hour weather, module/inverter parameters and loss conventions; report hourly RMSE/MAE/bias, annual kWh and % difference per loss stage.
- [ ] Solar position and transposition against an independent implementation; leap-year, time-zone, polar-site and EPW edge cases.
- [ ] Tracker backtracking angles against published equations / pvlib on flat and sloped terrain; bifacial view factors against a documented reference.
- [ ] Live public-URL Playwright tests on desktop and mobile: inputs, numerical results, module loading, 26,000-record browsing, reports, CSV exports, network errors.
- [ ] Engineering and software audit of every engine; challenge campaign of 200 cases.
- [ ] Engineering audit (Section 72): methods, editions, equations, assumptions, units, correction factors, limits, source applicability, independent results, unsupported conditions, false PASS results.
- [ ] Software audit: logic, architecture, types, validation, error handling, edge cases, duplication, hard-coded values, feature preservation.
- [ ] Data audit: units, types, sources, revisions, duplicates, conflicts, missing values, overrides.
- [ ] Integration audit: schemas, IDs, dependencies, STALE results, unit and method compatibility with connected calculators.
- [ ] UI/UX audit: workflow, labels, units, help, warnings, responsiveness (desktop/tablet/mobile), WCAG 2.2 AA accessibility, diagram accuracy, persistence.
- [ ] Security audit: authentication, authorisation, private/public separation, secrets, input validation, dependencies.
- [ ] Requirements coverage audit: every ID in Section 2 traced to code and tests.
- [ ] Research register and benchmark matrix (Section 11): relevant calculators, manufacturer tools and papers actually reviewed (count only real sources).

#### 7. Process to follow to finish everything

This is the Master Prompt V5.0 development process applied to this calculator. Follow it for every task.

##### 7.1 Rules that never change (Master Prompt Sections 5, 6, 27, 34)
- Never invent a standard, clause, formula, coefficient, equipment rating, curve, clearing time, test result or repository state. Missing critical input → **INPUT REQUIRED**; never a guessed value.
- Every numerical result comes from a deterministic, tested calculation engine — never from AI text.
- Every result shows a status: PASS, FAIL, REVIEW REQUIRED, INPUT REQUIRED, NOT APPLICABLE, NOT VERIFIED, NOT SUPPORTED or STALE. Never PASS when inputs, sources, methods or validation are missing. Every warning says what is wrong, why it matters, which input to check and how to fix it.
- Never claim "complete", "100 %", "certified" or "compliant" without the evidence of Section 8 of this file.
- Preserve working calculations and verified results; change a validated method only with a documented reason and tests (Sections 52, 58).
- No paid service, subscription or usage-based dependency without the owner's approval (Section 84). No credentials, secrets or private data in code or Git (Sections 6, 60, 82).

##### 7.2 Execution cycle for every task (Master Prompt Section 7)
**A Inspect** existing code, data, tests and behaviour → **B Research** primary standards, editions, manufacturer documents, papers and benchmark tools → **C Design** inputs, outputs, units, equations, sequence, error states, tests → **D Implement** the deterministic engine first, then UI, diagrams, data and integration, in small reversible changes → **E Test** (unit, golden, independent, boundary, invalid input, regression, UI, export, security, end-to-end) → **F Audit and fix** (separate engineering and software reviews; repeat until no critical/high defect) → **G Improve** (low-risk improvements; scope expansion only with approval) → **H Report** (files changed, tests run, results, limitations, next step). Do not stop after a plan, a draft or a good-looking screen.

##### 7.3 Phases (Master Prompt Section 62)
| Phase | Work for this calculator | Done when |
|---|---|---|
| 0 Inventory (read-only) | Export and reconcile the live code; module inventory; map R001–R024 and R379–R400. | Phase 0 report delivered: inventory, current state of every requirement with evidence, risks, batch plan, single best next task |
| 1 Engine | Repair/confirm resource, geometry/POA, shading, tracker, bifacial, plant-sizing and yield engines as separate engines with contracts. | Every engine has a master specification, version ID, typed contract and test harness, separate from UI |
| 2 Functional verification | Inputs, units, defaults, overrides, sequence, equations, intermediate values, invalid inputs, lookups, warnings, repeatability | All found defects fixed and retested |
| 3 Challenge and independent validation | Matched pvlib/PySAM benchmark, hand calculations for solar position and POA, golden yield cases for P1–P5, challenge campaign. | Golden and independent checks pass within justified tolerances; real challenge-case count reported |
| 4 Customer workflow | Which inputs are visible, auto-filled from the database, inherited from other calculators, optional or advanced | Workflow specification written |
| 5 UX/UI | Site and resource dashboard, sun path, shading views, tracker/bifacial views, monthly/hourly charts, loss waterfall. | Screens use the shared design system; visuals come from the same data as the calculation |
| 6 Integration | Connect to PV_String_MPPT (strings), cable calculators and Transformer_POI (losses), LV_Interconnection_Metering (export limits), Documentation_Schedules_QA (reports). | End-to-end tests pass; STALE tracking works |
| 7 Release | Staging and public-URL tests, monitoring, backup/restore, rollback | Owner approves the production release |

##### 7.4 Engineering rules for every engine (Master Prompt Sections 28–33, 46–48)
- Before coding: master specification (purpose, inputs with units and sources, outputs, standard and edition, equations, assumptions, valid range, warnings, test cases) and data-dictionary entries with stable names (no "Value1", "Factor2").
- Engine: defined inputs/outputs/units, equations with variable definitions, source references (standard, edition, clause/table/equation), assumptions, valid range, input validation, numerical precision/convergence, version ID; identical results for identical inputs; handles zero/negative/huge/tiny/missing values; never shows a failed calculation as a normal result; separate from the UI.
- Units: one central unit service; SI internally; feet/inches and metric display; never mix units silently; full precision internally, round only for display.
- Assumptions classified CONFIRMED / DEFAULT / ENGINEERING ASSUMPTION / NEEDS USER CONFIRMATION / REFERENCE-DERIVED. Show DATABASE VALUE, USER OVERRIDE and VALUE USED IN CALCULATION for every overridable value, with an audit trail.
- Equipment data only from the Master Engineering Database (Section 44), auto-populated from the selected record, with the database revision stored with every result.
- Traceability chain for every important result: user input → database value → intermediate values → correction factors → equation/method → final result, with the limit used, status, warnings, sources and engine version.
- Results passed to other calculators go through typed, versioned contracts; changed upstream inputs mark downstream results **STALE** (Sections 47–50). Never propagate a result whose study case, topology, voltage level or assumptions do not match.

##### 7.5 Testing rules (Master Prompt Sections 66–71)
- Golden tests with source of expected values; independent hand calculations or separate scripts (never only against code that shares the implementation); tolerances justified, never chosen to make a test pass.
- Boundary, invalid-input, unit-conversion, unsupported-case and regression tests; end-to-end tests for every applicable project type P1–P5.
- Challenge campaign toward **200 meaningful cases** designed to make the calculator fail; report the real number completed.
- Every reported defect: reproduce → trace UI → validation → lookup → calculation → result → saved state → fix the root cause → add a regression test → rerun all golden tests.

##### 7.6 Working method
- GitHub is the home of the code (Section 60): tag the known-good state before work, feature branch, small commits, pull request for engineering/data/security changes, tests run before merge, tagged releases with rollback points.
- Work in controlled batches (Section 61); after each batch give the delivery report of Section 87 (requested, done, requirement IDs, preserved, files changed, sources, tests run with results, remaining gaps, next step).
- Keep these records up to date in the repository (Section 85): REQUIREMENTS.md (traceability for every ID in Section 2), MASTER_SPECIFICATION.md, DATA_DICTIONARY.md, DECISIONS.md, SOURCE_REGISTER.md, TEST_MATRIX.md, ISSUES.md, PROGRESS.md, CHANGELOG.md.
- Ask the owner only when a decision changes safety, correctness, scope, cost, privacy or is irreversible (Section 5). The only open items the Master Prompt leaves to the owner are: jurisdiction and code editions to target first, hosting provider and budget, which calculator becomes the first fully verified reference module, whether 35 kV stays the MV limit (Appendix F) — plus approval of production releases and paid services.

##### 7.7 Calculator-specific rules carried over from the earlier documents
- One integrated Solar Resource / PV Production / Electrical platform; browser only; shared project inputs and contracts
- Temporary browser-local cases expiring within 24 h; no accounts or permanent storage without approval → **Decided: private project workspaces with accounts and server-side authorisation (Sections 79, 81); browser storage only as a non-authoritative draft cache (Section 44).**
- Never invent equipment specs or approvals; conservative assumptions labelled
- Units SI and imperial incl. ft/in; explicit internal conversions
- Match functions of PVsyst, HelioScope, SAM, PVWatts with original methods; never claim equivalence without evidence
- Engineering Source Register with page coverage; NEC 2023 only partly read
- Track every requirement with the Section 57 lifecycle (NOT IMPLEMENTED … ENGINEERING VALIDATED → DEPLOYED → APPROVED) instead of the earlier status words
- Public thermal-engine JS on the solar site may expose private source; review before public serving
- ChatGPT / Google AI Studio may assist input completion; never replace numerical kernels


#### 8. Final result — what "finished" looks like

- Separate, verified engines for resource, solar geometry/POA, shading, tracker/backtracking, bifacial, plant sizing and energy yield, all running on the website.
- Hourly, monthly and annual yield with a closed DC→POI loss waterfall, PR, every Group 16 loss, net export and alternative comparison.
- Documented, matched benchmark against pvlib and PySAM/SAM with error metrics; no claim of equivalence beyond that evidence.
- **Every requirement in Section 2** is implemented, or marked NOT APPLICABLE / NOT SUPPORTED with a written reason, with evidence in REQUIREMENTS.md (file, function, test). Status per requirement follows Section 57 (IMPLEMENTED — NOT VERIFIED → SOFTWARE TESTED → ENGINEERING VALIDATED → DEPLOYED → APPROVED).
- **Module completion checklist (Master Prompt Section 94) fully satisfied:** scope documented; existing behaviour preserved; methods and standard editions verified or flagged; deterministic engine separated from UI; inputs/outputs/units/assumptions/overrides/limits traceable; equipment data from the Master Database with revision recorded; golden and independent checks pass within justified tolerances; boundary, invalid-input, unsupported-case and regression tests pass; engineering and software reviews recorded; integration and STALE control work; UI and diagrams match the engine; reports and exports reproduce the screen; security and privacy satisfied; limitations disclosed; no known critical or high-severity defect.
- **Website acceptance (Section 95):** works on the public domain over HTTPS; its route opens directly and after refresh; desktop, tablet and mobile; private projects stay private; save/load/scenarios/recovery work; tested through the real public URL.
- **Reports and exports (Sections 77–78):** PDF calculation package, CSV, JSON (and schedules where applicable) containing inputs, units, equipment and database revision, method and standard edition, intermediate values, results with status, assumptions, warnings, sources and engine version — identical to the screen.
- **Honest status:** a final delivery report (Section 87) states exactly what is complete, what is not, and why.


#### 9. Plugins and tools

| Capability (Master Prompt Section 8) | Use it for | Example plugin / tool |
|---|---|---|
| Git repository connector | Branches, commits, pull requests, issues, diffs | GitHub connector |
| Coding agent + execution environment | Write code, run builds and tests, debug | Coding agent with terminal (e.g. Claude Code / similar), Node.js, Python |
| Web research and browser | Official standards information, manufacturer datasheets, benchmark tool documentation | Web search, web fetch, browser |
| PDF / OCR document reader | Read code books, datasheets, manuals, papers page by page | PDF reader with OCR |
| Owner's document storage | The owner's books and reference PDFs (for example the NEC books kept in Dropbox) | Google Drive / Dropbox connector |
| Academic research search | Peer-reviewed papers and DOIs | Semantic Scholar / Google Scholar / IEEE Xplore search |
| Independent maths check | Hand-check equations and reference values | Python (NumPy, SciPy, SymPy), Wolfram |
| Browser / UI automation | End-to-end tests on desktop and mobile, downloads, console errors | Playwright (Chromium, Firefox, WebKit) |
| Accessibility check | WCAG 2.2 AA audit | axe-core / Lighthouse |
| UI / diagram design | Wireframes, screen flows, engineering diagrams | Figma or similar design tool |
| Database connector | Schemas, migrations, records, permissions | Connector for the chosen database |
| Hosting / deployment connector | Deployments, logs, domains, rollback | Connector for the chosen host (Wix connector only for the existing live sites) |
| Security analysis | Dependency, secret and code scanning | Dependabot / npm audit / pip-audit, CodeQL, secret scanning |

Use a tool only when it has a real role, check access/permissions/quota/cost first, never claim a tool ran when it did not (Sections 8–9). Do not use Formula Genius or Coursera. When delegation is available, split work into specialist reviews — standards, independent calculation, equipment data, code/architecture, UX, security, testing — and have an independent checker that never sees the builder's code (Section 9).

**Specific to this calculator:**

| Purpose | Tool |
|---|---|
| PV modelling reference runs | pvlib-python, NREL PySAM / SAM SSC, PVWatts API |
| Weather data | PVGIS API, NSRDB / TMY / EPW sources |
| Feature benchmarking only | PVsyst, HelioScope, SAM documentation |

Benchmark tools are used to compare features and results only — never copy their code, data or methods without checking them (Section 12).


#### 10. First task and first reply

1. Reply in Hinglish: confirm that Master Prompt V5.0 governs and that this is **Production_Calculator**; say what you can and cannot access.
2. Do Phase 0 only (read-only): inventory, current state of every requirement in Section 2 with evidence, risks, batch plan for Phases 1–3, and the single best next task.
3. Wait for the owner to approve the Phase 0 report, then work through Sections 4, 5 and 6 using the process of Section 7 until Section 8 is true.
