# Detail Design Analysis — VFFS 1 kg Block-Brick Rice Machine

| | |
|---|---|
| **Document** | Detail design analysis (companion to the FRS) |
| **Parent** | `VFFS_Rice_FRS.md` Rev B, 2026-09-08 |
| **Basis** | The reference set in `packaging_machine/` (graded in §12) + derivations run here |
| **Date** | 2026-10-09 |
| **Status** | For review — contains **four proposed changes to FRS Rev B** (§10) |

---

## 0. How to read this

The FRS says *what the machine must do*. This document is the layer below: it works out
*whether the stated targets are mutually achievable*, and where they are not, it says which
one has to move. Four of the FRS Rev B numbers do not survive the analysis:

| FRS Rev B | Finding | §
|---|---|---|
| Brick 120 × 150 × 80 mm | Envelope is 13–23 % larger than 1000 g of settled rice. A brick built to this size **cannot be firm** — it will be slack whatever the settling does. Height must come down to ≈ 118–120 mm. | §5 |
| Brick dimensions ±3 mm | Unachievable under volume control. A fixed 1000 g charge across the stated 0.75–0.85 g/cm³ density band gives a **16.3 mm** height spread. Needs dimension control (hard-stopped shaping frame). | §5 |
| Zero grain entrapment, single cross-seal station | The gusset corners carry 4–6 film layers against 2 in the plain seal. Conduction-limited dwell scales with thickness², so the corner needs ~4–9× the dwell of the plain region. **No single-dwell jaw satisfies both** at any target rate. | §4 |
| Mean fill 1002 g "to protect the lower spec limit" | The band is symmetric, so centring at 1000 g both maximises Cpk (1.478 vs 1.330) **and** eliminates the giveaway. The real justification for a guard is checkweigher uncertainty, which belongs in the *reject limits*, not the *mean*. | §6 |

Everything else in Rev B holds up, and two of its open items (checkweigher moving-average
window, target offset for checkweigher accuracy) are closed in §6.

---

## 1. The architecture is forced, not chosen

Rev B calls intermittent box-motion a "baseline" with continuous motion as an upgrade path
above 45 bpm. The reference set narrows this more than the FRS does.

**Film transport method is not a free choice.** There are two ways to pull film: the *jaw
draw-off* (draw bar) principle, where the sealing jaws grip and pull the web down, and the
*belt* principle, where vacuum or friction belts push the web along the forming tube. Bosch
is explicit: with jaw draw-off, "because the sealing jaws pull on the material, **no bag with
a block bottom can be made**, but only pillow bags and gusset bags." [Bosch, ch. 3]

So the brick forces the **belt pick-up principle**, and that forcing carries three
consequences the FRS does not yet account for:

1. **Transport and cross-sealing serialise.** With belt pull, "the film transport and the
   sealing of the cross seam take place after each other. For each of these steps, one third
   of the machine cycle is available." [Bosch, ch. 3] Jaw draw-off would have combined them
   (7/12 down-stroke sealing + transport, 5/12 return) — that saving is unavailable here.
2. **Lower available pull force.** The belt principle's drawbacks are "the smaller forces and
   the fact that the film is not pulled downwards but pushed forwards," and "due to the extra
   resistance of bag spreaders or a block bottom mouthpiece, the film material can get bunched
   up around the forming tube." [Bosch, ch. 3] The brick's mouthpiece is exactly that extra
   resistance. This feeds directly into the forming-shoulder bound in §7.
3. **No jaw-carried soft landing.** Jaw draw-off and continuous motion both move the jaws down
   with the falling product, giving "a soft landing of the product in the bag." [Bosch, ch. 3]
   Belt-driven intermittent motion does not. With a 1 kg charge this matters — see §4.4.

**Belt type:** vacuum, not friction. Friction belts rely on belt-to-film friction exceeding
film-to-tube friction, and with "a smooth exterior and polyethylene on the interior (creating
extra friction), the friction difference can be so small that it can create problems."
[Bosch, ch. 3] A rice film is exactly that construction. Service life also differs by an
order of magnitude: friction belts "two to four weeks", vacuum-supporting belts "a few
months", vacuum belts "multiple years" on the same duty. [Bosch, ch. 3] Rockwell independently
specifies vacuum belts as the default, "which allows for better gripping control in dusty,
high moisture or cold operating environments." [Rockwell AutomationOfVFFS, Film Pull Belts]

> **FR-1 / FR-5 addition:** specify belt pick-up with **dual independent vacuum belts**, each
> servo- or VFD-driven with encoder feedback, with left/right position and velocity error
> monitored so "slight mechanical differences are not allowing one side to pull more than the
> other." [Rockwell] Web skew is the primary cause of back-seam and registration drift, and a
> single shared motor hides it.

**On the 45 bpm threshold.** Rev B's ">45 bpm ⇒ continuous-motion" rule is directionally right
but misattributed. Intermittent machines are not inherently capped near 45: Rockwell puts
"the typical intermittent machine" at **60–80 ppm maximum** against up to **180 ppm**
continuous. The brick's ceiling is lower than 60–80 not because the architecture is
intermittent but because of the dwell budget derived in §3–4. State the limit as a derived
consequence, not an architectural property — and note Rockwell's caveat that regardless of
architecture, "the highest machine speed attainable is determined by the weighing mechanism."

---

## 2. Functional decomposition and the control axes

Rockwell's four functional areas map cleanly onto the FRS flow: (1) mixing/weighing/dosing,
(2) forming, (3) feeding/aligning/registration, (4) closing/sealing/cutting. The axis list
for an intermittent bagger is small and worth fixing now because it determines the HMI recipe
structure in FR-11:

| Axis / loop | Control | Purpose | FRS ref |
|---|---|---|---|
| Film unwind | PID on dancer position, brake or servo mandrel | Constant web tension, minimal waste | FR-1 |
| Film pull belts (×2) | Servo, indexed to length **and** stopped on the registration mark | Bag index; left/right error monitored | FR-1, FR-2 |
| Registration | Mark sensor, **time-stamped** input | Minor corrections to seal and cut placement | FR-2 |
| Cross jaw | Servo or high-force pneumatic with sprung closure | Bottom-gusset seal, top seal, knife | FR-7, FR-8 |
| Back-seam heater | Temperature loop, engaged only while film moves | Longitudinal seal | FR-6 |
| Gusset / shaping frame | Pneumatic or servo (see §5) | Brick form | FR-5 |
| MHW timing | PLS channel / virtual master | Charge call-off, decoupled via memory hopper | FR-4 |

Two mechanisms from the Rockwell note are worth importing verbatim into FR-11:

- **Time-stamped registration.** Registration inputs record "not only the position of any
  physical axes independent of the process, but also the time the input occurred. This time
  can be used to determine precise positions for all other physical and virtual axes."
  This is what lets the seal and cut land correctly despite film stretch and slip during
  acceleration, instead of only correcting on the next bag.
- **Pending cams for empty-cycle handling.** "Pending cams on an otherwise continuously
  recurring cam motion bring the bagging hands to a stop and restart them within half a
  product cycle to control dwelling when product is unavailable." This is the clean
  implementation of the FRS's "empty-skip logic" (KPI 7.4, start-up waste) and it belongs in
  FR-11 as a named requirement rather than an aspiration.

**Back-seam heater — a detail the FRS omits.** The long seal bar is "eight to twelve inches in
length" (200–305 mm), and "linear film speed and type are factors in determining the correct
minimum length." Critically, a static bar "will be engaged into the film and forming tube
**only while the film is in motion** with slight timing delays to prohibit melting of the film
when stationary." [Rockwell, Forming Tube] With a belt-driven intermittent machine the film is
stationary for ~2/3 of every cycle, so the back-seam bar needs a **retract-on-dwell actuator
and its own timing cam** — not just a temperature setpoint. The alternative is the dynamic
heated-belt long sealer, which "can move with the film and also be adjusted to operate faster
than the base film speed or even slower." For a 1 kg brick with a long back seam, the dual-bar
arrangement (flaps held between two facing bars) is worth specifying: it "can provide stronger
seals/seams and the product ambient temperature (i.e. forming tube temperature) will have less
of a factor on the loop control."

---

## 3. Cycle-time architecture — where the 2.4 s actually goes

Rev B §8.1 gives cycle time as 60/bpm and stops there. With belt pull the cycle has a fixed
structure, so the budget can be allocated.

Cycle = film transport (≈ T/3) + jaw close/open travel + seal dwell + settle/shape.

