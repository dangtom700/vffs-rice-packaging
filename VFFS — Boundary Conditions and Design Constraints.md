# VFFS Rice Brick — Boundary Conditions and Design Constraints

**Consolidated register, Rev C basis · 2026-11-01**

This is the reconciliation layer over the four existing documents. It creates no new requirements of its own: every line traces to one of them, or to a regulation read directly. Where they disagree, this register rules and says why.

## How to use this, and which document governs what

| Document | Date | Authority in this register |
| --- | --- | --- |
| [[VFFS_Rice_Problem_Statement]] | 2026-09-08 | Objectives and scope. Not a constraint source. |
| [[VFFS_Rice_FRS]] (Rev B) | 2026-09-13 | **Baseline.** Governs anything not overridden below. |
| [[VFFS_Rice_Detail_Design_Analysis]] | 2026-10-09 | Governs mechanical, cycle-time and sealing constraints. Its §10 Rev C table is adopted **except change #3** — see C-1. |
| [[Fill-Quantity Spec Derivation — 1 kg Rice Brick (OIML R87 R51)]] | 2026-10-18 | **Governs all fill-quantity numbers.** Supersedes FRS §8.2–8.3. |
| [[VFFS Rice Brick — Canadian Regulatory & Compliance Specification (Rev A)]] | 2026-10-26 | Governs all RGx-nn compliance constraints and the deployment envelope. |

The documents are a reasoning chain, not four independent opinions, and they are mostly consistent. Read in date order they build on each other — which is exactly why one link needs cutting (C-1).

**ID scheme.** `BC-n` = boundary condition, something given to the design from outside. `DC-n` = design constraint, something derived that the design must satisfy. Each carries its source ID in the originating document so traceability survives.

**Status.** 🔒 frozen · 🟡 provisional, pending a named test · ⬜ open, needs a decision or an external answer.

---

# Part 1 — Boundary conditions (the givens)

These are not negotiable by the design. They come from the product, the buyer, the law, or physics.

## 1.1 Product and material

| ID | Boundary condition | Value | Source | Status |
| --- | --- | --- | --- | --- |
| BC-1 | Product | Dry milled or parboiled rice, free-flowing, a_w ≤ 0.6, non-oily, no declared allergen in the stream | FRS §1.4; Reg A §1 envelope | 🔒 |
| BC-2 | Loose bulk density | 0.75–0.85 g/cm³ | FRS §1.4 | 🔒 |
| BC-3 | Achievable consolidation ratio | ≈ 1.00–1.20 (vibratory settling + tamping) | DDA §5.2 | 🟡 **T-1** |
| BC-4 | Nominal quantity Qn | 1000 g, declared "1 kg" | FRS §1.4; RGM-09 | 🔒 |
| BC-5 | Granular consolidation scales with **taps, not seconds**; most consolidation in the first 50–100 taps | Physics | DDA §3 | 🟡 **T-1** |

## 1.2 Deployment envelope (the limit specification)

Reg A §1. This is what the machine is warranted for; outside it the compliance burden returns to the buyer. **State it in the quotation, not after the order.**

| ID | Boundary condition | Declared limit | Status |
| --- | --- | --- | --- |
| BC-6 | Cleaning regime | Dry cleaning + controlled wet spot-cleaning of removable parts. No CIP, no hose-down, no high-pressure washdown | 🔒 |
| BC-7 | Area classification | Indoor, unclassified, 5–40 °C, ≤ 80 % RH non-condensing | 🔒 |
| BC-8 | Electrical supply | Declared nominal V/ph/Hz with a declared SCCR at the supply terminals | ⬜ buyer must declare |
| BC-9 | Personnel | Trained industrial operators; no public access, no minors | 🔒 |
| BC-10 | Jurisdiction | Primary British Columbia; Ontario and Quebec per Reg A §11 | 🔒 |
| BC-11 | Rice dust is a combustible dust | If the buyer's dust hazard analysis classifies the room, the electrical design changes entirely | ⬜ buyer confirmation (RGE-09) |

## 1.3 Legal boundary conditions — fill quantity

The single most important block in this register, because it is where the FRS and the law actively disagreed.

