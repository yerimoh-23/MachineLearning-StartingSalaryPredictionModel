# Report
- May 7, 2024
- Eonbi Choi, Orion Cheung, Yerim Oh

## Title
Predicting Average Starting Salary Based on College Characteristics

## Abstract
Attending university is considered a major stepping stone to achieving a high-paying career and future. In this day and age, without the proper education, goals of achieving a successful career are nearly impossible. As college students, we often worry about our future and entrance into the workforce. It would be helpful if we were able to predict our early post undergraduate job salary. How much of the school you attend affects your starting salary? If we are able to predict average starting salary based on college characteristics, how does that affect the institution we attend and possibly the field in which we major in. Although there are a multitude of factors that ultimately play a role in starting career salary, by using a multivariate regression model and decision tree regression analysis, we aim to gain a brief understanding of the average early career pay one can expect depending on their university. 

## Introduction/Problem Statement
Our topic is “Predicting Average Starting Salary Based on College Characteristics” using Machine Learning. By analyzing multiple variables such as university tuition, enrollment, and location we aim to predict college students starting salaries.  We chose this topic due to its prevalence to us as current college students. As we enter the workforce, getting an estimate of our starting salary would be beneficial and helpful. University is often seen as a step stool for furthering one's career. Does the college you attend really have an impact on your career and starting salary? By exploring this question and topic, we can form a relationship between the characteristics of a student’s academic institution and their predicted career income. 

## Literature Review
We are interested in studying the relationship between the college one attends and their estimated career starting salary. There have been previous studies and research into the relationship between higher education and career earnings. “Does It Pay to Attend an Elite Private College? Cross-Cohort Evidence on the Effects of College Type on Earnings” is a research conducted by Dominic J. Brewer, Eric R. Eide, and Ronald G. Ehrenberg. Using data of model high school student’s choice of college type and estimate of net costs of attendance to determine the effects of college quality on wages and earnings and how this effect varies across time. Their research concluded that typically students that attended private institutions are more likely to have a higher career salary. “College Quality and Future Earnings: Where Should You Send Your Child to College?” conducted by Estelle James, Nabeel Alsalam, Joseph C. Conaty, and Duc-Le To. Their conclusion states that attending a private east-coast college has the most positive effect on future earnings. Additionally, having a higher GPA is a better investment. They claim that “what matters most is not which college you attend but what you do while you are there” is the most beneficial takeaway. Along with these two studies, the US government has also done some research into education level and lifetime earnings. Hubert Hu utilizes machine learning in a model to predict engineering graduate salaries. While some studies and models related to our topic exist, our specific model aims to predict early career salary using characteristics of college such as: type of institution, student demographic, location, etc. 

## Datasets
To create a model predicting the starting salary of students attending college/university in the U.S., we utilized data from the College tuition, diversity, and pay dataset available on Kaggle. This dataset comprises five CSV files: diversity_school, salary_potential, tuition_cost, tuition_income, and historical_tuition. 

- https://www.kaggle.com/datasets/jessemostipak/college-tuition-diversity-and-pay?select=diversity_school.csv

The ‘diversity_school’ table presents the race, ethnicity, and gender of students at colleges/universities in the fall of 2014. ‘Salary_potential’ displays potential alumni salaries by colleges based on median salaries for alumni with 0-5 years of experience. The ‘tuition_cost’ table provides sticker prices for the 2020-21 academic year, including school type, degree length, state, in-state vs out-of-state tuition. The ‘tuition_income’ table contains similar information to ‘tuition_cost’, but it details net costs—average tuition actually paid after scholarships/awards—based on three different income levels.

__tuition_income.csv__: 209,012 rows × 7 columns
- All Variables: name, state, total_price, year, campus, net_cost, income_lvl
- Study Variables:
  - `net_cost`:  true tuition after the scholarship or financial aid

__tuition_cost.csv__: 1,861 rows × 10 columns
- All Variables: name, state, state_code, type, degree_length, room_and_board, in_state_tuition, in_state_total, out_of_state_tuition, out_of_state_total
- Study Variables:
  - `type`: types of college; public/private
  - `in_state_total`: tuition of in-state students
  - `out_of_state_tuition`: tuition of out-of-state students

__salary_potential.csv__: 902 rows × 7 columns
- All Variables: name, state_name, early_career_pay, mid_career_pay, make_world_better_percent, stem_percent
- Study Variables:
  - `early_career_pay`: starting salary
  - `stem_percent`: proportion of students who are majoring in STEM field

