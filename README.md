# Starting Salary Prediction Model
Predicting early-career salary using institutional characteristics and regression-based modeling

## Project Objective
This project analyzes how institutional characteristics influence early-career salary outcomes across U.S. colleges.

Using publicly available data on tuition, enrollment, institutional type, diversity metrics, and STEM concentration, I built multiple regression-based models to estimate average starting salary and compare model performance.

The goal was to:
- Identify key institutional drivers of salary outcomes
- Evaluate linear vs regularized vs tree-based models
- Assess predictive stability using train/test evaluation

## Datasets
Source: **College Tuition, Diversity, and Pay dataset** 
- This dataset comprises five CSV files: `diversity_school`, `salary_potential`, `tuition_cost`, and `tuition_income`.
- https://www.kaggle.com/datasets/jessemostipak/college-tuition-diversity-and-pay?select=diversity_school.csv

### Data Preparation
1. Select colleges with at least 2000 enrolled students.
2. Engineered proportion variables (women, international)
3. Standardized numerical features
4. Encoded categorical variables
5. Train/test split: 80% / 20%

After cleaning and merging four datasets:
- Final dataset: 444 institutions × 10 variables
- Target variable: early_career_pay

Variable Used:
- `name`: The name of each college
- `total_enrollment`: The number of total enrollment in schools
- `women_proportion`: The proportion of women in schools
- `Foreign_proportion`: The proportion of international students in schools
- `stem_percent`: The proportion of students majoring in STEM fields in schools
- `net_cost`: Average cost of attendance after scholarship/financial aid
- `type`: Type of schools (private, public)
- `in_state_total`: Average total cost (room & board + in-state tuition) for in-state residents in USD
- `out_of_state_total`: Average total cost  (room & board + in-state tuition) for out-of-state residents in USD
- `early_career_pay`: Starting salary in USD

## Exploratory Data Analysis (EDA)
Before model development, a structured exploratory analysis was conducted to understand variable distributions, relationships, and potential multicollinearity.

### Insightful Graphs Generated from the Original Datasets
1. Pie chart of the average proportion of each demographic group ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Average%20Diversity%20Proportion.png "Graph1")
    * This pie chart depicts the average proportion of diversity among the institutions in our data.
      
2. Boxplot comparing the early career pay across different regions in the US ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Early%20Career%20Pay%20by%20Region.png "Graph2")
    * This boxplot tells us that among the four regions in the United States, the Northeast has the highest mean early career pay, whereas the South has the lowest average early career pay. 

### Plots Generated with the Final Dataset

#### Target Variable Distribution
1. Institution type (public vs private):
   
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Mean%20Early%20Career%20Pay%20by%20Institution%20Type.png "Graph3")
   - Private institutions exhibit higher average starting salaries.
      
3. Relationship between the proportion of women in schools and early career pay
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Proportion%20of%20Women%20vs%20Early%20Career%20Pay.png "Graph4")
  - Gender proportion show a moderate negative correlation.
      
3. Relationship between the number of total enrollment in schools and early career pay
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Total%20Enrollment%20vs%20Early%20Career%20Pay.png "Graph5")
  - Enrollment size shows weak correlation with salary outcomes.
      
4. Relationship between the tuition-related variables and early career pay
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/In%3AOut%20of%20State%20Total%20vs%20Early%20Career%20Pay.png "Graph6")
  - Tuition-related variables show strong positive correlation with early-career pay.
  ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Net%20Cost%20vs%20Early%20Career%20Pay.png "Graph9")
  - Average cost of attendance after scholarship/financial aid has limited linear impact to early-career pay.
      
5. Relationship between the proportion of international students in schools and early career pay
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Proportion%20of%20Internationals%20vs%20Early%20Career%20Pay.png "Graph7")
  - International proportion show limited linear impact.
      
6. Relationship between the proportion of students in STEM fields and early career pay
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/STEM%20Percentage%20vs%20Early%20Career%20Pay.png "Graph8")
  - STEM concentration demonstrates moderate-to-strong positive association.

#### Pair plot of variables

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Pair%20plot%20of%20variables%20of%20independent%20and%20response%20variables.png "Graph10")

## Model Specification & Methodology

### Feature Selection