| Rate | Cycle | Transport (T/3) | Jaw travel | Settle | **Dwell left** |
|---|---|---|---|---|---|
| 25 bpm | 2.400 s | 0.800 s | 0.15 s | 0.30 s | **1.150 s** |
| 30 bpm | 2.000 s | 0.667 s | 0.15 s | 0.30 s | **0.883 s** |
| 40 bpm | 1.500 s | 0.500 s | 0.15 s | 0.30 s | **0.550 s** |
| 45 bpm | 1.333 s | 0.444 s | 0.15 s | 0.30 s | **0.439 s** |

Bosch's independent rule of thumb agrees: on an intermittent machine, "sealing (which includes
the jaw close and open time) take up more than 1/3 of the machine cycle."

Against that, the measured sealing requirement from the LUT machine trials: **minimum dwell
0.4 s**, working range 0.5–2 s, and the recommended production point **130 °C with 1 s dwell**.
[Merabtene §3.4, §4.4]

So the plain-seal dwell requirement alone (1 s at the recommended point) is satisfied at
25 bpm, marginal at 30 bpm, and **infeasible at 40–45 bpm**. Pushing to the measured 0.4 s
floor buys the rate back only if the seal is a 2-layer seal — which at the gusset corners it is
not (§4).

**The settling allowance is the weakest number in the table, and it is mine, not the FRS's.**
Rev B specifies settling/de-aeration but never budgets time for it. Granular consolidation
scales with *number of taps*, not seconds; tapped-density practice puts most of the
consolidation in the first 50–100 taps. A 0.30 s window buys only:

| Shaker frequency | Taps in 0.30 s |
|---|---|
| 20 Hz | 6 |
| 30 Hz | 9 |
| 50 Hz | 15 |
| 60 Hz | 18 |

Six to eighteen taps is very unlikely to deliver repeatable consolidation. The design response
is **not** a longer serial settle — it is to **overlap settling with film transport and with the
next charge's flight time**, so settling occupies cycle time that is already spent. Concretely:
settle the pack *after* its bottom seal and *during* the next index, using a shaker/tamper that
grips the formed bag below the jaws rather than a dwell inserted between fill and top seal.

> **Proposed FR-5 requirement:** settling and shaping shall run concurrently with film
> transport and charge flight; no cycle time is allocated to settling as a serial step. The
> settle station shall be specified by **taps per brick** (target ≥ 30) and vibration
> amplitude, not by seconds.

**Film speed implications.** At an index length of 254–290 mm (§7), with a trapezoidal profile
using the LUT machine's 200 ms accel / 100 ms decel:

| Rate | Transport | Cruise | Peak web speed | Mean |
|---|---|---|---|---|
| 25 bpm | 0.800 s | 0.500 s | 0.45 m/s | 0.36 m/s |
| 40 bpm | 0.500 s | 0.200 s | 0.83 m/s | 0.58 m/s |
| 45 bpm | 0.444 s | 0.144 s | 0.99 m/s | 0.65 m/s |

(254 mm index gives 0.385 / 0.714 / 0.849 m/s respectively.) These are comfortable speeds —
Bosch's worked example has an intermittent machine at 1 m/s for a 300 mm bag at 60 bpm. Film
speed is **not** the constraint; dwell is. Note also that above ~45 bpm the cruise phase nearly
vanishes and the move becomes accel-decel only, at which point the 200/100 ms ramp times
themselves must be shortened — a servo sizing requirement, flagged for the detail-design phase.

---

## 4. Sealing — the binding constraint

### 4.1 The layer-count problem

This is the central finding of the analysis and it is not addressed anywhere in Rev B.

A hot-bar seal is conduction-limited: heat must cross the film stack from the jaw faces to the
sealant interfaces. Diffusion time scales with the **square** of stack thickness. A
block-bottom / quad-seal brick does not have one stack thickness — it has three:

| Location in the cross-seal | Film layers | Relative dwell (n/2)² | Dwell, anchored on the 0.40 s measured 2-layer floor |
|---|---|---|---|
| Plain cross-seal region | 2 | 1.0 | 0.40 s |
| Side-gusset fold | 4 | 4.0 | 1.6 s |
| Gusset + back-seam overlap corner | 5–6 | 6.3–9.0 | 2.5–3.6 s |

Compare against the dwell available (§3): **1.15 s at 25 bpm, 0.44 s at 45 bpm**. The gusset
corner requirement exceeds the available dwell *at every rate in the FRS range, including the
25 bpm baseline*. Conversely, setting the dwell long enough for the corner over-heats the
2-layer region, and the LUT trials show exactly what that costs: above the processing window
"the laminate layer becomes weaker leading to complete delamination," and at higher
temperatures "the sealing layer was significantly torched and teared." [Merabtene §4.4]

The LUT block-bottom trials are the empirical confirmation. Running block-bottom bags was
"among the hardest," with "fractures ... behind the longitudinal seam, holes at the closure
bottom gusset and above the bottom cross seam sealing jaws" — i.e. **failures concentrated at
precisely the high-layer-count locations predicted above**. [Merabtene §4.5.2]

*Caveat on the numbers:* the (n/2)² scaling is a clean conduction argument, but the 0.40 s
anchor is a measured, mixed-mechanism figure — it includes interface wetting and polymer chain
diffusion, not pure thermal penetration, and real machines do produce block-bottom bags. Treat
the column as **a bound and a ranking, not a prediction**. What is robust is the ordering and
the rough magnitude of the ratio; that alone rules out the single-dwell jaw. The absolute
numbers must be measured (§11, test T-3).

### 4.2 Four ways out, ranked

1. **Separate the gusset seal from the cross-seal (recommended).** Pre-form and pre-seal the
   bottom gusset at its own station, one index upstream, so each station sees a near-uniform
   layer count and each dwell is set independently. Costs one extra station and one extra
   index of film; buys back the whole conflict. Rockwell confirms the cross-jaw section
   routinely carries "gusset (single & double) creation mechanics and flat bottom bag
   mechanics" as discrete options, so this is conventional practice rather than invention.
2. **Profiled jaw with relief at the corners.** Deepen or relieve the jaw face where layer
   count is highest so corner pressure rises and contact resistance falls. The LUT pressure
   data shows how strongly profile drives surface pressure at constant jaw force (§4.3) — a
   narrow profile is "like a high heel [exerting] a higher surface pressure than a flat rubber
   sole" [Bosch ch. 4]. Cheap, but it manipulates pressure, which is the *least* significant
   of the three sealing parameters [Meka & Stehling; Yuan et al., via Merabtene §2.5.2] — so
   expect partial relief only.
3. **Impulse sealing at the gusset.** "With impulse sealing, jaws are heated to fusion
   temperature by a short powerful electric impulse. The seal area remains clamped and is
   cooled under pressure ... **When minor contamination is present, the impulse method may give
   a better seal.**" [Food Packaging Technology §7.9.2.3] Both halves of that are attractive
   here: it decouples heat input from dwell, and rice dust is exactly "minor contamination".
   Clamped cooling also directly addresses the hot-tack problem in §4.4.
4. **Go continuous-motion earlier than Rev B assumes.** Vertical-motion continuous jaws let
   "the sealing time ... simply be varied by having the jaws move along with the film for a
   shorter or longer time. A longer sealing time can be compensated by having the jaws move up
   more quickly, so that the packaging speed remains the same." [Bosch ch. 3] This is the only
   option that breaks the dwell/rate coupling outright — note it breaks it at *any* rate, not
   just above 45 bpm, which is a stronger argument for continuous motion than the one Rev B
   makes. Avoid fixed O- and D-curve jaws: "the sealing time can only be extended by reducing
   the machine speed."

### 4.3 Sealing parameters and jaw force — transferable numbers

From the LUT machine (GKS Compack CP350 Plus, 140 mm shoulder), with the caveat that the
materials were fibre-based and thin thermoplastics, not a 1 kg rice laminate:

| Parameter | Value | Note |
|---|---|---|
| Cross-jaw force | 2356 N | Per jaw pair, pneumatic at 6 bar supply |
| Longitudinal sealer force | 2587.5 N | At 5 bar supply |
| Surface pressure, 11 mm serrated (Tool 1) | 0.31 MPa | 7616 mm² seal area |
| Surface pressure, 3+4 mm twin-flat (Tool 2) | 0.70 MPa | 3360 mm² |
| Surface pressure, 5 mm flat (Tool 3) | 0.84 MPa | 2800 mm² |
| Surface pressure, 4 mm flat (Tool 4) | 1.05 MPa | 2240 mm² |
| Optimum, Tool 1 | 120–130 °C, 1–1.5 s | Widest window |
| Optimum, Tools 2 and 3 | 130 °C, 0.5–1 s | Narrower; torching above |
| Recommended production point | **130 °C, 1 s** | |
| Minimum dwell | 0.40 s | |