__diversity_school.csv__: 50,656 rows × 5 columns
- All Variables: name, total_enrollment, state, category (gender, race), enrollment
- Study Variables: 1941 rows × 11 columns
  - `total_enrollment`: total number of students enrolled in each university
  - `category`: ‘Women’ and ‘Non-Resident Foreign’ for getting the proportion of women and international students in each college

We intend to merge __diversity_school__, __salary_potential__,  __tuition_cost__, and __tuition_income__ to predict students' starting salaries. We are particularly interested in features such as students’ race, ethnicity, and gender as well as state, school type, degree length, and tuition when constructing a model to forecast early-career salaries. To prevent overfitting, we will extract the most significant features among these variables by calculating correlation coefficients for all the variables. 

## Data Processing
1. __salary_potential.csv__: Select variables `name` , `early_career_pay`, and `stem_percent`
2. __diversity_school.csv__:
    - Select colleges with at least 2000 enrolled students. 
    - Since we want to know the women and international students' proportion, take rows that consist of the categories 'Women' and 'Non-Resident Foreign' from the ‘category’ column. Then, divide two categories with the total enrollment to get the proportion of women and international students. Make two new tables with college names and each proportion, then merge the new `women_prop_df` and `foreign_prop_df` to the original diversity dataset.
    - Drop all unused variables and drop duplicates, then the table only has 4 variables: `name` (for merging purposes), `total_enrollement`, `women_proportion`, and `Foreign_proportion`.
3. __tuition_cost.csv__: Select variables `name` , `type`, `in_state_total`, and `out_of_state_total`
4. __tuition_income.csv__: Select `net_cost`. Group by `name` and select the first values to drop duplicates.
5. Merge all processed datasets into one and name it __college__.

### Finalized dataset: college.csv
- 444 rows × 10 columns
- Variables: `name`, `total_enrollment`, `women_proportion`, `Foreign_proportion`, `stem_percent`, `net_cost`, `type`, `in_state_total`, `out_of_state_total`, `early_career_pay`

## Exploratory Data Analysis + Visualization
### Insightful Graphs Generated From The Original Datasets 
1. Pie chart of the average proportion of each demographic group ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Average%20Diversity%20Proportion.png "Graph1")
    * This pie chart depicts the average proportion of diversity among the institutions in our data.
      
2. Boxplot comparing the early career pay across different regions in the US ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Early%20Career%20Pay%20by%20Region.png "Graph2")
    * This boxplot tells us that among the four regions in the United States, the Northeast has the highest mean early career pay, whereas the South has the lowest average early career pay. 

### Plots Generated With The College Dataset
1. Bar plots of mean early career pay for public and private institutions ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Mean%20Early%20Career%20Pay%20by%20Institution%20Type.png "Graph3")
    * Comparison of the average early career pay between the two types of institutions. Between Private and Public institutions, Private institutions have a higher mean early career pay. This means that, on average, those who attend a private institution are expected to have a higher starting salary than those attending public institutions.
      
2. Relationship between the proportion of women in schools and early career pay ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Proportion%20of%20Women%20vs%20Early%20Career%20Pay.png "Graph4")
    * The plot shows that the data has a strong negative correlation, that schools with a higher proportion of women have lower early career pay.
      
3. Relationship between the number of total enrollment in schools and early career pay ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Total%20Enrollment%20vs%20Early%20Career%20Pay.png "Graph5")
    * The plot shows that the data has a weak positive correlation, that schools with larger enrollment tend to have slightly higher early career pay.
      
4. Relationship between the tuition of in-state and out-of-state students in schools and early career pay ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/In%3AOut%20of%20State%20Total%20vs%20Early%20Career%20Pay.png "Graph6")
    * The plot shows that the data has a strong positive correlation, that schools with higher tuition of in and out-of-state students have higher early career pay.
      
5. Relationship between the proportion of international students in schools and early career pay ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Proportion%20of%20Internationals%20vs%20Early%20Career%20Pay.png "Graph7")
    * The plot shows that the data has a moderate positive correlation, that schools with higher tuition of in and out-of-state students have higher early career pay.
      
6. Relationship between the proportion of students in STEM fields and early career pay ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/STEM%20Percentage%20vs%20Early%20Career%20Pay.png "Graph8")
    * The plot shows that the data has a strong positive correlation, that schools with higher tuition of in and out-of-state students have higher early career pay.
      
