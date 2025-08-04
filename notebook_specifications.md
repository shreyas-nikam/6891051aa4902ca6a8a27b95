
# Jupyter Notebook Specification: PD Model Calibration and Validation Lab

## 1. Notebook Overview

**Learning Goals:**

*   Understand and validate Through-The-Cycle (TTC) and Point-In-Time (PIT) Probability of Default (PD) models.
*   Perform ongoing monitoring of PD model performance.
*   Generate model validation reports and inventory entries to demonstrate regulatory compliance.

**Expected Outcomes:**

*   A Jupyter Notebook that allows users to:
    *   Assess the calibration of TTC and PIT PD models.
    *   Evaluate the discriminatory power of PD models.
    *   Benchmark PIT PD model outputs against external data.
    *   Implement and visualize ongoing monitoring of PD model stability and performance.
    *   Generate validation reports and model inventory entries.

## 2. Mathematical and Theoretical Foundations

This section outlines the key mathematical concepts and formulas used in the lab.

**2.1 Probability of Default (PD):**

The Probability of Default (PD) represents the likelihood that a borrower will fail to meet their debt obligations within a specified time horizon. It is a crucial parameter in credit risk management.

**2.2 Through-The-Cycle (TTC) PD:**

TTC PD represents the average probability of default for a given rating grade over a long period, ideally spanning multiple economic cycles.

**2.3 Point-In-Time (PIT) PD:**

PIT PD is the probability of default at a specific point in time, considering current macroeconomic conditions.  It is dynamically adjusted based on factors like GDP growth, unemployment rate, and inflation.

**2.4 Brier Score:**

The Brier score measures the accuracy of probabilistic predictions. It quantifies the difference between predicted probabilities and actual outcomes. The Brier Score is calculated as:

$$BS = \frac{1}{N}\sum_{i=1}^{N}(f_i - o_i)^2$$

Where:

*   $N$ is the number of predictions.
*   $f_i$ is the forecast probability of event $i$.
*   $o_i$ is the actual outcome of event $i$ (0 if the event did not occur, 1 if the event occurred).
    * A lower Brier score indicates better calibration.

**2.5 Receiver Operating Characteristic (ROC) Curve and Area Under the Curve (AUC):**

The ROC curve plots the true positive rate (sensitivity) against the false positive rate (1 - specificity) at various threshold settings. AUC represents the area under the ROC curve and measures the model's ability to discriminate between defaulting and non-defaulting obligors.  An AUC of 1.0 represents perfect discrimination, while an AUC of 0.5 indicates performance no better than random chance.

**2.6 Kolmogorov-Smirnov (KS) Statistic:**

The KS statistic measures the maximum difference between the cumulative distribution functions of predicted PDs for defaulted and non-defaulted obligors. It quantifies the model's discriminatory power.

**2.7 Population Stability Index (PSI):**

PSI measures the change in the distribution of a characteristic (e.g., rating grade, predicted PD) between two time periods (e.g., training window and current quarter). It helps assess the stability of the portfolio and the model.

$$PSI = \sum_{i=1}^{N} (Actual\%_i - Expected\%_i) \cdot ln(\frac{Actual\%_i}{Expected\%_i})$$

Where:

*   $N$ is the number of categories.
*   $Actual\%_i$ is the percentage of observations in category $i$ in the current period.
*   $Expected\%_i$ is the percentage of observations in category $i$ in the baseline period.

    * A PSI value greater than 0.25 typically indicates a significant shift in the distribution.

## 3. Code Requirements

**3.1 Expected Libraries:**

*   `pandas`: For data manipulation and analysis.
*   `numpy`: For numerical computations.
*   `sklearn`: For machine learning tasks (e.g., ROC curve, Brier score, calibration curve).
*   `matplotlib`: For creating static plots and visualizations.
*   `seaborn`: For enhanced statistical data visualization.
*   `plotly`: For creating interactive plots and dashboards.
*   `scipy`: For scientific computing, including statistical functions like the Kolmogorov-Smirnov test.
*   `requests`: For fetching data from external APIs (e.g., FRED API).
*   `io`: For handing input and output operations
*   `nbconvert`: For automatic pdf report generation
*   `python-pptx`: For powerpoint slides generation

