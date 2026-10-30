# The Capex Line Isn't Where the Risk Is: South Pasopati Coal Project (Kalimantan)

**Case Study #14 in *The Feasibility Stress Test*. Primary lens: Capex Planning.**

An open-source Python reconstruction and browser simulator of a published real-options study of a small coal project in Indonesia. The paper compares three capex routes and reports a Monte Carlo standard deviation of NPV about twice its mean. This repo asks which input actually drives that spread.

## The case

South Pasopati is a small high-rank coal project (1.7 Mt, capacity capped at 500 kt/yr, four years) next to PT Borneo Indobara's operations. The authors compare a base east haul route (capex USD 6.71 M), a west route through third-party infrastructure (USD 8.26 M) and the east route with carbon-reduction equipment (USD 7.29 M). They run a DCF, one-at-a-time sensitivity, a 10,000-iteration Monte Carlo and a binomial real-options lattice with execute and abandon options.

## Citation

Trijayanto, T. & Hakam, D. F. (2025). *Economic Viability and Flexibility of the South Pasopati Coal Project, Indonesia: A Real Options Approach Under Market Volatility and Carbon Pricing.* Journal of Risk and Financial Management, 18(5), 225. https://doi.org/10.3390/jrfm18050225 (open access)

## Confirmed findings (paper's own text)

| Metric | Value |
|---|---|
| Capex Base / Alt 1 / Alt 2 | USD 6.71 M / 8.26 M / 7.29 M |
| DCF NPV Base / Alt 1 / Alt 2 (WACC 11.42%) | USD 7.10 M / 4.47 M / 1.32 M |
| IRR Base / Alt 1 / Alt 2 | 54% / 33.7% / 19.7% |
| Monte Carlo Base (10,000 iterations) | mean 7.19; SD 13.3; P10 -9.79; P90 24.16 |
| Monte Carlo Alt 1 / Alt 2 | mean 4.49, SD 0.94 / mean 1.24, SD 1.09 |
| Real option (Base) | underlying PV 14.55; option value 0.95; ENPV 8.05; 38% probability of abandoning |
| Tornado (Base) | -20% coal price => NPV -72%; +20% OPEX => NPV -60% |
| Post-decision outcome | not reported |

## Reconstruction results (ILLUSTRATIVE, not the authors' numbers)

| Check | Result |
|---|---|
| Break-even coal price | USD 85.87/t, 7.5% below base |
| Break-even capex | USD 15.3 M, 127% above plan |
| Base NPV after -20% price / +20% OPEX | -14.8 M / -11.5 M (paper tornado: -72% / -60% of NPV) |
| Base MC (price vol 13%, my assumption) | mean 6.6; SD 13.3; P(NPV<0) 28.6% |
| Loss probability implied by paper's mean and SD, normal approximation | 29.4% (my arithmetic) |
| Same price shocks on Alt 1 / Alt 2 | SD 13.5 / 13.9 (paper: 0.94 / 1.09) |
| Spearman with NPV | price +0.995; OPEX ratio -0.08; capex -0.03 |
| Free price floor at 90% / 95% of base | P(NPV<0) 28.4% / 1.6% |

## Honesty disclosure

- The paper's open-access HTML was read through an **automated extraction pass**, not a manual page-by-page review. Table 1 (real-option inputs), Table 2 (option values) and all figures were images and were **not readable**. Check numbers against the paper before reuse.
- The paper does **not** publish its cash-flow workbook, Monte Carlo distribution families or parameters, or its coal-price regression coefficients.
- Tax (22%), straight-line depreciation, even output, flat price, and the Monte Carlo distributions and 13% price volatility are **my assumptions**. One OPEX ratio per route is **calibrated** to the paper's NPV, so matching those NPVs is by construction. The calibrated ratio for Alt 2 (0.922) falls outside the paper's stated 83-89% range.
- The rebuild does **not** reproduce the paper's Monte Carlo statistics or real-option values. It disagrees with the paper's tornado and with the very low SD reported for the alternative routes. I could not resolve these from the published text.
- The price-floor test is my illustration. The floor is free (no premium, no counterparty risk), so it is an upper bound on benefit.
- The paper has its own internal inconsistencies (for example profitability-index values and a "discount rate of 54%" that is the IRR). These observations are mine, from the extracted text.

## Run it

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute notebook.ipynb
```

Or open `docs/index.html` in a browser for the interactive simulator (no build step). Enable GitHub Pages on the `docs/` folder to host it.

## License

MIT, copyright 2026 Eka Krisna Santoso. See `LICENSE`. The source paper remains the authors' work.

---
Part of **The Feasibility Stress Test**: real Monte Carlo and financial-engineering case studies for capex decisions.
