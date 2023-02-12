This project aims to advise homeowners on how home renovations could increase the appraised value of their homes The purpose of this statistical analysis is to help us understand
the relationship between the house's features and how these conditions are used to predict the price of the home.

Data Cleaning
I read the data into a pandas dataframe to start cleaning it. I drop unneeded features: waterfront, view, date, zip code, floors, long, sqft_basement.

Data Analysis

With the all data cleaned, I started to analyze it. Scatter plots of each variable against the target, and price, show which variables have apparent linear relationships. This also makes it more explicit which variables are categorical and which are continuous.
Histograms show which variables are normally distributed. I will have to register and transform those that are not normally distributed.

Train-Test

At this point, I create the train-test split using the sklearn method. I keep the 70% train and 30% test default and set a random state for repeatability.

Based on the Scatter plots, I decided to analyze the following variables:
sqft_above, sqft_living, grade, sqft_living15, bathrooms, bedrooms, sqft_lot, yr_built, yr_renovated.
At this point, I create the train-test split using the sklearn method. I keep the 70% train and 30% test default and set a random state for repeatability. I did the Regression Evaluation Metrics, comparing these metrics: Mean Average Error (MAE), the Mean Squared Error (MSE) and the Root Mean Squared Error (RMSE).

Ordinary Least Squares

With this, I can create the baseline model using statsmodel. Based on my summary model, I can interpret the data:
Adj. R-squared is how well the regression model explains observed data. The Adj. R-squared reveals that the regression model explains 61% of the variability observed in the target variable.
The Prob (F-statistic): tells the overall significance of the regression. This is to assess the significance level of all the variables together, unlike the t-statistic that measures it for individual variables. The null hypothesis under this is "all the regression coefficients are equal to zero". Prob(F-statistics) depicts the probability of null hypothesis is true. As per the above results, the probability is close to zero. This implies that, overall, the regression is meaningful. The p-value associated with the F-statistic <0.05: then, at least one independent variable is related to Y.

One of the assumptions of OLS is that the errors are normally distributed. An Omnibus test is performed in order to check this. Here, the null hypothesis is that the errors are normally distributed. Prob(Omnibus) is supposed to be close to 1 to satisfy the OLS assumption. In this case, Prob(Omnibus) is 0.000, which implies that the OLS assumption is not satisfied.

Jarque-Bera test is in line with the Omnibus test. It is also performed for the distribution analysis of the regression errors. It is supposed to agree with the results of the Omnibus test. A large value of Jarque-Bera test indicates that the errors are not normally distributed. In this case, the large J-B value of J-B suggests that errors are not normally distributed.

The p-value for all variables is 0.0 for eight variables and 0.011 for the sqft_above, which means that all the data provide enough evidence to reject the null hypothesis for the entire population. Changes in the independent variable are associated with changes in the dependent variable at the population level. These variables are statistically significant.

Sample quantiles characterize the Normal Q-Q plot of a sample from a right-skewed distribution at the high (right) end, being more positive than the Normal expected quantiles. The quantiles at the low (left) end are often less negative than the expected normal quantiles. The consequence is a concave pattern.
Notice the points form a curve instead of a straight line. The plots that look like this mean that the data are right-skewed.

Homoscedasticity

Homoscedasticity of variances is an assumption of equal or similar variances in different groups being compared. The observations in the plot are randomly scattered about the horizontal zero line such that the level of the scatter is roughly the same about this line as you move from the left to the right along the line, which would indicate that the data do not violate the linearity and homoscedasticity assumptions.

Categorical Variables

From the Scatter plot, I found the categorical and converted them because categorical independent variables cannot be directly entered into a multiple regression. Instead, they need to be converted into dummies variables.

After converting the categorical variables to dummies, I did another OLS using statsmodel.
The Adj. R-squared reveals that the regression model explains 57% of the variability observed in the target variable.

The Prob (F-statistic): 0.00. This means a low statistically significant result.
Prob(Omnibus) is supposed to be close to 1 to satisfy the OLS assumption. In this case, Prob(Omnibus) is 0.000, which implies that the OLS assumption is not satisfied.

In this case, the Jarque-Bera test shows that the sample data do not have a normal distribution.

The p-value is 0.0 for all the variables. I did the log transformations. The relationship between sqft_living and price is not linear, like the relationship between grade and price doesn't look linear.

Correlation

Then I started investigating the correlation between the variables and created a heatmap plot. A heat map is a two-dimensional data representation in which colours represent values. The varying intensity of colour represents the measure of correlation. Each square shows the correlation between the variables on each axis. Correlation ranges from -1 to +1. Values closer to zero mean no linear trend between the two variables. The close to 1 the correlation is, the more positively correlated they are; that is, as one increases, so does the other, and the closer to 1, the stronger this relationship is. A correlation closer to -1 is similar, but instead of both increasing, one variable will decrease as the other increases. In my analyses, I found a significant correlation between sqft_living and sqft_above; grade and sqft_living; sqft_living, sqft_living15 and sqft_above, grade.

Logarithmic function

I did a simple linear regression model, first using sqft_living as independent variable and then applying a log transformation to sqft_living by adding a column to our dataset called disp_log and seeing if using this column as our independent variable improves the model.
But the relationship doesn't look linear, and the Adj. R-squared value didn't improve; I then transformed the dependent variable, price, to improve the model and make the residuals more normal.
The Adj. R-squared value increased significantly to 0.483, was 0.374, and the QQ-plot is also improved. 
I also repeated the same steps for the variable grade and, even in this case, the Adj. R-squared value has increased a lot, going from 0.403 to 0.495.
From the graphs is possible to see that there is a strong correlation between the model's predictions and its actual results.

MinMaxScaler

After that, I did the Feature Scaling using Normalization and Standardization. I checked the correlation between sqft_living and grade with price and found a positive correlation between them.
I transformed the data by standardizing it using fit_transform method. I used a plot and scatter plot to see the effects of standardization more clearly. To normalize features, I used the MinMaxScaler class. In the end it’s possible to see the strong positive correlation between both of theme with the price with the feature.

Pipeline

Then I used Pipeline, which allows running several steps that are executed sequentially. The output of one step is passed to the next step, and so on. All the intermediate steps except the last are called transformers, and the last step is called an estimator.

Conclusion

Variables that are useful to determine house prices are sqft_above', 'sqft_living', 'grade', 'sqft_living15',bathrooms', 'bedrooms', 'sqft_lot', 'yr_built','yr_renovated.
The presence of these aspects or variables in a house will drive its price higher. The R-squared generated from the regression model is rather high, as 61% of these variables contribute to house prices. Based on results of the tests that have been carried out, this model is significant however there is room for improvement.
Especially sqft_living and grade seem to be significative with and high interaction with the price.
