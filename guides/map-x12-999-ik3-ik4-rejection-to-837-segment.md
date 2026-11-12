# How to Map ASC X12 `999` (`IK3` / `IK4`) Rejections Directly to Your `837` / `834` / `850` File

## Why `999` Implementation Acknowledgments Are Painful to Debug by Hand

When a healthcare clearinghouse, Medicare MAC, Medicaid payer, or retail trading partner rejects an outbound `837P`, `837I`, `834`, `270`, or `850` interchange, the returned **`ASC X12 999` (or `997`)** file does **not** report line numbers.

Instead, `IK3-02` reports a **1-based segment count relative to `ST` (`ST == 1`)**—ignoring `ISA` and `GS`, and breaking completely if your outbound file was single-line wrapped or contained hundreds of `HL` / `CLM` / `SV1` segments:

```edi
AK2*837*0001*005010X222A1~
IK3*CLM*7*2300*8~
CTX*CLM01:BADCLAIM77~
IK4*5:2*1332*7*B~
IK5*R*5~
```

In this rejection:
- `AK2*837*0001` identifies Transaction Set `837` with `ST-02` control number `0001`.
- `IK3*CLM*7*2300*8` means the **7th segment starting from `ST` (`ST` = 1)** in Loop `2300` (`CLM`) has data element errors (`8`).
- `IK4*5:2*1332*7*B` means **Element `5`, Composite Sub-Element `2`** (`CLM-05-2`) contains invalid code value `'B'`.

In a `15,000`-segment batch `837` where `IK3-02` says `9482`, counting segments manually or writing throwaway scripts wastes 20–30 minutes per rejection.

---

## Pinpointing `999` / `997` Rejections Offline in 2 Clicks with EdiPrism

**[EdiPrism X12 EDIFACT](../README.md)** includes an air-gapped **`999 / 997 Rejection-to-Source Pinpointer`** right inside `IntelliJ IDEA`, `PyCharm`, `DataGrip`, `Rider`, `GoLand`, and `WebStorm`:

1. Open **either** your `999`/`997` rejection file **or** your outbound `837`/`834`/`270`/`850` source file.
2. Click **`Pinpoint 999`** on the top editor banner (or open the **`999`** tab in the right-hand `EdiPrism` tool window and click **`Pinpoint 999 / 997 Rejection...`**) and select the counterpart file.
3. Click any row in the pinpointed table:
   - EdiPrism matches `AK2-02` to the exact `ST..SE` transaction slice,
   - Counts `IK3-02` segments from `ST = 1` (regardless of single-line vs. multi-line formatting),
   - Resolves `IK4-01` simple (`2`) or composite (`5:2`) positions, and **highlights the exact offending character span** inside your outbound source editor.
4. Once your cursor is on the broken claim, press **`Alt + Shift + X`** (**Slice & Scrub Unit-Test Fixture**) to extract *only* that broken claim—with re-numbered `HL` hierarchy, balanced `SE-01` trailers, and scrubbed HIPAA PHI—straight into `src/test/resources/` as a regression unit test.
