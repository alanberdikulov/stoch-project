# Cardinality-Constrained Portfolio Optimization via the Overlap Gap Property

MIT 6.S897 / Discrete Probability and Stochastic Processes, Spring 2026.
Project 7 (Practical, 25% of final grade).

## What is this?

We study the cardinality-constrained portfolio selection problem on $n=468$ S&P 500
constituents over a 10-year window ($T=2{,}512$ trading days):
maximize the equal-weight Markowitz objective

$$ J(\sigma) = \tfrac{1}{k}\hat\mu^\top\sigma - \tfrac{\gamma}{2k^2}\sigma^\top\hat\Sigma\sigma $$

over binary supports $\sigma\in\{0,1\}^n$ with $\sum_i \sigma_i = k$.

Three contributions:

1. **Empirical diagnostics** show that S&P 500 returns are sub-Gaussian, the sample
   covariance is dominated by a market factor with 32 outlier eigenvalues, and the
   Restricted Isometry Property fails uniformly.
2. **Theory** (Theorems 4.1, 4.2, Corollary 4.3 of `paper/main.pdf`): we prove a sharp
   first-moment detection threshold $a_\star(\rho)=\sqrt{2h(\rho)}/(1-\rho)$ and a
   forbidden-overlap interval $[\rho, y_a]$ for a planted-spike model, plus a hardness
   corollary for path-stable algorithms (LASSO, top-eigenvector, fixed-temperature MCMC).
3. **Algorithm** (Theorem 6.1, Lemma 6.2): we design **SLH** (SpectralLocalHybrid), a
   deterministic poly-time algorithm with a Bonferroni recovery guarantee that
   provably circumvents the OGP barrier via a discontinuous top-$k$ thresholding step.
   SLH matches the brute-force oracle on every cell of the $(n,k)$ benchmark, ~50x
   faster than its closest non-deterministic competitor.

## Layout

```
.
├── README.md
├── extract_data.ipynb        Phase 0:  fetch + clean S&P 500 daily returns
├── 01_diagnostics.ipynb      Phase 1:  empirical diagnostics
├── 02_oracle_baselines.ipynb Phase 2a: brute-force oracle + 4 baselines
├── 03_mcmc_sampler.ipynb     Phase 2b: constrained Metropolis sampler + mixing
├── 04a_ogp_theory.ipynb      Phase 3a: numerical verification of OGP theory
├── 04_ogp_experiment.ipynb   Phase 3b: empirical OGP experiment
├── 05_hybrid_algorithm.ipynb Phase 4a: SLH algorithm + recovery curve + benchmark
├── data/
│   ├── returns.npy           cleaned log-returns matrix
│   ├── returns.parquet       same, as Pandas DataFrame
│   ├── tickers.csv           column labels
│   ├── *.json                phase-by-phase numerical results
│   ├── *.csv                 aggregate tables
│   └── figs/                 generated PNGs (mirrored into paper/figs/)
└── paper/
    ├── main.tex              top-level wrapper
    ├── phase1_diagnostics.tex
    ├── phase2_oracle_mcmc.tex
    ├── phase3a_ogp.tex       OGP theory section
    ├── phase3b_empirical.tex
    ├── phase4_hybrid.tex     SLH algorithm section
    ├── conclusion.tex
    └── figs/                 PNG figures embedded in the writeup
```

## Reproducing the results

```bash
pip install numpy pandas matplotlib scipy cvxpy yfinance pyarrow lxml requests
jupyter nbconvert --to notebook --execute extract_data.ipynb        # Phase 0
jupyter nbconvert --to notebook --execute 01_diagnostics.ipynb      # Phase 1
jupyter nbconvert --to notebook --execute 02_oracle_baselines.ipynb # Phase 2a
jupyter nbconvert --to notebook --execute 03_mcmc_sampler.ipynb     # Phase 2b
jupyter nbconvert --to notebook --execute 04a_ogp_theory.ipynb      # Phase 3a
jupyter nbconvert --to notebook --execute 04_ogp_experiment.ipynb   # Phase 3b
jupyter nbconvert --to notebook --execute 05_hybrid_algorithm.ipynb # Phase 4a
```

Total wall-clock to regenerate every figure: under 5 minutes on a single laptop CPU.

## Building the writeup

```bash
cd paper
pdflatex main.tex
pdflatex main.tex   # second pass for cross-references
```

Or upload the `paper/` folder to Overleaf as a project (it is self-contained:
all figures live in `paper/figs/`).
