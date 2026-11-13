# Solar PV Engineering Platform — guarded continuation release

**Existing Wix site:** https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/  
**Site ID:** `ec984f80-a998-4e29-a38c-ca6b85dac4f4`  
**Authenticated published site revision:** `84a9e4ffdbafb03cc0474e9e9d82ee8df5e98809`  
**Published file-set:** 232 serving assets; 233 in reconstructed developer ZIP including `wix.config.json`.  
**Publication ID:** `cb6947f9-98d6-4c70-b1a8-f08b46ae96ad`.

## Changes now published to the SAME site

- New `grid-poi-screen.js` and `grid-poi-ui.js`: user-entered balanced three-phase bidirectional POI and separate PV feeder power/current/transformer rating snapshot. Independent import and export real-power caps; explicit net kvar, kVA and voltage at *each* conductor location. POI cap is not used to erase upstream branch current. No false claim to operating PPC/protection.
- `bess-dispatch.js` and `bess-ui.js`: optional 24-value daily UTC load profile, optional grid-import limit with rejection where demand cannot be physically supplied, peak grid import and load values. Original constant-load result maintained with omitted optional fields. AC bus is not POI.
- `main.js` and `index.html`: register POI screen and attach it to the existing engineering tab; include POI output in integrated Excel/print reporting; update the stale catalogue SHA-256 label to the actual 26,000-row `master_pv_modules.json` value. No master database edits.
- Added four exact old source files and one pre-release manifest in `release-backups/pre-grid-poi-bess-demand-20261025/`. All modified/deployed source files were read back through authenticated Wix helpers and all six FNV checks matched the tested local byte content. There were **zero deletions**. Wix `headless.files.patch` is **not atomic** against concurrent authors.

## Independently executed checks

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

## Remaining limits (not asserted complete)

- Complete model-to-model annual validation against installed `pvlib` and SSC/PySAM did not run; neither package is installed and the package index is unavailable. Pinned original pvlib function-level reference checks from earlier releases remain in the evidence ZIP. NLR/NREL's official SAM PySAM documentation makes clear that its SSC modules require matching default/model settings for meaningful comparisons.
- Original 26,000 rows remain preserved (SHA-256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`), but tens of thousands of records require independent manufacturer/document revision linkage and prior 58 STC discrepancies have not all been requalified. The CEC itself disclaims independent verification of manufacturer-reported properties.
- POI, MV equipment, NEC cable thermal, protection relay, BESS PCS/EMS, actual PPC dynamic response and finance Pxx continue to be research/preliminary screens; no protective relay setting, listing or construction certification was obtained.
- Network APIs and live Wix-hosted browser interactions were not fully acceptance-tested. Authenticated Wix deployment + file readback and local Chromium UI are distinct evidence.

## Recovery and file provenance

`Solar_PV_Engineering_Verified_Current_232_Assets_2026-10-25.zip` is **a reconstructed complete-source ZIP**, made from the prior exact published 225-file source archive and six authenticated modified/added files; it is NOT a new byte-for-byte ZIP downloaded from the Wix site. The generated `wix.config.json` must NOT be pushed as a serving asset. The on-site recovery manifest corresponds to the authenticated previous revision and original source fingerprints. Private complete backups should live in Library or private repository, not solely under publicly served `release-backups/` paths. **Never deploy an older ZIP to the site without a fresh revision check.**

Official reference URLs: https://www.se.com/us/en/faqs/FA338225/ (balanced three-phase current); https://www.energy.ca.gov/programs-and-topics/programs/solar-equipment-lists (CEC caveat); https://sam.nlr.gov/software-development-kit-sdk/pysam.html (PySAM/SSC).
