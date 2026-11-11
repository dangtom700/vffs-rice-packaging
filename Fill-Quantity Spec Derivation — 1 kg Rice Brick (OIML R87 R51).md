# Fill-Quantity Spec Derivation — 1 kg Rice Brick (OIML R87 / R51)

Oct 17, 2026 · @Son Tung Dang

## Scope

The fill numbers in the FRS can be derived entirely from legal metrology, which removes the need to attribute them to a customer requirement on a self-directed project.

Two standards divide the work:

- **OIML R87**, *Quantity of product in prepackages*, sets what the pack must contain. It defines the tolerable deficiency and the three acceptance rules a lot must satisfy. Every fill-weight figure in the specification derives from this document.
- **OIML R51**, *Automatic catchweighing instruments*, governs the checkweigher that enforces those rules. It constrains the instrument's own error and standard deviation, which must be small relative to the tolerance being policed or the measurement itself becomes a source of rejects.

The derivation runs in that order: R87 fixes the acceptance criteria, those criteria fix the target mean and the permitted process standard deviation, and R51 then confirms the checkweigher can resolve the thresholds it has to enforce.

## The governing criteria for a 1 kg pack

R87 Table 2 gives the tolerable deficiency **T** by nominal quantity for prepackages measured by weight. At Qn = 1000 g the value is **T = 15 g**, arrived at consistently from either adjacent band: the 500 to 1000 g band gives a flat 15 g, and the 1000 to 10 000 g band gives 1.5 % of Qn, also 15 g.

That sets two thresholds:

- **T1 = Qn − T = 985 g.** A pack below this is an inadequate prepackage.
- **T2 = Qn − 2T = 970 g.** A pack below this is a serious deficiency.

The acceptance system is three rules, not a symmetric tolerance band:

1. The average contents of the lot must be at least the nominal quantity, 1000 g. In sampling, the sample mean must not fall short by more than the sample error limit, which is the sample standard deviation times a correction factor from R87 Table 1.
2. No more than **2.5 %** of packs may fall below T1 at 985 g. The sampling plan expresses this as an allowed count rather than a percentage.
3. **No pack** may fall below T2 at 970 g. A single one rejects the lot.

The current Rev B figure of a 980 to 1020 g band does not come from any of these. It is a round ±2 % of nominal, and its lower limit sits 5 g below T1, so it would accept a 980 g pack that R87 counts as inadequate. Overfill, meanwhile, carries no legal limit at all: a 1020 g upper bound is a cost decision wearing the costume of a specification.

## Deriving the mean and the standard deviation

**The mean is pinned by Rule 1.** Overfill is pure giveaway, so the economic optimum is the lowest mean the rules allow, and Rule 1 puts a hard floor at the nominal 1000 g. Nothing below that is available however tight the process gets. The only question is how much margin to add for sampling error and drift; the Rev B target of **1002 g**, giving 2 g, is a reasonable choice and survives this derivation unchanged.

**The standard deviation ceiling follows from Rule 2.** Treating the fill distribution as normal, the requirement that at most 2.5 % of packs fall below 985 g is

```latex
\mu - 1.96\,\sigma \geq 985
```

At μ = 1002 g this gives σ ≤ 17 / 1.96 = **8.67 g**. Rule 3 is the weaker constraint in practice: requiring effectively no packs below 970 g at four sigma gives σ ≤ 32 / 4 = 8 g.

So the regulation alone permits roughly twice the spread Rev B specifies. The tighter figure needs a different justification, and the honest one is process capability rather than legality.

**Where Cpk belongs.** Cpk 1.33, equivalent to four sigma to the nearest specification limit, is the conventional minimum for a capable process in SPC practice. Applied here the lower limit should be the legal threshold, not an invented one. With a one-sided limit at T1:

```latex
C_{pk} = \frac{\mu - 985}{3\sigma} \geq 1.33
```

which gives σ ≤ 17 / 3.99 = **4.26 g**.

This is the number that should replace 4.51 g. The existing figure came from applying Cpk 1.33 against the self-chosen 1020 g upper bound with an 18 g margin; anchoring instead to the 17 g margin above T1 tightens it slightly and, more importantly, ties it to a threshold someone else defined. There is no upper specification limit in the Cpk calculation because no law caps overfill.

## Can the checkweigher police a 985 g line?

R51 covers automatic catchweighing instruments. Category X is the one for checking prepacked products, with classes XI, XII, XIII and XIIII, each marked with a factor (x) chosen by the manufacturer, written for example as XI(0.5).

