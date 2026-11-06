# Option Pricing Engine (`quant-option-pricer`)

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

> **Option pricing and Greeks engine in Python — from Black-Scholes to Heston stochastic volatility.**

A modular, vectorized Python library designed for pricing European derivative contracts, computing analytical and numerical Greeks, calibrating implied volatility surfaces, and simulating stochastic volatility dynamics.

---

## Key Features & Roadmap

- [x] **Phase 1: Closed-Form Analytical Engine**
  - Black-Scholes-Merton European call/put pricing.
  - Full vectorization using `numpy` across strike prices and maturities.
  - Analytical first- and second-order Greeks ($\Delta, \Gamma, \Theta, \nu, \rho$).
- [ ] **Phase 2: Numerical Monte Carlo Engine**
  - Geometric Brownian Motion (GBM) path simulation.
  - Variance reduction techniques (antithetic variates, control variates).
- [ ] **Phase 3: Volatility Calibration**
  - Implied volatility solver using Newton-Raphson & Brent's method.
  - Volatility smile fitting and surface interpolation.
- [ ] **Phase 4: Stochastic Volatility (Heston Model)**
  - Coupled SDE simulation via Euler-Maruyama schemes.
  - Characteristic function closed-form pricing via Fast Fourier Transform (FFT).

---

### Black-Scholes Closed-Form Solution

For a European option on a non-dividend paying stock with spot price $S$, strike price $K$, risk-free rate $r$, volatility $\sigma$, and time-to-maturity $T$:

$$
C(S, t) = S N(d_1) - K e^{-r T} N(d_2)
$$

$$
P(S, t) = K e^{-r T} N(-d_2) - S N(-d_1)
$$

where:

$$
d_1 = \frac{\ln(S / K) + \left(r + \frac{\sigma^2}{2}\right) T}{\sigma \sqrt{T}}, \quad d_2 = d_1 - \sigma \sqrt{T}
$$

### Heston Stochastic Volatility Model

The asset price $S_t$ and its variance $v_t$ follow the coupled stochastic differential equations:

$$
dS_t = \mu S_t dt + \sqrt{v_t} S_t dW_t^S
$$

$$
dv_t = \kappa (\theta - v_t) dt + \xi \sqrt{v_t} dW_t^v
$$
**
where $dW_t^S dW_t^v = \rho dt$, $\kappa$ is the mean-reversion speed, $\theta$ is long-term variance, and $\xi$ is the volatility of volatility.**

---

## Repository Architecture

```text
quant-option-pricer/
├── pricer/
│   ├── __init__.py
│   ├── black_scholes.py       # Closed-form pricing & Greeks
│   ├── monte_carlo.py         # Monte Carlo simulation engine
│   ├── volatility.py          # Implied vol & Newton-Raphson
│   └── heston.py              # Heston stochastic volatility model
├── tests/
│   ├── test_black_scholes.py  # Pytest validation suites
│   └── test_monte_carlo.py
├── notebooks/
│   └── 01_bs_vs_mc_validation.ipynb
├── README.md
├── requirements.txt
└── .gitignore
