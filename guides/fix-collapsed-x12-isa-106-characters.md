# How to Fix "ISA Segment Must Be 106 Characters" After Slack, Jira, or Teams Copy-Paste

## Why Copy-Pasted `ASC X12` Files Fail Strict EDI Parsers

In **ASC X12** (`837`, `835`, `270`, `271`, `850`, `856`, `810`, `999`), the `ISA` Interchange Control Header is a **strictly fixed-width `106`-byte record** (counting from `I` through the segment terminator `~`):

| Coordinate | Fixed Length | Common Failure When Pasted in Slack / Jira / Teams / Email |
| :--- | :---: | :--- |
| `ISA-01` | `2` | Preserved (`00`) |
| `ISA-02` | **`10`** | `10` blank spaces collapsed down to `1` or `0` spaces |
| `ISA-03` | `2` | Preserved (`00`) |
| `ISA-04` | **`10`** | `10` blank spaces collapsed down to `1` or `0` spaces |
| `ISA-05` | `2` | Qualifier (`ZZ`, `01`, `30`) |
| `ISA-06` | **`15`** | Trailing right-padded spaces stripped (`SUBMITTER01` becomes `11` chars instead of `15`) |
| `ISA-07` | `2` | Qualifier (`ZZ`, `01`, `30`) |
| `ISA-08` | **`15`** | Trailing right-padded spaces stripped (`PAYER01` becomes `7` chars instead of `15`) |
| `ISA-09`..`ISA-12` | `6`, `4`, `1`, `5` | Date (`YYMMDD`), Time (`HHMM`), Repetition (`^`), Version (`00501`) |
| `ISA-13` | **`9`** | Leading zeros stripped (`42` instead of `000000042`) |
| `ISA-14`..`ISA-16` | `1`, `1`, `1` | Ack Requested, Usage (`P`/`T`), Component Element Separator (`:`) |

Because strict X12 parsers read the element separator at byte index `3`, the component separator at byte index `104`, and the segment terminator at byte index `105`, **a single collapsed space shifts every delimiter offset and causes an immediate `Invalid ISA length (< 106)` crash**.

---

## 100% Offline 1-Click Fix Inside IntelliJ IDEA, PyCharm, DataGrip, Rider & GoLand

> **HIPAA / PCI / NDA Warning:** Never paste production `837` healthcare claims, `835` remittances, or `850` retail purchase orders into online "free EDI formatter" websites.

With **[EdiPrism X12 EDIFACT](../README.md)** (**Free Tier** — 100% offline, zero network calls):

1. Paste or open your broken `.x12` / `.837` / `.835` / `.edi` snippet directly inside your JetBrains IDE.
2. EdiPrism's lenient pre-normalizer detects delimiters token-by-token even when `ISA` is only `64–95` characters long, displaying a non-intrusive yellow banner:
   `X12 837 • Collapsed ISA (75/106 chars)`
3. Click **`Fix ISA (106c)`** on the editor banner:
   - Right-pads `ISA-02` (`10`), `ISA-04` (`10`), `ISA-06` (`15`), and `ISA-08` (`15`) with exact ASCII spaces,
   - Left-pads numeric `ISA-13` control numbers to `9` digits (`000000042`),
   - Preserves your active element, repetition, component, and segment delimiters (`Ctrl+Z` undoable).
4. Press **`Alt + Shift + E`** (or click **`Wrap (1/Line)`**) to toggle between single-line wire streams and readable 1-segment-per-line formatting.
