# Solar PV Engineering Platform — five-stage validation continuation

Date: 2026-10-26. **Existing site retained:** https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/

**Published release**: authenticated Wix revision `fe4b08869861d49f35cb540d7a4aa2f90dd98037`; 237 serving files; 39 rollback manifests. Wix upload ID `ed1407b0-b543-4427-b3f3-1ff0fcf83bbc`. Three active files replaced, one new active module, three on-site source backups and one rollback manifest added. No deletion, no change to 26,000-row canonical module JSON. Four active changed/added JS files were independently read back with matching version fingerprints. Wix patch is non-atomic; avoid concurrent writers.

## Stage 1 — solar production physics and reference checks

- Existing energy regression: **107 checks pass**.
- Independent Python/Scipy: **280** synthetic De Soto operating points, independently solved I–V roots and maximum power via brentq and bounded optimizer. Peak relative error less than **2e-6** on Voc/Isc/Vmp/Imp/Pmp; maximum absolute differences documented in `single_diode_scipy_comparison.json`.
- Pinned original pvlib 0.16.1 PVWatts inverter function: **2,004** tests, 0 kW maximum difference in these test vectors.
- Pinned original pvlib 0.16.1 shaded_fraction1d / projected_solar_zenith_angle functions: **2,500** tests; maximum absolute shaded-fraction error about 2.3e-15; projection error about 5.0e-14 degrees.
- Synthetic **8,760-hour** full PV stage simulation reconciled **78,840** independent Python stage-power checks from DC module through POI including AC/DC ohmic and transformer losses.
- **Not achieved:** complete pip-installed pvlib ModelChain plus SSC/PySAM annual resource/tracker/bifacial validation. Both packages failed installation in this environment. Pinned selected reference functions cannot substitute for this. No actual project weather and production benchmark supplied.

## Stage 2 — equipment catalogue

- Canonical 26,000 PV records and all record IDs preserved, catalogue SHA-256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`.
- Original historical SAM identity classification: 8,202 exact STC five-field matches, 236 near-matches within 2%, 58 larger differences, 12 ambiguous identities, and 17,492 without exact reference identities.
- Local electrical screening flagged **108 records with at least one STC/value issue** (includes 106 with nameplate product deviation over a 7% triage threshold, and rows with nonpositive fields). A flag does not establish incorrect OEM data; see file-level CSV.
- `equipment_reference_exception_queue.csv` prioritises **70** historical SAM differences/ambiguities; `equipment_local_physics_anomalies.csv` tracks the 108 independent local physics flags. Manufacturer revision/datasheet verification is not complete. No records automatically overwritten.
- CEC authoritative list: https://www.energy.ca.gov/programs-and-topics/programs/solar-equipment-lists. Inclusion there is **not** a blanket OEM engineering approval; verify listing per utility/AHJ.

## Stage 3 — electrical, BESS, grid and POI

- **30,036** independently reproduced 3-phase POI/BESS numerical checks, with 2024 leap-year and regular-year dispatch cases.
- Existing conductor, source current, three-phase apparent power, thermal and tracker screening regression tests remain green.
- 5 MW export cap does not establish import current/transformer limits or upstream PV branch rating; failures are explicitly flagged.
- Physical grid and PCS control, protection coordination, fault studies, conductor termination/OCPD selection, thermal environment, inverter/BMS approval require original project data and cannot be certified.

## Stage 4 — P50/P90/finance

- **New published research feature:** explicit user-confirmed same-project Pxx annual yield to nominal pre-tax P50/P90 finance **scenario sensitivities**, four scenario rows (P50, independent P90, correlated P90, high-correlation stress).
- **766 dedicated finance numerical assertions pass.** Invalid/nonphysical percentiles are never substituted with zero. Changing annual yield, correlation, finance, or source data invalidates the comparison before it appears in a download or integrated report.
- This **does not** produce a P90 NPV or financeable probability distribution. Tax, debt, incentives, tariff/BESS dispatch and measured covariance remain outside this research model.

## Stage 5 — website release and acceptance

- **90/90** local Chromium desktop/mobile integration checks passed, including the new linkage, explicit source attestation, fresh/stale browser report handling, CSV/JSON controls, and existing electrical/BESS/browser interactions. Zero uncaught script errors in checked synthetic scenarios.
- Authentic Wix revision, asset count, upload response, pre-release source fingerprints, four published source fingerprints and on-site rollback presence verified.
- Direct Chromium navigation to public Wix host returns **ERR_BLOCKED_BY_ADMINISTRATOR** in this environment. Hence direct hosted browser acceptance and external PVGIS/API integration are **not verified**. Source-readback is not equivalent to live user acceptance.

## Release artefacts and integrity

- `Solar_PV_Engineering_Current_237_Asset_Release_2026-10-26.zip` has 238 archive entries including generated `wix.config.json` and 237 actual site assets. It is **reconstructed** from the authenticated previous 232-asset release plus verified changed files, and includes rollback paths. It is not a freshly downloaded Wix ZIP.
- `Solar_PV_Engineering_Five_Stage_QA_2026-10-26.zip` contains runnable tests, independent vector results, equipment exception list, existing regression and browser results, and pinned reference context.
- `Solar_PV_Engineering_237_Asset_Release_Integrity_2026-10-26.json` contains counts, hashes, source mutation ledger, and scope limitations.

**Release outcome:** Incremental production software improvement completed, validated and published on the existing site. **Full independent annual-model equivalence, original manufacturer certification, site-specific electrical approval and public browser acceptance remain unproven** and must not be represented as done.
