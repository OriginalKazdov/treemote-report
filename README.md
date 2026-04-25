# TreeMoTE — Technical Report

This repository contains the public technical report and high-level result
summaries for **TreeMoTE**, a parse-given tree-recursive neural evaluator
for compositional depth generalization.

> **Status:** preliminary technical report (v0.1). Early results across
> three structured-reasoning task families. Full implementation,
> checkpoints, and large-scale training runs are under active development
> and not released at this stage.

## Author

Juan Cruz Dovzak — `juandovzak@gmail.com`

## Summary

Compositional depth decay in neural networks behaves like an exponential
"dispatch tax": each additional depth step costs a fixed multiplicative
factor in test accuracy on out-of-distribution depths. The decay rate
appears to be a property of how a model learns to combine intermediate
states — not of its parameter count.

A model that takes the parse as given and applies a single shared
typed-bilinear cell recursively (TreeMoTE) eliminates this decay across
the task families tested: modular RPN, Boolean RPN, and infix arithmetic.
Cross-task α (decay rate) is ~0 for TreeMoTE versus 0.18–2.50 for a
parameter-matched Transformer. A control architecture with the same
recursive structure but a generic MLP cell (TreeMLP) fails outside
saturated-coverage regimes, isolating the effect to the typed bilinear
operators.

The technical report covers data setup, architecture, the seven-gate
validation battery, mechanism (cell audits, shape transfer, parse
corruption), and an honest discussion of limitations.

## What's in this repo

```
treemote-report/
├── reports/
│   ├── TreeMoTE_Technical_Report_v0.1.pdf   ← main artifact
│   └── Experimental_Bitacora.pdf             ← chronological log
├── results/
│   └── summary_results.csv                   ← high-level numbers
├── docs/
│   └── claims_and_limitations.md             ← what we claim, what we don't
├── CITATION.cff
└── LICENSE
```

## What's not in this repo (and why)

- Training code, model implementation, data-generation pipeline.
- Pre-trained weights or checkpoints.
- Hyperparameter recipes, scaling configurations, infrastructure setup.
- Raw experimental logs.

These are deliberately withheld until further validation. The technical
report and result summary are sufficient to read, cite, and reproduce
the high-level claims; readers interested in implementation details are
welcome to reach out directly.

## Citation

See `CITATION.cff` or use:

```bibtex
@techreport{dovzak2026treemote,
  author      = {Juan Cruz Dovzak},
  title       = {The Compositional Depth Decay is a Dispatch Tax (v0.1)},
  institution = {Independent Research},
  year        = {2026},
  type        = {Technical Report},
  number      = {v0.1},
}
```

## License

See `LICENSE`. The text and figures of the technical report are released
for reading and citation. Reuse of the work as a basis for derivative
implementations or commercial products requires explicit permission.

## Contact

For questions, citation requests, or collaboration enquiries:
`juandovzak@gmail.com`.