**Two cautions on reusing this table.**

*First, a unit error in the source.* The thesis text states surface pressures "of 0.31 bars for
Tool 1, 0.7 bars for Tool 2 and 0.84 bars for Tool 3" [§3.4] while Table 6 gives the same
figures in **MPa**. Recomputing from force and area confirms MPa: 2356 N / 7616 mm² = 0.309 MPa
= 3.09 bar. All five horizontal-tool rows reconcile exactly. Use MPa.

*Second, two rows of that table do not reconcile.* The laboratory heat sealer (1256 N /
6096 mm² = 0.206 MPa, printed as 0.42) and the longitudinal length sealer (2587.5 N / 1920 mm²
= 1.348 MPa, printed as 0.65) are each off by a factor of ~2, in opposite directions. Since the
whole point of that table is to calibrate lab-sealer results onto the machine, **do not transfer
lab-sealer parameters to a machine setpoint using those two rows.** Re-derive the areas.

*Third, serration choice is unresolved for rice.* Tool 1 (11 mm serrated) gave the widest, most
forgiving processing window, which is what you want for a multi-layer-count brick. But a broad
serrated seal is also the profile most likely to trap grains in its valleys. This is a direct
trade-off between FRS OR-7 (zero entrapment) and the robustness of the seal window, and it
needs a dedicated trial (§11, T-3).

### 4.4 Hot tack and the 1 kg drop — an unspecified load case

Rev B's FR-7 names "drop-and-seal" and stripper plates, but never states the load the *fresh*
bottom seal must carry. It must, because in VFFS "the principle ... is to disperse the product
through dispensing dozer with the help of gravity directly after the opening of the hot jaws.
This would exert high force on the seam which could open-up the unsolidified seam."
[Merabtene §2.6]

Computed for this machine:

| Drop height | Impact velocity | Arrival window | Mean force on bottom seal | Per mm of 120 mm seal |
|---|---|---|---|---|
| 0.9 m | 4.20 m/s | 0.15 s | 37.8 N | 0.315 N/mm |
| 0.9 m | 4.20 m/s | 0.25 s | 26.6 N | 0.222 N/mm |
| 1.2 m | 4.85 m/s | 0.15 s | 42.2 N | 0.351 N/mm |
| 1.2 m | 4.85 m/s | 0.25 s | 29.2 N | 0.243 N/mm |

(Momentum transfer m·v/Δt plus static weight; a mean over the arrival window, so peak grain-bed
loading is higher.)

Set against the required **cold** seal strength for this product class: "Heavier product weights
and free-flowing products such as nuts, rice, pulses and frozen vegetables may have to have seal
strengths in excess of **1000 g/25 mm**" = **0.392 N/mm** [Food Packaging Technology §7.9.2.2]
— against 300 g/25 mm (0.118 N/mm) for small light products.

So the dynamic load is **0.22–0.35 N/mm, i.e. 57–90 % of the fully cured seal strength
requirement**, and it is applied while the seal is still molten, when hot tack is a fraction of
cured strength. This is not a comfortable margin; it is a design problem.

The LUT trials confirm it empirically at **less than half our charge mass**: to produce
block-bottom bags successfully with only 400 g of peas, "once the peas were placed in the
dispensing dozer, **the bags were held from the bottom to support the weight of the bag** and
minimize the fractures caused." [Merabtene §4.5.2] A machine that needs a hand under the bag at
400 g will not run unattended at 1000 g.

> **Proposed new requirement FR-7.4 — bottom support.** The machine shall mechanically support
> the filled brick's base from the instant of the bottom seal until that seal has cooled below
> its hot-tack-critical temperature. Options: a powered support shelf or cradle indexed with the
> jaws; a cooled bottom jaw insert (clamped cooling, per impulse sealing); or continuous-motion
> jaws that descend with the product. Verification: fill at 1.3 × nominal charge mass with no
> seal deformation.

A second mitigation is available free: lengthen the arrival window. Going from a 0.15 s to a
0.25 s window cuts the seal load by ~30 %, and the arrival window is set by the MHW discharge
gate and funnel geometry, not by the bagger (§8).

### 4.5 Film and sealant selection

The FRS specifies "airtight / moisture-barrier" without a structure. The constraints that
follow from the above:

- **Fin seals only.** "Using PE or cast PP as the sealant of a laminate exploits the easy flow
  nature of cast and low-melting point polymers. The molten polymer can flow into crevices and
  fill any gaps or holes in the seal." That flow is what buys grain tolerance. But "the inability
  of the laminates to seal inside to outside layers limits the application to f/f/s with fin
  seals, as compared with overlapping seals, along the length, and this uses slightly more
  material because of the extra width of film required." [Food Packaging Technology §7.9.2.2]
  **This decides the back-seam type** — fin, not overlap — and adds the extra film width in §7.
  Note it also conflicts with the Rockwell overlap-seam default, so it must be stated explicitly.
- **Wide sealing window, by carrier choice.** "PET and nylon PA films with their superior heat
  stability giving very wide sealing ranges do not normally develop split seals."
  [Food Packaging Technology §7.9.2.2] Given the layer-count spread in §4.1, a wide window is
  worth more here than in a normal pillow-bag application. A PET or PA carrier with a cast-PE or
  ionomer sealant is the indicated direction.
- **Hot tack is a selection criterion, not an afterthought.** Ionomer is called out for
  "excellent hot tack" and ionomer emulsion coatings with ~80 °C melting points "and a high
  level of hot tack have extended the sealing range of coated OPP to over 70 °C."
  Given §4.4, **specify a minimum hot-tack force at the fill temperature**, not just a cured
  peel strength.
- **Low-temperature-seal coatings help the start/stop problem.** "With high packaging speeds, it
  is normal to have high temperatures to melt the sealant in a very short dwell time. When the
  machine speed varies, the high temperature of the sealing jaws damages the film. With the LTS
  (low-temperature sealing) coating, lower heat settings are possible, thus avoiding film damage
  at slower speeds." An intermittent machine that starts, stops and jogs is exactly the variable
  speed case.

**Test methods already correct in Rev B** — ASTM F88 (peel), F1140 (burst), F2096 / D3078 (leak)
— match the LUT methodology (T-peel, 90° pull, 300 mm/min, F88 sample prep). Keep them, and add
a hot-tack method (ASTM F1921) to cover §4.4.

---

## 5. Brick geometry — the dimensions in Rev B do not close

### 5.1 The envelope is too big for the contents

Rev B §1.4 gives the brick as ≈ 120 W × 150 H × 80 D mm ≈ 1440 cm³ gross, reasoning that this
"holds loose fill + settling headspace." That reasoning conflates two different volumes: the
volume the charge needs *while falling into the tube* and the volume of the *finished sealed
brick*. Headspace is needed in the first; in the second it is slack.

1000 g of rice occupies:

| Density | Volume | As a fraction of the 1440 cm³ envelope |
|---|---|---|
| 0.75 g/cm³ (loose, low) | 1333 cm³ | 92.6 % |
| 0.80 g/cm³ (loose, nominal) | 1250 cm³ | 86.8 % |
| 0.85 g/cm³ (loose, high) | 1176 cm³ | 81.7 % |
| 0.88 g/cm³ (settled) | 1136 cm³ | 78.9 % |
| 0.90 g/cm³ (settled, high) | 1111 cm³ | 77.2 % |

A brick sealed at 1440 cm³ is **13–23 % empty**, and the settling step makes it *worse*, not
better — the denser the contents, the slacker the pack. The FRS's own goal of a "dense, square,
self-standing brick" is unreachable at these dimensions by construction.

Required height at the 120 × 80 mm footprint:

| Settled density | Height for a full brick |
|---|---|
| 0.80 | 130.2 mm |
| 0.85 | 122.5 mm |
| 0.88 | 118.4 mm |
| 0.90 | 115.7 mm |

### 5.2 Control the brick by dimension, not by volume

If the charge is fixed at 1000 g and density is left free across the stated 0.75–0.85 band, the
finished height of a 120 × 80 brick ranges **122.5 to 138.9 mm — a 16.3 mm spread**, against the
KPI 7.3 target of ±3 mm (a 6 mm band). Volume control cannot meet the dimensional KPI. Holding
±3 mm at a 120 mm nominal would require settled density controlled to 0.847–0.890 g/cm³, i.e.
**±2.5 %**, which is tighter than the incoming material band.