Initial candidate predictors included:
1. `total_enrollemnt`
2. `women_proportion`
3. `foreign_proprotion`
4. `Stem_percent`
5. `Net_cost`
6. `In_state_total`
7. `Out_of_state_total`
8. `type`

Referring to the covariance matrix, to reduce overfitting in predictive models and increase predictive power, we selected 6 variables with higher correlations to the predictor variables.
- Covariance Matrix with all 8 variables
  ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Covariance%20Matrix%20with%20all%208%20variables.png "Image1")
- `total_enrollment` and `net_cost` demonstrate weak correlation with salary outcomes.

**Final Feature Set**
The final predictors used in modeling:
- `women_proportion`
- `foreign_proportion`
- `stem_percent`
- `in_state_total`
- `out_of_state_total`
- `type`

Target variable:
- `early_career_pay`

## Modeling Approach
The dataset was split into training (80%) and testing (20%) subsets.

All numerical features were standardized prior to modeling.

Categorical variable `type` was encoded using indicator variables.
   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/pairplots%20against%20independent%20variables.png "Graph12")

Three regression-based models were implemented and compared.

### Model 1: Multivariate Linear Regression (Baseline Model)

Purpose:
- Establish interpretable benchmark model.
- Evaluate explanatory strength of institutional variables.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Multivariate_Linear_Regression.png)

Results:
- Test MSE ≈ 13.2M
- Test R² ≈ 0.80

**Interpretation**:
The linear model explains approximately 80% of variance in early-career salary, indicating strong structural relationships between institutional characteristics and salary outcomes.

### Model 2: Ridge Regression

Purpose:
- Address potential multicollinearity among tuition variables.
- Improve model stability via L2 regularization.

Method:
1. 30 alpha values logarithmically spaced from 10⁻⁵ to 10³.
2. Model performance evaluated across regularization strengths.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Ridge_regression.png)

Findings:
- Optimal performance observed at very small alpha values.
- Regularization did not significantly improve performance over OLS.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/TrTe%20Ridge%20Regression%20Model.png "Graph11")

**Interpretation**:
Multicollinearity exists but does not materially degrade predictive performance.

### Model 3: Decision Tree Regression

Purpose:
- Capture potential non-linear relationships.
- Compare interpretability vs performance trade-off.

Model Selection:
- Tree depth evaluated from 1–20.

   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Training%20and%20Testing%20Errors%20Across%20Model%20Complexity.png "Graph13")

   ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/R_square.png)

Results:
- Test MSE ≈ 22.8M
- Test R² ≈ 0.59

Optimal depth: 4 (based on MSE and R²)

**Interpretation**:
Although the tree model captures non-linear splits, it did not outperform linear approaches. This suggests that early career salary is primarily explained by relatively stable structural relationships rather than complex non-linear interactions.

#### Final Decision Tree Regression Model
![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Final%20Decision%20Tree%20Regression%20Model.png "Image8")

#### Practical Application
predicted early-career salary for **Mount Holyoke College**:
- Total annual cost: $86,702	
- International students proportion: 23%
- Women proportion: > 90%
- STEM percent: 40%

  ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/MHC%20Prediction.png "Image9")

- Predicted Salary: $55,720
- Actual Salary: $52,736

### Model Comparison

| Model             | Test R²        | Test MSE             | Performance  |
| ----------------- | -------------- | -------------------- | ------------ |
| Linear Regression | ~0.80          | ~13.2M               | Best overall |
| Ridge Regression  | Similar to OLS | Slightly more stable | Comparable   |
| Regression Tree   | ~0.59          | ~22.8M               | Weaker       |

#### Final Model Selection: Multiple Linear Regression
- Highest predictive accuracy
- Strong interpretability
- Stable generalization

### Results and Evaluation
* Code Repository: https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/final_code.ipynb

## Limitations

- Dataset combines multiple years.
- Salary outcomes limited to U.S.-based employment.
- Institutional averages mask within-school variability.
- Potential omitted variable bias (e.g., selectivity, major-level granularity).

## Ethical Considerations
- Institutional-level data reduces individual privacy risk.
- Salary prediction models may reinforce socioeconomic stratification.
- Care must be taken not to interpret correlation as causation. 