**3.2 Input/Output Expectations:**

*   **Input:**
    *   TTC PD mappings (CSV file).
    *   PIT PD model (Pickle file - Logistic Regression or Cox proportional hazard model).
    *   Transition matrix (NumPy array).
    *   Macroeconomic data (FRED API, IMF WEO CSV).
    *   Quarterly portfolio snapshots (CSV files).
    *   Out-of-time (OOT) back-test sample (CSV file).
    *   External agency default rate benchmarks (CSV file).
*   **Output:**
    *   Calibration table (CSV file).
    *   Brier score (Text file).
    *   AUC values by sector (CSV file).
    *   Benchmark level check results (CSV file).
    *   Monitoring metrics (CSV file).
    *   PSI report (CSV file).
    *   Validation report (PDF file).
    *   Quarterly monitoring slide (PPTX file).
    *   Model inventory entry (JSON file).
    *   Change log (MD file).

**3.3 Algorithms and Functions:**

*   **Calibration Analysis:**
    *   Calculate Brier score for the PIT PD model on the OOT sample.
    *   Generate a grade-level PD-vs-observed default rate table for the TTC model, comparing predicted PDs with realized default rates in the OOT sample.
    *   Generate a calibration belt (predicted vs. observed default rates with confidence ribbon) using `sklearn.calibration_curve` and `matplotlib`.
*   **Discriminatory Power Assessment:**
    *   Compute ROC/AUC for the PIT PD model on the OOT sample using `sklearn.metrics.roc_curve`.
    *   Calculate the Kolmogorov-Smirnov (KS) statistic to assess the separation between predicted PDs for defaulting and non-defaulting obligors in the OOT sample.
*   **Benchmarking:**
    *   Compare PIT PD outputs per rating grade with external agency default rate forecasts for the same grades and year.
*   **Ongoing Monitoring:**
    *   Implement a quarterly back-test loop to compare the model’s 12-month PD forecast with the realized default rates for the subsequent 12 months.
    *   Compute PSI for rating-grade mix and for the predicted PD distribution between the current quarter and the training window.
    *   Generate a traffic-light dashboard visualizing back-testing and PSI results for the last eight quarters.
*   **Reporting:**
    *   Generate a PDF validation report summarizing tests, conclusions, and key metrics for each model version.
    *   Generate a JSON file as a model inventory entry containing information about the model's owner, purpose, version, data used, and scheduled review dates.

**3.4 Visualizations:**

*   **Calibration Belt:** A plot showing predicted vs. observed default rates with a confidence ribbon.
*   **ROC Curve:** A plot showing the ROC curve with the AUC value annotated.
*   **Brier Score Heatmap:** A heatmap showing the Brier score by rating grade and year to identify any performance deterioration over time.
*   **PSI Bar Chart:** A bar chart showing PSI values by quarter to visualize portfolio drift.
*   **Traffic-Light Dashboard:** A grid of indicators (red, amber, green) showing the results of back-testing and PSI tests for the last eight quarters.

## 4. Additional Notes or Instructions

*   **Assumptions:**
    *   The provided datasets are in the specified format and contain the necessary information.
    *   External agency default rate benchmarks are available and compatible with the internal rating grades.
    *   Macroeconomic data is readily available from the specified sources.
*   **Constraints:**
    *   The lab should focus on validating existing PD models; retraining of the models is not within the scope.
    *   The visualizations should be clear, informative, and easy to interpret.
    *   Validation thresholds need to be customisable (i.e set in a global variable)
*   **Customization:**
    *   The notebook should be designed to allow users to easily modify input data paths, model parameters, and reporting options.
    *   Traffic light thresholds for the monitoring dashboard should be customisable.