The resolution is to make the shaping frame a **hard-stopped dimension-setting tool**: the brick
is compacted to a fixed envelope and the *compaction stroke* absorbs the density variation.
Density then becomes the dependent variable, fixed by ρ_required = 1000 g / V_envelope, and the
design question becomes whether that density is reachable from every incoming loose density
within the achievable consolidation ratio (~1.00–1.20 for vibratory settling plus tamping).

| Height | Envelope | ρ required | Ratio from 0.75 | from 0.80 | from 0.85 | Feasible? |
|---|---|---|---|---|---|---|
| 150 mm | 1440 cm³ | 0.694 | 0.93 | 0.87 | 0.82 | No — always slack |
| 140 mm | 1344 cm³ | 0.744 | 0.99 | 0.93 | 0.88 | No |
| 130 mm | 1248 cm³ | 0.801 | 1.07 | 1.00 | 0.94 | No — slack at high density |
| 125 mm | 1200 cm³ | 0.833 | 1.11 | 1.04 | 0.98 | Marginal |
| **120 mm** | **1152 cm³** | **0.868** | **1.16** | **1.09** | **1.02** | **Yes** |
| **118 mm** | **1133 cm³** | **0.883** | **1.18** | **1.10** | **1.04** | **Yes** |
| 115 mm | 1104 cm³ | 0.906 | 1.21 | 1.13 | 1.07 | No — exceeds ratio at low density |
| 110 mm | 1056 cm³ | 0.947 | 1.26 | 1.18 | 1.11 | No |

The window is narrow — **118 to 120 mm** at a 120 × 80 footprint — and it is narrow in both
directions, which is itself the finding: too tall and the brick is slack at high incoming
density; too short and the machine cannot consolidate enough at low incoming density.

> **Proposed FRS change (§1.4, OR-6, KPI 7.3):** brick **120 W × 120 H × 80 D mm (1152 cm³)**,
> target settled density **0.868 g/cm³**. Brick form controlled by a hard-stopped shaping frame;
> compaction stroke to accommodate a consolidation ratio of 1.02–1.16. Height tolerance ±3 mm
> becomes achievable because it is set by tooling, not by material. Add a *new* KPI: compaction
> stroke position per brick, logged — it is a free in-line density measurement and an early
> warning of incoming material drift.

**Note two knock-on effects.** The 150 → 120 mm height change (a) alters the retail pack
proportions, which interacts with the ORYZ pack design work — a 120 × 120 × 80 brick is nearly
square in front elevation, versus the portrait 120 × 150 originally drawn; and (b) shortens the
film index, improving §3's speed margin. The pack-design consequence should be checked before
the dimension is frozen.

### 5.3 Bag-length repeatability

An independent reason to distrust volume control: in the LUT trials, with the machine bag length
set to 230 mm and equal fills, "the results in the table conclude that the lengths are **not
consistent nor equal**," attributed tentatively to film thickness. [Merabtene §4.5.2] Bosch
devotes a tips-and-tricks section to "preventing differences in bag lengths." Registration-mark
indexing (FR-2) is the standard fix — "there is one mark for each bag length ... the photo cell
'sees' the mark and assures the transport of one bag length" — but note that *mark-based indexing
corrects the film index, not the brick height*. Only the shaping frame fixes the brick.

---

## 6. Dosing, capability, and the two open items in the FRS header

### 6.1 The FRS arithmetic checks out

Cpk = min(USL−μ, μ−LSL) / 3σ = min(1020−1002, 1002−980) / (3 × 4.51) = 18 / 13.53 = **1.330**. ✓
Rev B's σ ≤ 4.51 g target is correctly derived from its own μ = 1002 g assumption.

### 6.2 But the mean should be centred, not offset

The band is symmetric about 1000 g, so an upward offset spends capability *and* money:

| Mean | σ | Cpk | Out-of-band | Giveaway |
|---|---|---|---|---|
| **1000 g** | 4.51 | **1.478** | 9 ppm | 0.00 % |
| 1001 g | 4.51 | 1.404 | 14 ppm | 0.10 % |
| 1002 g | 4.51 | 1.330 | 33 ppm | 0.20 % |
| 1005 g | 4.51 | 1.109 | 441 ppm | 0.50 % |

Centring at 1000 g raises Cpk from 1.330 to 1.478, cuts out-of-band from 33 to 9 ppm, and
removes the giveaway. Rev B's stated justification — offset "to protect the lower spec limit" —
does not hold for a symmetric band: moving the mean up protects LSL only by moving the same
distance *toward* USL. The σ needed for Cpk = 1.33 is 5.013 g at μ = 1000 versus 4.511 g at
μ = 1002, so centring also **relaxes** the dosing requirement.

The giveaway at stake:

| Rate | Hours/yr | Bricks | Extra rice at 2 g/brick |
|---|---|---|---|
| 25 bpm | 2000 | 3.0 M | 6,000 kg |
| 25 bpm | 4000 | 6.0 M | 12,000 kg |
| 40 bpm | 4000 | 9.6 M | 19,200 kg |

### 6.3 Closing FRS open item: "how to adjust the target to compensate for checkweigher accuracy"

The guard against checkweigher uncertainty belongs in the **reject limits**, not the mean. The
checkweigher's own uncertainty u_cw inflates the observed spread (σ_observed² = σ_process² +
u_cw²) and, more importantly, means a brick reading 981 g might truly be 979 g. So reject inside
the spec limits by k·u_cw:

| u_cw | k | Reject band | Centred mean | σ_max for Cpk 1.33 |
|---|---|---|---|---|
| 0.5 g | 2 | 981.0 – 1019.0 | 1000 g | 4.76 g |
| 1.0 g | 2 | **982.0 – 1018.0** | **1000 g** | **4.51 g** |
| 1.0 g | 3 | 983.0 – 1017.0 | 1000 g | 4.26 g |
| 1.5 g | 2 | 983.0 – 1017.0 | 1000 g | 4.26 g |
| 1.5 g | 3 | 984.5 – 1015.5 | 1000 g | 3.88 g |

Note the convenient result on the highlighted row: **a centred mean of 1000 g with 2σ
guard-banded reject limits at 982/1018 g reproduces exactly the FRS's σ ≤ 4.51 g requirement**,
at u_cw = 1.0 g (a realistic dynamic checkweigher at this capacity and rate). So the FRS can
keep its headline dosing number, drop the 2 g giveaway, and gain a defensible derivation. This
is the recommended formulation.

Also note σ_observed = 4.51 g at u_cw = 1.0 g corresponds to σ_process = 4.4 g — the MHW is
being asked for slightly better than the FRS implies, because part of the observed spread is
the instrument. State the requirement on the **process**, and specify u_cw separately.

### 6.4 Closing FRS open item: "moving average of ___" and the data-logging update cycle

The mean-correction loop must not chase noise. With SE = σ/√n:

| n (bricks) | SE at σ = 4.5 g | 95 % CI half-width |
|---|---|---|
| 25 | 0.900 g | 1.76 g |
| 50 | 0.636 g | 1.25 g |
| 100 | 0.450 g | 0.88 g |
| 150 | 0.367 g | 0.72 g |

To resolve a mean drift of δ with 95 % confidence, n ≥ (1.96σ/δ)²:

| Drift to detect | n required (σ = 4.5 g) | Time at 25 bpm | at 40 bpm |
|---|---|---|---|
| 2.0 g | 19 | 0.8 min | 0.5 min |
| **1.0 g** | **78** | **3.1 min** | **2.0 min** |
| 0.5 g | 311 | 12.4 min | 7.8 min |

> **Recommended answers to the FRS header questions:**
> - **Moving average window: n = 100 bricks** (resolves a 1 g drift with margin; 4.0 min at
>   25 bpm, 2.5 min at 40 bpm).
> - **Correction update cycle: every 25 bricks**, on a rolling 100-brick window — so the loop
>   updates four times per window rather than stepping discontinuously.
> - **Correction authority: limited to ±1 g per update**, with an alarm rather than a correction
>   if the computed offset exceeds 3 g (that is a weigher fault, not a drift).
> - **Logging: per-brick** records (weight, dimension, compaction stroke, seal temperatures,
>   dwell, reject cause) at the line rate, with 1 min aggregates retained long-term. Per-brick
>   logging is what makes a capability study (KPI 7.2, 30–125 samples) a query rather than a
>   campaign.

A caution on the loop: feeding checkweigher data back to the MHW target corrects the *mean*. It
does nothing for σ, and if it is tuned aggressively it will *add* variance. Specify it as a slow
trim, not a controller.

