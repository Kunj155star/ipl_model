# IPL Model v4: Inter-Postsynaptic Functional LINK
## Biologically Realistic Crossing Dendrites — Trial-by-Trial Probabilistic Learning
### Volume: 10×10×10 = 1000 μm³ | 100 Simulations | Fixed Seed: 42

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19798352.svg)](https://doi.org/10.5281/zenodo.19798352)

**Cite this repository:** Vadakkan KI. Inter-Postsynaptic Functional LINK (IPL) Computational Model: Biologically Realistic Simulation of Associative Memory in Cortical Neuropil (v2.0). Zenodo. 2026. https://doi.org/10.5281/zenodo.19798352

---

## Table of Contents

1. [Overview](#1-overview)
2. [The Scientific Problem This Model Addresses](#2-the-scientific-problem-this-model-addresses)
3. [The IPL Solution](#3-the-ipl-solution)
4. [Operational Semblances](#4-operational-semblances)
5. [Biological Parameters and Justifications](#5-biological-parameters-and-justifications)
6. [Neuropil Structure — Four Spine Groups](#6-neuropil-structure--four-spine-groups)
7. [Learning Logic — Trial-by-Trial IPL Formation](#7-learning-logic--trial-by-trial-ipl-formation)
8. [Recall Logic — Unidirectional Propagation](#8-recall-logic--unidirectional-propagation)
9. [Statistical Analysis](#9-statistical-analysis)
10. [Reproducibility — Fixed Random Seed](#10-reproducibility--fixed-random-seed)
11. [Capacity Scaling](#11-capacity-scaling)
12. [Performance Results](#12-performance-results)
13. [Key Design Decisions and Biological Justifications](#13-key-design-decisions-and-biological-justifications)
14. [Project Files](#14-project-files)
15. [Quick Start](#15-quick-start)
16. [References](#16-references)

---

## 1. Overview

This model computationally validates the Inter-Postsynaptic functional LINK (IPL)
hypothesis for associative memory [Vadakkan, 2013; Vadakkan, 2019] within a
biologically realistic 1000 μm³ neuropil volume parameterised entirely from electron
microscopy data. It constitutes a proof-of-concept demonstration that the structural
substrate required by the IPL hypothesis — closely apposed dendritic spines from
different neurons at dendritic crossing points — exists in living cortical tissue at the
density and geometry the theory predicts, and that networks of such links are sufficient
to implement associative memory formation and retrieval without synaptic weight changes
or neuronal firing.

---

## 2. The Scientific Problem This Model Addresses

### 2.1 Input Degeneracy

A typical cortical pyramidal neuron possesses approximately 10,000 dendritic spines,
each representing one potential input site. Yet an action potential can be triggered
by as few as approximately 100–200 coincident EPSPs arriving within a 10–20 ms window,
regardless of which specific subset of spines is activated [Shadlen & Newsome, 1994;
Softky & Koch, 1993]. For a neuron with 10,000 inputs, the number of distinct synaptic
input combinations capable of generating a single action potential is:

    C(10000, 140) ≈ 2.79 × 10^318

This means the neuron's binary output — fire or not fire — carries virtually no
information about which specific subset of its inputs was responsible. This is the
input degeneracy problem: the neuron's output erases the very information needed to
retrieve the precise associative pairing learned during training.

### 2.2 Why Traditional Synaptic Weight Models Are Insufficient

Hebbian plasticity models store associations as changes in synaptic weights between
neurons. This approach faces two fundamental problems. First, the presynaptic neuron's
firing is itself subject to input degeneracy, so the specificity of which inputs caused
it to fire is lost before the signal reaches the synaptic weight. Second, the weight
records only that two neurons co-fired — not which specific subset of spines of each
neuron were active — losing fine-grained associative content at the very first step.

---

## 3. The IPL Solution

Inter-Postsynaptic functional LINKs (IPLs) bypass the degeneracy bottleneck by storing
associations at the level of individual dendritic spines, prior to and entirely
independently of somatic integration and neuronal firing [Vadakkan, 2013; Vadakkan, 2019].

An IPL is an activity-dependent functional connection formed directly between two
dendritic spines belonging to different neurons (or, in rare cases, to different
dendritic branches of the same neuron), at anatomical sites where those dendrites cross
and their membranes are apposed within 30 nm of one another [Spacek & Harris, 1998;
Harris et al., 1992; Tønnesen et al., 2018].

### 3.1 Formation

When a Bell stimulus and a Food stimulus repeatedly co-activate their respective spines
at such a crossing site, an IPL forms between those two spines. The co-activation must
be near-simultaneous — the millisecond timescale of coincident spine depolarisation —
consistent with the temporal constraints of associative learning [Rescorla & Wagner,
1972]. The association is stored as a spine-to-spine functional link, not as a synaptic
weight, preserving the identity of the specific input pair independently of how the
parent neurons subsequently fire.

### 3.2 Six Functional Properties

The IPL mechanism satisfies six constraints derived from the neuroscience of associative
memory [Vadakkan, 2013]:

(a) Sub-threshold operation: IPL-mediated depolarisation transfer occurs below somatic
firing threshold, enabling memory operations without behavioural output.
(b) Input specificity: IPLs form selectively between spines receiving coincidentally
active, behaviourally relevant inputs [Rescorla & Wagner, 1972].
(c) Bidirectionality: The LINK structure enables depolarisation propagation in either
direction. The present model implements unidirectional propagation as a conservative
test, not as a denial of this property.
(d) Rapid engagement: Spine-to-spine coupling operates on a millisecond timescale,
consistent with fast associative recall.
(e) Reversibility: Most IPLs reverse during early consolidation [Bhatt et al., 2009;
Holtmaat & Svoboda, 2009], modelled as p_reversal = 0.04 per trial.
(f) Persistence: Under consolidated conditions, IPLs persist stably as a memory trace
[Holtmaat & Svoboda, 2009].

### 3.3 Why Spines of Different Neurons

In paradigms where both conditioned stimulus (CS) and unconditioned stimulus (US)
produce distinct motor outputs before learning, the IPL mechanism must bridge separate
neuronal populations. An intra-dendritic coincidence mechanism cannot achieve this.
IPLs between spines of different neurons are the unique architectural solution to this
constraint [Vadakkan, 2013].

---

## 4. Operational Semblances

During memory retrieval, cue presentation (Bell) reactivates Bell spines via direct
presynaptic input. At established IPL sites, this depolarisation propagates
unidirectionally through the IPL to the inter-LINKed Food spine, depolarising it in
the absence of any direct presynaptic Food input. This IPL-mediated spine depolarisation
without direct presynaptic drive is termed an operational semblance [Vadakkan, 2013;
Vadakkan, 2019].

Two definitions are distinguished:

- Phenomenological semblance: The subjective first-person inner sensation of memory
  content. This cannot be modelled computationally [Vadakkan, 2019].
- Operational semblance: The biophysical event of spine depolarisation via IPL in the
  absence of direct presynaptic input. This is measurable and computable, and is what
  the present model quantifies.

A semblance is counted when a target (Food) spine's activation exceeds 0.5 following
IPL-mediated propagation, with no direct Food input present.

---

## 5. Biological Parameters and Justifications

All parameters are drawn directly from published electron microscopy and physiological
literature. No parameter was chosen to optimise model performance.

| Parameter | Value | Reference |
|---|---|---|
| Neuropil volume | 10×10×10 = 1000 μm³ | Standard cortical neuropil sample |
| Spine density | 1.6/μm³ | Kinney et al., 2013; Schikorski & Stevens, 1997 |
| Total spines | 1600 | Derived (1.6 × 1000) |
| Spine diameter | 0.6–0.7 μm | Harris & Stevens, 1989; Arellano et al., 2007 |
| Dendritic segments | 440 | Kasthuri et al., 2015; Mishchenko et al., 2010 |
| Total crossing points | 391 | EM density estimate |
| IPL-eligible crossings | 49 (Bell+Food) | Lavenex & Amaral, 2000; van den Heuvel & Sporns, 2011 |
| Other abutted pairs | 102 | Kasthuri et al., 2015 |
| Single segments | 240 | Mishchenko et al., 2010 |
| Spine separation (abutted) | 20–30 nm membrane-to-membrane | Spacek & Harris, 1998; Tønnesen et al., 2018 |
| IPL formation threshold | ≤30 nm membrane-to-membrane | Harris et al., 1992; Spacek & Harris, 1998 |
| Coupling strength | 0.7 | Vadakkan, 2013 |
| Bell pattern size | 121 spines (7.6%) | Wixted et al., 2014; Karlsson & Frank, 2008 |
| Food pattern size | 121 spines (7.6%) | Wixted et al., 2014; Karlsson & Frank, 2008 |
| Total activation | 242/1600 = 15.1% | Piskorowski & Chevaleyre, 2012; Spruston, 2008 |
| Learning trials | 20 | Classical conditioning acquisition literature |
| p_ipl per trial | 0.30 | Probabilistic IPL formation |
| p_reversal per trial | 0.04 | Bhatt et al., 2009; Holtmaat & Svoboda, 2009 |
| Propagation iterations | 0–5 | Within-recall dynamics |
| Recall direction | Unidirectional per retrieval event | Vadakkan, 2013 |
| Number of runs | 100 | Statistical robustness |
| Random seed | 42 (fixed, arbitrary) | Reproducibility |

### 5.1 Why 440 Dendritic Segments (Not Neurons)

The model uses dendritic segments as the anatomical unit, not neurons. The IPL mechanism
operates at individual spines independently of whether the parent neuron fires. Modelling
neurons would reintroduce exactly the degeneracy problem that IPLs resolve. The 440
segments are derived from EM volumetric data: approximately 3,500 μm of dendritic length
per 512 μm³ [Kasthuri et al., 2015] scales to approximately 6,836 μm per 1000 μm³,
divided by approximately 8 μm average segment length, giving 440 segments
[Mishchenko et al., 2010].

### 5.2 Why 49 IPL-Eligible Sites

The 49 Bell+Food convergence sites reflect the structured, non-random organisation of
associative pathways. Privileged pathways with rich-club organisation [van den Heuvel
& Sporns, 2011] have inter-neuronal connection probabilities 3–10× higher than chance
[Lavenex & Amaral, 2000], justifying 49 designated convergence sites within 391 total
crossing points.

### 5.3 Why 7.6% Activation per Pattern

Neuronal firing rates during encoding are approximately 2.5% [Wixted et al., 2014;
Karlsson & Frank, 2008]. Spine activation including subthreshold EPSPs is substantially
broader. The 7.6% figure reflects spine-level activation including both suprathreshold
and subthreshold inputs [Piskorowski & Chevaleyre, 2012; Spruston, 2008].

---

## 6. Neuropil Structure — Four Spine Groups

The 1600 spines are placed in four structurally distinct groups reproducing the geometry
of real cortical neuropil. Every spine receives a unique dendritic segment index,
enforcing the inter-postsynaptic character of IPLs — no IPL ever forms between two
spines of the same dendritic segment.

```
Group A — 49 IPL-eligible crossing pairs  :   98 spines   (Bell+Food → eligible for IPLs)
Group B — 102 other abutted crossing pairs :  204 spines   (other inputs → no IPL)
Group C — 240 single dendritic segments   :  240 spines   (no abutted partner)
Group D — randomly placed (bulk)          : 1058 spines   (background population)
──────────────────────────────────────────────────────────
Total                                     : 1600 spines
```

### Group A — 49 IPL-Eligible Crossing Pairs (98 spines)

Each pair: one spine from one dendritic segment and one from a different segment,
placed 20–30 nm apart membrane-to-membrane [Spacek & Harris, 1998] at a designated
Bell+Food convergence site. One spine receives Bell input, the other Food input.
Spine diameters of 0.6–0.7 μm [Harris & Stevens, 1989; Arellano et al., 2007] are
documented for biological context only — the 20–30 nm gap is the membrane-to-membrane
distance placed directly in coordinate space [Harris et al., 1992].

### Group B — 102 Other Abutted Crossing Pairs (204 spines)

Same geometry as Group A: two spines on different segments, 20–30 nm apart [Spacek &
Harris, 1998]. Receive neither Bell nor Food inputs. Explicitly excluded from both
input pools, preventing accidental Bell+Food co-activation at a non-designated site.
Represents the biological reality that most dendritic crossings are background crossings
unrelated to any specific associative event [Kasthuri et al., 2015; Mishchenko et al.,
2010].

### Group C — 240 Single Dendritic Segments (240 spines)

Single spines with no abutted partner within the 30 nm IPL threshold. Represents
dendrites passing through the volume without crossing another dendrite closely enough
for spine apposition [Mishchenko et al., 2010; Kasthuri et al., 2015].

### Group D — 1058 Randomly Placed Spines (1058 spines)

Distributed uniformly throughout the volume [Kinney et al., 2013], achieving the
biological density of 1.6 spines/μm³ [Kinney et al., 2013; Schikorski & Stevens, 1997].
Mean nearest-neighbour distance approximately 1 μm — far beyond the 30 nm IPL
threshold. These spines never form IPLs.

---

## 7. Learning Logic — Trial-by-Trial IPL Formation

The central advance of this model is its probabilistic, trial-by-trial learning
framework, directly analogous to classical conditioning acquisition [Rescorla & Wagner,
1972; Pavlov, 1927].

### 7.1 Initialisation

At the start of each simulation run, all IPL weights are set to zero. The 49 eligible
Bell+Food crossing pairs are identified. Before any learning, recall = 0.00%.

### 7.2 Each Trial: Two Steps in Order

**Step 1 — Reversal check (runs first)**

Each currently active IPL is independently tested for reversal. With probability
p_reversal = 0.04, the IPL is deactivated and its weight returned to zero. This reflects
the biological instability of nascent functional connections before consolidation
[Holtmaat & Svoboda, 2009; Bhatt et al., 2009]. Reversal runs before formation —
preventing a newly formed IPL from being immediately tested for reversal in the same
trial.

**Step 2 — Formation check (runs second)**

Each eligible site without an active IPL is independently tested for formation. With
probability p_ipl = 0.30, an IPL forms and its coupling weight is set to 0.7
[Vadakkan, 2013].

### 7.3 Expected Asymptotic IPL Count

    Expected proportion = p_ipl / (p_ipl + p_reversal)
                        = 0.30 / (0.30 + 0.04)
                        = 0.882

    Expected IPL count  = 0.882 × 49 ≈ 43.2

Observed mean across 100 runs: 44.7 ± 2.1. The slight excess over 43.2 reflects that
the system is still in the rising phase of the sigmoid at trial 20, where the formation
rate slightly exceeds the reversal rate.

### 7.4 Sigmoid Learning Curve

The sigmoid acquisition curve emerges naturally. In early trials (1–4), IPLs form
rapidly because most eligible sites are unoccupied. By trials 12–20, the system
approaches stochastic equilibrium and the curve plateaus. This matches the known shape
of classical conditioning acquisition curves [Rescorla & Wagner, 1972; Pavlov, 1927].

### 7.5 Recall Tested After Every Trial

Following each trial's IPL update, recall is immediately tested in both directions
(Bell→Food and Food→Bell), generating the trial-by-trial learning curve (Panel B)
and IPL accumulation curve (Panel C) in the comprehensive figure.

---

## 8. Recall Logic — Unidirectional Propagation

The two retrieval directions (Bell→Food and Food→Bell) are tested as entirely separate
events and never simultaneously.

### 8.1 Step-by-Step Propagation

1. Set all spine activations to zero.
2. Set all cue-pattern (Bell) spine activations to 1.0.
3. Record iteration 0: count Food spines with activation > 0.5 (baseline = 0).
4. Build a set of cue-spine indices for O(1) membership testing.
5. For each of 5 propagation iterations:
   - Take a snapshot of current activations.
   - For each active IPL (weight > 0):
     - Identify which of the two spines belongs to the cue set.
     - If the cue spine has snapshot activation > 0: add (snapshot × weight) to the target spine.
     - If neither spine is in the cue set: no propagation occurs.
   - Clip all activations to [0, 1].
   - Count Food spines with activation > 0.5 (semblances at this iteration).
6. Record final semblance count and recall percentage.

### 8.2 Why the Snapshot Is Essential

The activation snapshot taken before each propagation pass enforces unidirectionality.
Without it, a Food spine activated in iteration k could immediately propagate back to
a Bell spine in the same pass — producing bidirectional results with no biological
counterpart [Vadakkan, 2013].

### 8.3 Why Cue-Set Membership Gating Is Essential

The directional check at each IPL (which spine is the cue spine?) is the fix for the
bidirectionality bug present in earlier model versions. A snapshot alone is insufficient
— without direction gating, both spines of each IPL could initiate propagation, since
both accumulate activation across iterations. Checking cue-set membership at every IPL
ensures only the cue-side spine can initiate propagation through that link.

### 8.4 Semblance Threshold

A target spine is counted as a semblance when its activation exceeds 0.5. With coupling
strength 0.7, one propagation iteration from a fully activated (1.0) cue spine produces
0.7 at the target — above threshold. This means meaningful semblance generation
completes within 1–2 iterations, as confirmed by the propagation dynamics (Panel D),
which plateaus at iteration 2.

---

## 9. Statistical Analysis

### 9.1 Primary Test

A paired t-test compares before-learning recall (0.00% ± 0.00%) to after-learning
recall (36.93% ± 1.71%) across 100 runs. The test is paired because each run
contributes both a before and an after measurement.

### 9.2 Effect Size

Cohen's d is computed using the pooled-standard-deviation formula:

    d = (mean_after - mean_before) / sqrt((var_after + var_before) / 2)

Since var_before = 0 (before-learning recall is deterministically 0.00% in every run),
this simplifies to d = mean_after / (std_after / sqrt(2)). Reported value: 30.54.

### 9.3 Why p < 0.001 Is Reported

The raw p-value (approximately 3.46 × 10⁻¹³⁴) is technically correct but reflects the
model's deterministic before-learning baseline — every run begins with exactly 0 active
IPLs, so before-learning recall is exactly 0.00% in every run, leaving zero variance in
the before condition. The scientific content is fully captured by "p < 0.001", consistent
with standard reporting conventions.

### 9.4 All Results Reported as Mean ± SD

Sample standard deviation throughout (ddof=1).

---

## 10. Reproducibility — Fixed Random Seed

The simulation uses np.random.seed(42) as the first statement of main(), before any
other code executes. This single call fixes the entire random number sequence for all
100 runs and the capacity scaling analysis. Any user who runs the code will obtain
exactly the same results reported in the paper.

### 10.1 What the Seed Controls

- Spine positions in each run (Groups A, B, C, D)
- Directions of spine-pair offsets (20–30 nm gaps)
- Bell/Food assignment within each IPL crossing pair (50/50 random)
- Remaining Bell and Food spine draws from the non-IPL pool
- IPL formation decisions at each trial (p = 0.30)
- IPL reversal decisions at each trial (p = 0.04)
- Capacity scaling sub-runs

### 10.2 Why 42

The value 42 is entirely arbitrary. It is a conventional placeholder used in
computational science. Any fixed integer would produce equally valid and reproducible
results. The scientific requirement is that a seed is fixed and documented; its value
has no biological meaning whatsoever.

---

## 11. Capacity Scaling

The model is run across five neuropil volumes (6³, 7³, 8³, 9³, 10³ μm) to demonstrate
that recall efficiency is a property of spine-level geometry and coupling parameters,
not of absolute volume. All density parameters are held constant across volumes:

- Spine density: 1.6/μm³ [Kinney et al., 2013]
- Crossing proportions: same relative fractions
- Spine pattern fraction: 7.6% [Wixted et al., 2014; Karlsson & Frank, 2008]

Key finding: recall percentage remains approximately constant across all volumes at
approximately 36–40%, while the absolute number of operational semblances scales
linearly with volume. IPL-mediated associative memory is a volume-independent,
geometry-driven mechanism.

---

## 12. Performance Results

Results from 100 independent simulation runs, fixed seed 42.
All values: mean ± SD (sample SD, ddof=1).

| Metric | Value |
|---|---|
| Active IPLs after 20 trials | 44.7 ± 2.1 |
| Maximum possible IPLs | 49 |
| Recall Bell→Food (after training) | 36.93% ± 1.71% |
| Recall Food→Bell (after training) | 36.93% ± 1.71% |
| Operational semblances per retrieval | 44.7 ± 2.1 |
| Recall before learning | 0.00% ± 0.00% |
| Statistical significance | p < 0.001 *** |
| Cohen's d | 30.54 |
| Random seed | 42 (fixed, arbitrary) |
| Natural variance | σ > 0 (biological stochastic) |

The natural variance (σ > 0) reflects genuine biological trial-to-trial variability
from probabilistic IPL formation and reversal dynamics [Bhatt et al., 2009; Holtmaat
& Svoboda, 2009]. It is not a modelling artefact. This constitutes an experimentally
testable prediction: the standard deviation of associative recall performance across
subjects or sessions should be consistent with a stochastic IPL formation process
operating at p_ipl = 0.30 per trial [Vadakkan, 2013; Vadakkan, 2019].

---

## 13. Key Design Decisions and Biological Justifications

### 13.1 Reversal Before Formation in Each Trial

The biological sequence is: existing connections are tested for stability (reversal)
before new connections can form (formation). Running formation before reversal would
allow a newly formed IPL to be tested for reversal in the same trial — an artefact
with no biological counterpart [Bhatt et al., 2009; Holtmaat & Svoboda, 2009].

### 13.2 Uniform Coupling Strength at 0.7

A uniform coupling strength of 0.7 [Vadakkan, 2013] is the most conservative
parameterisation — it makes no assumption about distance-dependent variation within
the 20–30 nm range. The result (36.93% recall) therefore represents a lower bound on
what would be achievable with realistic distance-dependent coupling.

### 13.3 Random Bell/Food Assignment Within Each IPL Pair

Within each of the 49 Group A crossing pairs, which spine receives Bell input and which
receives Food input is assigned randomly (50/50) at the start of each run, reflecting
the biological reality that the two dendritic pathways can cross in either orientation.

### 13.4 Distance Matrix Computed in nm

Spine coordinates are placed in μm, but the distance matrix is multiplied by 1000
before comparison with the 30 nm threshold. This maintains consistency with all
published values stated in nm and avoids unit-conversion errors.

### 13.5 Group B Exclusion from Both Input Pools

The 204 Group B spines are excluded from both Bell and Food pools by building a
complete exclusion set before pattern assignment. Without this, spurious IPLs could
form at Group B sites, artificially inflating recall accuracy.

---

## 14. Project Files

| File | Description |
|---|---|
| `ipl_model_1000_crossing.py` | Main model — all simulation logic, learning, recall, statistics, figure generation |
| `ipl_model_1000_crossing_results_n100.json` | Complete results from 100 simulations (seed 42) |
| `ipl_comprehensive.png` | 6-panel performance figure (Panels A–F) |
| `ipl_key_differences.png` | IPL Framework vs. Traditional Hebbian — 3 panels |
| `ipl_network_architecture.png` | 4-phase neuropil architecture diagram |
| `README.md` | This documentation |

---

## 15. Quick Start

### Requirements

```bash
pip install numpy matplotlib scipy
```

### Run the full simulation (100 runs + figures)

```bash
python ipl_model_1000_crossing.py
```

This executes in sequence:
1. Sets the global random seed (42)
2. Runs 100 independent simulations (~15–20 seconds on a standard CPU)
3. Runs capacity scaling across 5 volumes (20 runs each)
4. Saves results to `ipl_model_1000_crossing_results_n100.json`
5. Generates and saves `ipl_comprehensive.png`

### Expected output (key lines)

```
Active IPLs : 44.7 ± 2.1 (range 38–49)
Bell→Food   : 36.93% ± 1.71%
Food→Bell   : 36.93% ± 1.71%
Bell→Food: p < 0.001 ***, Cohen's d = 30.54
```

---

## 16. References

1. Arellano JI, Benavides-Piccione R, DeFelipe J, Yuste R. (2007). Ultrastructure of
   dendritic spines: correlation between synaptic and spine morphologies.
   *Front Neurosci*, 1(1):131–138.
   https://doi.org/10.3389/neuro.01.1.1.010.2007

2. Bhatt DH, Zhang S, Gan WB. (2009). Dendritic spine dynamics.
   *Annu Rev Physiol*, 71:261–282.
   https://doi.org/10.1146/annurev.physiol.010908.163140

3. Harris KM, Stevens JK. (1989). Dendritic spines of CA1 pyramidal cells in the rat
   hippocampus: serial electron microscopy with reference to their biophysical
   characteristics. *J Neurosci*, 9(8):2982–2997.
   https://doi.org/10.1523/JNEUROSCI.09-08-02982.1989

4. Harris KM, Jensen FE, Tsao B. (1992). Three-dimensional structure of dendritic
   spines and synapses in rat hippocampus (CA1) at postnatal day 15 and adult ages.
   *J Neurosci*, 12(7):2685–2705.
   https://doi.org/10.1523/JNEUROSCI.12-07-02685.1992

5. Holtmaat A, Svoboda K. (2009). Experience-dependent structural synaptic plasticity
   in the mammalian brain. *Nat Rev Neurosci*, 10(9):647–658.
   https://doi.org/10.1038/nrn2699

6. Karlsson MP, Frank LM. (2008). Network dynamics underlying the formation of sparse,
   informative representations in the hippocampus. *J Neurosci*, 28(52):14271–14281.
   https://doi.org/10.1523/JNEUROSCI.4261-08.2008

7. Kasthuri N, Hayworth KJ, Berger DR, Schalek RL, Conchello JA, Knowles-Barley S,
   Lee D, Vázquez-Reina A, Kaynig V, Jones TR, et al. (2015). Saturated reconstruction
   of a volume of neocortex. *Cell*, 162(3):648–661.
   https://doi.org/10.1016/j.cell.2015.06.054

8. Kinney JP, Spacek J, Bartol TM, Bajaj CL, Harris KM, Sejnowski TJ. (2013).
   Extracellular sheets and tunnels modulate glutamate diffusion in hippocampal neuropil.
   *J Comp Neurol*, 521(2):448–464.
   https://doi.org/10.1002/cne.23181

9. Lavenex P, Amaral DG. (2000). Hippocampal-neocortical interaction: a hierarchy of
   associativity. *Hippocampus*, 10(4):420–430.
   https://doi.org/10.1002/1098-1063(2000)10:4<420::AID-HIPO8>3.0.CO;2-5

10. Mishchenko Y, Hu T, Spacek J, Mendenhall J, Harris KM, Chklovskii DB. (2010).
    Ultrastructural analysis of hippocampal neuropil from the connectomics perspective.
    *Neuron*, 67(6):1009–1020.
    https://doi.org/10.1016/j.neuron.2010.08.014

11. Pavlov IP. (1927). *Conditioned Reflexes*. Oxford University Press, Oxford.

12. Piskorowski RA, Chevaleyre V. (2012). Synaptic integration by different dendritic
    compartments of hippocampal CA1 and CA2 pyramidal neurons.
    *Cell Mol Life Sci*, 69(1):75–88.
    https://doi.org/10.1007/s00018-011-0769-4

13. Rescorla RA, Wagner AR. (1972). A theory of Pavlovian conditioning: variations in
    the effectiveness of reinforcement and non-reinforcement. In: Black AH, Prokasy WF
    (eds), *Classical Conditioning II*. Appleton-Century-Crofts, New York, pp. 64–99.

14. Schikorski T, Stevens CF. (1997). Quantitative ultrastructural analysis of
    hippocampal excitatory synapses. *J Neurosci*, 17(15):5858–5867.
    https://doi.org/10.1523/JNEUROSCI.17-15-05858.1997

15. Shadlen MN, Newsome WT. (1994). Noise, neural codes and cortical organization.
    *Curr Opin Neurobiol*, 4(4):569–579.
    https://doi.org/10.1016/0959-4388(94)90059-0

16. Softky WR, Koch C. (1993). The highly irregular firing of cortical cells is
    inconsistent with temporal integration of random EPSPs.
    *J Neurosci*, 13(1):334–350.
    https://doi.org/10.1523/JNEUROSCI.13-01-00334.1993

17. Spacek J, Harris KM. (1997). Three-dimensional organization of smooth endoplasmic
    reticulum in hippocampal CA1 dendrites and dendritic spines of the immature and
    mature rat. *J Neurosci*, 17(1):190–203.
    https://doi.org/10.1523/JNEUROSCI.17-01-00190.1997

18. Spruston N. (2008). Pyramidal neurons: dendritic structure and synaptic integration.
    *Nat Rev Neurosci*, 9(3):206–221.
    https://doi.org/10.1038/nrn2286

19. Tønnesen J, Inavalli VVGK, Nägerl UV. (2018). Super-resolution imaging of the
    extracellular space in living brain tissue. *Cell*, 172(5):1108–1121.
    https://doi.org/10.1016/j.cell.2018.02.007

20. Vadakkan KI. (2013). A supplementary circuit rule-set for the neuronal wiring.
    *Front Hum Neurosci*, 7:170.
    https://doi.org/10.3389/fnhum.2013.00170

21. Vadakkan KI. (2019). From cells to sensations: A window to the physics of mind.
    *Phys Life Rev*, 31:44–78.
    https://doi.org/10.1016/j.plrev.2019.10.002

22. van den Heuvel MP, Sporns O. (2011). Rich-club organisation of the human connectome.
    *J Neurosci*, 31(44):15775–15786.
    https://doi.org/10.1523/JNEUROSCI.3539-11.2011

23. Wixted JT, Squire LR, Jang Y, Papesh MH, Goldinger SD, Kuhn JR, Smith KA,
    Treiman DM, Steinmetz PN. (2014). Sparse and distributed coding of episodic memory
    in neurons of the human hippocampus. *Proc Natl Acad Sci USA*, 111(26):9621–9626.
    https://doi.org/10.1073/pnas.1408365111

---

**The IPL model demonstrates that associative memory can emerge from inter-spine
functional connections operating entirely below the level of neuronal firing
[Vadakkan, 2013; Vadakkan, 2019]. Forty-nine IPL sites embedded in a biologically
realistic neuropil of 391 crossing points [Kasthuri et al., 2015], forming
probabilistically across 20 learning trials [Rescorla & Wagner, 1972], produce
consistent and statistically robust operational semblance generation (36.93% ± 1.71%,
p < 0.001, Cohen's d = 30.54) across 100 independent simulations — with natural
biological variance, a sigmoid acquisition curve, and recall efficiency that is
independent of neuropil volume.**
