# VFFS 1 kg Rice-Brick Packaging Machine — Design & Regulatory Specification

A self-directed systems-engineering study: specify a Vertical Form-Fill-Seal (VFFS) line
paired with a multi-head weigher (MHW) to package dry parboiled / instant rice as a **1 kg
firm-fill, block-bottom "brick,"** and establish the **Canadian regulatory basis** for
building, manufacturing, and operating the line.

**Status:** in progress · self-directed (also the course project for MSE 426, Design
Optimization). This is a **design and specification study — not a built machine.**

**Author:** Son Tung Dang

## What this is
Working top-down from a market read (the 1–5 kg bulk pack is the VFFS sweet spot), the
project moves from problem statement → functional requirement specification → a detail
design analysis that stress-tests the FRS targets → a consolidated boundary-condition
register → an OIML-derived fill-quantity spec → a Canadian regulatory & compliance
specification. The analysis is quantitative: it identifies four FRS Rev B targets that do
not survive (brick envelope, ±3 mm dimensional tolerance under volume control, zero grain
entrapment at a single cross-seal station, and the 1002 g mean) and states which must move.

## Documents (read in order)
| Document | What it is |
|---|---|
| `VFFS_Rice_Problem_Statement.md` | Objectives, scope, constraints |
| `VFFS_Rice_FRS.md` | Functional requirement specification + engineering KPIs |
| `VFFS_Rice_Detail_Design_Analysis.md` | Quantitative analysis; where the FRS targets conflict and which must move |
| `VFFS — Boundary Conditions and Design Constraints.md` | Consolidated BC/DC register reconciling the documents above |
| `Fill-Quantity Spec Derivation — 1 kg Rice Brick (OIML R87 R51).md` | Fill-weight numbers derived from legal metrology (OIML R87 / R51) |
| `VFFS Rice Brick — Canadian Regulatory & Compliance Specification (Rev A).md` | Canadian conformity model, duty-holders, deployment envelope |
| `VFFS Regulatory Compliance Dossier — Project Plan.md` | 12-week plan binding a SolidWorks model package to the regulatory register |

## Machine scope
Film unwind → web tracking → block-bottom tube forming → MHW discharge → settle / densify /
shape → back, bottom-gusset & top sealing → cut → discharge. Cartoning and palletising excluded.

## Sources
Built on VFFS vendor literature, journal papers, OIML / CSA / Health Canada standards, and a
patent landscape. Those third-party documents are **not redistributed here** — see
[`references.md`](references.md) for the citations.

## Note on formatting
Documents use Obsidian-style `[[wikilinks]]` for cross-references; they resolve inside an
Obsidian vault and render as plain text on GitHub.
