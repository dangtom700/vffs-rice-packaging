# VFFS Rice Brick — Canadian Regulatory & Compliance Specification (Rev A)

Oct 25, 2026 · @Son Tung Dang

## 1. Scope and the Canadian conformity model

Canada has no CE-equivalent machine mark, so there is no single document that makes a VFFS line "compliant". Legality is assembled by three duty-holders at three different moments, and the builder controls only the first.

| Duty-holder | What they owe | When | Consequence of a gap |
| --- | --- | --- | --- |
| Builder / designer (you) | Safe design, certified or field-approved electrical work, food-safe construction, evidence package, manual, markings | Before shipment | Machine cannot be energised or legally sold into service |
| Employer / occupier (the rice packer) | Risk assessment, safeguarding kept in place, lockout procedure, worker training, preventive control plan under their SFCR licence | Before and during operation | Provincial OH&S order, stop-work, prosecution |
| Authority having jurisdiction | Electrical inspection and permit; in Ontario, a Pre-Start Health and Safety Review | At installation | Line cannot be commissioned |

A machine can hold a full certification mark and still be unlawful to run, and a safely guarded machine with an uncertified panel will still be red-tagged. Both failure modes are designed out here.

### Machine under specification

Per FRS Rev B: a vertical form-fill-seal machine producing a 1 kg block-bottom / quad-seal rice brick, firm-fill (non-vacuum), densified by vibratory settling and de-aeration with a shaping frame, fed by a multi-head weigher, running preprinted laminate film from a nominated supplier. Baseline 25 bricks/min, stretch 40+. Scope runs film unwind to brick discharge; cartoning, palletising and upstream rice handling are excluded.

### Declared deployment envelope

This envelope is the limit specification: it is what the machine is certified and warranted for, and anything outside it moves the compliance burden back to the buyer. State it in the quotation, not after the order.

| Parameter | Declared limit | Compliance consequence if the buyer exceeds it |
| --- | --- | --- |
| Product class | Dry milled or parboiled rice, water activity ≤ 0.6, non-oily, no declared allergen in the product stream | Hygienic design is dry-clean; a wet, oily or allergen-bearing product invalidates the cleaning validation and the material selection |
| Cleaning regime | Dry cleaning plus controlled wet spot-cleaning of removable parts; no CIP, no hose-down, no high-pressure washdown | Enclosure IP ratings, drainage and bearing selection are specified to this regime only |
| Area classification | Indoor, unclassified (non-hazardous) location, 5–40 °C, ≤ 80 % RH non-condensing | Rice dust is a combustible dust; if the buyer's dust hazard analysis classifies the room, the electrical design changes entirely |
| Electrical supply | Declared nominal voltage/phase/frequency with a declared short-circuit current rating at the supply terminals | A supply above the declared SCCR makes the panel non-compliant as installed |
| Personnel | Trained industrial operators, no public access, no minors | Safeguarding is specified for trained-operator exposure, not public-access exposure |
| Jurisdiction of use | Primary: British Columbia. Ontario and Quebec addressed in §11 | Sale outside Canada requires a delta assessment, not this specification |

### Requirement ID convention

Every requirement below carries an ID in the form `RGx-nn`: **RGE** electrical, **RGS** safety and safeguarding, **RGP** pressure and pneumatics, **RGF** food contact and hygiene, **RGM** metrology, **RGL** labelling and coding, **RGN** emissions, **RGD** documentation. Each states the obligation, its legal or normative source, an acceptance criterion that can be measured or inspected, and a verification method. §12 maps every ID to its evidence artifact.

## 2. Regulatory architecture

Ten separate legal instruments bite on this machine, administered by seven authorities, and none of them subsumes the others. The common design error is to treat "CSA" as the whole picture; CSA publishes standards but creates no obligation. The obligation always comes from a statute or regulation that *references* a standard.