7. Relationship between the net cost of attendance in schools and early career pay ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Net%20Cost%20vs%20Early%20Career%20Pay.png "Graph9")
    * The plot shows that the data has a weak positive correlation, that schools with higher net costs tend to have slightly higher early career pay
      
8. Pair plot of variables of independent and response variables ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Pair%20plot%20of%20variables%20of%20independent%20and%20response%20variables.png "Graph10")

## Model Parameters & Methodologies
Our initial variables of interest for estimating starting salary were:
1. `total_enrollemnt`
2. `women_proportion`
3. `foreign_proprotion`
4. `Stem_percent`: percent of the student body in stem
5. `Net_cost`:  average cost of attendance after scholarship/financial aid
6. `In_state_total`: average total cost (room & board + in-state tuition) for in-state residents in USD
7. `Out_of_state_total`: average total cost  (room & board + in-state tuition) for out-of-state residents in USD
8. `type`: public, private

These models were commonly seen in other studies mentioned in the literature review. Type of institution and net costs of attendance were the two parameters mentioned the most in other similar studies. Our variables of interest are overall consistent with other research and models studying the relationship between the college attended and career earnings.

Referring to the covariance matrix, to reduce overfitting in predictive models and increase predictive power, we selected 6 variables with higher correlations to the predictor variables.
  * Covariance Matrix with all 8 variables ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Covariance%20Matrix%20with%20all%208%20variables.png "Image1")
  * Covariance Matrix with 7 variables      ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Covariance%20Matrix%20with%207%20variables%20(excludes%20total_enrollment).png "Image2")
    * excludes total_enrollment

This covariance matrix suggests that the net_cost and total_enrollment have weak positive correlations to early_career_pay. As a result, these two variables were not selected.

The final independent variables that we use in our model are `total_enrollemnt`, `women_proportion`, `foreign_proprotion`, `Stem_percent`, `Net_cost`, `In_state_total`, `Out_of_state_total`, and `type`. The response variable is `early_carrer_pay` — estimated average early career pay in USD.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/FinalData%20Head.png "Image3")

### Ridge Regression Model
Before fitting the model, the categorical parameter ‘type’ was converted into indicator variables.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/convert%20type.png "Image4")

An array of 30 alpha values that are logarithmically spaced between 10-5  and 103 were generated. These values are used to tune the model.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/alpha%20values.png "Image5")

The variables were standardized to ensure they were on a consistent scale. The dataset was split into a training set and a test set. The training set was used to fit a ridge regression model and the test set was used to evaluate the model fitted with the training set. 

### Multivariate Linear Regression
The multivariate linear regression model evaluates the relationship between multiple dependent variables and a single predictor variable. One primary advantage of this model is its capability of capturing the complexity of real-world problems. By considering multiple independent parameters, we can accommodate more factors affecting the dependent variable. Consequently,  this helps minimize errors and biases in estimation. Similarly, the multivariate linear regression model estimates the average starting salary of graduates based on the independent variables listed at the beginning of this section. 

### Decision Tree Regression Model
The regression tree model is a variation of the decision tree model that predicts a continuous target variable. The regression tree model is more appropriate for this dataset because the outcome variable, early_career_pay, is a continuous numeric variable. We chose to fit the regression tree as it is advantageous in capturing both linear and non-linear relationships within our dataset. Our data consists of numerical and categorical data, the decision tree regression model can handle both. It is also easily interpretable and allows us to produce clear visualizations.

For the regression tree model, the categorical variable, type, is converted into a numerical format using the label encoding technique. This creates a binary representation of the 'type' variable where 'public' institutions are represented by 1 and ‘private’ are represented by 0. This encoding allows the model to work with the 'type' variable more effectively.

The data is split into subsets by selecting the best feature to split by to minimize the variance of the target variable (Early Career Pay) in each subset. Then, the data is recursively split, creating the tree structure where each internal node represents a decision based on a feature, and each leaf node contains a predicted value. Lastly, the predicted value is given by taking the average of the target values in the leaf node. Additionally, when training the regression tree model, setting a seed is necessary for retrieving consistent results. 

To fit the model, the dataset was split into training and testing subsets, allocating 80% for training and 20% for testing. Then, the model was trained using the training set, and its performance was evaluated on the testing set. The model was trained with varying levels of complexity. For each model, the mean squared error (MSE) was calculated. The MSE values tell how well our model performs at different levels of complexity.

## Results and Evaluation
* Code Repository: https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/final_code.ipynb

### Ridge Regression Model
The train and test scores were plotted across alpha values.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/TrTe%20Ridge%20Regression%20Model.png "Graph11")

