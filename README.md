# Optimizing Rooftop Solar Battery Dispatch Under India's PM-KUSUM Scheme

A bilevel (Stackelberg) optimization model of how an electricity distributor should price an incentive for rural households to discharge home batteries during grid peak hours.

**Tools:** Python (Gurobi, NumPy, pandas), Excel Solver, parametric sweep analysis

---

## Problem

Under PM-KUSUM, India's rooftop solar scheme for farmers and rural households, an electricity distributor (DISCOM) needs households to discharge their batteries at grid peak. 
The coordination problem: households decide for themselves when to charge, discharge, import or export, so the DISCOM can only influence behavior through the incentive it offers.

**Question:** What single incentive rate (Rs/kWh paid for peak discharge) should the DISCOM set to maximize feeder-wide peak discharge without exceeding a fixed daily incentive budget?

## Model

This is a bilevel (leader-follower) problem.

- **Lower level (households):** Each household minimizes its own daily electricity cost by choosing when to charge or discharge its battery, import from the grid, or export surplus solar, subject to battery and tariff constraints.
- **Upper level (DISCOM):** The distributor sets one incentive rate to maximize feeder-wide peak discharge, subject to a fixed daily incentive budget.

**Calibration:** Battery and tariff parameters are based on real Rajasthan specifications: IRENA 2023 battery costs and Rajasthan Electricity Regulatory Commission FY23-24 time-of-day tariffs.

**Scale-up:** The model starts with a single household (1HH), then scales to a 10-household feeder (10HH) with heterogeneous residential, agricultural and commercial demand profiles (2.1-6.2 kWh/day).

**Validation:** The model was built in Gurobi/Python and cross-validated in Excel Solver.

## Key finding

Household peak discharge behaves as a **step function**. It jumps from near zero to the full battery capacity the moment any positive incentive is offered, then flattens, because the household has nothing more to give.

This means the DISCOM's best move is not to pay more. It is to pay the **smallest positive rate that clears its budget**.

- At **Rs 0.01/kWh**, the household still delivered its full peak discharge, at a cost of just **Rs 0.08/day**.
- Rates above **Rs 3/kWh** wasted budget for zero additional peak relief.

## Repository contents

| File | Description |
|---|---|
| `1HH_sample_model.ipynb` | Single-household bilevel model (Python/Gurobi) |
| `10HH_model_with_precise_lambda.ipynb` | 10-household feeder model (Python/Gurobi) |
| `PMKUSUM_Battery_Optimization_1HH.xlsx` | Excel Solver workbook used to cross-validate the single-household model |
| `Advanced BA Project Report.docx.pdf` | Written project report |

## How to run

```bash
pip install gurobipy numpy pandas openpyxl jupyter
jupyter notebook
```

Start with `1HH_sample_model.ipynb` (single household), then run `10HH_model_with_precise_lambda.ipynb` (10-household feeder), and run all cells in each. Open the Excel workbook to see the Solver cross-check of the single-household case. Gurobi needs a license; the free restricted license is enough for models of this size.

## Notes and limitations

- Household parameters are calibrated to published Rajasthan figures, but the households themselves are modeled, not measured.
- The step-function result depends on the model's assumptions (a household that can discharge its full battery at any positive incentive), so it should be read as a feature of this model and not a forecast of real-world behavior.

## Author

Prathiba Reddy, MS in Business Analytics & AI, Johns Hopkins Carey Business School
[Portfolio](https://6a38b7b1cee129ca7a613700--comforting-queijadas-d1f251.netlify.app) | [LinkedIn](https://www.linkedin.com/in/)
