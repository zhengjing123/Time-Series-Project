

# Time Series Project - Modeling the temperature in Melbourne, Australia from 1971 to 1990

Author: Zheng Jing (Rstudio)<br />


## Question: 

Given the temperatre in Melbourne from 1971 to 1990, analyze the pattern (trend & seasonality) of the time series data; <br />
Also analyze the anomaly point, and make predictions for 1991 temperature. <br>



## Variable selection:

### Data source:

 <https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx>

Here we use survey data in the 2015 - 2016 cycle.

### Response variable: 

| Variable | Description                   | Data Type        | Dataset   |
| -------- | ----------------------------- | ---------------- | --------- |
| DIQ010   | Doctor told you have diabetes | Categorical. 0/1 | DIQ_I.XPT |

### Predictor variables:

| Variable  | Description                                         |         Data Type         | Dataset      |
| --------- | --------------------------------------------------- | :-----------------------: | ------------ |
| RIAGENDER | Gender(1: male;  2: female)                         |     Categorical. 1/2      | DEMO_I.XPT   |
| RIDAGEYR  | Age in years at screening                           | Positive Integer. [18,80] | DEMO_I.XPT   |
| ALQ130    | Average alcoholic drinks/day  in the past 12 months | Positive Integer. [0,15]  | ALQ_I.XPT    |
| DR1TSUGR  | Total sugar (gm)                                    |  Double. [0.33, 533.44]   | DR1TOT_I.XPT |
| DR1TTFAT  | Total fat (gm)                                      |  Double. [3.54, 498.63]   | DR1TOT_I.XPT |
| OCQ260    | Description of job/work situation                   |    Categorical. 1/2/3     | OCQ_I.XPT    |
| SLD012    | Sleep hours                                         |    Double. [3.0, 2.0]     | SLQ_I.XPT    |



## Method Introduction:

In statistics, the logistic model is used to model the probability of a certain class, such as pass/fail, win/lose, alive/dead or healthy/sick. <br />Here, our response variable is binary (diabetes or not). In ordinary linear model, the response variable range from negative infinite to positive infinite. Therefore, in order to  model a binary dependent variable, we need to use a logistic function as a link function: <br />

<div align="center"><a href="https://www.codecogs.com/eqnedit.php?latex=\\log(\frac{p}{1-p})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\\log(\frac{p}{1-p})" title="\\log(\frac{p}{1-p})" /></a></div>

Then we can use logistic regression to fit our model: <br />

<div align="center"><a href="https://www.codecogs.com/eqnedit.php?latex=\\log(\frac{p}{1-p})&space;=\eta&space;=&space;\beta_0&plus;\sum_{i=1}^{n}\beta_iX_{i}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\\log(\frac{p}{1-p})&space;=\eta&space;=&space;\beta_0&plus;\sum_{i=1}^{n}\beta_iX_{i}" title="\\log(\frac{p}{1-p}) =\eta = \beta_0+\sum_{i=1}^{n}\beta_iX_{i}" /></a></div>

Use maximum likelihood approach to find parameters that maximize the likelihood of the data:  <br />

<div align="center"><a href="https://www.codecogs.com/eqnedit.php?latex=\ell(\beta)&space;=&space;\sum_{i=1}^{n}[y_i({x_i}^\intercal\beta)-n_i\log(1&plus;exp({x_i}^\intercal\beta))]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\ell(\beta)&space;=&space;\sum_{i=1}^{n}[y_i({x_i}^\intercal\beta)-n_i\log(1&plus;exp({x_i}^\intercal\beta))]" title="\ell(\beta) = \sum_{i=1}^{n}[y_i({x_i}^\intercal\beta)-n_i\log(1+exp({x_i}^\intercal\beta))]" /></a></div>

Then we can get the probability of success (here, success means being told you have diabetes):  <br />

