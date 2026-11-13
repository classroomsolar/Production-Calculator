# Solar PV Engineering Platform — work executed on 26 October 2026

## Site and release proof

- Original site: https://headless-spqwsttbeuf-classroomsolar-140e.wix-site-host.com/
- Wix site ID: `ec984f80-a998-4e29-a38c-ca6b85dac4f4`
- Authenticated current revision: `1daff091e0192dd988a986888d6c191c1934b420`; **240 serving assets**.
- Guarded Wix publication ID: `7541f3c1-c132-4b1a-b362-0346b7c67a38`.
- Exactly 2 active catalogue source files updated, 2 previous originals + 1 manifest added to site backups; **0 deletions**.
- Both published active source files passed Wix authenticated readback FNV checks. FNV is a consistency check, not cryptographic proof. Sources in local archive include SHA256 checksums.
- All **26,000 canonical manufacturer-module source records and their unique IDs were retained**. Original JSON SHA256 `89eebca9d597ddd2361d5f81beba564fa1772857e366011e264fcba785c47f5c`.
- Backup archive is **reconstructed from authenticated prior 237-asset snapshot** and verified patch/backup bytes, not a fresh complete Wix ZIP download. Never blind-overwrite newer live revisions with the backup.

## New defect fixed in this work

Before the release, the catalogue UI inspected 26,000 records but exported only **250 selected anomaly examples**, potentially leaving users with an incomplete review register. Added `fullQualificationRegister` to produce a **20-column, all-26,000-record, read-only** manufacturer traceability and qualification register, including every source ID, electrical fields, STC consistency flags, source classification, external reference status and recommended review priority. The UI now exports this complete CSV instead of the first 250.

## Verified re-runs in this work

- **107/107** original engineering regression assertions passed.
- **90/90** embedded browser module desktop/mobile integration checks passed, with no unchecked code changes to the older engines.
- **13** new full-catalogue assertions passed against the complete 26,000-record canonical catalogue.
- **14/14** desktop/mobile real download UI checks passed with a 7-record browser fixture. Complete 26,000-row register data and identity were separately validated with Node and Python. The large 26,000-record browser fixture stalled; **do not describe that as a passing full-size browser acceptance**.
- **280** independently solved SciPy single-diode operating points passed.
- **766/766** Pxx-to-finance research sensitivity assertions passed.
- **100/100** independent Python checks matched three-phase POI current/power vectors.

## Manufacturer data accountability

- Full source register produced: **26,000 rows, 26,000 unique IDs**.
- **24,705** records have a blank source field and **1,159** have a non-external source reference (categories are disjoint based on the current source checker).
- **69** source records lack valid complete STC numeric/relationship conditions; **39** valid-STC records have >7% `Vmp×Imp/Pmp` discrepancies; no OEM source records were changed or erased.
- **Zero** original rows are automatically qualified for detailed PV engineering and construction; that is intentional, not evidence that all modules are defective. An external CEC/SAM match alone does not verify current OEM certification, source revision, exact SDE model or site-specific electrical limits.
- The existing reference queue separately lists 58 substantial historical SAM STC discrepancies plus 12 ambiguous model identities. Several thousand records have no exact SAM identity. The new full register is a review aid, not an OEM certification.

## Five-stage state

1. **PV numerical modelling:** Selected pvlib 0.16.1 source kernels, 280 independent SciPy SDM points, and 8,760-hour per-stage energy reconciliation were checked. Full installed pvlib ModelChain, PySAM SSC/PVWatts annual equivalence **could not run** because the Python environment cannot retrieve packages (DNS/approved index unavailable), and no authenticated site-level solar weather/plant benchmark was supplied. Do not claim annual yield equivalence.
2. **Equipment:** Full source-level register completed, exported, validated and added to original website. Most records still require original OEM specification evidence; CEC self-reported entries are not automatically certified by this software. CEC reference: https://www.energy.ca.gov/programs-and-topics/programs/solar-equipment-lists
3. **Electrical/BESS:** Existing controlled three-phase POI and BESS research models passed engineering regression and independent checks. Actual feeder, PCS/BMS, utility protection and AHJ approval cannot be established without exact equipment and grid studies.
4. **P50/P90/finance:** Existing explicitly linked screening sensitivity passed 766 checks. It does not model joint financial uncertainty, full tariffs, tax/debt or a bankable probability distribution.
5. **Website:** Code published/read-back verified at Wix with backups. Local integrated Chromium workflow passed 90 checks. The public URL and local HTTP localhost navigation are blocked by this environment's Chromium administrator policy; hence **live public-host browser and external weather API acceptance are unverified**.

## What the external qualifications require

Actual site weather and plant topology; original current-revision module/inverter/PCS/transformer/OCPD datasheets and tests; utility interconnection/control/short-circuit ratings and protection requirements; project load/temperature/soil/tray layout where applicable; economics/tariff/financing input; a runnable environment with actual pvlib and NREL-PySAM/SSC installed; a browser allowed to visit the public Wix URL. Without these, no credible 100% engineering or bankability certification is possible.

**Result: verified production update and full review register delivered. External and project-specific qualifications are open and explicitly labelled, not quietly marked complete.**
