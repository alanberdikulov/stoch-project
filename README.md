
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
