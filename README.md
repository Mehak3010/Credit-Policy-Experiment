# Credit Policy Experiment

A statistical and credit-risk analysis of a simulated lending-policy change, evaluating default reduction, approval tradeoffs, borrower risk, and expected credit loss.

---

## 🔎 Key Results

| Metric | Result |
|---|---:|
| Default Rate Reduction | **1.19 pp** |
| Relative Default Reduction | **5.93%** |
| Approval Rate Change | **−2.42 pp** |
| Z-Statistic | **−17.16** |
| P-Value | **2.55 × 10⁻⁶⁶** |
| ROC-AUC | **0.650** |
| PR-AUC | **0.310** |
| Modeled Expected-Loss Savings | **$83.64M** |
| Additional Rejections | **15,752** |
| Break-even Contribution | **$5,309.95 / rejection** |

> **Important:** Treatment outcomes are simulated. Expected-loss results are modeled using illustrative grade-based LGD assumptions and loan amount as an EAD proxy.

---

## 📊 Power BI Dashboard

The project includes a five-page Power BI dashboard designed to communicate the experiment results from an executive, statistical, risk, and economic perspective.

### Executive Summary

![Executive Summary](screenshots/01-executive-summary.png)

### Risk & Portfolio Analysis

![Risk & Portfolio Analysis](screenshots/02-risk-portfolio.png)

### Statistical Evidence

![Statistical Evidence](screenshots/03-statistical-evidence.png)

### Economic Decision

![Economic Decision](screenshots/04-economic-decision.png)

### Methodology & Assumptions

![Methodology & Assumptions](screenshots/05-methodology.png)

> **Note:** Treatment outcomes are simulated. Expected-loss estimates use illustrative grade-based LGD assumptions and loan amount as an EAD proxy.

### Power BI Report

The complete `.pbix` file is available in the `dashboard/PowerBI/` directory in the repository:

**[Open / Download the Power BI Report](dashboard/PowerBI/Credit-Policy-Experiment.pbix)**

The `.pbix` file can be opened using **Power BI Desktop**.

---

## 1. Project Objective

Credit policy decisions involve a tradeoff between **risk and growth**.

A policy that approves more borrowers may increase business volume but can also increase expected credit losses. A stricter policy may reduce defaults but reject borrowers who could have generated positive contribution.

This project evaluates that tradeoff through a simulated experimental framework.

The analysis has three main objectives:

1. Measure the effect of the simulated policy on default rates.
2. Measure the impact of the policy on approval rates.
3. Translate the risk reduction into expected credit loss and evaluate the economic tradeoff.

The project uses historical Lending Club data as the underlying borrower population. The policy experiment and treatment outcomes are **simulated** and should not be interpreted as evidence about an actual Lending Club policy.

---

## 2. Dataset

The project uses historical consumer loan data from Lending Club.

The raw data is stored in:

```text
data/raw/

├── loan.csv
└── LCDataDictionary.xlsx
```

The dataset contains borrower and loan characteristics such as:

- Annual income
- Loan amount
- Debt-to-income ratio
- Employment length
- Interest rate
- Loan grade
- Loan term
- Historical loan outcome information

The selected dataset does not provide all variables required for a complete production credit-risk model. In particular, a suitable borrower-level LGD field and credit-score field were not available in the selected data.

Therefore, some variables used in the policy simulation and expected-loss analysis are explicitly modeled rather than directly observed.

---

## 3. Experiment Design

The historical borrower population is used to construct a simulated randomized policy experiment.

Borrowers are assigned to:

```text
Control
   vs
Treatment
```

The experiment contains:

```text
Total borrowers:  1,302,848

Control:            651,872
Treatment:          650,976
```

The control group represents the existing policy.

The treatment group represents the simulated new credit policy.

The treatment policy is designed to reduce borrower risk while allowing most borrowers to remain eligible.

---

## 4. Primary Experiment Outcome

The primary outcome is the simulated default indicator:

```text
simulated_default
```

### Null hypothesis

\[
H_0: p_T = p_C
\]

The treatment and control policies have the same default rate.

### Alternative hypothesis

\[
H_1: p_T < p_C
\]

The treatment policy reduces the default rate.

A two-proportion Z-test is used to evaluate the difference.

---

## 5. Exploratory Data Analysis

The EDA stage examines:

- Data quality
- Missing values
- Borrower characteristics
- Loan characteristics
- Default patterns
- Treatment/control distributions
- Risk segmentation
- Income segments
- DTI segments
- Loan-size segments

The analysis also examines whether the treatment and control populations are sufficiently comparable before interpreting the policy effect.

---

## 6. Hypothesis Testing Results

The simulated experiment produced the following results:

| Metric | Control | Treatment |
|---|---:|---:|
| Default rate | 20.10% | 18.91% |
| Approval rate | 100.00% | 97.58% |

### Default effect

The treatment reduced the simulated default rate by:

**1.19 percentage points**

The relative reduction was:

**5.93%**

The 95% confidence interval for the treatment-minus-control difference was:

\[
[-1.33\%, -1.06\%]
\]

The two-proportion Z-test produced:

```text
Z-statistic: -17.162
p-value:     2.54787 × 10⁻⁶⁶
```

The result provides strong statistical evidence that the simulated treatment policy reduces the default rate.

However, statistical significance alone is not sufficient for a credit-policy decision because the treatment also reduces approvals.

---

## 7. Approval Tradeoff

The treatment approval rate was:

```text
Control:    100.00%
Treatment:   97.58%
```

This represents an approval-rate reduction of:

**2.42 percentage points**

Across the treatment population, this corresponds to approximately:

**15,752 additional rejected applications**

Therefore, the experiment produces the following tradeoff:

```text
Default rate
20.10% → 18.91%
       ↓
  1.19 pp reduction

Approval rate
100.00% → 97.58%
          ↓
     2.42 pp reduction
```

The economic analysis is therefore necessary before recommending the policy.

---

## 8. Power Analysis

The experiment was evaluated using a 5% significance level and an 80% target statistical power.

For a target default-rate reduction of 1.2 percentage points:

```text
Required total sample: approximately 34,182
Actual sample:                  1,302,848
```

The actual experiment therefore contains substantially more observations than required to detect the planned effect.

The estimated minimum detectable effect at 80% power is approximately:

**0.20 percentage points**

The achieved power for the planned effect is approximately:

**100%**

This means the experiment has very high statistical power.

As a result, the analysis places greater emphasis on:

- Effect magnitude
- Confidence intervals
- Risk impact
- Approval impact
- Expected loss

rather than interpreting a small p-value as evidence of economic importance by itself.

---

## 9. Logistic Regression

A logistic regression model is used as an interpretable borrower-level risk model.

The model uses observable borrower and loan characteristics including:

- Annual income
- Loan amount
- DTI ratio
- Employment years
- Interest rate
- Loan term
- Loan grade

Variables directly generated by the policy experiment are excluded from the predictive feature set to avoid leakage.

The model is intended to estimate and rank borrower risk rather than provide causal evidence about individual borrower characteristics.

### Model performance

```text
ROC-AUC: 0.650
PR-AUC:  0.310
```

The model provides moderate discrimination.

More importantly for this project, the predicted probabilities produce a clear risk ordering across deciles.

Example:

| Risk Decile | Predicted PD | Observed Default |
|---|---:|---:|
| D1 Lowest | 7.81% | 8.11% |
| D5 | 17.32% | 17.33% |
| D8 | 24.50% | 24.30% |
| D10 Highest | 36.01% | 36.79% |

The increasing observed default rate across risk deciles indicates that the model is useful for borrower risk segmentation.

---

## 10. Expected Loss Analysis

Expected loss is modeled using:

\[
EL = PD 	imes LGD 	imes EAD
\]

where:

- **PD** = probability of default
- **LGD** = loss given default
- **EAD** = exposure at default

### PD

The policy probability of default is used as the risk component in the expected-loss analysis.

### LGD

The dataset does not contain a suitable borrower-level LGD field.

To introduce risk segmentation without claiming that LGD is observed, the base expected-loss analysis uses illustrative grade-based LGD assumptions:

| Grade | Illustrative LGD |
|---|---:|
| A | 35% |
| B | 40% |
| C | 45% |
| D | 50% |
| E | 55% |
| F | 60% |
| G | 65% |

These values are modeling assumptions and are **not estimated historical LGDs**.

Sensitivity analysis is performed across alternative LGD assumptions.

### EAD

Loan amount is used as a transparent proxy for exposure at default.

Exposure is assigned only to approved borrowers.

---

## 11. Expected Loss Results

Under the stated LGD and EAD assumptions:

| Metric | Result |
|---|---:|
| Control expected loss | $865.64M |
| Treatment expected loss | $782.00M |
| Modeled expected-loss savings | **$83.64M** |
| Expected-loss reduction | **9.66%** |

The treatment therefore produces a modeled reduction in expected credit loss.

However, this is **not a realized profit estimate**.

The result depends on the assumed LGD framework and the use of loan amount as an EAD proxy.

---

## 12. Economic Tradeoff

The treatment produces:

```text
Expected-loss savings:      $83.64M
Additional rejections:       15,752
```

Based on these values, the approximate break-even contribution per additional rejected application is:

**$5,309.95**

This means that, under the modeled assumptions, the expected-loss benefit can offset up to approximately $5,310 of contribution sacrificed per additional rejection.

The policy becomes economically attractive only when:

\[
Expected\ Loss\ Savings >
Value\ of\ Lost\ Approvals
\]

The dataset does not contain sufficient information to estimate the complete contribution or profitability of each approved loan. Therefore, the project reports a break-even framework rather than claiming a precise profit figure.

---

## 13. Overall Findings

The simulated policy produces three important outcomes.

### Default risk decreases

```text
20.10% → 18.91%
```

An absolute reduction of:

**1.19 percentage points**

### Approvals decrease

```text
100.00% → 97.58%
```

A reduction of:

**2.42 percentage points**

### Modeled expected loss decreases

Under the stated assumptions:

```text
$865.64M → $782.00M
```

A modeled reduction of:

