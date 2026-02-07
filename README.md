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