Two limits matter.

**Maximum permissible standard deviation of the instrument.** On the x = 1 basis, for net loads from 500 to 1000 g the limit is **0.8 g at initial verification** and 1.0 g for in-service inspection. The adjacent band above 1000 g gives 0.08 % of load, also 0.8 g, so a 1 kg pack sits on a consistent boundary. The factor x multiplies this figure, and for classes XI and XII it must be below 1, so those classes are tighter still.

**Maximum permissible mean error.** For class XI in automatic operation the limit is ±0.5 e at initial verification and ±1 e in service, where e is the verification scale interval. Against a 15 g tolerable deficiency this is comfortable for any sensible choice of e: at e = 1 g the instrument's permitted mean error is ±0.5 g, about 3 % of T.

**What this does to the process budget.** The spread you observe at the checkweigher is the process and the instrument combined:

```latex
\sigma_{obs} = \sqrt{\sigma_{proc}^{2} + \sigma_{inst}^{2}}
```

To hold σ\_obs at the 4.26 g derived above with an instrument contributing 0.8 g, the filling process itself must reach √(4.26² − 0.8²) = **4.18 g**. The instrument costs about 2 % of the budget, which is to say it is not the limiting element. A multi-head weigher at 1 kg should achieve well inside this, so the design margin is real.

The open decision is which class and factor to specify. That is a procurement choice rather than a derivation, but the arithmetic above shows almost any compliant class X instrument will do; there is no need to buy accuracy the process cannot use.

## Resulting specification

| Requirement | Value | Derived from | Change from Rev B |
| --- | --- | --- | --- |
| Nominal quantity Qn | 1000 g | Product definition | unchanged |
| Tolerable deficiency T | 15 g | R87 Table 2, 1000 g row | new, was implicit |
| T1 threshold | 985 g | Qn − T | replaces the 980 g lower bound |
| T2 threshold | 970 g | Qn − 2T | new |
| Lot mean | ≥ 1000 g, target 1002 g | R87 Rule 1 plus sampling margin | target unchanged |
| Packs below T1 | ≤ 2.5 % | R87 Rule 2 | replaces 100 % within band |
| Packs below T2 | zero | R87 Rule 3 | new |
| Observed standard deviation | ≤ 4.26 g | Cpk 1.33 against T1 at μ = 1002 g | tightened from 4.51 g |
| Process standard deviation | ≤ 4.18 g | σ\_obs with 0.8 g instrument contribution removed | new |
| Checkweigher | Class X, 100 % inspection | R51; σ\_inst ≤ 0.8 g at 1 kg, x = 1 basis | class and factor still open |
| Upper fill limit | none specified | No legal cap on overfill | 1020 g bound removed |

Three substantive changes. The 980 g lower bound rises to 985 g and stops being a hard limit, becoming instead a 2.5 % allowance with an absolute floor at 970 g beneath it. The standard deviation tightens marginally to 4.26 g but is now anchored to a threshold set by the standard. And the 1020 g upper bound disappears from the specification; overfill is a cost to be minimized through the mean, not a conformance criterion.

The practical effect on the machine is small, since a process centred at 1002 g with σ near 4 g satisfies all three rules with wide margin. The effect on the document is larger: every figure now traces to a clause.

## Sources

- [OIML R87, Quantity of product in prepackages](https://www.oiml.org/en/files/pdf_r/r087-e04.pdf) — Table 2 for the tolerable deficiency, clause 3 for the acceptance rules.
- [OIML R51-1, Automatic catchweighing instruments: metrological and technical requirements](https://www.oiml.org/en/files/pdf_r/r051-1-e06.pdf) — accuracy classes, maximum permissible mean error and standard deviation.
- [Consumer Packaging and Labelling Regulations (C.R.C., c. 417)](https://laws-lois.justice.gc.ca/eng/Regulations/C.R.C.,_c._417/page-4.html) — the Canadian net-quantity regime, which should be checked against the OIML figures before the specification is finalized.

**Open items.** Canada enforces net quantity through the Weights and Measures Act and the regulations above rather than by adopting R87 directly, so the Canadian tolerance table should be confirmed to match before these numbers are treated as settled. The checkweigher class and factor remain a procurement decision. Seal integrity and brick dimensional tolerances have no equivalent legal table and are where published machine design work, including the theses, is the right reference.
