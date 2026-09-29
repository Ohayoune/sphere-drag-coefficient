# Measuring the Drag Coefficient of a Sphere

An experimental physics capstone project. We dropped ping pong balls of five different masses down a 12 m stairwell, tracked their fall on video, and asked whether the measured drag coefficient matches the textbook value for a sphere.

> **Result:** $C_d = 0.458 \pm 0.078$, compared with the theoretical $0.47$. That is a **2.7 % difference**, well within experimental uncertainty, at Reynolds numbers of about 25,000–32,000.

**Bellevue College · PHY 123 (Physics 3) · Spring 2026**
Xinyu Zhu, Eli Ohayon, Vishal Panthangi, Mason Chin

---

## Research question

When a sphere falls through air and reaches terminal velocity, does its measured drag coefficient match the value $C_d \approx 0.47$ predicted for a sphere at Reynolds numbers between $10^3$ and $10^5$?

## Theory

A sphere falling through air feels a quadratic drag force opposing its motion:

$$F_{drag} = \tfrac{1}{2}\,\rho\, C_d\, A\, v^2$$

At terminal velocity $v_t$, drag balances gravity, so the drag coefficient can be found from a measured terminal velocity:

$$mg = \tfrac{1}{2}\,\rho\, C_d\, A\, v_t^2 \quad\Longrightarrow\quad C_d = \frac{2mg}{\rho A v_t^2}$$

With $v \approx 9$–$12$ m/s, $D = 0.040$ m and $\mu = 1.85\times10^{-5}$ Pa·s, the Reynolds number $Re = \rho v D/\mu \approx 25{,}000$–$32{,}000$. That is inside the range where $C_d \approx 0.47$ applies.

**Finite drop height correction.** A ball that has fallen only a height $h$ has not fully reached $v_t$. Under quadratic drag, the speed it has actually reached is

$$v_{meas}^2 = v_t^2\left(1 - e^{-2gh/v_t^2}\right)$$

This equation can't be solved for $v_t$ directly. Instead we computed $v_{meas}$ for $v_t = 6$–$18$ m/s in steps of 0.01 m/s, and chose the $v_t$ whose prediction matched each ball's measured speed.

---

## How the project evolved

### 1 · First attempt: the data didn't make sense

We filled five ping pong balls with sugar. Four of them came to **2.9 g, 9.8 g, 17.7 g and 31.2 g**. The fifth broke during the final trial, so its mass was never measured. We dropped each ball from about 15 m, filmed it in iPhone slow motion, and tracked it in Tracker over five trials.

Our first analysis timed each ball through the last ~2 m of its fall and plotted average speed against mass. The error bars were huge and the trend was not physical. On closer inspection, **the same ball dropped from the same height gave very different speeds from trial to trial**: Ball 1 measured about 13 m/s in three trials, about 8 m/s in one, and about 2.8 m/s in another.

### 2 · Diagnosis

The [data quality report](01-first-attempt/data-quality-report.pdf) traced the problem to two causes:

- **Timing errors.** The slow-motion recordings did not have consistent frame timing across trials (the raw exports mix frame intervals), but the analysis assumed a constant frame rate.
- **Balls too heavy for the drop.** Theory predicts that after about 15 m of fall the 2.9 g ball reaches about 99 % of its terminal velocity, while the 31.2 g ball reaches only about 54 %. The heavier balls never got close to terminal velocity.

The report recommended **lighter balls** and a cleaner measurement method.

### 3 · Redesign

| | First attempt | Final experiment |
|---|---|---|
| Ball masses | 2.9 – 31.2 g | **2.83, 3.92, 5.30, 6.12, 7.59 g** |
| Drop height | ≈ 15 m | **12 m** indoor stairwell |
| Video | iPhone slow motion, frame timing not consistent | **60 fps**, tripod-mounted, perpendicular to the fall |
| Calibration | Meter stick | Meter stick placed in the plane of the fall |
| Speed estimate | Average over the final 2 m | **Linear fit** of position vs. time in Tracker |
| Model | Assumed the ball reached terminal velocity | **Finite drop height correction** applied |

