[README.md](https://github.com/user-attachments/files/32240309/README.md)
# Price Stimulation — Monte Carlo Modelling of Resistance Breakouts

A Monte Carlo simulation that studies how a stochastic price process repeatedly tests a resistance level before breaking through it. Asset prices are generated with Geometric Brownian Motion (GBM), and each path is analysed to count "touches" of a resistance band and to detect whether and when a breakout occurs.

**Author:** Michelle Lin

## Overview

The notebook models the intuition that a price often approaches a resistance level several times ("touches") before it finally crosses above it ("breakout"). Running the GBM process thousands of times lets us estimate the overall distribution of these events rather than reason from a single path, and examine how volatility changes the outcome.

## Requirements

- Python 3
- `numpy`
- `matplotlib`

Install with:

```bash
pip install numpy matplotlib
```

## How to Run

Open `Price_stimulation.ipynb` in Jupyter and run the cells in order:

```bash
jupyter notebook Price_stimulation.ipynb
```

A fixed random seed (`np.random.default_rng(20)`) is used so results are reproducible across runs.

## Key Concepts

- **GBM price path** — prices follow the model dS = μS dt + σS dW, simulated via its analytical solution with random standard-normal shocks at each step.
- **Touch** — the price enters the band `[R − ε, R)` (coming from below the band) without rising above `R`. `ε` is a small tolerance so numerical noise does not cause valid touches to be missed.
- **Breakout** — the price reaches or exceeds the resistance level `R`; the step of the first breakout is recorded.

## Parameters

| Parameter           | Value | Meaning                                   |
| ------------------- | ----- | ----------------------------------------- |
| `S0`              | 100.0 | Initial price                             |
| `R`               | 110.0 | Resistance level                          |
| `mu`              | 0.05  | Annual drift                              |
| `sigma`           | 0.25  | Annual volatility (baseline)              |
| `T`               | 1.0   | Time horizon (years)                      |
| `steps`           | 252   | Trading days per year                     |
| `n_sims`          | 10000 | Number of simulated paths                 |
| `n_paths_preview` | 20    | Paths shown in the preview plot           |
| `eps`             | 0.5   | Width of the "touch" band near resistance |

## Notebook Structure

1. **Step 1 — Setup.** Import libraries, fix the random seed, set a uniform figure size, and define the GBM parameters and the touch definition.
2. **GBM simulation.** `GeometricBrownianMotionSimulator` generates single and multiple price paths; a preview of 20 paths is plotted against the resistance level.
3. **Step 2 — Path analysis.** `analyze_path_touches_and_breakout()` counts touches and detects the first breakout for a single path.
4. **Step 3 — Monte Carlo.** `monte_carlo_touches_breakout()` runs the analysis over all simulated paths and reports breakout probability, average touches across all paths, and average touches conditional on a breakout.
5. **Step 4 — Visualisations.** A histogram of touch counts, plus breakout probability and average touches as functions of volatility (σ ∈ {0.15, 0.20, 0.25, 0.30, 0.35}).

## Outputs

- Sample GBM price paths with the resistance level marked.
- Printed statistics: breakout probability, average touches (all paths), average touches (conditional on breakout).
- Histogram of touch counts.
- Breakout probability vs. volatility and average touches vs. volatility.

## Findings

- Most paths register very few touches; prices often either never reach the resistance or break through it almost immediately.
- Higher volatility raises the breakout probability, since larger fluctuations make crossing the resistance easier.
- Average touches rise slightly with volatility, but most paths still make only a few attempts before breaking out, if at all.

## Notes

The GBM implementation is adapted and simplified from QuantStart's "Geometric Brownian Motion Simulation with Python," rewritten to fit this project.
