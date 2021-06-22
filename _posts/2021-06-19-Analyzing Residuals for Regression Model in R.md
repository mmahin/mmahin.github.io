## Rigression models and correlation analysis

Regression analysis is a famous tool to find relation between a dependent variable and one or more independent variables. In any experiemnt dependent variables are dependent on the values of the independent variables. 

Based on the dependent and the independent variable we can derive a regression equation:

Y = β<sub>0</sub> + β<sub>1</sub>.X

Here, Y is our dependent variable, which depends on the independent variable X.   The intercept, β<sub>0</sub> is the value when independent variable X is 0. β<sub>1</sub> is the slope which indicantes the average change of values in Y for change in X. The slope can give us some indication about the correlation between the two variables. For example if the slope rises with increase of values of X, there is positive correlation, if it goes downhill, there is negative correlation. 

## Example

```
One aspect of wildlife science is the study of how various habits of wildlife are
affected by environmental conditions. In this exercise, we are concerned about
the effect of air temperature on the time that the "lesser snow geese" leave their
overnight roost sites to fly to their feeding areas. The data is given in Geese.txt.
1. Obtain the necessary plots.
2. Compute the LM coefficients for the ’time vs temp’ model (β0 , β1).
3. Obtain the regression equation.
4. Obtain the confidence intervals for β1.
5. Is there correlation between the temperature and time in the Geese data set?
```

### Initial
Here we are predicting the effect of air temperature on the time that the ”lesser snow geese” leave their overnight roost sites to fly to their feeding areas. The prediction and plots are predicting time from the temperature. Data is available with the folder. Dataset available here: https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/Geese.txt

### Regression Plot

From the regression plot it is apparent that there is a positive correlation between the air temperature and the departure time. With the increase of temperature, the departure time increases. There is an apparent linear relation between the air temperature and the departure time. 
Required packages:

library(tidyverse)
library(caret)
library(broom)
library(ggplot2)

```r 
my_data <- read.delim("Geese.txt",header = TRUE, sep = " ")

plot(my_data$temp, my_data$time,col = "blue",main = "The effect of air temperature on the time  Regression",
     abline(lm(my_data$time ~ my_data$temp)), cex = 1.3, pch = 16,
     xlab = "Tempareture",
     ylab = "Time")

```

![Regression Plot](https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/000003.png)

## Judging the Regression Plot 

Not all data sets are well suited for regression. We can always find if a data set is well suited for the regression or not by evaluting the quality of the residuals. Residuals are errors. Generally, when we draw regression lines within data points, most of the points do not fall on the line. Some data points fall above the line and some below the line. These errors are known as the residuals. So, residual is,

Residual = Observed value – predicted value

Following figure shows the residuals,
```r 
model <- lm(time ~ temp, data = my_data)

model.diag.metrics <- augment(model)
ggplot(model.diag.metrics, aes(temp, time )) +
    geom_point() +
    stat_smooth(method = lm, se = FALSE) +
    geom_segment(aes(xend = temp , yend = .fitted), 
                 color = "red", size = 0.3)

```
![Residuals](https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/000004.png)

To judge a data set for regression, we need to judge mainly three things:

1. Heteroscedasticity : Points at widely varying distances from the line 
2. Non-liniarity
3. Outliers 

Lets see a few plots useful for that.

### Residual plot

A residual plot has the Residual Values on the vertical axis; the horizontal axis displays the independent variable. Following figure shows a residual plot:
```r 
plot(model,1)
```
![Residual Plot](https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/000005.png)

In a good residual plot, data points need to be homegenously scattered both side of the regression line. It should not show any pattern. It also shows if there are outliers. From the residual plot if we see high curvature, we can assume the points are not well separeble using linear line. In our above plot, we can see the residuals are having little variability and curvature, indicating their might be a little non-linearity in the plot. As the points seems almost spread evenly two side of the zero, so it does not seem to be huge issue.  Also, from the plot, it shows several outleiers at the middle and end of the plot [sample 30, 21 and 4].  

### Standard Residual vs Fitted Plot

It is a standard version of rsidual plot. It explains the same things as the residual plot except they are standardized and non-negative. This plot is good for seeing equal variance or homoscedasticity. We do not want any pattern for our red line here. Pattern or slope with red line means problem. Following figure shows a plot:

![Standard Residual Plot](https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/000006.png)

From the Standard Residual vs Fitted Plot, we can verify the little lon-linearity and outliers discovered using  Residual vs Fitted Plot. Here we can see, the variability increases a little on the middle, but comparatively large at the end. Here we expected our red line to be flat, but it is not the case here. So, it shows a little trend at the end. So the errors are non-constant, which proves the presence of heteroscedasticity. Another words our dependent variable requires transformation for accurate prediction.


### Normal Q-Q plot

Normal Q-Q plot is another plot to understand the normality of the risiduals. If residuals are normally distributed, they fall on the straight line. It is important to check for this normality if we want to do any kind of testing like significance test that assumes normal distribution of the data. In those places, we want residuals are at least close to normal or should not deviates too much from the normal. If the residual deviates too much from the normal, we should go for regression and testings that do not assumes normality like robust regression . Normal Q-Q plot for our data given below:

```r 
plot(model, 2)
```
From the Normal Q-Q Plot of residuals we can see the residuals are not fully normally distributed. There is a little deviation from the normality at the beginning and end of the plot.  We can also check normality using tests like Shapiro-Wilk, Anderson-Darling and Jarque-Bera tests, which will give us some quantative values. 

![Q-Q Plot](https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/000007.png)

### Standard Residual vs Leverage Plot and Cook Distance

Standard Residual vs Leverage Plot is a good tool to check the data points those are extreame and influencial at the same time. High values does not mean a data point has strong influence on the regression. Using Standard Residual vs Leverage Plot with Cook distance we can easily identify the significant outlier points. Lets go with our example:
```{r }
par(mfrow = c(1, 2))
# Cook's distance
plot(model, 4)
# Residuals vs Leverage
plot(model, 5)
```
![Q-Q Plot](https://github.com/mmahin/mmahin.github.io/blob/5e0c62a5336c59f6d6038e1e851d3bc276fbfa9c/_includes/000008.png)
From the cook distance we can see the most influential points for our model. We can see sample 4,12 and 21 are most influential. But there are some more influential points present. 

From the Standard Residual vs Leverage Plot, we can see the leverage of sample quite spread. That means the samples themselves do not have quite a dense zone. Also the influence of points are also quite spread. And some sample like 4 has high leverage and high influence. 

## Regression Equation 

Finally we can get our regression equation using the following way:

### Coefficients
```r
coefs = coefficients(model)
coefs[1]
coefs[2]
```

### Prediction Equation
```r
temp = 30
time = coefs[1] + coefs[2]*temp
```

### Confidence Interval for any Prediction
```r
newdata = data.frame(temp = 30) 
predict(model, newdata, interval="confidence")

```