All balls are regulation 40 mm celluloid ping pong balls, so the cross-sectional area $A$ is the same for all of them. The sugar is sealed inside with tape, and each ball was weighed on a triple-beam balance before and after its trials to check that no sugar was lost.

### 4 · Final results

Before the correction, the measured $C_d$ **rose steadily with mass**, from 0.453 to 0.696 (mean 0.58). That trend means the heavier balls had not reached terminal velocity within 12 m. After the finite drop height correction, all five values fall close to 0.46, and the trend with mass disappears.

| Mass (g) | $v_{meas}$ (m/s) | $C_d$ raw | $v_t$ adjusted (m/s) | $C_d$ adjusted |
|---:|---:|---:|---:|---:|
| 2.83 | 9.04 | 0.453 ± 0.041 | 9.36 | 0.423 ± 0.044 |
| 3.92 | 9.85 | 0.529 ± 0.050 | 10.48 | 0.467 ± 0.058 |
| 5.30 | 10.76 | 0.599 ± 0.059 | 11.98 | 0.483 ± 0.076 |
| 6.12 | 11.43 | 0.613 ± 0.061 | 13.34 | 0.450 ± 0.084 |
| 7.59 | 11.95 | 0.696 ± 0.069 | 14.63 | 0.464 ± 0.103 |
| **Mean** | | 0.58 | | **0.458 ± 0.078** |

**Uncertainty.** The final ±0.078 combines, by root-sum-square, the scatter between the five balls (about 4 %) and the propagated instrument uncertainty: drop height, air density, video frame period and position. Inverting the correction model amplifies velocity errors, and $C_d \propto 1/v_t^2$ doubles that effect again. As a result, the heaviest ball, which reached only about 82 % of $v_t$, has the largest error bar (about 22 %, against about 10 % for the lightest).

---

## Repository contents

```
.
├── 01-first-attempt/
│   ├── raw-data/trial-1.csv … trial-5.csv
│   ├── average-speed-analysis.xlsx
│   └── data-quality-report.pdf
└── 02-final-experiment/
    ├── drag-coefficient-analysis.xlsx
    └── poster.pdf
```

| File | Description |
|---|---|
| [`01-first-attempt/raw-data/`](01-first-attempt/raw-data) | Raw Tracker exports from the first attempt, one file per trial. Each file has five blocks, one per ball from lightest to heaviest, with columns `t, y, Vy, Ay`. |
| [`01-first-attempt/average-speed-analysis.xlsx`](01-first-attempt/average-speed-analysis.xlsx) | The first analysis: average speed through the final ~2 m against ball mass, with an instrument error table. |
| [`01-first-attempt/data-quality-report.pdf`](01-first-attempt/data-quality-report.pdf) | Write-up of why the first attempt failed: position–time plots per ball, a theoretical comparison, and recommendations. |
| [`02-final-experiment/drag-coefficient-analysis.xlsx`](02-final-experiment/drag-coefficient-analysis.xlsx) | Final analysis: position–time data for 5 masses × 5 trials, terminal velocity fits, $C_d$ with error propagation, and the terminal velocity model (`Vt model` sheet) used for the correction. |
| [`02-final-experiment/poster.pdf`](02-final-experiment/poster.pdf) | The final research poster. |

## Tools

- [Tracker](https://physlets.org/tracker/) for frame-by-frame video analysis and tracking the center of mass
- Microsoft Excel for data reduction, curve fitting, error propagation and figures
- A smartphone camera (60 fps) on a tripod, a meter stick for scale, and a triple-beam balance

## References

1. Taylor, J. R. *Classical Mechanics*. University Science Books, 2005. §2.4, Quadratic Air Resistance.
2. Resnick, R., Halliday, D., Krane, K. *Physics*, 5th ed. Wiley, 2002.
3. Brown, D. *Tracker Video Analysis and Modeling Tool*. [physlets.org/tracker](https://physlets.org/tracker/). Accessed 2026.
