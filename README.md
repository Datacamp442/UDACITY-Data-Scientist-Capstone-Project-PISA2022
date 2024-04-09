# Capstone Project PISA Study 2022 (Udacity - Data Scientist Nanodegree Program)
## by Data Camp 442


## Project Overview and Motivation

PISA (Program for International Student Assessment) is the largest international school performance study. Almost 700,000 students from 81 countries and regions took part in 2022. PISA does not ask for factual knowledge, but rather tests whether participants can apply their knowledge and combine information in a meaningful way - key skills for being successful in the information society of the 21st century. The PISA study takes place every three years and covers the areas of reading, mathematics and science (https://www.oecd.org/berlin/themen/pisa-studie/).


Since I already had the pleasure of examining the PISA study 2012 during my previous Udacity certification as a Data Analyst (PART_I_exploration_PISA_study_data442camp442.html, not published, submitted to UDACITY in 10/2022), I wanted to build on this and take a closer look at the current study from 2022. In particular, the focus of the 2012 PISA study was on visual data analysis. In this current analysis, I will re-run some of the visual analysis. Then I want to apply machine learning models and try to make predictions on math and reading scores.

I will focus my analysis on the survey of students from EU and EFTA countries. Due to the size of the original SAS file, only the data already reduced to the selected countries and necessary columns is used within this analysis.

## Problem Statement
My central questions are:

To what extent does the students' family background influence the students results in the PISA study?
Is it possible to predict the read and math score based on the students' family background?
This work examines the extent to which the math and read score result of the PISA study depends on

the level of education of the parents,
the presence of siblings at Home,
gender and
country (EU and EFTA countries only)
Finally, I would like to find out whether the read and math score can be predicted using the available data on the family situation.


## Content

pisa_short.csv -    a dataset that contains the PISA 2022 results of European Countries 

                    It has been shortened to the following columns: 
                    CNT, CNTSTUID, ST004D01T, ST230Q01JA, MISCED, FISCED, PV1MATH, PV1READ
                    
                    Original data: 
                    The OECD publishes the data from the Pisa studies on its website as SAS data file: 
                    https://www.oecd.org/pisa/data/2022database

20240409_Capstone_PISA_study_data442camp.ipynb - Jupyter Notebook used for the analysis and regression models

## Used Libaries
- numpy 
- pandas
- matplotlib.pyplot
- seaborn
- train_test_split, GridSearchCV from sklearn.model_selection
- RandomForestRegressor, GradientBoostingRegressor from sklearn.ensemble
- mean_squared_error from sklearn.metrics

## Blog post
Please also see my blog post on Medium:

https://medium.com/@datacamp442/pisa-2022-are-we-able-to-predict-the-scores-727673865789

## End- to- End Process Summary
First, we needed to to some data cleaning, like dropping for my analysis unnecessary columns, handling NaN's and converting values to descriptive text.

- Gender: 
Conversion of the float values ​​into a more descriptive text (female and male)

- Siblings:
Conversion of the the float values ​​into a more descriptive text (e.g. 1 = None, 2 = One)

- Education level parents: 
Definition of new variables in which the respective ISCED groups are assigned to the categories of low, medium, high and very high education.


Then, an initial analysis was carried out using data visualizations:

- Univariate Exploration
- Bivariate Exploration
- Multivariate Exploration

In order to be able to predict the reading or math score, we first had to to convert categorical data into dummy variables.

The first model of the RandomForestRegressor gave poor reading score results of the MAPE. This was particularly due to the fact that extreme values ​​were included in the results.

In the second experiment, we previously removed the extreme values ​​from the data. The forecasts of our RandomForestRegressor now became significantly better.

In the next step we used cross-validation to find the best parameters of the RandomForestRegressor and improved the model again.

Finally, we tried to achieve a further improvement with the GradientBoostingRegressor. Once again an improvement on the MAPE was achieved, but only a very small one.

We then displayed the feature importances of the optimal GradientBoostingRegressor model to confirm our assumptions from the univariate, bivariate and multivariate analysis.

## Summary of the results
### Math Score
The result of the math test is symmetrically normally distributed around the mean score of 478.3. Although the range of results in the math score is between 117 and 853, the interquartile range is relatively short (between 410 and 540). There are also outliers with very low scores around 120 and very good scores from highly gifted students around 850. No transformations had to be carried out.

### Reading Score
In contrast to the math score, the reading score appears somewhat left skewed around the mean score of 476.4, which is a little smaller than the median of 480.61. Interestingly, the reading score shows an even higher range of results achieved (40 to 883 score value). Here too, the interquartile range is quite short and there seems to be a large number of outliers: There are students who only achieve a reading score of 40, which means that young people at this level cannot properly understand and confirm the meaning of short, syntactically simple sentences on a literal level. On the other hand, there are students who master the test with almost perfect results of 883 points. No transformations had to be carried out.

### Gender
The gender entries are defined as float (1 for female and 2 for male). In order to be able to analyze the data more clearly, I converted the float values ​​into a more descriptive text (female and male). The number of male and female students is almost perfectly balanced at about 50% in each group.

### Country
Spain sends more than 30,000 students to take part at the PISA test. That is almost 5 times as many students as in the rest of Europe. The median here is 6,303 students per country. Only 3000 students from the microstate Malta took part. No transformations had to be carried out.

### Siblings
The entries of the count of siblings were defined as float, e.g. 1 for no siblings and 2 for one sibling. In order to be able to analyze the data more clearly, I convert the float values ​​into a more descriptive text (e.g. 1 = None, 2 = One) Less than a quarter of the students have no siblings, 43% have only one sibling also 43% of students grow up with two ore more siblings.

### Parents education
Most of the mothers and fathers of the students tested reached an high educational qualification like a Bachelor or Master degree. I decided to define new variables in which I assign educational categories based on the ISCED groups (low, medium, high and very high education). Thanks to the newly created data fields, you can now see at a glance that most mothers have received an high education level and that most fathers received an medium education level. If you compare the absolute numbers, mothers tend to be better educated than fathers. Compared to mothers, the number of fathers with unknown educational levels is higher. This could be due to the fact that children of single mothers have little or no contact with their father and therefore cannot or do not want to answer the question.

### Boys are better in maths than girls and girls have better reading skills.
For boys, the distribution of math test results is slightly skewed to the left, while the distribution for girls appears slightly skewed to the right. The distribution is clearly skewed to the left, while the distribution of boys appears to be skewed to the right.

### "Birds of a feather flock together" - This saying applies to the level of education of the individual parents.
Based on the matrix above, 55% of parents remain true to their level of education when finding a partner. This is particularly true for highly and very highly educated parents. About 18% of the mothers have children's fathers with a higher level of education, about 26% have children's fathers with a lower level. This is consistent with the above statement that mothers tend to be better educated than fathers.

### Students of uneducated parents tend to have lower scores.
In both categories, the results of students whose parents had low or no educational qualifications are significantly below average. In contrast, the mean mathematics and reading scores of students whose parents had a high or very high level of education are above average. Students of medium-educated parents score below the general average.

### Having a single sibling has a positive effect on the result. 
This applies to all genders and tests. The scores of only children are slightly worse in all categories than those of students who have one sibling. The results are clearly worse for children with more than three siblings.

### Reading and math score prediction

The best predictions can be made using the GradientBoostingRegressor. The average error in the prediction is 72 for the reading score (with a percentage error of 16,25%) and 65 for the math score (with a percentage error of 14,5%).

### Feature Importances of the regression model

Reading score is significantly influenced positively if the father has a very high level of education and the gender of the student is female, but also negatively if the student has more than three siblings and lives in Bulgaria.

The math score is significantly influenced positively if the father has a high or very high level of education, and negatively if the student has more than three siblings and lives in Bulgaria.
In contrast to the reading score, the gender of the students is not really taken into account when predicting the math score.

## Acknowledgements
Many thanks to Udacity for the impetus for this project and the OECD for making the data available to the public.
Further references can be found in the Jupyter Notebook.