### 6.5 Legal metrology — an open item Rev B does not raise

The ±2 % band is an engineering tolerance. It is not obviously a *legal* one. Under the EU
average-quantity (e-mark) system, for example, a 1000 g nominal pack has a tolerable negative
error of 15 g, making 985 g the individual-pack floor — which is **above** the FRS's 980 g lower
limit, so a pack at 980 g would be non-conforming there even though it passes the FRS. Canadian
net-quantity rules under the Consumer Packaging and Labelling Act differ again.

I have not verified the current requirement for the intended market and this should not be taken
as a compliance statement — but the FRS should not freeze 980 g without someone checking.
Flagged as a new open item.

---

## 7. Forming shoulder, forming tube and web handling

### 7.1 The shoulder design method you now have

The Bath paper gives what the FRS currently lacks: a *theoretical* basis for choosing shoulder
geometry from material properties, rather than trial and error. Its result is that film flow over
a shoulder obeys Euler's belt-friction relation despite the surface being far from cylindrical:

P = Q · exp(μ(π − θ₀)), and in bounded form 0 < (1/μ)·ln(P/Q) < k(π − θ₀)

where P is pull force, Q the resistive web tension, μ the film-to-collar friction coefficient,
θ₀ the back angle, and k a safety factor (the data supports k = 2.7 to cover all results, k = 2
for most; k = 3 is used in their worked design example). Rearranged, this gives a **minimum
allowable back angle**:

θ₀,min = π − ln(P/(kQ))/μ

| μ | P/Q | θ₀,min (k = 3) |
|---|---|---|
| 0.15 | 4 | 70.1° |
| 0.20 | 4 | 97.6° |
| 0.20 | 6 | non-binding |
| 0.30 | 4 | 125.1° |
| 0.30 | 6 | 47.6° |
| 0.40 | 4 | 138.8° |
| 0.40 | 10 | 7.5° |

Their worked example: μ = 0.2, Q = 40 N, film elastic limit 160 N with P capped at half
(80 N), k = 3 → θ₀ = 114°.

**Applied to this machine.** The 415 mm web (§7.2) is far stronger than their test strip: at
80 µm and 12–30 MPa yield, web yield force is 398–996 N, so a 50 %-of-yield pull cap is
199–498 N. Against a web tension Q of order 40 N that is P/Q ≈ 5–12, which makes the friction
inequality **non-binding across the plausible μ range** — the constraint is satisfied by any
back angle.

That is a useful negative result, and it relocates the design driver. For this bag the shoulder
is **not** limited by film tearing; it is limited by (a) tracking, which wants a *steep* run-in
angle, and (b) the pull force actually available, which §1 showed is *reduced* by the belt
principle and further eaten by the block-bottom mouthpiece. Bosch: "the big benefit of a steeper
run-in angle is better tracking behaviour ... For a steeper run-in angle, the forces of the
machine must however be sufficient," and "a steeper run-in angle demands greater forces because
the resistance on the collar is higher. In addition, the collar will have a 'sharper' form,
causing it to be more susceptible to wear and tear — and a worn collar creates risk of damage to
the packaging material."

> **Design direction for FR-3:** take the back angle as steep as the *belt* pull capacity allows
> (not as steep as the film allows), and size the belt drive so that the mouthpiece resistance
> is inside its capacity with margin. Then apply the Bath inequality as a **check** rather than
> a driver, and use it as the acceptance criterion when a new film is qualified — that is where
> it earns its keep, since a thinner or bio-based film can move μ and the yield force at once.

**Two important caveats on the Bath model.** First, its geometry is developed for a **circular**
tube (bending curve z = R·f(ξ) about tube radius R). A quad-seal brick needs a non-circular
former. The paper points at the relevant extensions — rhombic sections, linear segments and
trapezoidal sections [its refs 10–12] — and **those are the papers to chase next**; the model
here should not be applied to a rectangular former without them. Second, their tests reveal "a
second order effect in which the pull force required seems to decrease as the retarding force
increases. The reason for this remains an open question." Keep k ≥ 3.

### 7.2 Tube and mouthpiece geometry

| Quantity | Value |
|---|---|
| Tube perimeter (120 × 80 brick) | 400 mm |
| Film width, + 15 mm back-seam overlap | **415 mm** |
| Equivalent round tube | d = 127.3 mm, open area 12,732 mm² |
| Rectangular tube 120 × 80 | open area 9,600 mm² |
| Penalty of rectangular vs round | round offers **33 % more** open area |

Bosch confirms the ordering: "for a certain bag width, a round forming tube provides the largest
opening ... for a given bag width, [the rectangular] shape has the smallest opening," and "a
smaller opening slows the maximum possible packing speed." There are two routes to the block
bottom and the choice has a throughput consequence:

1. **Round tube + mouthpiece** (Bosch's standard): "a mouthpiece is attached that extends from
   the round shape of the forming tube to the rectangular form of the bottom of the bag ... Just
   before the cross seam sealing jaws close, the film is folded inwards by horizontally mounted
   spreaders around the mouthpiece." Keeps the 33 % larger fill opening; adds mouthpiece drag
   (which §1 flagged as a belt-force problem) and one more transition for the film to negotiate.
2. **Rectangular forming tube** throughout: "some manufacturers use a rectangular forming tube
   instead of a round one." Simpler film path and a more directly square brick; costs 33 % of
   the fill opening.

**Recommendation: round tube with a block-bottom mouthpiece**, because §8 shows the fill window
is a cycle-time constraint and §4 shows the cycle has no slack to give. Revisit if mouthpiece
drag proves to exceed the belt capacity.

**The gap.** Rev B says nothing about the shoulder-to-tube gap, and it deserves a line, because
Bosch calls it "critically important to uninterrupted production and perfect looking bags. If
the gap is too tight, the film material will have the tendency to bunch up around the forming
tube. A gap that is too wide gives the film material too much space, and the bag will exhibit
diagonal folds (the so-called 'lightning strikes')."

**Shoulder material and manufacture.** Both sources converge on plastic/composite over metal.
Bosch: plastic shoulders "are light and wear-resistant, and they are not more expensive than
shoulders of other materials," while solid metal shoulders "have sharply dropped in popularity"
for weight and cost. Merabtene adds the manufacturing-route argument, and it is the more
important one: traditional hand-bent sheet fabrication "develops a sharp fillet edge. Thus, it
creates distortion along the bending curve which consequently makes the film not suitable to run
smoothly over the shoulder" — and his wrinkling defects were traced to exactly that fillet, not
to jaw pressure as he first assumed. His recommendation is CAD/CAE plus 3D printing or
fibreglass; the Bath team instead produced theirs "in aluminium using 5-axis milling" with NC
instructions generated from the mathematical collar model, then hard-anodised, so that the
shoulders "agreed (to within small manufacturing tolerances) with the exact model of the geometry
and were not subject to errors incurred by the need to bend and join together separate parts."

> **FR-3 requirement:** the shoulder shall be produced by a route that reproduces the
> mathematically defined collar surface directly (5-axis machining or additive), **not** by
> bending and joining sheet. Specify a maximum collar surface roughness and a minimum fillet
> radius at the collar. This single requirement eliminates the dominant defect mode in the
> reference trials.

### 7.3 Web handling, tension and registration

The LUT machine's deficiencies are a useful inverse specification — it had only 10 guide rolls
and "the film lacks tension when the transport belt is not operated," with the fix being "a
spring mounted dancer arm [using] the torsion spring mechanism to control the tension of the web
film," additional transverse rollers, and — from the newer GKS Leaf series — "a transverse motion
control of the spindle roll to match with the width of the shoulder using PLC system."
[Merabtene §5.3] Note also his warning that it is not a case of more-is-better: "very high film
tension leads to uncontrollable stretching of the film which creates difficulty in pulling the
film."

Rockwell's sensor set should be adopted wholesale into FR-1/FR-2: diffuse sensor for end of roll,
inductive sensor for tension-arm over/under travel and missing lug, registration sensor for mark
detection, retro-reflective photo-eye for outlet jams. Add the dancer's analog position feedback
into the unwind PID, and note that the dancer doubles as "a film accumulator area for bag index
purposes, **especially on an intermittent type machine**" — which is precisely this machine, with
its stop-start web.

> **Specify a web tension band, not a setpoint.** Rev B's FR-1 says "tension control" without a
> number. The Bath inequality needs Q explicitly (their example used 40 N), and Merabtene shows
> both too-low and too-high fail. Target: a commissioning-determined band per film, recipe-stored,
> with high and low alarms.

**Static.** One mechanism from Rockwell that bears directly on OR-7 and is absent from Rev B:
"high static charges are generated when plastic packaging film is fed from a wind-off roll. The
static charge attracts powder and lightweight dust from the filling product onto the internal
film surface, thus preventing effective sealing. A static eliminator bar or contaminant spray can
be used to reduce this problem." For rice with broken-grain dust, a **static eliminator bar at
the unwind is a cheap requirement with a direct seal-integrity payoff.**

### 7.4 Reel autonomy and changeover

| Film | Reel OD | Length | At 25 bpm × 290 mm | At 40 bpm |
|---|---|---|---|---|
| 60 µm | 650 mm | 5455 m | 12.5 h | 7.8 h |
| 80 µm | 650 mm | 4091 m | 9.4 h | 5.9 h |
| 80 µm | 600 mm | 3478 m | 8.0 h | 5.0 h |
| 100 µm | 650 mm | 3273 m | 7.5 h | 4.7 h |

(76 mm core.) At 80 µm on a 650 mm reel, a roll lasts one shift at 25 bpm but **not** at 40 bpm,
so at the stretch rate the reel change becomes a planned mid-shift stop. That is an availability
input (OR-9, ≥ 90 %) that Rev B has not costed. If 40 bpm is pursued, specify either a larger
reel OD or a **splice/dual-mandrel arrangement** — Rockwell lists dual mandrels, roll lifts and
vacuum splice bars as standard unwind options.

---

## 8. Dosing, fill dynamics, and the fill window

### 8.1 What actually sets the fill time

Bosch is emphatic that intuition misleads here: "the speed with which bags can be made is mainly
determined by the sealing time of the packaging material and **the length of the product
stream**," while "often it is thought that the falling time of the product also determines speed,
but that is a misunderstanding — with some exceptions. If there are high speeds, multiple batches
can be underway simultaneously. The one precondition is that there be adequate distance and time
between each sequential portion to allow for the cross seam jaws to close again."

| Drop height | Flight time | Impact velocity |
|---|---|---|
| 0.6 m | 0.350 s | 3.43 m/s |
| 0.9 m | 0.428 s | 4.20 m/s |
| 1.2 m | 0.495 s | 4.85 m/s |

Flight time of 0.35–0.50 s is **dead time that must be overlapped**, not spent — at 40 bpm
(1.5 s cycle) it is a third of the cycle. The FRS's memory hopper already enables this; make the
overlap an explicit FR-4/FR-11 requirement with a stated number of charges in flight.

The *fill window* — the time product is actually crossing the seal plane — is set by the
discharge gate duration, and stream length grows with height:

| Drop height | Gate open | Stream length at the bag | Fill window |
|---|---|---|---|
| 0.9 m | 0.10 s | 0.42 m | 0.10 s |
| 0.9 m | 0.15 s | 0.63 m | 0.15 s |
| 1.2 m | 0.25 s | 1.21 m | 0.25 s |

A 0.63–1.21 m product stream entering a 120 mm bag means **the stream is 5–10 bag-heights long**.
This is the mechanism behind grain-in-seal risk: the tail of the charge is still arriving long
after the head has landed, so the jaws cannot close until the whole stream has passed.

There is a genuine tension here with §4.4, and it should be stated as such rather than optimised
blindly:

- **Short gate / short window** → shorter product stream, jaws can close sooner, higher rate,
  **but** higher impulse on the fresh bottom seal (0.315 N/mm at 0.15 s vs 0.222 N/mm at 0.25 s).
- **Long gate / long window** → gentler on the seal, **but** longer stream and less cycle margin.

The resolution is to lower the drop height and widen the window, then recover the cycle time by
overlapping flight with transport — not by shortening the gate.

### 8.2 Flow area check

Passing a 1250 cm³ charge through the tube:

| Tube | Window | Bulk flow | Mean descent velocity |
|---|---|---|---|
| Round d = 127 mm | 0.15 s | 8333 cm³/s | 0.65 m/s |
| Round d = 127 mm | 0.25 s | 5000 cm³/s | 0.39 m/s |
| Rect 120 × 80 | 0.15 s | 8333 cm³/s | 0.87 m/s |
| Rect 110 × 70 (with wall clearance) | 0.15 s | 8333 cm³/s | 1.08 m/s |

All are well below free-fall velocity at the same station (3.4–4.9 m/s), so the tube is **not**
flow-limited in bulk terms at either geometry — the charge is dispersed, not packed. This
confirms the §7.2 recommendation is about stream length and hourglass risk rather than a hard
area limit.

### 8.3 Funnel and hourglass — specify, don't discover

Rev B treats the MHW-to-tube transition as an interface (§6) with no requirement on it. Bosch
devotes its first tips-and-tricks section to it: "the design of this funnel is very important for
packaging speed ... If there is a large product volume and a small inflow opening of the forming
tube, the hour glass effect easily occurs because at the end of the funnel, the product is slowed
down greatly." Three stated countermeasures, all cheap:

- Drop the product onto **the slanted side**, not the middle, so "a compact product ball turns
  into a product stream";
- A **thin rod suspended in the middle** of the funnel to prevent bridging;
- **Vertical strips dividing the inner wall** into segments, creating slides that stop the
  product spinning.

Where the dosing outlet is much larger than the tube inlet, use "a rectangular model with three
straight sides and one slanted side ... the dispensing happens on the slanted side, functioning
as a slide." Bosch also offers a practical development method worth adopting for FAT: prototype
funnels in acetate film, which is "sturdy," easy to reshape, and transparent "so it is also
possible to clearly see how the product is moving through the funnel."

> **New requirement FR-4.x:** the MHW-to-tube funnel is a designed, qualified component with an
> acceptance criterion (fill window ≤ X s with no bridging over N consecutive charges), not a
> fabricated transition.

### 8.4 Dust suction — the missing answer to OR-7

OR-7 demands **zero** grain entrapment, and FR-7 answers with timed drop-and-seal plus stripper
plates. There is a second, independent mechanism in Bosch that Rev B does not use, and for rice
it may matter more.

The problem: "when packing 'dusty' products, poor seal seams can be created by dust that blows
around downwards in the forming tube and ends up in the sealing seam when the cross seam sealing
jaws close. **It is not an option to wait until all the dust is in the bag** because this would
severely decrease the packaging speed." The cause is counter-flowing air: falling product pushes
air up the tube (it cannot go down — the bag is sealed), and that rising air strips dust out of
the product mass and leaves it drifting slowly downward, arriving *after* the grain. Worse: "this
problem compounds when the space between the wall of the forming tube and the product ball is
tight. As the space gets smaller, the air flow will propel through the product with greater
force."

Parboiled/instant rice carries broken-grain fines, and our tube is relatively close-fitting, so
this applies. The stated fix is mechanical, not pneumatic brute force:

1. A **second tube mounted inside the forming tube** — product falls through the inner tube,
   "while the air between both tubes is pushed upwards";
2. **Suction at the opening of both tubes** to improve the escaping air flow;
3. Critically: **split the annulus in half** — "on one side, the air can be suctioned away from
   the bottom, no vacuum is created in the bag because the air is sucked up along the other side
   from the top of the tube." Without the split, strong suction risks pulling a vacuum in the bag
   being filled.

> **New requirement FR-7.5:** a double-tube with split-annulus dust extraction at the forming
> tube, sized so that no vacuum is developed in the forming bag. Verification: fines count in
> the seal zone, and seal-strength distribution with and without extraction.

Note how this interacts with §7.2: the round-tube-plus-mouthpiece option gives more annulus room
for the inner tube than a rectangular tube would. Another point for option 1.

### 8.5 Why MHW, confirmed

Rockwell's dosing comparison places the multi-head weigher as the "high precision and accurate
dosing" option using "load cells for weight of products," against volumetric cup fillers
("dependent on time and gravity") and augers (powders/pastes). This supports the FRS's choice
without qualification. Keep in mind Rockwell's constraint from §1: at the top end, "the highest
machine speed attainable is determined by the weighing mechanism" — so the 40 bpm stretch target
needs the MHW's own rated bagging rate at 1 kg verified, not assumed.

---

## 9. Grain-in-seal detection — a correction to the FRS's reasoning

Rev B §2.3 asserts that "free-flowing grain cannot be reliably protected by servo 'product-in-seal'
*sensing* alone (that technique suits larger discrete items — Appendix B, Rovema)." The conclusion
is right; the reasoning is too absolute and discards a useful instrument.

Rockwell: "Often, position and torque data are monitored for the cross jaws. This data can easily
be used to detect product in the seal or between the jaws." A single rice grain will not show in
jaw torque. But the *distribution* of closure position and torque per cycle is a sensitive
signature of (a) grain accumulation, (b) stripper-plate wear, (c) film thickness variation and
(d) gusset misalignment — all of which are real failure modes in the reference trials.

> **Reframe FR-12/KPI 7.3:** keep mechanical clearing (drop-and-seal + strippers) as the primary
> defence, and specify jaw closure position and torque as a **logged process signature with
> statistical limits**, not as a per-brick grain detector. This is also what makes the FRS's
> existing "inline seal-force" KPI implementable.

Also from Rockwell, two jaw-mechanics details to put in FR-7: "it is very common for the jaws to
contain built-in springs to allow for some closure error as well as a default force" (which is
what makes a torque signature interpretable at all), and the knife "is recessed and is activated
by a pneumatic actuator" in one jaw against an anvil in the other — relevant to FR-8 and to
keeping the cut away from the sealed zone.

