# VFFS Regulatory Compliance Dossier — Project Plan

Two tracks run in parallel for 12 weeks: SolidWorks practice that converges on real VFFS components, and an agent-gathered regulatory register that closes the open note at the top of `VFFS_Rice_FRS.md` — "looking into the actual spec of the machine from the industrial standards." The final project is a machine package: geometry plus the compliance basis for building, manufacturing and operating the line in Canada.

## Scope and deliverables

The project produces three artifacts, and the regulatory one is the spine — the CAD exists to give the regulatory findings something concrete to bind to.

1. **Regulatory register** — every Canadian instrument that governs building, manufacturing and operating this machine, each mapped to the FRS requirement it constrains, with the specific clause and what it demands. This is the agent-gathered deliverable.
2. **FRS Rev C** — the existing `VFFS_Rice_FRS.md` revised so each affected requirement cites its governing standard, and the three open notes under the header are closed or converted into stated assumptions.
3. **SolidWorks model package** — the forming and sealing subassembly, modelled to the dimensional constraints the register turns up (guard gaps, hygienic clearances, access for cleaning). Not the whole machine.

Explicitly out of scope: fabrication, EPLAN panel drawings, any actual certification submission, and the cartoning and palletising already excluded by the FRS. The register documents what compliance *would* require; it does not claim compliance.

## Track A — SolidWorks, converging on the machine

The practice set stops being practice at Level 9. `SolidWorks_practice/` holds 376 problems across 18 levels, and the ladder already has its own gates built in: `Level10_CSWAExamLevel` and `Level18_CSWPExamLevel`. Levels 1–9 cover sketching, end conditions, extrude/revolve/sweep, reference geometry, patterns and assemblies — which is everything needed to model a forming shoulder and a jaw assembly. So the last nine levels are not a prerequisite; they are optional depth you take only if the machine parts expose a gap.

| Phase | Levels | Covers | Output |
| --- | --- | --- | --- |
| A1 | 1–6 (153 problems, sample them) | Sketch, end conditions, extrude/cut, fillet, reference geometry, revolve | One part per sitting, logged |
| A2 | 7–9 (88 problems) | Feature patterns, sweep, assemblies and mates | Mate competence — the gate for A3 |
| A3 | Level 10 as a checkpoint | CSWA-level timed parts | Decide whether to book the exam |
| A4 | None — real parts | Forming shoulder, forming tube, jaw assembly, film mandrel | The model package in deliverable 3 |

**Rule for A1 and A2: one part per sitting, 20–40 minutes, one line appended to `SolidWorks_practice/log.md` — date, problem number, minutes.** Do not sample broadly or reorganise the folders. The sweep in Level 8 matters more than its 14 problems suggest, because the forming shoulder is a swept surface and it is the hardest geometry in the machine.

The convergence point is A4, and it is what makes this a project rather than a practice set: the parts you model there have dimensions the register constrains — guard opening sizes, clearance for cleaning access, radii that can actually be wiped down. Track B supplies those numbers.

## Track B — the regulatory register

"Built, manufactured and operated" are three different regulatory layers, and splitting the work that way keeps the agent from conflating them.

- **Manufactured** — the machine as a product: construction standards and the certification mark it would need to be sold.
- **Built and installed** — the machine as a one-off on a floor: field evaluation, since a custom machine carries no certification mark.
- **Operated** — the machine in production: worker safety, food safety, and the legal accuracy of what it puts in the bag.

### Work packages

Six packages, each sized to one agent session. Each produces register rows, not prose.

1. **WP1 — Machine safety and guarding.** Risk assessment method, guard dimensions and reach distances, interlocks, emergency stop, safety-function performance level. Governs the jaw guard, the film path and the weigher access doors.
2. **WP2 — Electrical construction and field approval.** Panel construction, the Canadian Electrical Code path, and what a field evaluation of an uncertified machine actually inspects.
3. **WP3 — Hygienic and food-safety design.** Food-contact surface finish, cleanability, disassembly for sanitation, drainage. Governs the forming tube, the weigher buckets and the product-contact path.
4. **WP4 — Net quantity and trade measurement.** This one is load-bearing and it already has a finding — see the table below.
5. **WP5 — Food-contact materials.** The barrier film, not the machine. Confirms the scope boundary rather than adding machine requirements.
6. **WP6 — Provincial operating duties.** BC worker-safety obligations on the operator, and whether a pre-start safety review applies.

### Register schema

One row per discrete requirement. The columns that matter are the last two — a row that does not name an FRS requirement and a verification method is a note, not a register entry.