**$83.64M / 9.66%**

The results therefore support the following conclusion:

> The simulated treatment policy provides strong statistical evidence of lower default risk and produces lower modeled expected credit loss, but the policy decision depends on whether the credit-loss benefit is sufficient to compensate for the economic value of the additional rejected applications.

---

## 14. Project Structure

## 14. Project Structure

```text
Credit-Policy-Experiment/
│
├── dashboard/
│   └── PowerBI/
│       └── Credit-Policy-Experiment.pbix
│
├── screenshots/
│   ├── 01-executive-summary.png
│   ├── 02-risk-portfolio.png
│   ├── 03-statistical-evidence.png
│   ├── 04-economic-decision.png
│   └── 05-methodology.png
│
├── data/
│   ├── processed/
│   │   ├── .gitkeep
│   │   └── credit_policy_experiment.csv
│   │
│   └── raw/
│       ├── loan.csv
│       └── LCDataDictionary.xlsx
│
├── notebooks/
│   ├── 01_data_generation.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_hypothesis_testing.ipynb
│   ├── 04_power_analysis.ipynb
│   ├── 05_logistic_regression.ipynb
│   └── 06_expected_loss.ipynb
│
├── reports/
├── src/
├── tests/
│
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## 15. Notebook Workflow

The notebooks are intended to be executed in sequence:

```text
01 → 02 → 03 → 04 → 05 → 06
```

Notebook 01 generates the processed experiment dataset used by the subsequent notebooks.

---

## 16. Key Learnings

### Statistical significance is not business significance

With more than one million observations, relatively small effects can become highly statistically significant.

The size and confidence interval of the effect therefore matter alongside the p-value.

### Experiment design matters

Power analysis shows how sample size, effect size, significance level and statistical power interact.

### Risk models and experiments answer different questions

The randomized experiment evaluates the **policy effect**.

The logistic regression evaluates **borrower risk**.

These should not be treated as the same analysis.

### Probability calibration matters

For credit-risk applications, estimating a useful probability of default can be as important as ranking borrowers correctly.

### Credit policy is a tradeoff

Reducing default risk can require rejecting additional borrowers.

A policy should therefore be evaluated using both risk outcomes and economic outcomes.

### Assumptions must be explicit

LGD is not directly observed in the selected dataset. The project makes this limitation explicit and performs sensitivity analysis rather than presenting the assumptions as historical facts.

---

## 17. Limitations

This project is a **simulated credit-policy experiment**, not a production underwriting system.

Important limitations include:

1. Treatment outcomes are simulated.
2. LGD is assumed rather than directly observed.
3. Loan amount is used as a proxy for EAD.
4. Full loan economics are unavailable.
5. Interest revenue, funding costs, operating costs, capital charges and other profitability components are not modeled.
6. The logistic regression is an interpretable predictive model, not a production credit scorecard.
7. Model coefficients represent associations and should not be interpreted as causal effects.
8. The historical dataset does not contain all information that would typically be available in a production credit-risk environment.

The results should therefore be interpreted as a **credit-policy evaluation framework** rather than a prediction of real-world financial performance.

---

## 18. Reproducibility

### Requirements

Python 3.11 is used for the project.

Install the required packages:

```bash
pip install -r requirements.txt
```

Run the notebooks in order:

```text
01 → 02 → 03 → 04 → 05 → 06
```

Notebook 01 must be run before the subsequent notebooks because it generates the processed experiment dataset.

---

## 19. Power BI Dashboard

The Power BI dashboard provides an executive-level view of the credit-policy analysis across five pages.

### Dashboard Pages

| Page | Focus |
|---|---|
| Executive Summary | Policy impact, default rate, approval rate, and modeled loss savings |
| Risk & Portfolio Analysis | Credit-grade risk, risk deciles, ROC-AUC, and PR-AUC |
| Statistical Evidence | Hypothesis testing, confidence intervals, and power analysis |
| Economic Decision | Expected loss, savings, additional rejections, and break-even analysis |
| Methodology & Assumptions | Experiment design, PD/LGD/EAD methodology, and limitations |

The dashboard complements the Python notebooks by translating the statistical and credit-risk analysis into a business decision framework.

### Power BI Report

The complete `.pbix` file is available in the `dashboard/` directory:

[Download the Power BI Report](dashboard/Credit-Policy-Experiment.pbix)

The report can be opened using Power BI Desktop.

---

## 20. Conclusion

This project evaluates a simulated credit-policy change from three perspectives:

```text
Experimentation
      ↓
Does the policy change default?

Risk Modeling
      ↓
How can borrower risk be estimated and segmented?

Credit Economics
      ↓
Does the reduction in expected loss justify
the reduction in approvals?
```

The simulated treatment reduces default by **1.19 percentage points**, with strong statistical evidence of the difference. It also reduces approvals by **2.42 percentage points**.

Under the stated illustrative LGD and EAD assumptions, the treatment produces approximately **$83.64M in modeled expected-loss savings** across the simulated experiment population.

The final policy decision therefore depends not only on the statistical evidence, but also on the economic value of the additional applications that would be rejected.
