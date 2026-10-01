# Finance AgentX

> An auditable personal-finance decision agent that evaluates whether a purchase is safe today, should be split into installments, or should wait.

Finance AgentX turns a user's financial history into a transparent purchase recommendation. Instead of producing an opaque yes/no answer, it combines transaction analysis, recurring-expense detection, income estimation, a 90-day cash-flow forecast, and deterministic policy rules to explain *why* a decision was made.

> **Disclaimer:** This is an educational software project, not financial advice. Outputs should not be treated as investment, credit, lending, or professional financial advice.

## The problem

A purchase can look affordable from today's balance while still creating a cash shortfall after upcoming bills, subscriptions, repayments, or irregular income. Finance AgentX evaluates the purchase against an expected financial timeline rather than relying on a single current-balance check.

Given historical financial data and a proposed purchase, the system recommends one of the following actions:

- **Pay in full** — the purchase can be made without violating the configured cash-safety threshold.
- **Split into installments** — spreading the cost produces a safer projected cash position.
- **Wait** — neither paying in full nor available installment options meet the safety rules.

## Highlights

- **90-day cash-flow forecasting** that models expected balances after income, recurring commitments, and the proposed purchase.
- **Deterministic decision engine** that makes recommendations reproducible and inspectable.
- **Recurring-expense detection** to account for subscriptions, bills, and other repeated transactions.
- **Income and transaction analysis** to infer financial patterns from historical data.
- **Installment-plan evaluation** to compare payment options against projected cash safety.
- **Evidence extraction** from receipt images and messages through dedicated extraction modules.
- **Validation and diagnostics** to check assumptions, data quality, and decision outputs.
- **Auditable outputs** that preserve the reasoning behind every recommendation.

## How it works

```text
Historical transactions + profile data + purchase request
                         │
                         ▼
      Income / recurring-expense / evidence extraction
                         │
                         ▼
             90-day balance forecast
                         │
                         ▼
       Deterministic rules + installment solver
                         │
                         ▼
    Pay in full • Split into installments • Wait
             with rationale and validation
```

The system deliberately keeps the final financial decision in a deterministic policy layer. This makes a recommendation easier to test, reproduce, calibrate, and explain than an LLM-only decision flow.

## Repository structure

```text
finance-agentx/
├── code/
│   ├── main.py                # Entry point
│   ├── data.py                # Data loading and preparation
│   ├── income.py              # Income analysis
│   ├── recurring.py           # Recurring-expense detection
│   ├── forecast.py            # Cash-flow forecasting
│   ├── decide.py              # Decision policy
│   ├── solver.py              # Installment-option solver
│   ├── rulefit.py             # Rule fitting/calibration support
│   ├── validate.py            # Output validation
│   ├── diagnose.py            # Diagnostics
│   ├── extract_images.py      # Receipt-image extraction
│   ├── extract_messages.py    # Message extraction
│   └── config.py              # Configuration
├── dataset/                   # Input datasets
├── evaluation/                # Evaluation assets
├── output.csv                 # Generated output example
├── requirements.txt           # Python dependencies
├── problem_statement.md       # Challenge and domain context
└── INTERVIEW_PREP.md          # Project discussion notes
```

## Tech stack

- **Language:** Python
- **Data processing:** Pandas and CSV-based workflows
- **Decisioning:** Deterministic financial rules and constraint-based installment evaluation
- **Forecasting:** Time-based balance projections
- **Evidence layer:** Receipt-image and message extraction utilities
- **Quality controls:** Validation, diagnostics, calibration, and evaluation scripts

## Getting started

### Prerequisites

- Python 3.10+
- pip

### Installation

```bash
git clone https://github.com/aditya-c7/finance-agentx.git
cd finance-agentx
python -m venv .venv
```

Activate the virtual environment:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

### Run

The implementation lives in the `code/` directory. Run the entry point from the repository root:

```bash
python code/main.py
```

Use the datasets and configuration included in the repository. Depending on the input path or workflow you want to evaluate, you can also use the supporting utilities:

```bash
python code/validate.py
python code/diagnose.py
python code/calibrate.py
```

## Decision principles

Finance AgentX is designed around a few safety-oriented principles:

1. **Forecast before deciding.** Current balance alone is not enough; upcoming obligations matter.
2. **Protect a cash buffer.** A recommendation must preserve the configured safety threshold during the forecast horizon.
3. **Prefer explainability.** Each action should be traceable to explicit financial evidence and rules.
4. **Compare alternatives.** If paying in full is unsafe, installment plans are tested before recommending a delay.
5. **Validate inputs and outputs.** Financial data and recommendation results are checked before they are used.

## Example recommendation

```text
Recommendation: Split into installments

Reasoning:
- A full payment would bring the projected balance below the safety buffer.
- A monthly installment plan preserves the buffer across the forecast horizon.
- Expected recurring commitments have been included in the projection.

This is an educational recommendation generated from configured rules and supplied data.
```

## Evaluation

The repository includes an `evaluation/` directory and supporting analysis scripts for reviewing decision behavior. Useful scripts include:

- `explore_samples.py` — inspect representative data examples
- `explore_deep.py` — deeper exploratory analysis
- `compare_messages.py` — compare extracted message evidence
- `calibrate.py` — calibrate decision rules
- `diagnose.py` — investigate system behavior and anomalies
- `validate.py` — validate outputs and decision constraints

## Limitations

- Forecast quality depends on the completeness and accuracy of supplied historical data.
- Financial patterns can change unexpectedly; future income and expenses are estimates.
- The project does not connect to banks, execute payments, provide credit decisions, or offer regulated financial advice.
- Any extracted receipt or message evidence should be reviewed before it is used in a real financial workflow.

## Future improvements

- Interactive dashboard for purchase scenarios and forecast visualisation
- Configurable financial goals and user-specific risk tolerance
- Additional evaluation datasets and automated regression tests
- Privacy-preserving local-first data ingestion
- Richer explanation reports and forecast charts

## Author

Built by [Aditya C](https://github.com/aditya-c7).

If this project is useful or interesting, consider starring the repository.
