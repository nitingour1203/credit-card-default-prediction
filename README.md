# credit-card-default-prediction
                              
                              
                              
![image](https://user-images.githubusercontent.com/48796009/226186452-a9212d01-315b-4ac6-a14a-e4b37dfd8613.png)

# About Dataset
# *Dataset Information

This dataset contains information on default payments, demographic factors, credit data, history of payment, and bill statements of credit card clients in Taiwan from April 2005 to September 2005.




# *Inspiration
 
 
 Some ideas for exploration:
 
 1.-How does the probability of default payment vary by categories of different demographic variables?
 2.-Which variables are the strongest predictors of default payment?

# Key Findings from EDA

1.  Males have more delayed payment than females in this dataset. Keep in mind that this finding only applies to this dataset, it does not imply this is true for other datasets.

2.  Customers with higher education have less default payments and higher credit limits.

3.  Customers aged between 30-50 have the lowest delayed payment rate, while younger groups (20-30) and older groups (50-70) all have higher delayed payment rates. However, the delayed rate drops slightly again in customers older than 70.

4.  There appears to be no correlation between default payment and marital status.

5.  Customers being inactive doesn’t mean they have no default risk. We found 317 out of 870 inactive customers who had no consumption in 6 months then defaulted next month.

# Model Comparison

In these 3 models, Logistic Regression model has the highest recall but the lowest precision, if the firm expects high recall, then this model is the best candidate. If the balance of recall and precision is the most important metric, then Random Forest is the ideal model. Since Random Forest has slightly lower recall but much higher precision than Logistic Regression, we recommend the Random Forest model.
![image](https://user-images.githubusercontent.com/48796009/226187197-d85456d3-01e2-486b-a75a-b98166cf7fa4.png)

# Recommendations Based on Modeling

Below is our suggested recall plot. Note the threshold can be adjusted to reach higher recall.
![image](https://user-images.githubusercontent.com/48796009/226187258-f588aeb4-bf57-418d-b775-aee6d899767d.png)

# Data acquisition

Load the data (stored in excel format) into Rstudio working environment. Ensure that you have set up the working directory to the path where your dataset file is located.

dat = read_excel(‘default of credit card clients.xls’, sheet=”Data”, range = “B2:Y30002”)

str(dat)

summary(dat)

print(paste(“Is there any missing value? “,sum(apply(dat,2, function(x) any(is.na(x))))))

![image](https://user-images.githubusercontent.com/48796009/226187477-00e8686f-0e94-4341-83ae-e3b02d0a32df.png)

# Histogram

![image](https://user-images.githubusercontent.com/48796009/226187564-a4267cf7-4bb7-4657-bd9f-450448c03a9f.png)

the distribution of predictors skewed to the right (heavy tails on the right end with most data points lying on the left side). This further verify the previous inference that continuous predictors are positively skewed.

# Correlation matrix

![image](https://user-images.githubusercontent.com/48796009/226187705-9cb91b74-eb6d-4dba-bb53-a137c53e6f17.png)

Amount of bill statement from April to September in the year 2005 are highly (linear) correlated.

# Categorical features

Contingency table

Contingency table is a good way to show the counts of particular combination of two categorical variables. The following is the code snippet to generate contingency 
table between education and default variables.
![image](https://user-images.githubusercontent.com/48796009/226187764-c05d54cc-928e-4a20-aba9-898ec4d948bf.png)

It is shown that for samples in ‘others’ and ‘unknown’ categories under ‘EDUCATION’ predictor, only around 7% of them default, in contrast to 19% and more for other categories under ‘EDUCATION’. You can visualize the relationship between other categorical features and target by changing feature name EDUCATION .

# Chi-square test of independence

Chi-square test is a nonparametric test to determine statistical independence between 2 categorical variables. The null hypothesis for chi-square test is the two variables are independent. Chi-square test compares the cell counts (frequencies) in the contingency table to the expected counts provided that the null hypothesis is true. Please refer to this statistic blog for more details.

The built-in chisq.test function is a convenient way to compute chi-square statistic and to identify the categorical predictors that are associated to ‘default’ variable.
![image](https://user-images.githubusercontent.com/48796009/226187818-aef40b7f-e47a-4ac4-a463-029b6eb86d17.png)
SEX, EDUCATION, MARRIAGE are some predictors that are associated with credit card default. To visualize the actual count and expected count, grouped bar plots can be constructed as below.

![image](https://user-images.githubusercontent.com/48796009/226187856-4e5445d2-d6bf-49e6-a2de-82413a6bfa1d.png)

                       Grouped bar plots for education background.
                       
![image](https://user-images.githubusercontent.com/48796009/226187887-71c9d4b9-12e7-4c22-9571-7d1428b7048e.png)

                       Grouped bar plots for marital status.
                       
![image](https://user-images.githubusercontent.com/48796009/226187910-b1184028-a856-4e2f-90b2-b75353b6098a.png)

                       Grouped bar plot for gender.
                       
we have 2 data instances that differ in terms of education background, marital status and gender individually while other variables are the same, then we can safely infer that: male or married or credit card holder with university level education is more likely to default. In other words, we cannot make this kind of argument: male credit card with university level education is more likely to default as we haven’t investigate the interaction between SEX and EDUCATION and its effect towards the response, default. What we can claim is that male (compared to female) is more likely to default, provided others explanatory variables is the same.
                       
# Summary of findings
Data visualization and exploration is not only an integral step before subsequent predictive modeling, it is also an imperative tool to make data more understandable.

Thanks to data visualization, we managed to gain some important insights before progressing towards modeling stage. There are some pitfalls that might cause issues in modeling and are worth to highlight:

1. All the continuous features are positively skewed.

2. Amount of bill statements for previous months (‘BILL_AMT1’, … , ‘BILL_AMT6’) are strongly linear correlated.

3. The class distribution is imbalanced, with higher proportion of non-default clients. Thus, accuracy might not be appropriate when it comes to model’s performance evaluation.






