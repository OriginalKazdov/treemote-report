# Priority Record — Claims

**Author:** Juan Cruz Dovzak
**Date of priority:** 2026-04-24
**Email:** juandovzak@gmail.com

## Primary claims

### Claim 1: The compositional depth decay law (empirical)

Out-of-distribution accuracy on compositional depth tasks follows a two-parameter empirical law:

$$\text{acc}(d) = \text{floor}_{\text{arch}} + (\text{acc}(d_{\min}) - \text{floor}_{\text{arch}}) \exp(-\alpha_{\text{arch}}(d - d_{\min}))$$

validated across 42 runs on modular reverse-Polish arithmetic with $p=7$. Leave-one-out prediction of unseen depths from fit on in-training depths achieves mean absolute error of 1.7–3.2 percentage points.

### Claim 2: Architectural parameters are measurable and distinguish architectures

- Transformer: α = 0.78 ± 0.18, floor = 0.16 ± 0.02
- Typed-operator Transformer (MoTE): α = 0.76 ± 0.05, floor = 0.16 ± 0.01
- Differentiable soft stack: α = 0.42 ± 0.09, floor = 0.22 ± 0.02

The two parameters separately and orthogonally characterize compositional competence.

### Claim 3: The dispatch tax hypothesis

$\alpha_{\text{arch}}$ is interpreted as the per-depth log-cost of learning dispatch during forward pass. Evidence: dispatch entropy of trained soft-stack controller correlates monotonically with fitted $\alpha$ across curricula.

### Claim 4: Causal intervention — tree-recursive shared-cell evaluator

An architecture that receives the expression parse tree as input and applies a single learned cell at each internal node achieves 100% accuracy at test depth $d=8$ on balanced RPN mod-7 (training depths $\{2, 3, 4, 5\}$), and $93$–$100\%$ at depths $\{6, \ldots, 12\}$.

The fitted decay rate is $\alpha \approx 0.01$ – a $40$–$80\times$ reduction vs. Transformer.

### Claim 5: Mechanistic verification

(a) The trained cell reproduces the complete $\mathbb{Z}/7\mathbb{Z}$ operator table exactly (147 of 147 $(a, b, \text{op})$ triples correct).

(b) The same cell, trained on balanced RPN, applied to linear RPN without retraining: accuracy $\geq 0.98$ at $d \leq 4$ and $\geq 0.79$ at $d=8$ (bidirectional).

(c) Parse corruption (random operand-position shuffling at test time) collapses accuracy to chance at $d=8$: 100% → 15.8% as corruption rate 0 → 1.

### Claim 6: Decomposition of compositional-generalization failure

Compositional generalization failure decomposes into three separable components:

1. Parsing (tokens → computation graph)
2. Local operator learning (per-node function fitting)
3. Recursive evaluation (applying the function along the graph)

The intervention addresses only (3). Components (1) and (2) remain standard problems: the intervention does not parse, and its cell requires sufficient coverage for operator learning.

### Claim 7: Boundary conditions

- At larger modular arithmetic ($p=31$ with 200K training samples, 60K steps), the cell does not fully converge. The architectural advantage over Transformer persists ($5\times$ chance vs. $1.4\times$ chance at $d=5$ OOD).
- Value-space extrapolation (operands restricted during training) is not solved by the architecture; it remains a cell-learning problem.
- The approach requires an explicit parse; natural-language compositional reasoning is not addressed.

## Explicit non-claims

We explicitly do NOT claim:

- That recursive or tree-augmented neural networks are novel
- That natural-language compositional generalization is solved
- That our results extend to models larger than 3.2M parameters or training budgets longer than 50K steps
- That cell learning is trivial at arbitrary moduli
- That the dispatch tax is the only source of compositional generalization failure

## Corrections to earlier drafts

The original uniform $1/p$ chance baseline was miscalibrated: the empirical label distribution of balanced RPN mod-7 is non-uniform (majority-class $\approx 0.20$ due to multiplication collapsing on zero operands). Correcting to rejection-sampled uniform labels reduces the previously reported gap by approximately a factor of two. All quoted numbers in this priority record use the corrected baseline.

One single-seed configuration ($d_{\text{model}}=128$, balanced labels, 15K steps) showed an anomalous $+16$pp spike that did not reproduce on extended training. It has been removed as outlier noise.

## Timestamp

This document was created on 2026-04-24 and is intended to be archived with a persistent DOI as the public priority record.