<div align="center"><a href="https://www.codecogs.com/eqnedit.php?latex=p&space;=&space;P(Y&space;=&space;1)&space;=&space;\frac{e^{\beta_0&space;&plus;&space;\sum_{i=1}^{n}\beta_iX_{i}}}{&space;1&space;&plus;&space;e^{\beta_0&space;&plus;&space;\sum_{i=1}^{n}\beta_iX_{i}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?p&space;=&space;P(Y&space;=&space;1)&space;=&space;\frac{e^{\beta_0&space;&plus;&space;\sum_{i=1}^{n}\beta_iX_{i}}}{&space;1&space;&plus;&space;e^{\beta_0&space;&plus;&space;\sum_{i=1}^{n}\beta_iX_{i}}}" title="p = P(Y = 1) = \frac{e^{\beta_0 + \sum_{i=1}^{n}\beta_iX_{i}}}{ 1 + e^{\beta_0 + \sum_{i=1}^{n}\beta_iX_{i}}}" /></a></div>

In our project, to get a basic idea about relationship between the response and some predictors, we first create a few contingency tables for each categorical predictor and the response.<br />

Then we fit a logistic regression using diabetes as the response variable and all the others as predictors, without any interaction terms or transformations. <br />



## File description:

Our group uses three kinds of tools to carry out the core analysis: R, python and Stata.

1. data_cleaned.csv: a csv file shows our final dataset after cleaning and it would be used for analysis.
2. 506project.R file: a R script uses data.table to clean data and use "stats" package and glm function to fit model. 
3. 506project.do file: a Stata script uses Stata to clean data and fit model. 
4. 506project.ipynb file: a jupyter notebook uses python to clean data and fit model. 
5. python_output.html file: the output of 506project.ipynb file.
6. project_report.Rmd file: a Rmarkdown file includes data describing, code of three tools and result analysis. This file is our final group project report.
7. project_report.html file: a html file by knitting the project_report.Rmd file to better present our group project.



## Outline：

1. Data cleaning:

   (1) Select variables from multiple datasets mentioned above and merge them together.

   (2) Remove missing values and unqualified values:

   ​	a) remove all missing values

   ​	b) recode diabetes from (2,1) to (0,1)

   ​	c) drop alcohol which is 777 or 999

   ​	d) reduce occupation from 5 levels to 3 levels and transform it into a factor type

   ​	e) center age, sleep hours, total sugar and total fat 

2. Model establishing:

   (1) In R and Python, we create a few contingency tables for each categorical predictor and the response. In this way, we can get some basic idea about their relationship.

   (2) Then we fit a logistic regression using diabetes as the response variable and all the others as predictors, without any interaction terms or transformations. 

3. Model interpretation: 

   Get result from different tools, interpret our model and answer our question.

4. Other things can be improved:

   (1) Variable selection: There may be other variables in other datasets that may have stronger associations with the prevalence of diabetes. Due to the limited time, we were not able to explore as many datasets/variables as we wanted.

   (2) Model selection: In addition to the logistic regression, there may exist other models that also fit the data well such as lasso or ridge regression. If possible, we could try these models.

   (3) We can improve the model performance by adding interaction terms (two-way or three-way).

   (4) Our goal was to explore the data and do the inference. But beyond that we are also interested in prediction. For doing so we can split dataset into training and testing sets and use MSE to evaluate our model.


## Group Collaboration:
Although we have been working in different programming environments, we peer reviewed each other's script and we had some group meeting to discuss some problems. Here are some points:

1.  In Stata, we first incorrectly used the individual food dataset as the total nutrient data, which would result in multiple rows with the same SEQN.

2.  In R, we first recoded the diabetes from (1,2) to (1,0), which would lead to the wrong sign of the coefficients in the logistic output. 

3.  We forgot to center the continuous variable, which would make interpretation of intercepts difficult. In logistic regression, we always need to center predictors. 

4.  In python, we forgot to remove the meaningless values such as "999" and ”777" for alcohol. 

5.  In python, the categorical variables need to be manually added to the design matrix before running the logistic regression. 

6.  The old alcohol variable is not appropriate (ALQ110). Therefore we use "ALQ130" instead and it represents the average alcoholic drink per day. 



## References:

1. Faraway, Julian J. *Extending the linear model with R: generalized linear, mixed effects and nonparametric regression models*. Chapman and Hall/CRC, 2016.
2. [American Diabetes Association](https://www.diabetes.org/), 1995 - 2019.

