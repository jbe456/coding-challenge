# Rejection Reasons

This question was asked by: Affirm.

Source: [Interview Query](https://www.interviewquery.com/)

## Description

Suppose we have a binary classification model that classifies whether or not an applicant should be qualified to get a loan. Because we are a financial company we have to provide each rejected applicant with a reason why.

Given we don't have access to the feature weights, how would we give each rejected applicant a reason why they got rejected?

## Solution

[This notebook](affirm-rejection-reasons.ipynb) creates a binary classification model on loans to classify their status into two categories: approved or denied. Using [partial dependence plots](https://scikit-learn.org/stable/modules/generated/sklearn.inspection.plot_partial_dependence.html#sklearn-inspection-plot-partial-dependence), we can determine that the following features are affecting the outcome the most (since inside their domain, the average predicted outcome crosses 50%): `Credit_History`, `Self_Employed_Yes`.

Therefore for the below data, the lack of `Credit_History` (value equal to `0`) would be the main reason for rejection:

```python
ApplicantIncome            3180.0
CoapplicantIncome             0.0
LoanAmount                   71.0
Loan_Amount_Term            360.0
Credit_History                0.0
Gender_Female                 1.0
Gender_Male                   0.0
Married_No                    1.0
Married_Yes                   0.0
Dependents_0                  1.0
Dependents_1                  0.0
Dependents_2                  0.0
Dependents_3+                 0.0
Education_Graduate            1.0
Education_Not Graduate        0.0
Self_Employed_No              1.0
Self_Employed_Yes             0.0
Property_Area_Rural           0.0
Property_Area_Semiurban       0.0
Property_Area_Urban           1.0
Name: 396, dtype: float64
```