| Column | Content |
| --- | --- |
| `REG-ID` | Sequential, e.g. `REG-012` |
| Instrument | Act, regulation or standard, with edition or SOR number |
| Clause | Exact section or clause — not "various" |
| Layer | Manufactured / Built / Operated |
| Requirement | What it demands, in one sentence |
| FRS requirement | The `FR-nn` or KPI it constrains, or `NEW` if the FRS is silent |
| Verification | FAT, SAT, document review, or third-party |
| Source | Link to the page actually opened |

### Agent brief

Each session gets the same shape: the work package, the FRS and problem statement as context, and a hard instruction to return clause-level citations with links or to report that a clause could not be located. **Standards text behind a paywall is the normal case** — CSA standards are not free. Where the agent can only establish that a standard applies and what its scope covers, that is a valid row with Clause marked `scope only, text not obtained`. An invented clause number is the one unacceptable output, so the brief should say so explicitly.

## Regulatory landscape

Eight instruments bind this machine, and they split cleanly across the three layers. This is the starting map for the register, not the register itself — each row below expands into several clause-level entries.

| Layer | Instrument | What it governs | FRS impact |
| --- | --- | --- | --- |
| Manufactured | [CSA Z432:23 Safeguarding of Machinery](https://www.csagroup.org/store/product/2430287/) | Risk assessment, guard construction, reach and opening distances, interlocks, emergency stop | Guard geometry — feeds the CAD directly |
| Manufactured | [CSA C22.2 No. 301:23 Industrial electrical machinery](https://scc-ccn.ca/standards/notices-of-intent/csa-group/industrial-electrical-machinery) | Electrical construction of linked mechanical/electrical equipment up to 1000 V, non-hazardous locations, 0–40 °C ambient | FR-11 control system, panel design |
| Built | [CSA SPE-1000:2025 Model Code for the Field Evaluation of Electrical Equipment](https://scc-ccn.ca/accreditation/bulletins/update-csa-spe-10002025-inspection-requirements) | Field approval of equipment carrying no certification mark — the path a one-off machine takes. 6th edition, December 2025; references C22.2 No. 301:23 for skids | Commissioning sequence, FAT/SAT scope |
| Built | Canadian Electrical Code Part I (CSA C22.1) | Installation, bonding, disconnecting means | Site interface section |
| Operated | [Safe Food for Canadians Regulations, SOR/2018-108 s. 53](https://laws-lois.justice.gc.ca/eng/regulations/SOR-2018-108/section-53.html) | Equipment must be corrosion-resistant, durable, withstand repeated cleaning and sanitizing, easy to disassemble for sanitation; food-contact surfaces smooth, non-absorbent, free of pitting, cracks and flakes; **and must carry instruments to control, indicate and record the parameters needed to prevent contamination** | Hygienic design across the product path; the recording duty lands on FR-11/FR-12 |
| Operated | [Consumer Packaging and Labelling Regulations s. 38–39, Schedules I–II](https://competition-bureau.canada.ca/en/accuracy-requirements-net-quantity-declarations) | Net quantity accuracy and the lot inspection test | §3 dosing objectives — see the finding below |
| Product | [Food and Drug Regulations B.23.001; Health Canada packaging materials](https://www.canada.ca/en/health-canada/services/food-nutrition/food-safety/packaging-materials.html) | Chemical safety of the barrier film; voluntary Letter of No Objection | Film spec only — confirms the machine is out of scope here |
| Operated (BC) | [WorkSafeBC OHS Regulation Part 12](https://www.worksafebc.com/en/law-policy/occupational-health-safety/searchable-ohs-regulation/ohs-regulation/part-12-tools-machinery-and-equipment) | Tools, machinery and equipment — safeguarding duties falling on the operator | Operating manual, lockout procedure |

### Finding: the FRS tolerance band is not the legal requirement

The FRS asks for 100% of bricks inside **980–1020 g**, a symmetric ±2%. Canadian law is asymmetric and tighter on the low side. Under [Schedule I, Part III, row 6](https://laws.justice.gc.ca/eng/Regulations/C.R.C.,_c._417/page-4.html), a declared net weight over 500 g up to 1 kg carries a tolerance of **15 g** — so the individual-package floor is **985 g, not 980 g**. A brick at 980 g is short by more than the tolerance, and the FRS's own acceptance band permits it.

The lot test has three conditions, and only the first is a capability question:

1. The sample's weighted average must be at least the declared quantity, **with no tolerance applied**.
2. The count of packages short by more than 15 g must stay under a sample-size-dependent limit.
3. No more than one package may be short by more than 30 g.

Two consequences for the design. First, overfill is not a legal violation at all — it is purely a cost, which means giveaway and compliance are separate objectives rather than two sides of one band. Second, the FRS's symmetric band and its Cpk ≥ 1.33 target should be restated as an asymmetric spec: a hard 985 g floor, a lot mean at or above 1000 g, and overfill minimised subject to those. That is the correction Rev C has to carry, and it changes what the check-weigher feedback loop is tuned for.

## Schedule

&#91;embedded content: two tracks, Oct 2026 to Jan 2027 · 1 gate\]

The structure is a dependency, not a calendar: the CAD track cannot model machine parts until the register has produced its dimensional constraints, so Register v1 on 6 December gates everything below it. Term finals pause both tracks for two weeks, which means the real delivery window is the winter break — roughly four working weeks, not twelve.

## Risks

| Risk | Why it is live here | Fallback |
| --- | --- | --- |
| Track A joins the graveyard | Four downloaded curricula sit at zero progress — `plc_exercises.md`, `cad_schematics_exercises.md` and `training_exercises.md` in `old_resumes/`, and `cpp_practice/` with no `.cpp` in it. The pattern is reliable | `log.md` is the only tracker. If week 2 ends with fewer than 5 parts modelled, cut Track A to Level 9 and drop the CSWA checkpoint rather than carrying a dead ladder |
| Standards are paywalled | CSA Z432, C22.2 No. 301 and SPE-1000 are all paid documents. The agent can establish scope and applicability from public pages but not quote clauses | Check SFU library access in week 1 — it changes WP1 and WP2 depth materially. Without it, those rows are `scope only` and the register says so |
| The agent invents clause numbers | Plausible-sounding citations are the characteristic failure mode, and a register is exactly the artifact where one is hardest to spot | Every row carries a link to a page actually opened. Spot-check one row in ten against its source before the gate |
| The winter window is shorter than it looks | Four working weeks, not twelve, and the SMSCP exam also lands in January | The gate date moves, the scope does not. Cut WP5 first — it confirms a scope boundary rather than adding requirements |
| Register expands into machine design | Regulatory research naturally pulls toward "and therefore the machine should…", which is a different and much larger project | The register records what compliance would require. It does not redesign the machine and it does not claim compliance |

## Decisions needed before week 1

Five, and the first one is the only one that blocks anything.

- [ ] **Does Rev C change the acceptance band, or annotate it?** The 980 g floor is below the 985 g legal floor. Either the FRS adopts the asymmetric legal spec and the dosing objectives are rewritten, or it keeps ±2% as an internal engineering target and records that the legal constraint is separate and tighter. Both are defensible; they produce different documents.
- [ ] **Which subassembly is the A4 CAD target** — the forming shoulder and tube, or the cross-seal jaw assembly? The shoulder is harder geometry and better practice; the jaw assembly is where the guarding and interlock requirements actually land, so it binds more tightly to the register.
- [ ] **Do you have SFU library access to CSA standards?** Worth ten minutes in week 1. It is the difference between clause-level rows and scope-only rows across WP1 and WP2.
- [ ] **Book the CSWA, or treat Level 10 as a self-check?** Booking it makes the December checkpoint real; not booking it costs nothing and keeps the ladder optional.
- [ ] **One work package per agent session, or batched?** One per session keeps the citations traceable and the output reviewable. Batching is faster and harder to audit.

## Sources

Pages opened for the landscape table and the net-quantity finding. Everything else in this plan comes from the project folder.

- [CSA Z432:23 — Safeguarding of Machinery](https://www.csagroup.org/store/product/2430287/) — current edition, paid
- [CSA C22.2 No. 301 — Industrial electrical machinery](https://scc-ccn.ca/standards/notices-of-intent/csa-group/industrial-electrical-machinery) — scope and exclusions
- [SCC bulletin — CSA SPE-1000:2025 inspection requirements](https://scc-ccn.ca/accreditation/bulletins/update-csa-spe-10002025-inspection-requirements) — 6th edition, December 2025
- [Safe Food for Canadians Regulations, SOR/2018-108 s. 53](https://laws-lois.justice.gc.ca/eng/regulations/SOR-2018-108/section-53.html) — conveyances and equipment
- [Competition Bureau — Accuracy requirements for net quantity declarations](https://competition-bureau.canada.ca/en/accuracy-requirements-net-quantity-declarations) — the three-condition lot test
- [Consumer Packaging and Labelling Regulations, Schedule I](https://laws.justice.gc.ca/eng/Regulations/C.R.C.,_c._417/page-4.html) — tolerance table; Part III row 6 gives 15 g for 500 g–1 kg
- [Health Canada — Packaging materials](https://www.canada.ca/en/health-canada/services/food-nutrition/food-safety/packaging-materials.html) — B.23.001 and the Letter of No Objection
- [WorkSafeBC OHS Regulation Part 12 — Tools, machinery and equipment](https://www.worksafebc.com/en/law-policy/occupational-health-safety/searchable-ohs-regulation/ohs-regulation/part-12-tools-machinery-and-equipment)