| # | Domain | Instrument that creates the duty | Authority | What it actually bites on | Block |
| --- | --- | --- | --- | --- | --- |
| 1 | Worker safety | BC: Workers Compensation Act + OHS Regulation. ON: O. Reg. 851. QC: RSST | WorkSafeBC / MLITSD / CNESST | Guarding, interlocks, lockout, noise, ergonomics, pre-start review | RGS |
| 2 | Electrical safety | Provincial electrical regulation adopting CSA C22.1 (Canadian Electrical Code Part I) under the BC Safety Standards Act | Technical Safety BC / ESA (ON) / RBQ (QC) | Whether the machine may be energised at all; approval mark or field evaluation; permit | RGE |
| 3 | Pressure equipment | BC Power Engineers, Boiler, Pressure Vessel and Refrigeration Safety Regulation; CSA B51 | Technical Safety BC / TSSA (ON) | Air receivers and vacuum vessels above the registration threshold | RGP |
| 4 | Food safety (process) | Safe Food for Canadians Act + Safe Food for Canadians Regulations | CFIA | Equipment construction, cleanability, maintenance, the packer's preventive control plan | RGF |
| 5 | Food contact chemical safety | Food and Drugs Act s. 4(1)(a); Food and Drug Regulations Division 23 | Health Canada, Bureau of Chemical Safety; enforced by CFIA | Whether any material touching rice can impart a harmful or objectionable substance | RGF |
| 6 | Legal metrology | Consumer Packaging and Labelling Act + Regulations (food: administered by CFIA); Weights and Measures Act + Regulations | CFIA + Measurement Canada (ISED) | Accuracy of the 1 kg declaration; whether the weigher or checkweigher needs type approval | RGM |
| 7 | Food labelling | SFCR Part 11; Food and Drug Regulations Part B; CPLA/CPLR | CFIA | Mandatory label content, bilingual presentation, lot identification, date marking | RGL |
| 8 | Radio and EMC | Radiocommunication Act; ICES-003; RSS standards | ISED | Conducted and radiated emissions from drives, servos and any wireless module | RGN |
| 9 | Combustible dust | Provincial OHS plus CSA C22.1 Section 18 where the area is classified | WorkSafeBC (and the occupier's dust hazard analysis) | Whether rice dust in the enclosure forces a classified-location electrical design | RGS, RGE |
| 10 | Language of documentation | Charter of the French Language (Quebec only) | OQLF | Manual, HMI text and safety signage language | RGD |

### Two consequences worth stating up front

First, **the standards are not law until a regulation adopts them**, and the adopted edition is frozen at the date of adoption. A provincial regulation may still point at an older CSA edition than the one CSA currently sells, so the edition named in the regulation governs, and buying the newest edition is not automatically correct. §14 records editions and how to verify currency.

Second, **three of the ten domains are not yours to discharge**. Domains 4 and 7 belong to the licence holder packing the rice, and domain 1's operating duties belong to the employer. Your obligation in those domains is to build a machine that *lets* them comply and to hand over the evidence they need. Writing a specification that promises CFIA compliance of the finished pack is overreach and unenforceable; the FRS should promise capability, not the packer's outcome.

## 3. RGE — Electrical conformity and certification route

The machine may not be energised in BC until every electrical assembly bears either a certification mark from a certification body accredited by the Standards Council of Canada, or an approval label issued under s. 10 of the Safety Standards Act — the [Technical Safety BC bulletin](https://www.technicalsafetybc.ca/regulatory-resources/regulatory-notices/approved-certification-marks-electrical-products) names "the SPE1000 model code field approval" as an example of the latter. This is the single hardest gate in the whole specification, because it is binary, it is enforced by an inspector at the site, and it cannot be retrofitted cheaply.

FR-14.3 currently says electrical compliance shall be met "per the destination market (e.g. machinery safety directive/standards, UL/CE controls)". For a Canadian machine that sentence names no applicable instrument: there is no machinery directive in Canada, CE marking has no legal standing here, and a plain UL mark without the Canadian `c` prefix is not accepted. The requirements below replace it.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGE-01 | Every electrical assembly shall carry an accepted certification mark or a field approval label before energisation | Electrical Safety Regulation, B.C. Reg. 100/2004, under the Safety Standards Act | Mark or label present and legible on each assembly; certificate or field approval report on file | Inspection at FAT and SAT; document review |
| RGE-02 | Installation design shall conform to the edition of CSA C22.1 in force in the province of installation | CSA C22.1 as adopted provincially | BC: BC Electrical Code 2024 edition (CSA C22.1:24, 26th ed.), in force 4 March 2025 | Design review by the responsible engineer |
| RGE-03 | The machine's electrical equipment shall be designed to CSA C22.2 No. 301:23, *Industrial electrical machinery* | The Canadian counterpart to NFPA 79; the basis on which a certification body will evaluate the machine | Clause-by-clause compliance for supply disconnect, overcurrent protection, control-circuit voltage and isolation, bonding, conductor identification, stop categories, markings and supplied documentation | Third-party evaluation report |
| RGE-04 | The control enclosure shall be built and certified as an industrial control panel to CSA C22.2 No. 286 | Panel-level certification standard | Panel bears a certification mark or is covered by the machine-level field approval | Certification body report |
| RGE-05 | A short-circuit current rating shall be determined and marked on the panel nameplate | CSA C22.2 No. 286 / No. 301 | Marked SCCR ≥ the available fault current declared by the buyer at the supply terminals | Calculation on file; buyer's written declaration of available fault current |
| RGE-06 | All components shall bear a Canadian certification mark (cCSAus, cULus, cETLus, cQPSus or equivalent) | Prerequisite for both certification routes | Zero uncertified components in the bill of materials; any unlisted component identified before build | BOM audit against certificates |
| RGE-07 | Enclosure and device ingress ratings shall match the declared cleaning regime, not exceed it | CSA C22.2 No. 94.2 enclosure types | Control enclosure Type 12; field devices and motors in the product zone Type 4X stainless; no Type 4X/IP69K requirement on the main panel | Inspection; cleaning trial under §6 |
| RGE-08 | The supply disconnecting means shall be lockable in the off position and be the single point of electrical isolation | CSA C22.2 No. 301; CSA Z460 | One lockable disconnect; any retained live circuit clearly marked and listed in the manual | Functional test; inspection |
| RGE-09 | The design assumes an unclassified location; classified-location design is excluded unless contracted | CSA C22.1 Section 18; buyer's dust hazard analysis | Statement in the quotation; buyer confirms the area is unclassified in writing | Buyer's written confirmation |

### Choosing the route, and what it costs

Two routes satisfy RGE-01, and the choice is commercial rather than technical. Decide it before the panel is built, because the evidence each route needs is gathered during the build, not after.

**Factory certification** means a certification body evaluates the design once and the mark is applied to every unit produced to that design. The engineering cost is front-loaded and the per-unit cost is near zero, so it is the correct route for anything built more than two or three times. It also removes the site-inspection risk entirely.

**Field evaluation** under [CSA SPE-1000:2025](https://blog.ansi.org/ansi/csa-spe-1000-2025-field-evaluation-code/) (6th edition, which superseded the 2021 edition) means an accredited body inspects the specific machine, at the site, and applies a label to that unit only. It suits a one-off or a prototype, which is what a capstone machine is. The risks are that it happens after the machine is installed, that failures mean rework at the customer's site, and that each additional unit pays the full fee again.

For this project the realistic answer is a field evaluation, so the design must be built to pass one: use only certified components, build to CSA C22.2 No. 301:23 as if a factory evaluation were coming, and book the evaluation body before shipping rather than after. A field evaluator who finds an uncertified sensor or an undetermined SCCR will stop, and the line stays dark.

&#91;embedded content: electrical approval route · 1 decision, 2 routes, 1 shared gate\]

The prerequisites sit above the decision deliberately: they are identical on both routes, so deferring the route choice does not defer the build discipline it depends on.

## 4. RGS — Machine safety, safeguarding and functional safety

In British Columbia the legally binding safeguarding standard is the **1994** edition of CSA Z432, because [OHS Regulation s. 12.3](https://www.worksafebc.com/en/law-policy/occupational-health-safety/searchable-ohs-regulation/ohs-regulation/part-12-tools-machinery-and-equipment) requires that the application, design, construction and use of safeguards, including guard openings and reach distances, meet "CSA Standard Z432-94". CSA currently sells CSA Z432:23. Buying the current edition and assuming it satisfies the regulation is the mistake to avoid.

The workable position: design to CSA Z432:23 and ISO 13857, which are more demanding and are what a competent engineer would defend, then **additionally** verify guard openings and reach distances against the Z432-94 tables, and record both checks. Where the two editions disagree, meet the stricter. For a packaging machine specifically, ANSI/PMMI B155.1-2023 is the task-based risk-assessment standard written for this machine class and is the right framework to run the assessment in, even though no Canadian regulation adopts it.

Two honest gaps in the BC regulation are worth knowing. Part 12 contains **no general emergency-stop requirement** and **no general machine risk-assessment requirement** — the written risk assessment duties at ss. 12.17.1 and 12.98 are for log loaders and abrasive blasting. The obligation to assess and to fit an e-stop therefore arrives indirectly, through s. 12.3's adoption of Z432, the employer's general duty, and the designer's own due diligence. Treat them as mandatory anyway; an incident is judged on what a competent designer should have done, not on the thinnest reading of the regulation.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGS-01 | A documented task-based risk assessment shall cover every lifecycle phase: operation, changeover, jam clearing, cleaning, maintenance | ANSI/PMMI B155.1-2023; CSA Z432:23; ISO 12100 | Every identified hazard carries a risk-reduction measure and a stated residual risk; assessment sealed by a professional engineer | Document review; required as the PSR report in Ontario |
| RGS-02 | Guard openings and reach distances shall be verified against both CSA Z432-94 and ISO 13857, meeting the stricter | OHS Regulation s. 12.3 | Dual-column check table for every opening, with the governing value marked | Measurement at FAT against the check table |
| RGS-03 | Fixed guards shall not be removable without a tool | OHS Regulation s. 12.5 | No captive-latch or hand-removable fixed guard anywhere on the machine | Inspection |
| RGS-04 | Access to the seal jaws, forming tube, film path and weigher discharge shall be through interlocked movable guards designed against defeat | ISO 14119; CSA Z432:23 | Coded magnetic or RFID interlocks; no spare actuators shipped; defeat-motivation assessed and recorded per ISO 14119 Cl. 7 | Functional test of every interlock; defeat attempt documented |
| RGS-05 | Each safety function shall have a required performance level determined and achieved | ISO 13849-1 | See the safety-function table below; achieved PL ≥ required PL, with the calculation on file | Validation per ISO 13849-2; safety-PLC report |
| RGS-06 | An emergency stop shall be provided at the operator station and at the weigher platform, with manual reset that does not restart the machine | ISO 13850 | Stop category 1 on the servo film drive and jaw drive, category 0 on the heater contactors; reset arms but does not start | Functional test; stopping-time measurement |
| RGS-07 | Hot surfaces shall be guarded, insulated or limited | ISO 13732-1; CSA Z432:23 thermal hazards | No touchable bare-metal surface above 65 °C in normal operation; jaws enclosed and marked; residual hot surface declared in the manual | Thermal survey at steady state |
| RGS-08 | Stored energy shall be dissipated or positively blocked before access | OHS Regulation Part 10; CSA Z460 | Lockable pneumatic isolation valve with downstream bleed; jaw assembly mechanically blockable; film tension releasable | Functional test with gauge at zero |
| RGS-09 | The machine shall ship with a written lockout procedure and an energy-source inventory | OHS Regulation ss. 10.3–10.4; CSA Z460 | One lockable electrical disconnect, one lockable air isolation, every residual energy source listed with its control step | Document review |
| RGS-10 | Hazard signage shall follow the referenced signage standards and be bilingual where required | OHS Regulation s. 12.13 (CSA Z321-96, ANSI/ISO signage) | Pinch, crush, hot-surface and electrical signage at each hazard; French added for Quebec | Inspection against the signage schedule |
| RGS-11 | Machine noise shall not require hearing protection in normal operation | OHS Regulation Part 7 (85 dBA Lex exposure limit) | ≤ 80 dBA at 1 m, 1.6 m height, at rated speed | Sound-level measurement at FAT |
| RGS-12 | Manual handling of film reels shall stay inside safe-lifting limits or be mechanically assisted | OHS Regulation Part 4, MSI prevention | Declared maximum manual reel mass 23 kg; above that a reel lift or shaft hoist is supplied as part of the machine | Reel mass calculation; lift aid supplied |
| RGS-13 | The machine shall not introduce an ignition source into a rice-dust atmosphere | Provincial OHS; NFPA 61 and NFPA 652 as good practice | All rotating product-contact assemblies bonded and grounded; dust-tight product zone; no exposed hot surface in the dust envelope above the declared limit | Continuity test; design review |
| RGS-14 | Reaching into the seal or fill zone to clear a jam shall be impossible without an interlocked stop | ANSI/PMMI B155.1-2023; CSA Z432:23 | Jams clearable from outside the guard with a supplied tool, or via a hold-to-run reduced-speed mode; no mode that allows jaw closure with the guard open | Task trial during FAT with the real failure modes |

### Safety-function and performance-level schedule

This is the table a field evaluator and a PSR engineer will both ask for, and it is the part most student designs omit. Severity, frequency and avoidance are assigned per ISO 13849-1 Annex A; the jaw hazard scores worst because it combines a crushing force with a 150–200 °C surface, so an injury is irreversible.

| Safety function | Hazard controlled | S / F / P | Required PL | Architecture | Notes |
| --- | --- | --- | --- | --- | --- |
| Cross-seal jaw guard interlock | Crushing plus burn at the jaws | S2 / F2 / P2 | PL d | Cat. 3, dual-channel, safety relay or safety PLC | The defining safety function of the machine |
| Film-path and forming-tube guard interlock | Draw-in and nip at the unwind and shoulder | S2 / F2 / P2 | PL d | Cat. 3 | Draw-in injuries are not avoidable once started |
| Weigher access interlock | Entanglement in feed pans and buckets | S2 / F1 / P2 | PL d | Cat. 3 | Lower frequency, same severity |
| Emergency stop | All hazards | S2 / F2 / P2 | PL d, stop cat. 1 | Cat. 3 | ISO 13850; complementary measure, not a safeguard |
| Heater over-temperature cut-out | Fire and film decomposition | S2 / F1 / P2 | PL c | Cat. 2 with independent thermal cut-out | Independent of the temperature controller |
| Reduced-speed jam-clear mode | Jaw and nip access during clearing | S2 / F2 / P2 | PL d | Cat. 3 with hold-to-run and enabling device | Only if RGS-14 is met by a mode rather than a tool |

## 5. RGP — Pneumatics, vacuum and compressed air

An air receiver bolted to the machine frame is a regulated pressure vessel in British Columbia, not a component. The [Technical Safety BC air receiver bulletin](https://www.technicalsafetybc.ca/regulatory-resources/regulatory-notices/information-bulletin-air-receivers) covers "stand-alone air receivers; and machine-mounted air receivers" alike, requires a Canadian Registration Number "stamped on the vessel nameplate" containing "a digit '1' to the right of the decimal point" to be valid for BC, and requires an installation notification plus an operating permit "prior to placing the vessel into operation".

That creates a design lever worth taking early: **if the machine carries no receiver of its own and runs on plant air, the whole registration and permitting chain disappears.** A small on-machine accumulator sized below the regulation's exemption threshold is the next-best option. Carrying a convenience receiver without checking the threshold is how a machine arrives on site and cannot legally be pressurised.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGP-01 | The design shall either omit an on-machine air receiver or keep it below the exemption threshold in the BC Power Engineers, Boiler, Pressure Vessel and Refrigeration Regulation | Power Engineers and Boiler and Pressure Vessel Safety Act; CSA B51 | A written determination, before procurement, of whether the chosen receiver is exempt; the threshold confirmed against the regulation text, not from a vendor's claim | Engineering determination on file; confirmation from Technical Safety BC |
| RGP-02 | Any receiver supplied shall bear a CRN registered for British Columbia | CSA B51; TSBC bulletin | Nameplate CRN present, with the digit "1" to the right of the decimal point | Nameplate inspection and photograph in the evidence pack |
| RGP-03 | The buyer shall be notified in writing of the installation notification and operating permit duties | TSBC bulletin | Handover letter names the installation notification and the requirement for an operating permit before operation | Document review at handover |
| RGP-04 | Pressure relief shall be provided and scheduled for maintenance | TSBC bulletin; CSA B51 Table 5 | PRD set pressure ≤ the nameplate MAWP; annual lift test and service or replacement at least every five years written into the maintenance schedule | Maintenance schedule review; PRD certificate |
| RGP-05 | The pneumatic system shall be designed to the recognised fluid-power safety standard | ISO 4414 | Circuit diagram, component ratings and a stated behaviour on loss of supply for every actuator | Design review against the circuit diagram |
| RGP-06 | Loss of air pressure shall not cause hazardous movement | ISO 4414; ISO 13849-1 | Seal jaws and shaping frame either spring-returned to a safe position or mechanically retained; verified by deliberate supply dump at pressure | Air-dump test at FAT, jaw position recorded |
| RGP-07 | A lockable air isolation valve with downstream bleed shall be fitted at the machine inlet | OHS Regulation Part 10; CSA Z460 | One lockable valve; residual pressure reads zero on the machine gauge within a stated time after isolation | Functional test with gauge |
| RGP-08 | Compressed air contacting product, the film's inner surface or the pack headspace shall meet a declared purity class | SFCR s. 47(2); BCAS Guideline 102 s. 7 | ISO 8573-1:2010 Class 1:2:1 at the point of use, with point-of-use filtration; tested at least twice a year per BCAS s. 8.2; microbial risk considered per ISO 8573-7 | Point-of-use air quality test report |
| RGP-09 | Vacuum pressure equipment is excluded from this revision | FRS Rev B is a non-vacuum firm-fill brick | Specification states the exclusion; the parallel vacuum-brick concept requires a separate pressure-equipment assessment before it is costed | Scope statement in the quotation |

RGP-09 matters for the vacuum-brick variant under exploration alongside the FRS. Adding a vacuum chamber and pump changes this section materially: a vacuum vessel is still pressure equipment, the pump becomes a noise and oil-mist source in a food zone, and the brick's seal integrity moves from a quality requirement to a pressure-differential one. Do not carry this section across to that concept unchanged.

## 6. RGF — Food contact materials and hygienic design

Rice is a low-moisture food, so the hygienic design target is **dry-cleanable**, and 3-A Sanitary Standards are the wrong reference to cite. 3-A is written for liquid dairy equipment cleaned in place; applying it here would specify drainage slopes, CIP circuits and flood-cleanable joints that serve no purpose on a rice line and that actively create a hazard, because introducing water into a dry rice environment converts a dormant *Salmonella* risk into a growth risk. The relevant guidance is EHEDG Doc 22 on dry particulate materials and the low-moisture sanitary design principles used across the dry-food industry.

The binding text is [SFCR s. 53](https://laws.justice.gc.ca/eng/regulations/SOR-2018-108/page-5.html), which requires equipment to be "designed, constructed and maintained to prevent contamination of the food", materials "suitable for their intended use", food-contact surfaces "smooth, free from pitting, cracks and flakes and non-absorbent", equipment "accessible", and "equipped with instruments to control, indicate and record any parameters" needed to prevent contamination. That last clause is often missed and is a direct requirement on the control system, not just the mechanics.

### The document your draft got wrong

The earlier research note suggested requesting a voluntary Health Canada Letter of No Objection for proprietary food-contact materials in the machine. That is not available for this purpose: per [CFIA guidance](https://inspection.canada.ca/en/preventive-controls/food-establishments), "The CFIA no longer pre-approves such materials and products", CFIA "no longer issues LOAs for food-contact work apparel or construction materials", and Health Canada does not issue LONOs for construction materials at all. LONOs cover packaging materials, and even there submission is voluntary and the letter is not a legal approval.

The instrument that actually applies to machine parts is the **supplier Letter of Guarantee**, which CFIA says "must be included in your Preventive Control Plan". So the deliverable is a LOG pack: one letter per food-contact material, naming the composition and the intended use, which the packer files in their PCP. Promising the buyer a LONO is a promise you cannot keep.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGF-01 | Metallic food-contact surfaces shall be austenitic stainless steel, continuously welded and dressed | SFCR s. 53(b),(c) | 304 minimum, 316L where cleaning chemicals contact; welds continuous, ground and passivated; surface finish Ra ≤ 0.8 µm; no pitting, cracks or flakes | Mill certificates to EN 10204 3.1; roughness measurement; weld inspection |
| RGF-02 | Every non-metallic food-contact part shall have documented food-contact compliance | Food and Drug Regulations B.23.001; SFCR s. 53(b) | Supplier Letter of Guarantee on file for each polymer, elastomer and coating, naming composition and intended use; FDA 21 CFR 170–199 status accepted as supporting evidence, not as the sole basis | LOG pack audit against the food-contact BOM |
| RGF-03 | No Health Canada LONO or CFIA acceptance shall be represented as obtainable for machine construction materials | CFIA guidance on construction materials | Quotation and FRS contain no promise of a LONO or LOA for machine parts | Document review |
| RGF-04 | The product zone shall be designed for dry cleaning with controlled wet spot-cleaning of removable parts only | EHEDG Doc 22; SFCR s. 50(2) | No CIP circuit, no flood or hose-down; written dry-clean procedure validated | Cleaning trial with swab or ATP verification |
| RGF-05 | The product zone shall be fully accessible and disassemble without tools | SFCR s. 53(c) "accessible" | Forming tube, shoulder, transfer chute, shaping frame and jaw faces removable by hand or with one supplied tool; target changeover and clean-down time stated in the FRS | Timed disassembly trial at FAT |
| RGF-06 | The product zone shall contain no product traps | SFCR s. 53; FRS zero-entrapment requirement | No horizontal ledges, blind holes, hollow bodies, exposed threads or unsealed lap joints inside the product zone; gusset-forming hardware designed so grain cannot lodge | Visual and borescope inspection; grain-residue count after a clean-down |
| RGF-07 | Hygiene zones shall be defined on a drawing and respected by the mechanical layout | EHEDG Doc 22 | Food zone, splash zone and non-food zone drawn; no bearing, gearbox, lubricated joint or fastener above an open food zone unless shielded with captive drip containment | Design review against the zoning drawing |
| RGF-08 | All lubricants used in or above the food zone shall be food-grade registered | SFCR s. 52 — agents "suitable for its intended use" | NSF H1 registered, ISO 21469 certified where incidental contact is credible; lubrication schedule names the registered product and its registration number | Lubricant schedule; registration certificates |
| RGF-09 | Compressed air that contacts product, the film's inner surface or pack headspace shall be treated | SFCR s. 47(2); good practice for food-grade air | Oil-free or oil-removed to a declared ISO 8573-1 class with filtration; filters listed in the maintenance schedule | Air quality test report at the point of use |
| RGF-10 | The machine shall not introduce foreign-material hazards | SFCR s. 47(1) physical hazards | No paint, glass, brittle or shatter-prone plastic in or above the product zone; fastener inventory with torque and inspection interval; captive fasteners above the open tube | Foreign-material design review; fastener audit |
| RGF-11 | The control system shall record the parameters that prevent contamination | SFCR s. 53(d) | Seal temperature, dwell and pressure recorded per pack or per batch with time stamps, retained and exportable | Functional test of the record export |
| RGF-12 | A hygiene dossier shall be delivered with the machine | SFCR s. 47; the packer's PCP obligation | Zoning drawing, food-contact BOM, LOG pack, validated cleaning procedure, lubricant schedule, all in one indexed package | Document review at handover |

RGF-11 deserves emphasis. The FRS already requires seal-parameter control for seal integrity, which is a quality requirement. SFCR s. 53(d) turns the same capability into a *regulatory* requirement, because a defective seal on a 1 kg brick is a contamination pathway. The same sensors satisfy both, but only if the data is retained and exportable, so specify retention and export format now rather than treating it as an MES nice-to-have.

## 7. RGM — Legal metrology and fill control

**The number that governs a 1 kg declaration is 985 g, and FRS Rev B currently sets the lower reject limit at 980 g.** That is the one place in the project where the specification and the regulation actively disagree, and it is free to fix.

[Schedule I of the Consumer Packaging and Labelling Regulations](https://laws-lois.justice.gc.ca/eng/regulations/C.R.C.,_c._417/FullText.html) sets the tolerance for a declared net quantity "more than 500 to not more than 1 kilogram" at a flat **15 g** — not a percentage. So a 1 kg brick below 985 g is deficient by more than the tolerance, and one below 970 g is deficient by more than twice the tolerance. The FRS's ±2 % process band (980–1020 g) was derived from capability economics and happens to straddle the legal threshold.

### How a lot is actually judged

Under s. 39(4) a lot fails if **any one** of three tests fails. Sample size comes from Schedule II Part I: 32 units for a lot of 129 to 4 000, rising to 125 above 12 000.

| Test | The rule | Applied to FRS Rev B (mean 1002 g, σ ≤ 4.51 g) | Verdict |
| --- | --- | --- | --- |
| s. 39(4)(a) | The weighted average of the sample is below the declared quantity. There is no tolerance on the lot average | Weighted average adds t/√n · s to the sample mean, about 0.485 s at n = 32, giving 1002 + 2.2 = 1004.2 g against 1000 g | Passes, 4.2 g of margin |
| s. 39(4)(b) | Too many units are below declared by more than the 15 g tolerance. A sample of 21–32 fails at 3 deficient units; 33–50 fails at 4 | Fraction below 985 g is Φ(−3.77) ≈ 82 ppm, so about 0.003 deficient units in a sample of 32 | Passes on probability, but not by design |
| s. 39(4)(c) | Two or more units are below declared by more than twice the tolerance, i.e. below 970 g | Fraction below 970 g is Φ(−7.1), effectively zero | Passes |

So the lot average is not the binding constraint — the FRS has comfortable margin there, and the weighted-average adjustment works in the packer's favour. The binding constraint is the **count of individual deficient units**, and that is governed entirely by where the checkweigher's lower reject limit sits.

### The fix

With the reject limit at 980 g, every brick between 980 g and 985 g ships as a legally deficient unit. At the FRS's own capability figures that is about **81 packs per million** — far inside the s. 39(4)(b) allowance, so it would not fail an inspection, but it is avoidable exposure for no benefit.

Moving the lower reject limit to **985 g** makes the deficiency rate zero by construction: no unit below the tolerance can reach the market, so tests (b) and (c) become unfailable regardless of process drift. The cost is the extra rejects between 980 g and 985 g, which is the same 81 ppm — roughly 81 extra rejects per million bricks. That is a negligible price for removing a legal failure mode, and it is the single highest-value change this specification recommends to the FRS.

Note the asymmetry this creates. There is **no upper legal tolerance**: overfilling is lawful and merely expensive, while underfilling is unlawful. The 1020 g upper limit is therefore a giveaway control, not a compliance limit, and the mean should be set by giveaway economics subject to a hard 985 g floor — which is exactly the logic FRS §8.3 already uses, now with the correct floor.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGM-01 | The checkweigher lower reject limit shall be set at 985 g | CPLR Schedule I, 15 g tolerance at a 1 kg declaration | Reject limit 985 g, enforced in the recipe and not operator-editable without authorisation | Recipe inspection; reject trial with a 983 g test pack |
| RGM-02 | The dosing mean shall be set so the lot weighted average cannot fall below 1000 g | CPLR s. 39(4)(a) | Mean target ≥ 1000 g plus a documented margin; FRS's 1002 g accepted, with the derivation retained | Lot statistics from a production trial |
| RGM-03 | Every brick shall be weighed and non-conforming bricks positively rejected | CPLR s. 39(4)(b),(c); FRS FR-12.1 | 100 % checkweigh; reject confirmed by sensor; reject bin lockable and reconciled; failure to confirm a reject stops the machine | Functional test including a forced reject failure |
| RGM-04 | Tare shall be determined per film reel, not once per format | CPLR — net quantity excludes packaging | Film mass per unit length recorded for each reel lot and applied to the checkweigher tare; tolerance on film mass agreed with the film supplier | Reel-to-reel tare verification; supplier film mass certificate |
| RGM-05 | Per-pack weights and per-lot statistics shall be retained and exportable | CPLR s. 39; SFCR s. 53(d) | Per-pack weight with time stamp and lot code; per-lot n, mean, σ, computed weighted average, deficient-unit count; retained for the packer's stated period | Export test; record review |
| RGM-06 | A written determination shall be obtained from Measurement Canada on whether the checkweigher requires type approval and inspection | Weights and Measures Act and Regulations; [Measurement Canada bulletin GEN-41](https://ised-isde.canada.ca/site/measurement-canada/en/laws-and-requirements/gen-41-type-approval-and-inspection-devices-clarification-what-considered-trade) | Written response on file before the checkweigher is ordered | Correspondence in the evidence pack |
| RGM-07 | Pending that determination, a type-approved device shall be specified by default | Risk control on RGM-06 | Checkweigher model carries a Measurement Canada approval number | Approval number recorded in the BOM |
| RGM-08 | The checkweigher shall be verifiable with certified test masses on a stated interval | Good metrological practice; the packer's PCP | Test mass set supplied or specified; verification interval and acceptance limits in the manual; results logged | Verification procedure review; witnessed verification at FAT |
| RGM-09 | The net quantity shall be declared in metric, formatted per the regulation | CPLR ss. 25–27 | Grams below 1 000 g, kilograms at 1 000 g and above, so "1 kg"; shown to three figures per s. 25 | Artwork review against the regulation |

RGM-06 is an open item, not a conclusion. GEN-41 defines trade as "the selling, purchasing, exchanging, consigning, leasing or providing of any commodity" on the basis of measure, and distinguishes individually measured commodities from standard quantity commodities, whose devices "do not need approval and inspection" when used exclusively for that purpose under s. 4(2) of the Regulations. A 1 kg brick is a standard quantity, which points toward exemption — but the bulletin does not name fillers, multihead weighers or checkweighers, so the reading is not settled. Get it in writing and specify an approvable device in the meantime; the cost delta is small and it removes the risk of being told no after procurement.

## 8. RGL — Labelling obligations created by preprinted film

Preprinted film freezes the entire label at the supplier except one mandatory element, so the machine owns exactly two labelling duties: **apply the lot code**, and **do not destroy the preprinted mandatory information**. Both are usually treated as features. Both are legal requirements.

FR-9 says the machine "shall apply a legible date/lot code". [SFCR s. 92(2)](https://inspection.canada.ca/en/food-safety-industry/traceability/traceability) makes that mandatory for consumer prepackaged food, and notably a unique identifier is **not** an acceptable alternative to a lot code for consumer packs. So the coder is not an option to be value-engineered out, and its failure is a regulatory stop condition, not a quality defect.

### Where brick geometry becomes a labelling constraint

For food, the *form* of the net quantity declaration comes from the SFCR, while its *accuracy* comes from CPLR Schedule I (§7). Per [CFIA](https://inspection.canada.ca/en/food-labels/labelling/industry/net-quantity), numerals must be bold and at least the height set by SFCR 229 against the area of the principal display surface:

| Principal display surface | Minimum numeral height |
| --- | --- |
| ≤ 32 cm² | 1.6 mm |
| > 32 to ≤ 258 cm² | 3.2 mm |
| > 258 to ≤ 645 cm² | 6.4 mm |
| > 645 to ≤ 2580 cm² | 9.5 mm |
| > 2580 cm² | 12.7 mm |

This couples directly to the brick design. At the FRS's bulk density of 0.75–0.85 g/cm³ a 1 kg brick occupies roughly 1 180–1 330 cm³. A 180 × 120 × 60 mm brick has a 216 cm² front face and needs 3.2 mm numerals; stretch the face to 200 × 130 mm and it crosses 258 cm², forcing 6.4 mm. The breakpoint sits inside the plausible design range for this product, so the brick's face dimensions must be fixed before artwork is committed. Preprinted film makes that irreversible: a geometry change after the film is printed scraps the film.

The same logic applies in reverse, and this is where the limit specification earns its keep. The machine consumes film area in the back seam, the bottom gusset and the top seal, and all mandatory content must be bilingual per SFCR 206(1). The deliverable to the film supplier is therefore a **dimensioned exclusion-zone drawing**: the seam, gusset and seal footprints, the code window, and the registration tolerance, with everything outside it available to their designer. Issuing that drawing constrains their design space legitimately, and it is the correct answer to the project's stated aim of bounded mutual constraint.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGL-01 | A lot code shall be applied to every consumer brick | SFCR s. 92(2) | 100 % of packs coded; a missing or unreadable code stops the machine rather than passing the pack | Functional test with a disabled coder; reject trial |
| RGL-02 | The code shall remain legible through distribution on the preprinted surface | SFCR s. 92; FR-9.1 | Adhesion and abrasion test on the actual preprinted film, after the seal cycle; code legible after a defined rub test | Abrasion test report on production film |
| RGL-03 | The code shall be applied inside a defined code window and shall not obscure mandatory information | SFCR ss. 206, 221 | Code window dimensioned on the artwork drawing; measured code position inside it on every format | Artwork review; measured samples at FAT |
| RGL-04 | Seam, gusset and seal footprints shall be issued to the film supplier as exclusion zones | SFCR ss. 206, 221, 229 | Dimensioned drawing issued and countersigned by the supplier before artwork is printed | Countersigned drawing on file |
| RGL-05 | Print registration error shall stay within the exclusion-zone margin | FR-2; the above | Registration tolerance ≤ half the narrowest exclusion margin, measured over a full reel | Registration capability run on a full reel |
| RGL-06 | Net quantity numeral height shall match the final brick face area | SFCR s. 229 | Brick face area calculated and the height class recorded before artwork release; declaration bold, on the principal display panel, in distinct contrast | Artwork review against measured brick face |
| RGL-07 | Printable area remaining after forming shall accommodate bilingual mandatory content | SFCR s. 206(1) | Supplier confirms in writing that all mandatory bilingual content fits outside the exclusion zones | Supplier confirmation; proof review |
| RGL-08 | Front-of-package nutrition symbol applicability shall be determined before artwork release | Food and Drug Regulations, FOP nutrition symbol, compliance date 1 January 2026 | Written determination; plain milled rice is not expected to exceed the 15 % DV thresholds for saturated fat, sugars or sodium, but a seasoned or fortified variant must be re-checked and space reserved | Nutrient data against the thresholds |
| RGL-09 | Durable life date capability shall be provided even though it is not legally required | Food and Drug Regulations B.01.007 — a best-before date is required only where durable life is 90 days or less | Coder supports a date field; the FRS records that the date is commercial, not mandatory, for rice | Coder configuration test |
| RGL-10 | Traceability data shall be exportable in the form CFIA can demand | SFCR s. 91(1) | Export produces a single plain-text file, manipulable by standard commercial software, in English or French, deliverable within 24 hours; lot code, date, time and per-pack weight included | Export test against the stated format |

RGL-10 is worth designing now rather than retrofitting. SFCR s. 91(1) requires documents to reach CFIA within 24 hours of a request, in a single plain-text file that standard software can manipulate, and a shorter window applies where there is a risk of injury to health. A machine that logs to a proprietary binary or to an HMI screen only cannot support that. A timestamped CSV per lot, carrying lot code and per-pack weight, satisfies RGL-10 and RGM-05 together.

## 9. RGN — Emissions and radio compliance

ISED regulates emissions by self-declaration, so there is no certification body to pass and no mark to buy — which makes this the easiest section to forget and the easiest to satisfy. Per ISED, interference-causing equipment "isn't formally certified or licensed"; instead it "requires a self-declaration of compliance (SDoC)" and must bear a label representing that declaration before being marketed or imported.

The scope question matters. [ICES-003](https://ised-isde.canada.ca/site/certification-engineering-bureau/en/wireless-equipment-standards-knowledge-center/interference-causing-equipment-standards) covers digital apparatus using timing signals or pulses at 9 kHz or above together with digital techniques for computation, display, control or data processing. A machine with a PLC, an HMI and servo drives meets that description, so ICES-003 is the applicable standard even though the page does not name industrial machinery. ICES-001 is different: it covers equipment that generates RF energy locally for industrial purposes, which resistive cartridge heaters in a seal jaw do not.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGN-01 | A self-declaration of compliance to ICES-003 shall be issued and labelled on the machine | ICES-003; ICES-GEN administrative requirements | SDoC statement present on the nameplate or adjacent label, in the form ICES-GEN prescribes; declaration signed and retained | Nameplate inspection; signed SDoC on file |
| RGN-02 | An emissions test report shall support the declaration | ICES-GEN | Conducted and radiated emissions test report for the assembled machine, or a documented rationale relying on component-level compliance | Test report or engineering rationale on file |
| RGN-03 | If induction sealing is adopted, ICES-001 shall be assessed in addition | ICES-001, ISM equipment generating RF locally | A written scope determination before an induction sealing head is specified | Design review record |
| RGN-04 | Any wireless module shall be a pre-certified module with its ISED certification number recorded | Radiocommunication Act; applicable RSS | ISED certification number in the BOM and on the host label as required; no uncertified radio | BOM audit; module certificate |
| RGN-05 | Drives and safety circuits shall be installed for electromagnetic immunity, not just emissions | ISO 13849-1 requires safety functions to withstand expected disturbance; IEC 61000-6-2 as the immunity reference | Shielded motor cables bonded at both ends, segregated safety wiring, drive filters per the manufacturer's installation manual | Installation inspection; safety-function test with drives at full load |

RGN-04 interacts with FR-16.2, which exposes KPIs over OPC UA or EtherNet/IP. If that link is wired, no radio compliance applies and this requirement is void. If anyone adds a Wi-Fi gateway for convenience during commissioning, it is in scope and must be a certified module. Decide the connectivity architecture before shipping so the declaration matches the machine as built.

## 10. RGD — Documentation, marking and the handover package

The handover package is what proves every other section, and it is the part a student project reliably under-builds. A field evaluator, a pre-start review engineer and a CFIA inspector each arrive asking for a different document, and none of them accepts "we did that" without the artifact. Build the package during the project, not at the end: most of these documents can only be produced while the work is happening.

| ID | Requirement | Source | Acceptance criterion | Verification |
| --- | --- | --- | --- | --- |
| RGD-01 | A permanent nameplate shall carry the identification and rating data | CSA C22.2 No. 301; ICES-GEN | Manufacturer name and address, model, serial number, year of build, supply voltage/phase/frequency, full-load current, marked SCCR, machine mass, air supply pressure and consumption, ICES self-declaration statement | Nameplate inspection against the data sheet |
| RGD-02 | An operation and maintenance manual shall be supplied covering intended use and residual risk | CSA Z432:23; ANSI/PMMI B155.1-2023 | Declared deployment envelope from §1; list of residual risks with the protective measures required of the employer; safety-function schedule with required and achieved PL; energy-source inventory and lockout procedure; validated cleaning procedure; lubricant schedule with NSF registration numbers; maintenance schedule including PRD testing | Document review against this clause list |
| RGD-03 | Safety-related spare parts shall be listed with exact part numbers and a like-for-like replacement warning | ISO 13849-1 | Every interlock, safety relay, e-stop and thermal cut-out listed; manual states that substitution voids the achieved performance level | Parts list review |
| RGD-04 | As-built electrical and pneumatic drawings shall be supplied with the machine and stored in the enclosure | CSA C22.2 No. 301; ISO 4414 | Schematics, I/O list, safety-circuit diagram and pneumatic circuit, matching the machine as built, in a document holder on the panel | Drawing-to-machine audit at FAT |
| RGD-05 | The risk assessment shall be dated, signed and sealed by a professional engineer | O. Reg. 851 s. 7(11),(13) for Ontario; due diligence elsewhere | Sealed report describing measures needed for compliance and any protective measures required before testing | Document review |
| RGD-06 | Hazard signage shall follow a documented signage schedule | OHS Regulation s. 12.13 (CSA Z321-96, ANSI/ISO signage) | Drawing locating every sign; pinch, crush, hot surface, electrical and lockout-point markings; French added for Quebec | Inspection against the signage drawing |
| RGD-07 | Documentation and inscriptions shall be in French for a Quebec installation | Charter of the French Language s. 51.1; software s. 52.1 | Manual, warranty, accompanying documents and safety inscriptions in French, with any other language no more prominent; HMI available in French unless no French version of the software exists | Document and HMI review |
| RGD-08 | A single evidence register shall index every requirement to its artifact | This specification §12 | One table, RGx-nn to document name, revision and location; no requirement without an artifact | Register completeness check at handover |
| RGD-09 | A voluntary declaration of conformity may be issued, and shall not imply a CE marking | No Canadian equivalent exists | Declaration lists the standards actually met, with editions; no CE mark or CE language anywhere on the machine or documents | Document review |
| RGD-10 | The design file shall be retained after delivery | Product-liability defence; due diligence | Risk assessment, calculations, test records, certificates and as-builts retained for the machine's service life | Retention policy stated |

RGD-03 is more consequential than it looks. The performance levels claimed in §4 depend on specific components with specific diagnostic coverage and MTTFd figures. If a maintainer replaces a coded interlock with a cheaper mechanical switch, the achieved PL drops below the required PL and the machine is no longer the machine that was assessed. The parts list and the warning are the only thing standing between a valid safety claim and a quiet invalidation.

## 11. Provincial deployment matrix

The federal layer — food safety, food contact, metrology, labelling, emissions — is identical everywhere in Canada. Everything in the safety, electrical and pressure layers changes at the provincial border, and Ontario adds a gate that does not exist in BC.

| Item | British Columbia (primary) | Ontario | Quebec |
| --- | --- | --- | --- |
| Electrical authority | Technical Safety BC | Electrical Safety Authority | Régie du bâtiment du Québec |
| Adopted electrical code | BC Electrical Code 2024 (CSA C22.1:24, 26th ed.), in force 4 March 2025 | Ontario Electrical Safety Code, provincial edition of CSA C22.1 | Quebec Construction Code, Chapter V — Electricity |
| Equipment approval | Certification mark from an SCC-accredited body, or an approval label such as an SPE-1000 field approval under Safety Standards Act s. 10 | Certification mark or field evaluation accepted by ESA | Certification mark or field evaluation accepted by RBQ |
| Adopted safeguarding standard | CSA Z432-**94**, via OHS Regulation s. 12.3 | O. Reg. 851 ss. 24–28 prescribe guarding directly | RSST machine-safety provisions |
| Pre-start review | Not required | **Required** where a s. 7 Table circumstance applies; report by a professional engineer, sealed, before the machine is used | Not required |
| Lockout | OHS Regulation Part 10 | O. Reg. 851 ss. 42, 75, 76 | RSST |
| Pressure equipment | CRN valid for BC (digit "1" after the decimal), installation notification, operating permit before operation | TSSA registration and permitting | RBQ |
| Documentation language | English | English | French required per Charter s. 51.1 |

### The Ontario gate, and how to design around it

Under [O. Reg. 851 s. 7](https://faolex.fao.org/docs/pdf/on133240.pdf), a pre-start health and safety review is triggered by Table item 2 when safeguarding devices such as light curtains, safety mats, two-hand controls or **interlocked barrier guards** are used as protective elements. RGS-04 requires exactly that, so a PSR is presumptively engaged for any Ontario installation. Item 5 is a second route: a dust collector presenting an ignition or explosion risk, which a rice-dust aspiration system could engage.

The review must be conducted by a professional engineer (s. 7(11)), the report must describe the measures needed for compliance and be dated, signed and sealed (s. 7(4), (13)), and the machine must not be operated until the review is done and the measures are taken or notice is given to the joint health and safety committee (s. 7(3)).

There is a documented way out. Section 7(5) exempts item 2 where the protective element already **meets current applicable standards and is installed in accordance with the manufacturer's instructions**, and s. 7(10) requires the documents establishing the exemption to be kept readily accessible in the workplace. So the design strategy is:

1. Select interlocks, safety relays and light curtains certified to current standards, and keep the certificates.
2. Install each one strictly per its manufacturer's instructions, including mounting distances and wiring, and record it.
3. Ship those certificates and installation records as a named part of the handover package under RGD-08, so the buyer can establish the exemption.
4. Do not promise the exemption. It depends on the installation and on documents being present on site, so the correct commercial position is that the machine is built to support it.

This matters commercially as much as technically: a PSR costs the buyer an engineering engagement and calendar time before they can run the line. A machine that arrives with the exemption documentation is worth measurably more than one that does not, and the cost of producing it during the build is close to zero.

## 12. Verification and evidence plan

Each requirement already carries its own verification method, so this section answers the question those tables cannot: **when** each one is discharged, and what happens if it is attempted late. The ordering is the point. Four requirements must be settled before anything is ordered, and if they slip past procurement they become expensive or impossible.

| Gate | When | Requirements cleared | Evidence artifact | Owner |
| --- | --- | --- | --- | --- |
| G0 — Pre-procurement determinations | Before any long-lead order | RGE-05, RGE-09, RGP-01, RGM-06, RGM-07, RGL-08 | Buyer's written declaration of supply voltage, available fault current and area classification; receiver exemption determination; Measurement Canada written response; FOP applicability note | Design lead |
| G1 — Design review | Before detail design is frozen | RGE-02, RGE-03, RGE-04, RGS-01, RGS-02, RGS-05, RGF-07, RGP-05, RGN-05 | Sealed risk assessment; safety-function and PL schedule; dual-edition guard-opening check table; hygiene zoning drawing; pneumatic and safety circuit diagrams | Professional engineer |
| G2 — Supplier documentation | Before and during procurement | RGE-06, RGF-01, RGF-02, RGF-08, RGL-04, RGL-07 | Component certificate pack; mill certificates; Letter of Guarantee pack; lubricant registrations; countersigned exclusion-zone drawing | Procurement |
| G3 — Build inspection | During assembly | RGS-03, RGE-07, RGF-06, RGF-10, RGD-01, RGD-04, RGD-06 | Inspection records; borescope and residue photos; fastener audit; nameplate and signage check against drawings | Build lead |
| G4 — Factory acceptance test | Before shipment | RGS-04, RGS-06, RGS-07, RGS-08, RGS-11, RGS-13, RGS-14, RGP-06, RGP-07, RGF-05, RGF-11, RGM-01, RGM-03, RGM-04, RGM-05, RGM-08, RGL-01, RGL-02, RGL-03, RGL-05, RGL-10 | FAT protocol with per-requirement pass/fail; interlock and defeat log; thermal survey; sound measurement; air-dump jaw test; reject trial with a 983 g test pack; registration capability run; CSV export sample | Test engineer |
| G5 — Certification or field evaluation | After FAT, before energising on site | RGE-01, RGN-01, RGN-02 | Certification report and mark, or SPE-1000 field approval label and report; signed ICES declaration and test report | Accredited body |
| G6 — Site acceptance and commissioning | At installation | RGP-02, RGP-03, RGP-08, RGF-04, RGF-09 | Receiver nameplate photo; handover letter on permits; point-of-use air quality report; cleaning validation with swab or ATP results | Commissioning lead |
| G7 — Handover | At transfer of custody | RGS-09, RGF-03, RGF-12, RGM-09, RGD-02, RGD-03, RGD-05, RGD-07, RGD-08, RGD-09 | Manual; lockout procedure and energy inventory; hygiene dossier; evidence register; declaration of conformity; artwork review record | Project lead |
| G8 — In service | Recurring, buyer's duty | RGM-08 intervals, RGP-04 PRD testing, RGF-04 cleaning, RGP-08 air testing | Logs kept by the packer under their preventive control plan | Buyer |

### What this means for a capstone deliverable

Gates G5 and G6 cannot be completed for a machine that is designed but not built, and no amount of documentation substitutes for them. The honest deliverable at that stage is a **field evaluation readiness pack**: everything G0 through G4 produces, assembled as though an accredited body were booked, with G5 and G6 shown as open with named prerequisites. That is a stronger submission than a claim of compliance, because it is true and because it demonstrates the reviewer understands which gates a designer can close and which belong to someone else.

One scheduling warning. G0 holds four items that are easy to defer and costly to recover: the buyer's fault current and area classification, the air receiver determination, and the Measurement Canada response. A checkweigher ordered before RGM-06 comes back may have to be replaced; an air receiver ordered without RGP-01 may turn a component into a permitted vessel. Treat G0 as a hard gate.

## 13. Compliance gap register

Ten gaps, ranked by what happens if they are left alone. The first three stop either the machine or the product; the rest cost money, time or credibility. "Source" says where the gap currently lives — the FRS Rev B, the earlier compliance research note, or both.

| # | Gap | Source | Consequence if unaddressed | Severity | Closed by |
| --- | --- | --- | --- | --- | --- |
| 1 | No Canadian electrical conformity route. FR-14.3 points at "machinery safety directive/standards, UL/CE controls", none of which apply in Canada | FRS FR-14.3; research note | Machine cannot be energised. An inspector red-tags it and the line stays dark until a field evaluation is passed, with rework on site | Critical | RGE-01 to RGE-09 |
| 2 | Checkweigher lower reject limit at 980 g, below the 985 g legal threshold for a 1 kg declaration | FRS §8.2–8.3 and the ±2 % band | Deficient packs reach the market, roughly 81 ppm at the FRS's own capability. Not an automatic inspection failure, but an avoidable legal exposure | Critical | RGM-01 |
| 3 | Lot coding treated as a machine feature rather than a legal requirement | FRS FR-9 | A pack without a lot code is non-compliant under SFCR s. 92(2), and a unique identifier is not an alternative. A coder fault becomes a product problem, not a downtime problem | Critical | RGL-01, RGL-02 |
| 4 | No legal metrology analysis at all — no tolerances, no lot-failure tests, no device-approval question | Research note omitted the domain entirely | Fill strategy derived from capability economics alone, with no check against the rule that actually judges the lot | High | §7 in full |
| 5 | Wrong instrument named for food-contact evidence: a Health Canada LONO for machine materials | Research note | The packer cannot close their preventive control plan, because the document they need is a supplier Letter of Guarantee, which nobody collected during the build | High | RGF-02, RGF-03 |
| 6 | No functional safety work. CSA Z432 cited, but no performance levels, no architecture, no validation | Research note; FRS FR-14.4 | The safety claim is unverifiable. In Ontario a pre-start review engineer will not seal a report without it; elsewhere it is the weak point in any incident defence | High | RGS-05 and the safety-function schedule |
| 7 | Hygienic design aimed at washdown and CIP, citing 3-A and IP69K, for a dry low-moisture product | Research note | Over-specified enclosures and drainage that cost money, plus a real hazard: wet-cleaning a dry rice line converts a dormant microbial risk into a growth risk | High | RGF-04 to RGF-07 |
| 8 | Air receiver treated as a component, not a registered pressure vessel | Both | A machine-mounted receiver without a BC-valid CRN cannot be legally pressurised, and no operating permit can be issued | Medium | RGP-01 to RGP-04 |
| 9 | No exclusion-zone drawing issued to the film supplier | FRS FR-2 and the artwork workflow | Preprinted film with mandatory content inside a seal or gusset footprint, or numerals below the required height, is scrap. The cost lands on a printed reel, not on a drawing | Medium | RGL-04 to RGL-07 |
| 10 | Combustible rice dust, ICES emissions and Quebec language all unaddressed | Both | Each is individually small and individually embarrassing: an ignition source in a dust envelope, an unlabelled machine, or a French-only jurisdiction receiving English-only documents | Medium | RGS-13, RGN-01, RGD-07 |

### What the earlier research note got right

Worth recording, because the gaps above should not read as a dismissal. The note correctly identified the three-layer structure of Canadian compliance, correctly named SPE-1000 as the field evaluation route, correctly flagged Ontario's pre-start review when most summaries miss it, correctly identified NSF H1 lubricants and the Division 23 chemical migration duty, and correctly cited ANSI/PMMI B155.1 as the packaging-machinery standard. The problem was not the map. It was that the map had no requirements on it: nothing measurable, nothing verifiable, and no owner. That is the difference this specification is meant to close.

### Recommended FRS amendments

Four edits convert the FRS from aspirational to auditable on the compliance side.

1. **Replace FR-14.3 in full.** Delete the destination-market language and reference this specification's RGE and RGS blocks, naming Canada and the province of installation.
2. **Change the checkweigher lower reject limit to 985 g** in §8.2, §8.3 and FR-12.1, and add a sentence distinguishing the ±2 % process band from the 985 g legal floor.
3. **Promote FR-9 to a mandatory requirement** with legibility, permanence and code-window acceptance criteria, and add a no-code stop condition.
4. **Add a deployment envelope clause** to §1.2 carrying the table from §1 of this document, so the limit specification is contractual rather than implied.

## 14. Normative references and editions

Editions matter more here than in most engineering work, because a regulation freezes the edition it adopted. Where the adopted edition and the current edition differ, both are listed.

### Legal instruments

| Instrument | Level | Applies to |
| --- | --- | --- |
| Safe Food for Canadians Act; Safe Food for Canadians Regulations, SOR/2018-108 | Federal | Equipment design and sanitation (ss. 50–57), hazard control (s. 47), traceability and lot code (ss. 90–92), label content and net quantity form (ss. 206, 210, 221, 229–234) |
| Food and Drugs Act; Food and Drug Regulations, Division 23 (B.23.001) and B.01.007 | Federal | Food-contact chemical safety; durable life date applicability |
| Consumer Packaging and Labelling Act; Consumer Packaging and Labelling Regulations, C.R.C. c. 417 | Federal | Net quantity accuracy: Schedule I tolerances, Schedule II sampling, ss. 25–27 and 38–40 |
| Weights and Measures Act; Weights and Measures Regulations, C.R.C. c. 1605 | Federal | Device type approval and inspection where used in trade |
| Radiocommunication Act and ISED interference-causing equipment standards | Federal | Emissions self-declaration; radio modules |
| BC Safety Standards Act; Electrical Safety Regulation, B.C. Reg. 100/2004 | BC | Equipment approval, permits, inspection |
| BC Workers Compensation Act; Occupational Health and Safety Regulation, Parts 4, 7, 10, 12 | BC | Ergonomics, noise, lockout, safeguarding |
| BC Power Engineers, Boiler, Pressure Vessel and Refrigeration Safety Regulation | BC | Air receivers and pressure equipment |
| O. Reg. 851, Industrial Establishments | Ontario | Guarding, lockout, pre-start health and safety review (s. 7) |
| Charter of the French Language, ss. 51.1 and 52.1 | Quebec | Product inscriptions, accompanying documents, software |

### Standards

| Standard | Edition to use | Status note |
| --- | --- | --- |
| CSA C22.1 | C22.1:24, as the BC Electrical Code 2024 (26th ed.) | In force in BC from 4 March 2025. A later edition exists on the national cycle only once a province adopts it |
| CSA C22.2 No. 301 | :23 | Industrial electrical machinery; the Canadian counterpart to NFPA 79 and the main machine-level standard |
| CSA SPE-1000 | :2025, 6th ed. | Supersedes the 2021 edition; the field evaluation code |
| CSA Z432 | Design to :23; **additionally verify against Z432-94** | BC OHS Regulation s. 12.3 adopts the 1994 edition. Meet the stricter of the two |
| CSA C22.2 No. 286 | Current edition — confirm | Industrial control panels and assemblies; edition not verified here |
| CSA C22.2 No. 94.2 | Current edition — confirm | Enclosure type ratings |
| CSA Z460 | Current edition — confirm | Control of hazardous energy |
| CSA Z321 | Z321-96 | The edition cited by BC OHS Regulation s. 12.13 |
| CSA B51 | Current edition — confirm | Boiler, pressure vessel and pressure piping code; Table 5 for relief device intervals |
| ANSI/PMMI B155.1 | 2023 | Packaging machinery safety and risk assessment |
| ISO 12100, 13849-1, 13849-2, 13850, 13857, 14119, 14120, 13732-1 | Current editions — confirm each | Risk assessment, functional safety, e-stop, safety distances, interlocks, guards, touch temperature |
| ISO 4414 | Current edition — confirm | Pneumatic fluid power safety |
| ISO 8573-1 | 2010, Class 1:2:1 | Per BCAS Guideline 102 s. 7 for air in direct or indirect food contact; ISO 8573-7 for microbial assessment |
| ISO 21469 | Current edition — confirm | Incidental food-contact lubricants |
| IEC 61000-6-2 | Current edition — confirm | Immunity in industrial environments |
| ICES-003 | Issue 7 | Digital apparatus; with ICES-GEN for administrative and labelling requirements |
| EHEDG Doc 22 | Current version — confirm | Hygienic design for dry particulate materials |
| NFPA 61, NFPA 652 | Current editions — confirm | Combustible dust in food processing; referenced as good practice, not adopted law |

Seal-integrity standards (ASTM F88, F1140, F2054, F2096, D3078) are already recorded in FRS Appendix B and are not repeated here.

### How to verify currency

The editions above were checked in October 2026. Rows marked "confirm" were not independently verified and should not be cited in a submission until they are. The procedure, in order:

1. Check the **regulation text** first, not the standards catalogue. The regulation names the edition that binds, and BC's s. 12.3 reference to Z432-94 shows how far apart the two can drift.
2. Check the Standards Council of Canada standards database for the current edition and whether a standard has been withdrawn or reaffirmed.
3. For electrical code adoption dates, check the provincial authority's bulletins rather than the national publication date.
4. Record the date of each check in the evidence register under RGD-08. An edition claim without a check date is the kind of detail a reviewer tests first.

### Sources

Pages opened for this specification: [CPLR full text](https://laws-lois.justice.gc.ca/eng/regulations/C.R.C.,_c._417/FullText.html) · [Accuracy requirements for net quantity declarations](https://competition-bureau.canada.ca/en/accuracy-requirements-net-quantity-declarations) · [Measurement Canada GEN-41](https://ised-isde.canada.ca/site/measurement-canada/en/laws-and-requirements/gen-41-type-approval-and-inspection-devices-clarification-what-considered-trade) · [SFCR Part 4](https://laws.justice.gc.ca/eng/regulations/SOR-2018-108/page-5.html) · [CFIA traceability](https://inspection.canada.ca/en/food-safety-industry/traceability/traceability) · [CFIA net quantity](https://inspection.canada.ca/en/food-labels/labelling/industry/net-quantity) · [CFIA guidance on construction and packaging materials and non-food chemicals](https://inspection.canada.ca/en/preventive-controls/food-establishments) · [Health Canada packaging materials](https://www.canada.ca/en/health-canada/services/food-nutrition/food-safety/packaging-materials.html) · [WorkSafeBC OHS Regulation Part 12](https://www.worksafebc.com/en/law-policy/occupational-health-safety/searchable-ohs-regulation/ohs-regulation/part-12-tools-machinery-and-equipment) · [Part 10](https://www.worksafebc.com/en/law-policy/occupational-health-safety/searchable-ohs-regulation/ohs-regulation/part-10-de-energization-and-lockout) · [O. Reg. 851](https://faolex.fao.org/docs/pdf/on133240.pdf) · [Technical Safety BC certification marks](https://www.technicalsafetybc.ca/regulatory-resources/regulatory-notices/approved-certification-marks-electrical-products) · [Technical Safety BC air receivers](https://www.technicalsafetybc.ca/regulatory-resources/regulatory-notices/information-bulletin-air-receivers) · [ISED interference-causing equipment standards](https://ised-isde.canada.ca/site/certification-engineering-bureau/en/wireless-equipment-standards-knowledge-center/interference-causing-equipment-standards) · [Food-grade compressed air and BCAS Guideline 102](https://www.airchecklab.com/topic/food-grade-air-in-manufacturing/) · [Front-of-package labelling](https://www.blg.com/en/insights/2025/05/mandatory-canadian-front-of-package-labelling-by-2026-are-you-prepared) · [Quebec Charter requirements](https://www.smartbiggar.ca/insights/publication/quebecs-french-language-requirements-for-commerce-and-business-reform-of-the-charter-of-the-french-language) · [CSA SPE-1000:2025](https://blog.ansi.org/ansi/csa-spe-1000-2025-field-evaluation-code/) · [CSA Z432:23 record](https://scc-ccn.ca/standardsdb/standards/4032044) · [CSA C22.2 No. 301:23 record](https://scc-ccn.ca/standardsdb/standards/4032091) · [BC Electrical Code 2024 adoption](https://eca.bc.ca/2024-canadian-electrical-code-adoption-update/)

## 15. Assumptions, exclusions and limits

### Assumptions made in writing this

These were chosen rather than confirmed, and each is cheap to change if wrong.

1. **Primary jurisdiction is British Columbia**, with Ontario and Quebec covered at matrix level in §11. A different primary province changes §§3, 4 and 5, not the federal sections.
2. **The machine is a one-off or very low volume**, which is why §3 recommends field evaluation over factory certification. A production run of more than two or three units reverses that recommendation.
3. **The product is plain milled or parboiled rice with no added ingredients.** A seasoned, fortified or flavoured variant reopens RGL-08 and may introduce an allergen, which changes §6 substantially.
4. **The brick is the non-vacuum firm-fill design of FRS Rev B.** The parallel vacuum-brick concept is explicitly not covered; see RGP-09.
5. **The buyer holds the SFCR licence** and operates their own preventive control plan. This specification makes the machine capable of supporting it and does not assume any of the buyer's duties.
6. **Rice dust is treated as combustible** and the area as unclassified, pending the buyer's own dust hazard analysis.

### Excluded from scope

Upstream rice handling and storage; cartoning, case packing and palletising; the buyer's facility, drainage, lighting and pest control; the buyer's preventive control plan, SFCR licence and recall procedure; the artwork itself, which is the film supplier's deliverable against the §8 exclusion zones; export compliance to any market outside Canada; and robot safety under CSA Z434, since no robot is in the FRS architecture.

### Limits of this document

This is an engineering specification written from primary regulatory sources, not legal advice, and three kinds of uncertainty remain in it.

Some requirements rest on determinations nobody has made yet. RGM-06 is the clearest: Measurement Canada's GEN-41 does not resolve whether a prepackaging checkweigher needs type approval, and the specification handles that by requiring a written determination and specifying an approvable device meanwhile. RGP-01 and RGE-09 are the same shape. Treat these as open items with a named closure path, not as settled positions.

Some editions were not verified. The rows marked "confirm" in §14 are exactly that, and the specification says so rather than presenting them as checked. The ones that were verified carry their dates and sources.

And some figures are derived from the FRS's own numbers rather than measured. The 82 ppm and 81 ppm deficiency rates in §7 follow from a normal distribution at a 1002 g mean and σ = 4.51 g, which is the FRS's capability target rather than a measured process. Real rice fill distributions are typically right-skewed by the multihead weigher's combination algorithm, so the true tail below 985 g will differ — most likely smaller. The recommendation to move the reject limit to 985 g does not depend on the distribution being normal; it removes the failure mode regardless, which is why it is the right change even before any process data exists.
