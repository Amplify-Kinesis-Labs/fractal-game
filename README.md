# Fractal Chaos Game

An interactive browser-based playground for generating fractals using the [chaos game](https://en.wikipedia.org/wiki/Chaos_game) algorithm. Place vertices of a regular polygon, then iteratively jump a fraction of the distance toward a randomly chosen vertex to reveal stunning self-similar fractal geometry.

## How It Works

1. **N vertices** are placed equidistant on a hidden construction circle
2. A random starting vertex is selected
3. Each iteration: pick a random vertex, move fraction **F** of the distance from the current position toward it, and plot a dot
4. Repeat millions of times — a fractal gasket emerges from the randomness

The classic example: 3 vertices with F = 1/2 produces the **Sierpinski Triangle**.

## Features

- **2 to 12 vertex** regular polygons
- **Optimal F auto-calculation** using the kissing ratio formulas (sub-copies just touch)
- **Logarithmic point slider** from 1 to 100,000,000
- **Three color modes**: single color, random, or vertex-based (colors by which vertex was chosen)
- **Dot size and opacity** controls
- **5 presets**: Sierpinski Triangle, Pentagonal Gasket, Hexagonal Gasket, Octagonal Gasket, Dodecagonal Gasket
- **Save As** PNG export with descriptive filenames
- **ImageData fast path** for pixel-level rendering at high point counts

## Optimal Ratios

The contraction ratio F where the iterated function system sub-copies just touch (the "kissing ratio") depends on vertex count N:

| N | Shape | F | Formula |
|---|-------|---|---------|
| 3 | Triangle | 0.5000 | 1/(1 + 2sin(pi/6)) |
| 5 | Pentagon | 0.6180 | 1/(1 + 2sin(pi/10)) = 1/phi |
| 6 | Hexagon | 0.6667 | 1/(1 + sin(pi/6)) = 2/3 |
| 8 | Octagon | 0.7071 | 1/(1 + tan(pi/8)) |
| 12 | Dodecagon | 0.7887 | 1/(1 + tan(pi/12)) |

General formulas:
- **n % 4 == 0**: `F = 1/(1 + tan(pi/n))`
- **n % 2 == 0**: `F = 1/(1 + sin(pi/n))`
- **odd n**: `F = 1/(1 + 2*sin(pi/(2n)))`

Note: N = 4 (square) is unique — it fills uniformly without vertex selection restrictions, so no fractal structure appears in the unrestricted chaos game.

## Mathematical Insights

### Three formulas, not one

The kissing ratio is not a single universal formula — it splits into **three cases** depending on `n mod 4`:

| Condition | Formula | Why |
|---|---|---|
| n % 4 == 0 | `1/(1 + tan(pi/n))` | 4-fold symmetry creates edge-to-edge packing via tangent |
| n % 2 == 0 (not 4) | `1/(1 + sin(pi/n))` | Even polygons without 4-fold symmetry pack differently |
| odd n | `1/(1 + 2*sin(pi/(2n)))` | Odd vertex counts create vertex-to-edge gap geometry (note `2n`, not `n`) |

The three cases arise from how the shrunken polygon copies pack together geometrically. The symmetry class of the polygon determines whether copies meet edge-to-edge, vertex-to-vertex, or vertex-to-edge — each producing a different trigonometric relationship.

### The square is uniquely degenerate

N = 4 is the **only** regular polygon where the unrestricted chaos game fails to produce a fractal. Because `tan(pi/4) = 1`, the optimal F works out to exactly 0.5, but the square's symmetry causes the attractor to have no holes — it fills uniformly. Every other polygon (3, 5, 6, 7, ..., 12) produces a gasket with visible self-similar structure at its optimal ratio. To get fractals from a square, you need vertex-selection restrictions like "cannot pick the same vertex twice in a row."

### The golden ratio hides in the pentagon

The pentagon's optimal F is **1/phi** (the golden ratio inverse, ~0.618). This falls out of the algebra: since `sin(pi/10) = (sqrt(5) - 1) / 4`, the formula simplifies to `1 / (1 + 2 * (sqrt(5) - 1) / 4) = 2 / (1 + sqrt(5)) = 1/phi`. The golden ratio appearing in pentagonal geometry is well-known in other contexts (the diagonal-to-side ratio of a regular pentagon is phi), but seeing it emerge independently from the IFS contraction formula is a satisfying confirmation that the same deep structure governs both.

#### How novel is this connection?

We investigated whether the explicit link between the pentagon's chaos game ratio and 1/phi had been previously recognized. The answer is nuanced — **the underlying pieces are ancient, but the bridge between them was rarely stated.**

**The trigonometric identity is classical.** The fact that `sin(pi/10) = (sqrt(5) - 1) / 4` and equivalently `phi = 1 + 2*sin(pi/10)` traces back to Euclid's *Elements* (~300 BC) and likely the Pythagoreans before him. It appears in Wolfram MathWorld, Wikipedia, ProofWiki, and standard trigonometry courses. This is settled, 2,300-year-old mathematics.

**The chaos game literature mostly missed the connection.** A survey of the fractal literature reveals a consistent pattern: authors either give the general trigonometric formula without simplifying it, or give the decimal 0.618 without recognizing what it equals.

| Source | What they state | Mention phi? |
|---|---|---|
| [Abdulaziz & Said (2021)](https://www.sciencedirect.com/science/article/abs/pii/S096007792100494X) — *Chaos, Solitons & Fractals* | General formula `K_n = 1/(1 + 2*sin(pi/2n))` for odd n | No |
| [Tom Bates (2019)](http://archive.bridgesmathart.org/2019/bridges2019-139.html) — Bridges Conference | "0.618 is the pentagonal kissing number" | No |
| [Larry Riddle](https://larryriddle.agnesscott.org/ifs/pentagon/sierngon.htm) — Agnes Scott College | Lists r = 0.381966 for Sierpinski pentagon | No |
| [Christoffer Tarmet (2025)](https://arxiv.org/html/2505.18669) — arXiv | States `r_opt = 1/phi` for pentagon in table | **Yes** |

The first explicit statement we found in the academic literature is from a **May 2025 arXiv preprint** by Christoffer Tarmet ("The Optimal Ratio of a Generalized Chaos Game in Regular Polytopes"), which lists `r_opt = 1/phi` directly in a table for the pentagon case.

**Verdict: not a unique discovery, but a genuinely under-recognized connection.** The identity `sin(pi/10) = (sqrt(5)-1)/4` is classical. The optimal chaos game ratio for the pentagon being 0.618 is published. But the explicit observation that these combine to give `r_opt = 1/phi` — bridging fractal mathematics and golden ratio mathematics — was essentially absent from the chaos game literature until 2025. Most authors kept the two worlds separate, either giving a trigonometric formula without simplifying it, or a decimal without recognizing its algebraic identity.

Interestingly, there are actually **two** golden-ratio-related pentagon fractals with different construction methods:
- **Chaos game kissing ratio** (6 copies including center): `r = 1/phi ≈ 0.618`
- **Sierpinski pentagon IFS** (5 copies at vertices only): `r = (3 - sqrt(5))/2 = phi^(-2) ≈ 0.382`

Both ratios are powers of the golden ratio, and they sum to 1. The golden ratio's fingerprints are all over pentagonal fractal geometry — it just took a while for the literature to say so explicitly.

### What "optimal" actually means

The kissing ratio is the **phase boundary** between three regimes:

- **F below optimal**: the shrunken copies are separated — the attractor becomes fractal "dust" (disconnected Cantor-set-like points)
- **F at optimal**: copies just touch — a perfect gasket with crisp holes and maximal fractal detail
- **F above optimal**: copies overlap — the attractor starts filling in, and with enough overlap becomes a solid blob

This explains why naive guesses like F = 1/3 for a hexagon produce a filled mass rather than a fractal: 1/3 is well below the optimal 2/3, so the attractor *is* fractal dust, but each sub-copy is so large relative to its spacing that at the macro scale they overlap into an apparently solid region. The structure only becomes visually apparent at exactly the right contraction ratio.

## Usage

Open `fractal-chaos-game.html` in any modern browser. No build step, no dependencies — it's a single self-contained HTML file.

## Gallery

| Sierpinski Triangle (N=3, F=0.5) | Pentagonal Gasket (N=5, F=0.618) | Hexagonal Gasket (N=6, F=2/3) |
|---|---|---|
| Classic self-similar triangle | Golden ratio produces pentagonal symmetry | Hexagonal snowflake-like structure |

## License

MIT
