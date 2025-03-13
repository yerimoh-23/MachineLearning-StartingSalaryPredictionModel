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

`tuition_income.csv`: 209,012 rows × 7 columns
- All Variables: name, state, total_price, year, campus, net_cost, income_lvl
- Study Variables:
  - net_cost:  true tuition after the scholarship or financial aid

tuition_cost.csv
1,861 rows × 10 columns
All Variables: name, state, state_code, type, degree_length, room_and_board, in_state_tuition, in_state_total, out_of_state_tuition, out_of_state_total
Study Variables:
type: types of college; public/private
in_state_total: tuition of in-state students
out_of_state_tuition: tuition of out-of-state students

salary_potential.csv
902 rows × 7 columns
All Variables: name, state_name, early_career_pay, mid_career_pay, make_world_better_percent, stem_percent
Study Variables:
early_career_pay: starting salary
stem_percent: proportion of students who are majoring in STEM field

diversity_school.csv
50,656 rows × 5 columns
All Variables: name, total_enrollment, state, category (gender, race), enrollment
Study Variables: 1941 rows × 11 columns
total_enrollment: total number of students enrolled in each university
category: ‘Women’ and ‘Non-Resident Foreign’ for getting the proportion of women and international students in each college
We intend to merge ‘diversity_school’, ‘salary_potential’,  ‘tuition_cost’, and ‘tuition_income’ to predict students' starting salaries. We are particularly interested in features such as students’ race, ethnicity, and gender as well as state, school type, degree length, and tuition when constructing a model to forecast early-career salaries. To prevent overfitting, we will extract the most significant features among these variables by calculating correlation coefficients for all the variables. 
