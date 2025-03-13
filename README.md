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

### Data Processing
1. __salary_potential.csv__: Select variables `name` , `early_career_pay`, and `stem_percent`
2. __diversity_school.csv__:
    - Select colleges with at least 2000 enrolled students. 
    - Since we want to know the women and international students' proportion, take rows that consist of the categories 'Women' and 'Non-Resident Foreign' from the ‘category’ column. Then, divide two categories with the total enrollment to get the proportion of women and international students. Make two new tables with college names and each proportion, then merge the new `women_prop_df` and `foreign_prop_df` to the original diversity dataset.
    - Drop all unused variables and drop duplicates, then the table only has 4 variables: `name` (for merging purposes), `total_enrollement`, `women_proportion`, and `Foreign_proportion`.
3. __tuition_cost.csv__: Select variables `name` , `type`, `in_state_total`, and `out_of_state_total`
4. __tuition_income.csv__: Select `net_cost`. Group by `name` and select the first values to drop duplicates.
5. Merge all processed datasets into one and name it __college__.

#### Finalized dataset: college.csv
- 444 rows × 10 columns
- Variables: `name`, `total_enrollment`, `women_proportion`, `Foreign_proportion`, `stem_percent`, `net_cost`, `type`, `in_state_total`, `out_of_state_total`, `early_career_pay`

### Exploratory Data Analysis + Visualization
#### Insightful Graphs Generated From The Original Datasets 
1. Pie chart of the average proportion of each demographic group ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Average%20Diversity%20Proportion.png "Graph1")
    * This pie chart depicts the average proportion of diversity among the institutions in our data. 
2. Boxplot comparing the early career pay across different regions in the US ![alt text](https://github.com/yerimoh-23/MachineLearning-StartingSalaryPredictionModel/blob/main/Graphs/Early%20Career%20Pay%20by%20Region.png "Graph2")
    * This boxplot tells us that among the four regions in the United States, the Northeast has the highest mean early career pay, whereas the South has the lowest average early career pay. 

__Plots Generated With The College Dataset__
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

### Model Parameters & Methodologies
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
![alt text]( "Graph")
This covariance matrix suggests that the net_cost and total_enrollment have weak positive correlations to early_career_pay. As a result, these two variables were not selected.

The final independent variables that we use in our model are `total_enrollemnt`, `women_proportion`, `foreign_proprotion`, `Stem_percent`, `Net_cost`, `In_state_total`, `Out_of_state_total`, and `type`. The response variable is `early_carrer_pay` — estimated average early career pay in USD.
