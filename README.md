<!-- solve-problem:6209b65f-25eb-4992-ac84-7884b35a322d -->
# Near-log-squared deterministic list edge coloring with 1+ε palettes

A [Tangle Solve problem](https://solve.tangle.tools/problems/6209b65f-25eb-4992-ac84-7884b35a322d).

## What is the problem?

**Setting: DISTRIBUTED CONGESTED CLIQUE / LOCAL model** — most likely the CONGEST model (message size O(log n) bits per round over each edge), the standard setting for distributed edge coloring. We have an n-vertex graph G with maximum degree Δ. Each edge e has a *list* (palette) L(e) of colors, |L(e)| ≥ (1+ε)Δ for a fixed constant ε > 0, with Δ sufficiently large as a function of ε (say Δ ≥ Δ₀(ε)). Goal: a deterministic distributed algorithm that properly edge-colors G from these lists — adjacent edges receive distinct colors, each edge gets a color from its own list — in Õ(log² n) rounds (polylog factors suppressed). The census record does not specify the model explicitly; the natural reading is deterministic CONGEST, where randomized Õ(polylog n)-round algorithms for (1+ε)Δ list edge coloring exist but deterministic ones at Õ(log² n) rounds with (1+ε)Δ palettes do not. Ambiguity to resolve up front: LOCAL vs CONGEST, and whether lists are arbitrary or "nice" (e.g., each edge's list drawn from a small universe of size poly(Δ)).

## Why does it matters?

Deterministic list edge coloring is the workhorse subroutine of distributed graph decomposition, and fast deterministic (1+ε)Δ versions would immediately accelerate deterministic network decomposition, coloring, and MIS to near-optimal polylog bounds. The list version models channel/frequency assignment in wireless networks where radios have heterogeneous available channels. It is also the key test of whether derandomization overhead (currently roughly a log-factor or palette-size penalty) can be eliminated for one of the most-used primitives in the LOCAL/CONGEST toolkit.

## What's already known?

- **Randomized:** Õ(polylog n)-round (1+ε)Δ list edge coloring is known (via e.g. the Ghaffari–Su framework and later improvements). Not the target — the problem demands determinism.
- **Deterministic, larger palettes:** Chang–Dolatabadi (2026), via "introvert clustering," achieve deterministic list edge coloring at the Õ(log² n)-round scale but with palettes of size ≈ 3Δ/2 — a constant-factor slack of 1/2 beyond the (1+ε) target. This is the best-known result at the desired round complexity and is explicitly flagged as "not the missing result."
- **Deterministic, general:** Generic deterministic derandomization gives polylog n rounds for list edge coloring with (1+ε)Δ palettes but with round complexity worse than log² (larger polylog, or with worse Δ-dependence). (verify exact state of the art, which moves quickly.)
- The gap is quantitative: get from 3Δ/2 down to (1+ε)Δ while keeping Õ(log² n) rounds, presumably for Δ ≥ polylog or Δ ≥ Δ₀(ε).

## What would count as solving it?

A deterministic distributed algorithm (state the model; CONGEST preferred, LOCAL acceptable) that, for every fixed ε > 0 and every Δ ≥ Δ₀(ε), properly list-edge-colors any n-vertex graph of max degree Δ with lists of size (1+ε)Δ in O(log² n · polylog log n) rounds (Õ(log² n)), with a full proof of correctness and round complexity. Partial credit tiers: (a) any deterministic (1+ε)Δ palette at o(log³ n) rounds in CONGEST; (b) removing introvert clustering's 3/2 slack to 1+ε for large Δ; (c) a lower bound showing Ω(log² n) rounds are necessary, proving optimality. Negative results (e.g., an Ω(log² n) barrier arguments can't beat) would also resolve the "near" qualifier.

## How to start

1. **Read Chang–Dolatabadi (arXiv:2609.10044) first**, especially the palette-size analysis: identify exactly where the 3/2 slack arises (likely a slack of Δ/2 needed to make clusters safely decolorable / independent-set-based recursion work). This is the single number to attack.
2. Trace the pipeline: modern list edge coloring reduces to (i) degree reduction / clustering, (ii) coloring a "hard core" of high-degree vertices via palettes, (iii) post-processing. Check whether steps (i)/(iii) tolerate (1+ε)Δ and only (ii) forces 3Δ/2; if so, focus on the core.
3. Natural approach: combine introvert clustering with a sharper degree-splitting / palette-shrinking argument (à la arboricity-based splitting) so each subproblem needs only (1+ε)Δ. Main obstacle: deterministic coordination without randomness typically "pays" in palette slack; you must find a derandomizable probabilistic argument that works with slack εΔ for large Δ.
4. Check the randomized (1+ε)Δ analysis for slack terms that are o(Δ) but ≥ Δ/2 under naive pessimistic derandomization — that's usually where the constant hides.

## Key references

- Chang, Y.-J., & Dolatabadi, S. (2026). *Introvert Clustering for Distributed Graph Algorithms*. arXiv:2609.10044. Introduces a clustering technique for deterministic distributed algorithms and applies it to list edge coloring achieving ~3Δ/2 palettes in Õ(log² n) rounds — the direct baseline to improve. (Citation as given in census record; verify bibliographic details before citing.)
- Background (verify current versions): Ghaffari & Su, *Distributed Degree Splitting, Edge Coloring, and Orientations* — degree-splitting machinery underlying (1+ε)Δ edge coloring. (verify)
- Background (verify): Barenboim & Wawrzyniak / Rozhoň–Ghaffari work on deterministic network decomposition — the derandomization toolkit and the main consumer of the target result. (verify)

## Warnings

- The census record is sparse and its citation (arXiv 2609.10044) cannot be verified from context; independently confirm the paper exists and its exact palette bound before building on it.
- Model ambiguity: "Õ(log² n) rounds" — confirm CONGEST vs LOCAL; results and difficulty differ substantially.
- "Δ sufficiently large relative to ε" is a real precondition, not decoration; the problem is likely false or open for small Δ (list chromatic index can exceed (1+ε)Δ for tiny Δ).
- Do not chase generic polylog derandomization — the record explicitly states general polylog algorithms don't solve the problem; only Õ(log² n) with (1+ε)Δ counts.

---
*`ALGO-000137` · unverified*

## Acceptance guidance

A deterministic distributed algorithm (state the model; CONGEST preferred, LOCAL acceptable) that, for every fixed ε > 0 and every Δ ≥ Δ₀(ε), properly list-edge-colors any n-vertex graph of max degree Δ with lists of size (1+ε)Δ in O(log² n · polylog log n) rounds (Õ(log² n)), with a full proof of correctness and round complexity. Partial credit tiers: (a) any deterministic (1+ε)Δ palette at o(log³ n) rounds in CONGEST; (b) removing introvert clustering's 3/2 slack to 1+ε for large Δ; (c) a lower bound showing Ω(log² n) rounds are necessary, proving optimality. Negative results (e.g., an Ω(log² n) barrier arguments can't beat) would also resolve the "near" qualifier.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/6209b65f-25eb-4992-ac84-7884b35a322d?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000137.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000137)
