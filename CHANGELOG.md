# Changelog

All notable changes to **EdiPrism X12 EDIFACT** are documented in this file.

## [1.1.0] - 2026-11-11

### Added
- **`999` / `997` Rejection-to-Source Pinpointer (`[Pro]`, `999` tab & banner action):**
  - Correlates `AK1`/`AK2`/`IK3`/`AK3`/`CTX`/`IK4`/`AK4`/`IK5`/`AK9` acknowledgments against outbound `837`/`834`/`270`/`850` X12 files, resolving 1-based `ST`-relative segment counts and `IK4-01` element/composite coordinates (`e` or `e:c`) directly to exact character selections in the source editor.
- **1-Click Unit-Test Fixture Slicer & HIPAA Scrubber (`[Pro]`, `Alt+Shift+X` & `Scrub & Slice` tab):**
  - Extracts the single broken `837` `2300` `CLM` claim, `835` `2100` `CLP` claim payment, `850`/`810` `PO1`/`IT1` line item, or `EDIFACT` `LIN` item at the cursor while retaining ancestor envelopes, re-numbering `HL-01`/`HL-02` sequentially (`1..N`) with zero orphan gaps, re-balancing `BPR-02`/`CTT-01`/`SE-01`/`UNT-01`, padding `ISA` to 106 bytes, and scrubbing PHI.
- **Native Loop Code Folding (`[+]`/`[-]`) & Inline Ghost-Text Hints (`[Pro]`, `Alt+Shift+I`):**
  - Collapses multi-line `X12` & `EDIFACT` transactions and business loops (`2000A/B/C`, `2300`, `2400`, `2100`, `PO1`, `SG2`, `SG25`) in the editor gutter, adds live `Path:` breadcrumbs in the `Loops` tab, and toggles non-mutating end-of-line loop/amount badges.
- **Git-Shared `.ediprism` Trading-Partner Schemas (`[Pro]`):**
  - Automatically discovers repository `.ediprism/partner-schema.{xml,json}` (or `.ediprism.{xml,json}`) walking up to the project root and merges partner validation rules into the `Validate` tab across all team seats.
- **Air-Gap-Safe Feedback Menu & Sanitized Crash Diagnostics (`?` ToolWindow Header & `IDE Fatal Errors`):**
  - Adds a zero-socket `?` Feedback & Support action on the `EdiPrism` ToolWindow header bar (`Report Bug`, `Suggest Feature`, and `Copy Air-Gapped Diagnostic Template`) plus a native IntelliJ `<errorHandler>` that redacts local file paths and raw exception messages so HIPAA PHI can never leak into a diagnostic report.

## [1.0.0] - 2026-11-11

### Added
- **Free Tier (Zero Startup Modals):**
  - Lenient `ASC X12` (`ISA` `<106`-byte Slack/Jira/Teams collapsed spaces + `CRLF`) and `UN/EDIFACT` (`UNA`) pre-normalizer with non-intrusive editor repair banner.
  - Stateless incremental lexer and customizable syntax highlighting engineered for `20–50+ MB` EDI batch files.
  - Hover / Quick-Doc (`Ctrl+Q`) segment, element, composite sub-element, and repetition coordinate tooltips (`NM1-08`, `CLM-02`, `SV1-01-2`).
  - 1-click **Wrap / Unwrap Single-Line EDI** (`Alt+Shift+E`) with `UN/EDIFACT` release-character (`?'`) safety.
- **`[Pro]` Tier (14-Day Free Trial via JetBrains Marketplace):**
  - **Hierarchical `HL` & Clean-Room Loop Tree (`EdiPrism` ToolWindow):** Reconstructs `837` (`1000A/B -> 2000A/B/C -> 2300 -> 2400`), `835` (`1000A/B -> 2000 -> 2100 -> 2110`), `850`/`856`/`810`, and `UN/EDIFACT` (`SG2 NAD -> SG10 CPS -> SG25 LIN`) with two-way editor caret & element-table sync.
  - **1-Click (`Alt+Shift+D`) & Native `Ctrl+D` Semantic EDI Diff:** Masks volatile `ISA`/`GS`/`ST`/`BHT` and `UNB`/`UNH`/`UNT`/`UNZ` timestamps & control numbers while normalizing single-line vs. multi-line streams.
  - **Offline Trailer, Orphan `HL` & `BigDecimal` Financial Balancing Validator:** Checks `SE01`/`GE01`/`IEA01`, `UNT01`/`UNZ01`, duplicate/self/orphan `HL` parent IDs, `CLM-02 == sum(SV1-02/SV2-03/SV3-02)`, `BPR-02 == sum(CLP-04) - sum(PLB)`, and `850` `CTT-01 == count(PO1)`.
  - **Deterministic Offline HIPAA / PII Test-Fixture Scrubber:** Width-preserving `HmacSHA256` anonymizer maintaining cross-file `837 <-> 835` referential integrity, Safe Harbor `>89` birth-date clamping (`19000101`), standard Luhn check digits, and `80840` ISO Luhn 10-digit NPI check digits.
  - **Bring-Your-Own-Schema (`BYOS`) & Offline Export:** Supports native `StAEDI` v3/v4 XML schemas and JSON schemas plus offline Hierarchical JSON and Tabular CSV export.
