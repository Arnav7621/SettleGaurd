Problem
When a trade is executed, the seller must deliver shares and the buyer must pay funds. In India this runs through the clearing corporations (NSE Clearing, ICCL) and the depositories (NSDL, CDSL). A trade can fail because of:

Short delivery: the seller does not hold enough deliverable shares
Funds shortfall: the buyer lacks funds at pay-in
Late custodian confirmation: institutional trades (FPIs, mutual funds) not confirmed in time
Detail mismatch: quantity, price, or account details that do not agree
Failed deliveries can go to auction settlement, which brings price differences, penalties, funding costs, and client dissatisfaction. Brokers, custodians, and institutional desks have thousands of trades a day and a fixed number of ops staff, so the real question is not only which trades will fail but which ones should we fix first.

What SettleGuard Does
Predicts the probability that each pending trade fails to settle on time
Prices the failure: expected cost = probability x (exposure + auction differential + penalty + funding cost)
Ranks trades by expected cost per minute of ops effort
Explains every flag using SHAP, so analysts see the reasons
Simulates an ops day to show that cost-based prioritisation beats first-in-first-out (FIFO)
Lets users test what-ifs: earlier cutoff, smaller team, custodian outage, expiry-day volume spike


How It Works:
Synthetic trade generator (calibrated to public NSE statistics)
        |
Validated trade data (Parquet + DuckDB)
        |
Feature pipeline -> LightGBM -> Probability calibration
        |                              |
  SHAP explanations              P(fail)
                                       |
              Cost model -> Expected cost per trade
                                       |
        Ranking engine + SimPy ops-desk simulation (vs. FIFO)
                                       |
              FastAPI service -> Streamlit dashboard
                                                                          |

### Tech Stack

| Area                        | Tools                                  |
| --------------------------- | -------------------------------------- |
| Language and Data           | Python, pandas, NumPy                  |
| ML                          | scikit-learn, LightGBM                 |
| Explainability and Tuning   | SHAP, Optuna                           |
| Simulation and Optimisation | SimPy, OR-Tools                        |
| Storage                     | DuckDB, Parquet                        |
| App                         | FastAPI, Streamlit                     |
| MLOps                       | MLflow, Docker, GitHub Actions, pytest |              