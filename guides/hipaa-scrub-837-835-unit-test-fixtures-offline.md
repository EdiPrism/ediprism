# How to Create HIPAA-Safe `837` & `835` Unit-Test Fixtures Offline (Without Breaking NPIs or Balancing)

## Why Regex Search-and-Replace Breaks Healthcare EDI Test Fixtures

Engineering teams working on `ASC X12 837` claims and `835` Electronic Remittance Advice (`ERA`) pipelines cannot commit raw production payloads to Git or paste them into AI coding assistants under **HIPAA (`45 CFR § 164.514`)**.

However, manually editing or regex-masking production EDI files breaks downstream parsers in four predictable ways:

1. **Invalid `80840` NPI Check Digits:** Replacing `NM1*85*...*XX*1234567893~` with `9999999999` fails ISO/IEC 7812 Luhn validation (which prepends the US healthcare prefix `80840` before computing the check digit).
2. **Broken `837 <-> 835` Referential Integrity:** Randomizing subscriber IDs (`NM1-09`) or patient control numbers (`CLM-01` / `CLP-01`) breaks reconciliation tests that match outbound claims to inbound remittances.
3. **HIPAA Safe Harbor Age `> 89` Leakage:** Leaving `DMG*D8*19300514~` unmasked violates Safe Harbor rules requiring birth dates over age 89 to be aggregated (`19000101`) and ZIP codes (`N4-03`) to be zeroed/generalized.
4. **Orphan `HL` Loops & `SE-01` / `BPR-02` Imbalances When Trimming Huge Files:** Deleting 499 claims out of a 500-claim `837` leaves gaps in `HL-01`/`HL-02` (`HL*1` -> `HL*342*1`), wrong `SE-01` segment counts, and mismatched `835` `BPR-02 == sum(CLP-04)` totals.

---

## Deterministic Offline Slicing & Scrubbing in JetBrains IDEs

**[EdiPrism X12 EDIFACT](../README.md)** executes **100% locally in-process** (zero network sockets, zero cloud uploads, zero telemetry):

- **Full-File Undoable HIPAA Scrub (`Scrub & Slice` -> `Scrub All HIPAA / PII in Active Editor`):**
  - Uses keyed local `HmacSHA256` hashing so identical member IDs, names (`NM1`, `PER`), addresses (`N3`, `N4`), and `REF` identifiers map deterministically across `837` and `835` files while preserving field widths.
  - Generates valid synthetic 10-digit NPIs (`NM1-08 == XX`) passing `80840` Luhn validation and preserves standard Luhn check digits on numeric card/member IDs.
  - Clamps `DMG-02` birth dates over age 89 to `19000101`.
- **1-Click Single-Claim Unit-Test Slicer (`Alt + Shift + X`):**
  - Place your caret on *any* single `837` `CLM` claim, `835` `CLP` claim payment, `850` `PO1` order line, or `UN/EDIFACT` `LIN` item and press **`Alt + Shift + X`**.
  - EdiPrism extracts **only** that claim + its required `ISA`/`GS`/`ST`/`1000A`/`1000B` + ancestor `HL` chain, re-numbers `HL` IDs sequentially (`1..N`), recalculates `BPR-02`/`CTT-01`/`SE-01`/`GE-01`/`IEA-01`, pads `ISA` to 106 bytes, and runs the HIPAA scrubber—producing a clean, 20-line regression fixture ready for Git and JUnit/Pytest/xUnit!
