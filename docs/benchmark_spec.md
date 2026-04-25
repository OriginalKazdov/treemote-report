# Benchmark specification (high-level)

This document describes the structured-reasoning task families used to
evaluate compositional depth generalization. Implementation details
(generators, hyperparameters, exact distributions) are intentionally
withheld; the level of detail here is sufficient to verify the
*nature* of the claims, not to replicate the implementation.

## Common framing

Each task is a family of expressions with a parameter `depth` controlling
the number of binary operators. The model is trained on shallow depths
(e.g. depth ∈ {2..5}) and evaluated on held-out deeper depths
(e.g. depth ∈ {6,7,8}). All splits are checked for:

- balanced label distribution per split and per depth
- explicit train/test overlap measurement (zero overlap on held-out depths)
- coverage of the local operator space
- shape distribution

Models are compared at *matched parameter counts* per task; reported
results use a single seed at the v0.1 stage. Multi-seed validation is in
progress.

## Task families

### Boolean RPN

- Operands: `{0, 1}`
- Operators: AND, OR, XOR
- Surface form: postfix RPN
- Output: single bit
- Chance baseline: 0.5

### Modular RPN (mod p)

- Operands: integers in `[0, p)`
- Operators: addition, multiplication, subtraction (mod p)
- Surface form: postfix RPN
- Output: integer in `[0, p)`
- Reported with `p ∈ {7, 11, 17, 31, 97}` (results in technical report
  cover a subset)
- Chance baseline: 1/p

### Arithmetic AST infix

- Operands: integers in `[0, p)`
- Operators: addition, subtraction, multiplication (mod p)
- Surface form: infix with explicit parentheses
- Source-of-truth: AST (tree-recursive models receive the parse)
- Surface tokens supplied to token-based baselines
- Output: integer in `[0, p)`
- Chance baseline: 1/p

## Architectures evaluated

- **Transformer (token baseline):** standard decoder-only Transformer
  receiving the surface tokens.
- **TreeMLP-parse (control):** receives the parse arrays; uses a generic
  MLP cell with the operator embedded as input.
- **TreeMoTE-parse:** receives the parse arrays; uses a shared
  typed-bilinear cell selected by operator id.

All tree-recursive variants apply the same cell at every internal node
of the parse, in post-order traversal; they share parameters across
depths and across positions in the tree.

## Validation gates (covered in detail in technical report)

1. Cell audit — does the learned cell match the ground-truth operator
   table?
2. Shape transfer — does the learned cell generalise to parse shapes
   not seen at train time?
3. Parse corruption — does randomising the parse degrade the model to
   chance?
4. Operator-coverage scan — how does accuracy depend on the fraction
   of the local operator space seen during training?
5. p-scaling — does the architectural advantage hold across modulus
   sizes?
6. α-law fit — does the depth–accuracy curve follow the dispatch-tax
   law `acc(d) = floor + (acc(d_min) − floor) · exp(−α(d − d_min))`?
7. Multi-seed (v0.1: single seed; multi-seed validation in progress)

## What is *not* part of the public spec

- Exact training hyperparameters (LR, batch size, schedule, etc.)
- Exact dataset sizes, sampling protocols, balancing rules
- Implementation of the cell modules
- Scaling-curve protocols (parameter sweeps, FLOPs accounting)
- TPU-specific code paths

These are recorded internally and may be released in a future version
of this report or under direct collaboration.