### 9.1 Gusset mechanism — learn from the documented failure

The LUT block-bottom failure analysis is worth importing directly into FR-5/FR-7 as design
constraints, because it is a documented root-cause investigation on exactly this bag style:

- **Root cause of crinkled bags and fractures: gusset misalignment.** "The top and bottom gussets
  are miss aligned which is the root cause of crinkles shaped bags and possible fracture. It was
  also observed that **the tightening nob for the gusset is only from one end which shifts gussets
  towards the left**." → Requirement: symmetric, independently adjustable, positively located
  gusset spreaders with repeatable datum — no single-ended clamping.
- **Gusset insertion depth is a tuned parameter.** The successful bags came from pulling "the top
  and bottom gusset ... slightly, approximately 2 mm to the back, to reduce the depth of inserted
  gusset." → Requirement: gusset depth adjustable with a scale and recipe-stored, ±0.5 mm
  repeatability.
- **Gusset force is bounded on both sides.** "These gusset spreads are too sharp when directed to
  fiber-based material. **Lowering the gusset pressure from pneumatic valves reduced the
  performance of gusset spreads.**" → There is no single pressure that is both gentle enough and
  effective; the geometry (spreader tip radius) must carry the load, not the pressure. Requirement:
  specify spreader tip radius and surface finish.
- **Pneumatic adjustment must be accessible.** "As it is inconvenient to open the pneumatic box
  every time, it is recommended to place the valves near the control panel or implement a digitized
  control unit." → For a recipe-driven machine (FR-13, ≤ 20–30 min SMED changeover across a
  0.5–5 kg family), gusset and belt/tension pressures must be **electronically set from the recipe**,
  not manually trimmed behind a panel. This is a changeover-time requirement masquerading as a
  pneumatics detail.

### 9.2 Jaw temperature uniformity

Rev B specifies seal temperatures but not uniformity across the jaw. The LUT measurements show why
that is a gap: with a setpoint of 130 °C and a single end-mounted heat source, tools read 127.4 to
132.5 °C (±2 %) across the bar, with "the highest temperature recorded at the origin of the heat
source." One tool was 15 % out, reading 10–20 °C below setpoint, traced to thick PTFE coating
acting as an insulator. Recommendation from the source: "it is preferable to have one heat source
in the middle or two heat sources in the ends to have accurate heat transfer across the sealing
jaws."

At a 120 mm seal width this is less severe than on their 140 mm+ bar, but it compounds with §4.1:
a jaw that is 10 °C cool at one end is cool *at a gusset corner* that already needs more energy
than anywhere else.

> **FR-7 requirement:** two heating elements per jaw (or one centred), each with its own
> thermocouple; uniformity ≤ ±3 °C across the seal width, verified at FAT by thermal imaging, not
> by a single probe. Specify PTFE/release-coating thickness as a controlled parameter — Rockwell
> notes each jaw has "one or two heating elements as well as a thermocouple," so two is standard
> practice. Also note Bosch's warning that jaw movement itself causes cooling, which matters more
> on continuous or draw-bar machines than here.

---

## 10. Consolidated changes proposed to FRS Rev B → Rev C

| # | FRS item | Rev B | Proposed Rev C | Driver |
|---|---|---|---|---|
| 1 | §1.4 brick dimensions | 120 × 150 × 80 mm (1440 cm³) | **120 × 120 × 80 mm (1152 cm³)**, settled ρ 0.868 g/cm³ | §5.1 — 1440 cm³ cannot be a firm brick |
| 2 | OR-6 / KPI 7.3 brick form | "dense, square, self-standing", ±3 mm | **Dimension-controlled** via hard-stopped shaping frame; log compaction stroke per brick | §5.2 — volume control gives 16.3 mm spread |
| 3 | OR-5 mean fill | ≈ 1002 g | **1000 g centred**, with reject limits guard-banded to 982/1018 g at 2·u_cw | §6.2–6.3 — higher Cpk, zero giveaway, same σ target |
| 4 | FR-7 cross-seal | Single station, drop-and-seal + strippers | **Separate the bottom-gusset seal from the cross-seal station**; dwell set per layer count | §4.1 — gusset needs 4–9× the plain-seal dwell |
| 5 | OR-1/OR-2 rate | ≥ 25 bpm, stretch 40+ | Keep, but **state the limiter as the dwell budget**, and re-derive the continuous-motion threshold from §3 rather than asserting 45 bpm | §1, §3 |
| 6 | FR-5 settling | Settle/densify step | **Concurrent** with transport and charge flight; specify **taps per brick (≥ 30)**, not seconds | §3 — 0.30 s buys only 6–18 taps |
| 7 | FR-1 film transport | "tension control" | **Belt pick-up, dual independent vacuum belts**, left/right error monitored; web tension **band** with alarms; static eliminator bar at unwind | §1, §7.3 |
| 8 | FR-3 shoulder | Block-bottom shoulder | Produced by **5-axis machining or additive from the mathematical collar surface**; specify collar fillet radius, surface roughness, shoulder-to-tube gap; **round tube + mouthpiece** | §7.1–7.2 |
| 9 | FR-6 back seam | "fin/lap" | **Fin seal** (laminate cannot seal inside-to-outside); specify bar length 200–305 mm with **retract-on-dwell**, or dual-bar / heated-belt | §4.5, §2 |
| 10 | FR-7 (new .4) | — | **Bottom support** of the filled brick until the bottom seal cools | §4.4 — 0.22–0.35 N/mm on a molten seal |
| 11 | FR-7 (new .5) | — | **Double-tube, split-annulus dust extraction** | §8.4 |
| 12 | FR-4 (new) | Interface only | **Funnel is a qualified component** with anti-bridging features and an acceptance test | §8.3 |
| 13 | §5 seal integrity | "meet film/laminate spec" | **≥ 1000 gf/25 mm (0.392 N/mm)** cured seal strength; add a **hot-tack** minimum at fill temperature (ASTM F1921) | §4.4–4.5 |
| 14 | FR-11 control | PLC/servo/HMI | Add **time-stamped registration**, **pending cams** for empty-skip, per-axis list of §2 | §2 |
| 15 | FR-12 / KPI 7.3 | Inline seal-force | Reframe jaw position/torque as a **logged statistical signature**, not a grain detector | §9 |
| 16 | FR-16 data | "data, traceability" | **Per-brick logging**; mean-trim loop on a **rolling 100-brick window, updated every 25, authority ±1 g** | §6.4 |
| 17 | §11 open items | — | **New open item: legal metrology.** Verify the 980 g floor against net-quantity rules for the target market | §6.5 |
| 18 | FR-5 / FR-13 | Gusset mechanics | Symmetric positively-located gusset spreaders, recipe-set depth ±0.5 mm, specified tip radius, **electronically set pneumatics** | §9.1 |
| 19 | FR-7 | Seal temperature | **Two heaters + two thermocouples per jaw**, uniformity ≤ ±3 °C across seal width, verified by thermal imaging | §9.2 |
| 20 | OR-9 availability | ≥ 90 % | Note **reel autonomy at 40 bpm is 5–8 h** — specify splice or dual mandrel if the stretch rate is pursued | §7.4 |

---

## 11. Test programme to close what analysis cannot

The analysis above has three load-bearing assumptions that should be measured before the design
is frozen. Everything else in §10 is derivable; these are not.