According to the training score and test score plot, the ridge regression model performs better with very small alpha values. It suggests that the model is performing better with minimal regulation and that the multicollinearity in the dataset is low; the dataset is not very complex, and it is not unstable.

### Multivariate Linear Regression Model
The mean squared error (MSE) calculated for this model is approximately 13,155,129.58 and $$R^2$$ approximately equal to 0.80. While the MSE might appear large, it's not unexpected given the range of the response variable, early_career_pay, which spans from $32,500 to $86,300.  The multivariate linear regression model accounts for a large portion of the variance observed in the data, suggesting a relatively strong relationship between the predictors and starting salary.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Multivariate%20Linear%20Regression%20MSE%20R2.png "Image6")

### Regression Tree Model
A pairplot with each plot comparing 'Early Career Pay' against the independent variables while distinguishing between different types of colleges (public, private)
![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/pairplots%20against%20independent%20variables.png "Graph12")

The plot shows mean squared error (MSE) values for regression trees trained on both training and testing data across different maximum depths, ranging from 1 to 20. The error visualization helps in identifying the optimal max depth, where the model balances performance and generalization.
![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Training%20and%20Testing%20Errors%20Across%20Model%20Complexity.png "Graph13")

It can be observed from the graph that the MSE for the testing error of the regression tree with a maximum depth of 4 is the smallest. That is, the optimal regression tree for forecasting average early career pay is a model with a maximum of four levels of decision nodes. The MSE and r-score for our regression tree model are approximately 22,763,805.714 and 0.5875, respectively.

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Regression%20Tree%20Model%20MSE%20R2.png "Image7")

## Decision Tree Regression Model
A variation of Decision Tree Model to predict a continuous target variable.

Steps:
1. Splitting: selects the best feature to split the whole dataset into subsets
     - split is chosen to minimize the variance of the target variable in each subset
2. Recursive Process: repeat splitting recursively, creating a tree structure
     - each internal node of the tree represents a decision based on a feature
     - each leaf node contains a predicted value
3. Prediction: predicted value is the average of the target values in the leaf node

__Advantages__
- capture both linear and non-linear relationships in data
- interpretable and can be visualized
- handle both numerical and categorical data

__Disadvantages__
- easily overfit the training data, leading to poor generalization
- sensitive to small variations in the data
- can become complex and deep, making them difficult to interpret

### Final Decision Tree Regression Model
![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/Final%20Decision%20Tree%20Regression%20Model.png "Image8")

The mean squared error (MSE) of the multivariate linear regression model is smaller than that of the regression tree model. This indicates that the multivariate linear model performs better than the regression tree in predicting the starting salary of graduates based on the parameters.

One factor to consider when using this model is that it is easy to overfit the training data, which will lead to overall poor generalization. It is also sensitive to small variations in the data; because of this, the model can become quite complex and deep, making it difficult to interpret our results. 

### Prediction of the Early Salary of MHC Graduate
__Mount Holyoke College__
- Total annual cost: $86,702	
- International students proportion: 23%
- Women proportion: > 90%
- STEM percent: 40%

![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Images/MHC%20Prediction.png "Image9")

__Predicted Salary: $55,720__

__Predicted Salary: $52,736__

## Biases and Ethical Issues
One of the primary ethical concerns in collecting and analyzing education data is managing the privacy and confidentiality of student information. Our model heavily relies on information and data about students attending university in the United States, private data may be shared and used within our model. 

Additionally, some colleges and universities may choose to share more information and data, which could create unintentional biases within our datasets. For example, if a particular institution has higher-earning starting career salaries on average, they may be more willing to release more information and data. The size of the institution may also add some biases. Schools with larger enrollment may have a larger amount of students earning a high salary, but a smaller school may have a higher percentage of students that earn a high salary. 

## Limitations & Future Work
One concern we anticipate is merging data from different years, as the data in the ‘diversity_school’ table is from 2014, whereas the data from the ‘tuition_cost’ table pertains to the 2020-21 academic year. While we have access to tuition data from 2014, filling in this data for each school would need to be done manually. 

Also, one of the variables we have used to predict is the proportion of international students in each college. Since the dataset only collects graduates who work in the United States, it does not contain the students who are working in other countries. Many international students might go back to their countries, and some domestic students would also work in other countries, which is a limitation in collecting the data of all the graduates from the colleges. Since all members of our team are international students, the information collected from the international students of the colleges would be more helpful for us to predict our starting salary.
