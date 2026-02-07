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

## Usage

Open `fractal-chaos-game.html` in any modern browser. No build step, no dependencies — it's a single self-contained HTML file.

## Gallery

| Sierpinski Triangle (N=3, F=0.5) | Pentagonal Gasket (N=5, F=0.618) | Hexagonal Gasket (N=6, F=2/3) |
|---|---|---|
| Classic self-similar triangle | Golden ratio produces pentagonal symmetry | Hexagonal snowflake-like structure |

## License

MIT