| # | Test | Measures | Closes | Method |
|---|---|---|---|---|
| **T-1** | Tapped/settled density of the actual rice | Consolidation ratio vs loose density, and taps-to-asymptote | §5.2 envelope choice — the whole brick dimension rests on the 1.00–1.20 ratio assumption | Tapped-density apparatus; sweep 10–200 taps at 2–3 loose densities; also measure amplitude dependence |
| **T-2** | Seal processing window for the **chosen rice laminate** | Seal strength vs temperature × dwell contour, per layer count (2, 4, 6) | §4.1 — the (n/2)² scaling and the 0.40 s anchor | Lab sealer, Merabtene's method: T-peel 90°, 300 mm/min, ASTM F88 prep, contour-mapped |
| **T-3** | Grain-in-seal vs jaw profile | Entrapment rate, serrated vs flat, with/without dust extraction | §4.3 serration trade-off and §8.4 | Run trials with deliberate fines loading; seal-strength distribution + leak test |
| T-4 | Hot tack at fill temperature | Hot-tack force vs time after seal | §4.4 bottom-support sizing | ASTM F1921 |
| T-5 | Charge discharge profile | Gate duration, stream length, arrival window at the seal plane | §8.1 — the 0.15/0.25 s window assumption | High-speed video on the MHW + funnel rig; acetate funnel prototypes |
| T-6 | Belt pull capacity vs mouthpiece drag | Available pull force with mouthpiece fitted | §7.1 back-angle driver | Instrumented belt drive, torque measurement |
| T-7 | Checkweigher uncertainty u_cw | Dynamic weighing repeatability at 1 kg, at line rate | §6.3 guard band width | Repeat weighings of certified masses at rate |

T-1 and T-2 are on the critical path: T-1 sets the brick dimension and T-2 sets the achievable
rate. They are also both cheap and do not need the machine.

---

## 12. Source inventory — what is in the folder and what it is worth

Three of the eleven PDFs are not about this subject, and one of them is an acronym collision that
would be easy to cite by mistake.

### Core sources — used heavily above

| File | What it is | Value |
|---|---|---|
| `Evaluation and optimization of a vertical form, fill and seal production machine for flexible packaging papers.pdf` | **Merabtene, M. (2020)**, MSc thesis, LUT University (examiners Leminen, Tanninen). Heat-sealing processing windows on a GKS Compack CP350 Plus, 3 jaw profiles, fibre and thermoplastic films, incl. **block-bottom bag trials** | **Highest.** The only source with measured machine parameters, a documented block-bottom failure analysis, and transferable force/pressure/temperature/dwell data. Caveat: materials are fibre-based and thin plastics, not a rice laminate |
| `modelling-the-material-flow-and-web-tension-in-the-vertical-...pdf` | **Matthews, Hicks, Mullineux, Goodwin & Burke**, University of Bath (DEFRA project AFM267/BIOFFS FT1560). Euler belt theory applied to forming-shoulder design; derives a back-angle bound from friction | **High**, and unique — the only theoretical shoulder-design method in the set. Caveat: circular tube geometry; its refs 10–12 (rhombic / linear-segment / trapezoidal sections) are the follow-ups needed for a quad-seal former |
| `Guide_to_Vertical_Form_Fill_Seal_Baggers.pdf` | **Bosch Packaging Technology (2014)**, author Gerard Verduyn. 90 pp practical guide: film path, shoulder/tube, seal systems, dosing, synchronisation, tips | **High.** The source of the jaw-draw-off/block-bottom exclusion, the cycle-fraction rules, the funnel/hourglass and dust-suction mechanisms. Vendor-authored but descriptive and consistent with the academic sources |
| `motion-at002_-en-p.pdf` | **Rockwell Automation (2012)**, *Integrated Motion Applications: Vertical Form Fill and Seal*, MOTION-AT002A-EN-P | **High** for FR-11. Axis architecture, time-stamped registration, pending cams, intermittent vs continuous rate figures (60–80 vs 180 ppm) |
| `AutomationofVFFSMachine.pdf` | **Rockwell Automation** white paper, *Automation of Intermittent VFFS Machine* | **High** for component-level detail: long-seal bar types and lengths, vacuum belts, cross-jaw construction, jaw torque sensing, static charge, dosing-method comparison |
| `Food-Packaging-Technology.pdf` | **Coles, McDowell & Kirwan (eds.)**, *Food Packaging Technology*, 362 pp reference text | **Medium–high**, selectively. Source of the **1000 gf/25 mm seal-strength requirement for free-flowing rice**, impulse sealing, fin-seal constraint, hot tack and LTS coatings, WVTR/barrier tables |
| `How-does-VFFS-machine-work.pdf` | Synda Pack vendor blog article, 10 pp | **Low.** Introductory marketing; nothing not better covered by Bosch. Fine as orientation, not as a citation |

### Off-target — do not cite these as VFFS sources

| File | What it actually is | Note |
|---|---|---|
| `Master_Thesis_Final_SHM_vanderVoort.pdf` | **van der Voort, S.H.M. (2021)**, TU Delft MSc, *Development of an instrumented mooring system for VFFS model testing* | ⚠️ **Acronym collision.** Here **VFFS = Very Flexible Floating Structures** — floating-structure hydrodynamics and mooring-force sensor calibration in a towing tank. Nothing to do with packaging. This is the one to watch: it is a 123-page thesis with "VFFS" in the title and would look authoritative in a bibliography |
| `Masters_Thesis_Kohtaniemi_Tommi.pdf` | **Kohtaniemi, T. (2021)**, LUT MSc, *Mechanical modification of packaging material structures* — embossed frames protecting NFC/RFID tags and printed conductors on paperboard | Packaging, but not VFFS and not machine design. Possible marginal relevance if the ORYZ pack ever carries a smart tag. Same LUT lab as Merabtene, which is probably why it was collected |
| `663b3174f2408.pdf` | **Ustundag & Cevikcan (2018)**, *Industry 4.0: Managing The Digital Transformation*, Springer, 294 pp | General Industry 4.0 management text. Usable only as background for the FR-16 connectivity/OEE framing; contributes nothing to mechanical design |
| `final1-212544-1108585-project-final-report.pdf` | **NAFISPACK** EU FP7 final report (grant 212544), *Natural Antimicrobials For Innovative and Safe Packaging* | Active/antimicrobial packaging for perishables. Not applicable to a dry, shelf-stable product and not machine design |

### Not yet mined

The `pattern/` folder holds **25 US patents** (US7302785 through US11628965 and several
applications). These are the natural next source for the §4.1 problem specifically — gusset and
block-bottom forming mechanisms, and multi-layer seal-jaw geometry, are exactly what gets
patented. Worth a targeted read against the four options in §4.2 before committing to the
separate-gusset-station architecture.

---

## 13. Where the design now stands

The brick is buildable, but two of its FRS numbers were set independently of each other and do
not intersect: **the 1440 cm³ envelope and the "firm brick" requirement**, and **the single-station
cross-seal and the zero-entrapment requirement at 25–45 bpm**. Both have clean resolutions —
shrink the brick to ~1152 cm³ and control it dimensionally; split the gusset seal into its own
station — and neither resolution costs throughput. The third-order finding is that the rate
ceiling is set by seal dwell at the gusset corners, not by film speed, motion architecture, or
dosing; everything in the cycle budget has slack except the dwell.

The cheapest next step is T-1 and T-2 in §11. Neither needs the machine, and between them they
fix the brick dimension and the achievable rate — the two numbers everything else hangs off.

---

### Sources

- Merabtene, M. (2020). *Evaluation and optimization of a vertical form, fill and seal production machine for flexible packaging papers.* MSc thesis, LUT University.
- Matthews, J., Hicks, B.J., Mullineux, G., Goodwin, J. & Burke, A. *Modelling the material flow and web tension in the vertical form-fill-seal packaging process.* University of Bath (DEFRA AFM267 / BIOFFS FT1560).
- Bosch Packaging Technology (2014). *Guide to Vertical Form-Fill-Seal Baggers* (G. Verduyn).
- Rockwell Automation (2012). *Integrated Motion Applications: Vertical Form Fill and Seal*, MOTION-AT002A-EN-P.
- Rockwell Automation. *Automation of Intermittent VFFS Machine* (white paper).
- Coles, R., McDowell, D. & Kirwan, M.J. (eds.). *Food Packaging Technology.*
- Secondary citations as credited within Merabtene: Hishinuma (2009); Theller (1989); Meka & Stehling (1994); Stehling & Meka (1994); Yuan et al. (2007); Najarzadeh & Aiji (2014); Mueller et al. (1998); Bamps et al. (2019); Boersma & Molenaar (1995); Desoki, Morimura & Hagiwara (2010); McPherson et al. (2005).
- Derivations in §3–§8 computed for this document; assumptions stated inline. Thermal diffusivity for the PE sealant taken as 1.3 × 10⁻⁷ m²/s.