**Tolerance confirmation (new this revision).** The Fill-Quantity Derivation left one open item: *"Canada enforces net quantity through the Weights and Measures Act and the regulations above rather than by adopting R87 directly, so the Canadian tolerance table should be confirmed to match before these numbers are treated as settled."*

**That is now closed.** Read directly from the regulation: *Safe Food for Canadians Regulations* (SOR/2018-108), **Schedule 4, Table 3, Item 6** — consumer prepackaged food, non-catch-weight, metric mass, "> 500 g but ≤ 1 kg" → **tolerance 15 g**. Identical to OIML R87's T = 15 g. A 1 kg declaration falls in Item 6 (the band closes at ≤ 1 kg). Rice is non-catch-weight, so Table 1's 1 % catch-weight figure does not apply.

Two refinements this produces:

- **Citation.** Reg A §7 cites CPLR Schedule I (C.R.C. c. 417). For a *food* product the operative instrument is SFCR Schedule 4, administered by CFIA. Both give 15 g, so the number is robust either way — but cite SFCR Schedule 4 Table 3 Item 6 as primary and CPLR Schedule I as parallel.
- **Rule 3 differs between the two regimes.** OIML R87 allows **no** pack below T2; Canada (SFCR s. 200(4)(c), per the Competition Bureau's worked tables) fails a lot at **two or more** below T2, so one is tolerated. Canada is marginally more lenient. Design to the OIML rule — it is stricter and the design rejects at a higher limit anyway, so this is free.

| ID | Boundary condition | Value | Source | Status |
| --- | --- | --- | --- | --- |
| BC-12 | Tolerable deficiency T | **15 g** | SFCR Sch. 4 Tbl. 3 Item 6 = OIML R87 Tbl. 2 | 🔒 **confirmed** |
| BC-13 | T1 — individual-pack floor | **985 g** (Qn − T) | derived from BC-12 | 🔒 |
| BC-14 | T2 — serious deficiency | **970 g** (Qn − 2T) | derived from BC-12 | 🔒 |
| BC-15 | Rule 1 — lot average | ≥ **1000 g**, **no tolerance** on the average | SFCR s. 199(5); OIML R87 Rule 1 | 🔒 |
| BC-16 | Rule 2 — packs below T1 | ≤ **2.5 %** of the population (sampling plan: 32 units for lots 129–4 000; fails at 3 deficient) | OIML R87 Rule 2; SFCR s. 200(4)(b) | 🔒 |
| BC-17 | Rule 3 — packs below T2 | **zero** (design to OIML; Canada tolerates one) | OIML R87 Rule 3 | 🔒 |
| BC-18 | **There is no upper legal limit.** Overfill is lawful and merely expensive | — | Reg A §7; Fill-Qty Derivation | 🔒 |

> BC-18 is the hinge. It is why C-1 below goes the way it does, and why FRS OR-4 ("100 % within 980–1020 g") cannot stand as written.

## 1.4 Legal boundary conditions — the hard gates

| ID | Boundary condition | Value | Source | Status |
| --- | --- | --- | --- | --- |
| BC-19 | The machine may not be energised without an accepted certification mark or a field approval label | Binary, inspector-enforced at site, not cheaply retrofittable | RGE-01; BC Reg. 100/2004 | 🔒 ⬜ route undecided |
| BC-20 | Adopted safeguarding standard in BC is **CSA Z432-94**, not the current Z432:23 | Design to :23 + ISO 13857, **additionally** verify guard openings and reach distances against the -94 tables, meet the stricter, record both | RGS-02; OHS Reg. s. 12.3 | 🔒 |
| BC-21 | Adopted electrical code in BC | BC Electrical Code 2024 (CSA C22.1:24, 26th ed.), in force 2025-03-04 | RGE-02 | 🔒 |
| BC-22 | A lot code is a legal requirement, not a feature. A unique identifier is **not** an acceptable alternative for consumer packs | Coder failure is a regulatory stop condition | RGL-01; SFCR s. 92(2) | 🔒 |
| BC-23 | Ontario triggers a pre-start health and safety review because interlocked barrier guards are used | P.Eng-sealed report before use; s. 7(5) exemption route exists but depends on certificates + install records being on site | Reg A §11; O. Reg. 851 s. 7 | 🔒 |
| BC-24 | Health Canada issues **no** LONO for machine construction materials; the instrument is a supplier **Letter of Guarantee** | Promising a LONO is unkeepable | RGF-02, RGF-03 | 🔒 |
| BC-25 | Three compliance domains are **not the machine builder's to discharge** — food safety (process), food labelling content, and the employer's operating duties | Promise *capability*, not the packer's outcome | Reg A §2 | 🔒 |
| BC-26 | Dry low-moisture product ⇒ 3-A and IP69K are the **wrong** references; EHEDG Doc 22 governs. Wet-cleaning a dry rice line converts a dormant *Salmonella* risk into a growth risk | — | RGF-04; Reg A gap #7 | 🔒 |

---

# Part 2 — Design constraints (what the machine must satisfy)

## 2.1 Fill quantity and dosing

Governed by the Fill-Quantity Derivation, amended by C-1 and C-2 below.

| ID | Design constraint | Value | Derivation | Status |
| --- | --- | --- | --- | --- |
| DC-1 | Design mean fill μ | **1002 g** | BC-15 floor + sampling/drift margin. **Retained against DDA change #3 — see C-1** | 🔒 |
| DC-2 | Observed standard deviation σ_obs | ≤ **4.26 g** | Cpk 1.33 one-sided vs T1: (1002 − 985)/3.99 | 🔒 |
| DC-3 | Process standard deviation σ_proc | ≤ **4.18 g** | √(4.26² − 0.8²), removing instrument contribution | 🔒 |
| DC-4 | Checkweigher instrument σ | ≤ **0.8 g** at 1 kg (x = 1 basis, initial verification) | OIML R51-1, 500–1000 g net load band | 🔒 |
| DC-5 | Capability index | Cpk ≥ **1.33**, stated **one-sided against T1 = 985 g**. No USL term — no law caps overfill | OR-3 reframed; BC-18 | 🔒 |
| DC-6 | Lower reject limit | **987 g** = T1 + k·u_cw at k = 2, u_cw = 1.0 g. **Guard band sits above the legal floor, not below it — see C-2** | RGM-01 + DDA §6.3 method | 🔒 |
| DC-7 | Upper reject limit | **None on weight.** Over-mass is rejected on *compaction over-stroke* (DC-13), a mechanical limit, not a weight-conformance one | BC-18; DDA §5.2 | 🔒 |
| DC-8 | Resulting reject rate at DC-6 | ≈ 215 ppm — P(X < 987) at μ = 1002, σ = 4.26. Well inside KPI 7.2's ≤ 0.5 % | derived | 🔒 |
| DC-9 | Mean-trim loop | Rolling **100-brick** window, updated every **25** bricks, authority **±1 g** per update, alarm (not correction) if the computed offset exceeds 3 g. Specify as a slow trim, **not** a controller — it corrects μ, does nothing for σ, and will add variance if tuned hard | DDA §6.4 | 🔒 |
| DC-10 | Giveaway | 0.20 % ≈ 2 g/brick ⇒ ≈ 6 000 kg/yr at 25 bpm × 2 000 h. **Accepted** as the price of DC-2 and BC-15 margin — see C-1 | DDA §6.2 table | 🔒 |
| DC-11 | Tare | Determined **per film reel**, not once per format | RGM-04 | 🔒 |

## 2.2 Brick geometry

| ID | Design constraint | Value | Derivation | Status |
| --- | --- | --- | --- | --- |
| DC-12 | Brick envelope | **120 W × 120 H × 80 D mm (1152 cm³)**, target settled density **0.868 g/cm³**. Supersedes FRS §1.4's 120 × 150 × 80 (1440 cm³), which is 13–23 % empty and cannot be a firm brick by construction | DDA §5.1–5.2, change #1 | 🟡 **T-1** |
| DC-13 | Brick form control | **Hard-stopped shaping frame** sets the envelope; the **compaction stroke** absorbs incoming density variation. Density becomes the dependent variable | DDA §5.2, change #2 | 🟡 **T-1** |
| DC-14 | Feasible height window | **118–120 mm** at a 120 × 80 footprint. Narrow in *both* directions: taller ⇒ slack at high incoming density; shorter ⇒ cannot consolidate enough at low incoming density | DDA §5.2 table | 🟡 **T-1** |
| DC-15 | Compaction stroke position | Logged **per brick**. It is a free in-line density measurement and the early warning of incoming material drift | DDA change #2 | 🔒 |
| DC-16 | Height tolerance | ±3 mm — achievable **only** because set by tooling. Volume control gives a 16.3 mm spread and cannot meet it | DDA §5.2; KPI 7.3 | 🔒 |
| DC-17 | Loose charge volume to be accommodated during flight | up to **1333 cm³** (1000 g at 0.75 g/cm³) — the *tube* must pass this even though the *finished brick* is 1152 cm³ | FRS §8.4; DDA §5.1 | 🔒 |

> **New finding — DC-12 does not cross the labelling breakpoint.** Reg A §8 warns that the SFCR s. 229 numeral-height breakpoint at 258 cm² "sits inside the plausible design range" and that preprinted film makes a late geometry change scrap. Checked: the Rev B front face is 120 × 150 = **180 cm²**; the Rev C face is 120 × 120 = **144 cm²**. Both sit in the > 32 to ≤ 258 cm² band ⇒ **3.2 mm numerals either way**. The dimension change is safe on this axis. Neither document checked this.
>
> **But** it cuts front-face print area by 20 %, which tightens RGL-07 (all mandatory bilingual content must fit outside the exclusion zones). Re-confirm the fit with the film supplier before freezing DC-12, and re-check the ORYZ pack proportions — a 120 × 120 face is nearly square, versus the portrait face originally drawn.

## 2.3 Throughput and cycle time

| ID | Design constraint | Value | Derivation | Status |
| --- | --- | --- | --- | --- |
| DC-18 | Baseline rate | ≥ **25 bpm** ⇒ 2.400 s cycle | OR-1 | 🔒 |
| DC-19 | Cycle budget at 25 bpm | transport 0.800 s + jaw travel 0.15 s + settle 0.30 s ⇒ **1.150 s dwell available** | DDA §3 | 🔒 |
| DC-20 | **The rate limiter is seal dwell at the gusset corners** — not film speed, not motion architecture, not dosing. Everything else in the cycle has slack | — | DDA §3, §13 | 🔒 |
| DC-21 | Plain 2-layer seal dwell | 0.40 s minimum; 1 s at the recommended 130 °C production point; working range 0.5–2 s | DDA §4.3 (LUT trials) | 🟡 **T-2** |
| DC-22 | Gusset dwell penalty | Conduction-limited, ≈ (n/2)² in layer count ⇒ **4× at 4 layers, 9× at 6 layers** | DDA §4.1 | 🟡 **T-2** |
| DC-23 | Film web speed | 0.45 m/s peak at 25 bpm; 0.99 m/s at 45 bpm. **Not** the constraint | DDA §3 | 🔒 |
| DC-24 | Settling | **Concurrent** with film transport and next-charge flight. **No cycle time allocated to settling as a serial step.** Specify as **≥ 30 taps per brick** plus amplitude — not seconds | DDA change #6 | 🔒 |
| DC-25 | Reel autonomy at 40 bpm | 5–8 h ⇒ specify splice or dual mandrel if the stretch rate is pursued | DDA change #20 | ⬜ |

> **New finding — the baseline rate is infeasible with a single-station cross-seal, not just the stretch rate.** On the analysis's own numbers: a 4-layer gusset needs 4 × 0.40 s = **1.6 s** at the dwell floor, against **1.150 s available at 25 bpm** (DC-19). At the recommended 130 °C / 1 s point it needs 4 s. So DDA change #4 — separating the bottom-gusset seal from the cross-seal station — is a **requirement for OR-1**, not an enabler for OR-2. The DDA frames the dwell problem as what makes 40–45 bpm infeasible; carried through to the gusset it bites at 25 bpm too. Pending **T-2**, which is on the critical path for exactly this reason.

## 2.4 Sealing

| ID | Design constraint | Value | Derivation | Status |
| --- | --- | --- | --- | --- |
| DC-26 | Architecture | **Separate the bottom-gusset seal from the cross-seal station**; dwell set per layer count | DDA change #4; see finding above | 🟡 **T-2** |
| DC-27 | Cured seal strength | ≥ **1000 gf/25 mm (0.392 N/mm)** | DDA change #13 (Coles et al., for free-flowing rice) | 🔒 |
| DC-28 | Hot tack | Minimum specified **at fill temperature** — the fresh bottom seal carries the 1 kg drop at 0.22–0.35 N/mm on a molten seal. An unspecified load case in Rev B | DDA §4.4, change #13 | 🟡 **T-4** |
| DC-29 | Bottom support | The filled brick shall be **supported until the bottom seal cools** | DDA change #10 | 🔒 |
| DC-30 | Back seam | **Fin seal** — the laminate cannot seal inside-to-outside. Bar 200–305 mm with retract-on-dwell, or dual-bar / heated belt | DDA change #9 | 🔒 |
| DC-31 | Jaw temperature uniformity | **≤ ±3 °C** across the seal width; two heaters + two thermocouples per jaw, verified by thermal imaging | DDA change #19 | 🔒 |
| DC-32 | Acceptance requires **both** strength and leak testing | ASTM **F88** (strength) **and** F2096 / D3078 (leak), plus F1140 (burst), F1921 (hot tack). A seal can pass burst and still leak through a channel | FRS §5, §10; DDA change #13 | 🔒 |
| DC-33 | Grain-in-seal | Target **zero**. Mechanical clearing first (strippers + timed drop-and-seal) on every seal; serrated profile to bite through dust | OR-7; FRS §5 | 🟡 **T-3** |
| DC-34 | Jaw position/torque signal | A **logged statistical signature**, **not** a grain detector. Reframes FR-12 | DDA change #15 | 🔒 |
| DC-35 | Seal parameters recorded per pack or batch, retained and exportable | Quality requirement **and** a regulatory one — a defective seal on a 1 kg brick is a contamination pathway | RGF-11; SFCR s. 53(d) | 🔒 |

> **Unresolved trade-off (DC-33 vs DC-21).** Tool 1 (11 mm serrated) gave the **widest, most forgiving** seal window — which is what a multi-layer gusset needs. But a broad serrated seal is also the profile **most likely to trap grain in its valleys**. OR-7 and seal-window robustness pull in opposite directions and no analysis resolves it. **T-3 is the only way out.**

## 2.5 Material handling, web and forming

| ID | Design constraint | Value | Source | Status |
| --- | --- | --- | --- | --- |
| DC-36 | Film transport | Belt pick-up, **dual independent vacuum belts**, left/right error monitored; web tension **band** with alarms; static eliminator bar at unwind | DDA change #7 | 🔒 |
| DC-37 | Forming shoulder | Produced by 5-axis machining or additive **from the mathematical collar surface**; specify collar fillet radius, surface roughness, shoulder-to-tube gap; round tube + mouthpiece | DDA change #8 | 🔒 |
| DC-38 | Registration | Mark-based indexing corrects the **film index**, not the brick height. Only the shaping frame fixes the brick | DDA §5.3 | 🔒 |
| DC-39 | Registration error | ≤ half the narrowest exclusion-zone margin, over a full reel | RGL-05 | 🔒 |
| DC-40 | Funnel | A **qualified component** with anti-bridging features and an acceptance test — not an interface | DDA change #12 | 🔒 |
| DC-41 | Dust extraction | **Double-tube, split-annulus** extraction | DDA change #11 | 🔒 |
| DC-42 | Gusset hardware | Symmetric, positively located spreaders; recipe-set depth ±0.5 mm; specified tip radius; electronically set pneumatics | DDA change #18 | 🔒 |
| DC-43 | Weigher–bagger decoupling | Backup/memory hopper so the bagger never waits on a combination and the weigher can take an extra cycle for a tighter one | FRS §8.5 | 🔒 |

## 2.6 Compliance design constraints

Full requirement text in Reg A. Listed here only where they constrain the *mechanical or control design* rather than the paperwork.

| ID | Design constraint | Value | Source | Status |
| --- | --- | --- | --- | --- |
| DC-44 | Jaw guard interlock is **the defining safety function**: crushing force + 150–200 °C surface ⇒ irreversible injury | **PL d**, Cat. 3, dual-channel | RGS-05 schedule | 🔒 |
| DC-45 | Film-path / forming-tube and weigher-access interlocks | **PL d**, Cat. 3 | RGS-05 | 🔒 |
| DC-46 | E-stop | **PL d**, stop cat. 1 on servo film and jaw drives, cat. 0 on heater contactors; reset arms but does not start | RGS-06 | 🔒 |
| DC-47 | Heater over-temperature cut-out | **PL c**, Cat. 2, **independent** of the temperature controller | RGS-05 | 🔒 |
| DC-48 | Touchable surfaces | No bare metal above **65 °C** in normal operation; jaws enclosed and marked | RGS-07; ISO 13732-1 | 🔒 |
| DC-49 | Noise | ≤ **80 dBA** at 1 m, 1.6 m height, at rated speed. Interacts with DC-24 — high-amplitude settling is the likely source | RGS-11 | 🔒 |
| DC-50 | Manual reel mass | ≤ **23 kg**, else a lift aid ships as part of the machine | RGS-12 | 🔒 |
| DC-51 | Loss of air pressure shall not cause hazardous movement | Jaws and shaping frame spring-returned to safe or mechanically retained; verified by deliberate supply dump | RGP-06 | 🔒 |
| DC-52 | **Omit the on-machine air receiver**, or keep it below the BC exemption threshold | Carrying one without checking means the machine cannot legally be pressurised on arrival | RGP-01 | 🔒 ⬜ determination needed |
| DC-53 | Product-contact air purity | ISO 8573-1:2010 **Class 1:2:1** at point of use, point-of-use filtration, tested ≥ 2×/yr | RGP-08; RGF-09 | 🔒 |
| DC-54 | Food-contact metal | 304 min, 316L where cleaning chemicals contact; continuous dressed welds, passivated, **Ra ≤ 0.8 µm** | RGF-01 | 🔒 |
| DC-55 | Product zone | No product traps: no horizontal ledges, blind holes, hollow bodies, exposed threads or unsealed laps. Disassemble by hand or one tool | RGF-05, RGF-06 | 🔒 |
| DC-56 | Enclosure ratings matched to BC-6, **not exceeded** | Control enclosure Type 12; product-zone field devices Type 4X stainless. **No IP69K on the main panel** | RGE-07 | 🔒 |
| DC-57 | Lubricants | NSF H1 registered, ISO 21469 certified where incidental contact is credible | RGF-08 | 🔒 |
| DC-58 | Traceability export | Single plain-text file (timestamped CSV per lot, lot code + per-pack weight), deliverable within 24 h. A proprietary binary or HMI-only log **cannot** satisfy this | RGL-10, RGM-05; SFCR s. 91(1) | 🔒 |
| DC-59 | ICES-003 SDoC | Self-declaration + nameplate label. PLC + HMI + servo drives are in scope | RGN-01 | 🔒 |

---

# Part 3 — Conflicts resolved

## C-1 · Mean fill: 1002 g retained; DDA change #3 rejected

**The conflict.** DDA §6.2 and §10 change #3 recommend centring the mean at **1000 g** with reject limits guard-banded to 982/1018 g, claiming it "raises Cpk from 1.330 to 1.478, cuts out-of-band from 33 to 9 ppm, and removes the giveaway." The Fill-Quantity Derivation and Reg A both retain **1002 g**.

**Ruling: 1002 g stands. Change #3 does not carry into Rev C.**

**Why.** DDA §6.2 states its own premise explicitly — *"The band is symmetric about 1000 g, so an upward offset spends capability and money"* — and the whole argument rests on that symmetry. The same document's §6.5 then flags that the premise is unverified: *"The ±2 % band is an engineering tolerance. It is not obviously a legal one… the FRS should not freeze 980 g without someone checking."* It even names 985 g as the likely floor.

Nine days later the Fill-Quantity Derivation checked, and **falsified the premise**: there is a hard floor at 985 g and **no upper limit at all** (BC-18). The band is not symmetric. It is one-sided.

So §6.2's reasoning was sound *conditionally*, §6.5 named the condition, and the derivation resolved it against the condition. Change #3 simply never got retracted from the §10 table. This is a stale link in an otherwise sound chain, not an error of analysis.

**What adopting it would actually cost.** Once Cpk is one-sided against T1 (DC-5), it is *monotonically increasing* in μ — so lowering the mean lowers capability:

| μ | Cpk at σ = 4.26 g | σ needed for Cpk 1.33 | σ_proc after 0.8 g instrument |
| --- | --- | --- | --- |
| 1000 g | **1.17** ✗ fails OR-3 | 3.76 g | 3.67 g |
| **1002 g** | **1.33** ✓ | **4.26 g** | **4.18 g** |
| 1005 g | 1.56 | 5.01 g | 4.95 g |

Centring at 1000 g would **tighten** the process σ requirement by 12 % (4.18 → 3.67 g) — the exact opposite of §6.2's conclusion that "centring also relaxes the dosing requirement," which was true only against the upper limit that no longer exists. It would also sit the process exactly on BC-15's no-tolerance lot-average floor, with no margin for drift.

**The giveaway is real and is accepted.** 2 g/brick ≈ 6 000 kg/yr at 25 bpm × 2 000 h (DC-10). That is the price of 12 % σ relief plus Rule 1 margin. If it later needs attacking, the lever is **raising** σ performance and then trimming μ down toward 1000 g — in that order, never the reverse.

## C-2 · Guard band: 987 g, not 982 g

DDA §6.3's method is correct and is adopted: guard the *reject limit* against checkweigher uncertainty, not the mean, because a brick reading 981 g might truly be 979 g. But its arithmetic was computed inward from the **980 g** limit, giving 982 g at k = 2, u_cw = 1.0 g.

With the floor now at **985 g** (BC-13), the guard band must sit **above** it: **987 g** (DC-6). A reject limit of 982 g would pass legally deficient packs — the very failure mode RGM-01 exists to close.

## C-3 · Weight conformance: OR-4 cannot stand

FRS OR-4 requires "100 % of shipped bricks within 980–1020 g". Replaced by:

- **100 % of shipped bricks ≥ 987 g** by checkweigher rejection (DC-6), which makes BC-16 and BC-17 unfailable by construction regardless of process drift.
- **No upper weight conformance limit** (DC-7). Upper-side control is the mean-trim loop (DC-9), not rejection — rejecting a 1025 g brick wastes 1 kg of rice to recover 25 g of giveaway.
- Over-mass is caught as **compaction over-stroke** (DC-13, DC-15): a mechanical limit on what the hard-stopped frame can accept, with "over-stroke" as the reject reason rather than "overweight". This is what couples the dosing and brick-geometry problems, and writing the upper bound as a weight rule is what hid that coupling in Rev B.

## C-4 · σ target: 4.26 g, not 4.51 g

FRS §8.2's 4.51 g is correctly derived from its own two-sided assumptions. It is superseded by DC-2's 4.26 g, anchored one-sided to T1 — a threshold someone else defined. Rev B's figure came from the self-chosen 1020 g bound.

## C-5 · Brick envelope: 1152 cm³, not 1440 cm³

No real dispute — FRS §1.4's 1440 cm³ conflates the volume the charge needs *in flight* with the volume of the *finished brick*. Adopted per DDA change #1 (DC-12), 🟡 pending **T-1**.

## C-6 · Not a conflict, worth recording

FRS FR-14.3 ("per the destination market, e.g. machinery safety directive/standards, UL/CE controls") names **no applicable instrument in Canada**: there is no machinery directive here, CE marking has no legal standing, and a plain UL mark without the Canadian `c` prefix is not accepted. Delete and replace with the RGE and RGS blocks per Reg A §13 amendment 1.

---

# Part 4 — Open items

## 4.1 On the critical path — tests that unlock frozen numbers

| Test | Fixes | Blocks | Why it is cheap |
| --- | --- | --- | --- |
| **T-1** Tapped/settled density of the actual rice; consolidation ratio vs loose density, taps-to-asymptote, amplitude dependence | BC-3, BC-5, DC-12, DC-13, DC-14 | **The entire brick geometry.** The 118–120 mm window rests wholly on the 1.00–1.20 ratio assumption | Tapped-density apparatus. Does not need the machine |
| **T-2** Seal processing window for the **chosen rice laminate** — strength vs temperature × dwell, per layer count (2, 4, 6) | DC-21, DC-22, DC-26 | **The achievable rate, including whether 25 bpm needs a separate gusset station** | Lab sealer, T-peel 90°, 300 mm/min, ASTM F88 prep. Does not need the machine |
| **T-3** Grain-in-seal vs jaw profile, serrated vs flat, ± dust extraction, with deliberate fines loading | DC-33 | The only resolution of the OR-7 vs seal-window trade-off | Seal-strength distribution + leak test |

T-1 and T-2 together fix the brick dimension and the achievable rate — the two numbers everything else hangs off. Neither needs the machine. **This is still the cheapest next step.**

Also open: **T-4** hot tack at fill temperature (DC-28), **T-5** charge discharge profile, **T-6** belt pull vs mouthpiece drag, **T-7** checkweigher uncertainty u_cw at line rate (confirms DC-6's k·u_cw).

## 4.2 Decisions and external answers

| # | Open item | Owner | Blocks |
| --- | --- | --- | --- |
| 1 | **Electrical conformity route** — factory certification vs SPE-1000:2025 field evaluation. Must be decided **before the panel is built**; the evidence is gathered during the build, not after | Project | BC-19. A field evaluator who finds an uncertified sensor or an undetermined SCCR stops, and the line stays dark |
| 2 | **Is 40+ bpm contractual?** Drives box-motion vs continuous / twin-former and the whole dwell budget | Buyer | DC-18, DC-25, DC-26 |
| 3 | **Measurement Canada written determination** on whether the checkweigher needs type approval (GEN-41). A 1 kg brick is a standard quantity, which points toward exemption, but the bulletin does not name checkweighers | Measurement Canada | RGM-06. Specify an approvable device meanwhile (RGM-07) |
| 4 | **Air receiver exemption threshold** determination against the regulation text, not a vendor claim | Project / TSBC | DC-52 |
| 5 | **Buyer declarations**: available fault current at supply terminals; written confirmation the area is unclassified | Buyer | BC-8, BC-11 |
| 6 | **Exclusion-zone drawing** issued and countersigned by the film supplier before artwork is printed; re-confirm bilingual content fits the 20 %-smaller face from DC-12 | Project + supplier | DC-39, RGL-04, RGL-07 |
| 7 | **ORYZ pack proportions** re-check against the 120 × 120 near-square face | Design | DC-12 |
| 8 | Reel dimensions, film structure/stiffness, print/registration scheme | Buyer / supplier | DC-36, DC-30 |
| 9 | Shift volume, to finalise the giveaway case | Buyer | DC-10 |
| 10 | Downstream conveyor interface | Buyer | FRS §6 |

## 4.3 Closed this revision

- ✅ **Canadian tolerance table confirmed to match OIML R87** (BC-12). Closes the Fill-Quantity Derivation's open item and DDA change #17 / §6.5's flagged item.
- ✅ **DC-12 does not cross the SFCR s. 229 numeral-height breakpoint** — 180 cm² and 144 cm² are both in the 3.2 mm band.
- ✅ **DDA change #3 ruled stale** (C-1) and change #4 promoted from stretch-enabler to baseline requirement (DC-26).

---

# Part 5 — The three things this register is really saying

1. **The fill spec is now entirely external.** Every number in §2.1 traces to SFCR Schedule 4 or OIML R87/R51, not to a round percentage. That was the original reason for wanting regulatory constraints, and it is done — for fill quantity. It is the one domain where the law genuinely *is* the design driver.

2. **Two Rev B numbers were set independently and still do not intersect.** The 1440 cm³ envelope versus "firm brick" (C-5), and the single-station cross-seal versus zero entrapment at 25 bpm (DC-26). Both have clean resolutions that cost no throughput. The second is worse than the DDA stated: it bites at the baseline rate, not just the stretch.

3. **The wall is seal dwell at the gusset corners.** Not regulation, not film speed, not dosing, not motion architecture — everything else in the cycle budget has slack. And the one place where OR-7 and the seal window genuinely fight each other (serration, DC-33) is unresolvable by analysis. T-1, T-2 and T-3 are the project.

---
See also: [[VFFS_Rice_FRS]] · [[VFFS_Rice_Detail_Design_Analysis]] · [[Fill-Quantity Spec Derivation — 1 kg Rice Brick (OIML R87 R51)]] · [[VFFS Rice Brick — Canadian Regulatory & Compliance Specification (Rev A)]] · [[VFFS Regulatory Compliance Dossier — Project Plan]] · [[VFFS_Rice_Problem_Statement]]
