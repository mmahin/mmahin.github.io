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
Here we are predicting the effect of air temperature on the time that the ”lesser snow geese” leave their overnight roost sites to fly to their feeding areas. The prediction and plots are predicting time from the temperature. Data is available with the folder. 

### Regression Plot

From the regression plot it is apparent that there is a positive correlation between the air temperature and the departure time. With the increase of temperature, the departure time increases. There is an apparent linear relation between the air temperature and the departure time. 

```r 
my_data <- read.delim("Geese.txt",header = TRUE, sep = " ")

plot(my_data$temp, my_data$time,col = "blue",main = "The effect of air temperature on the time  Regression",
     abline(lm(my_data$time ~ my_data$temp)), cex = 1.3, pch = 16,
     xlab = "Tempareture",
     ylab = "Time")

```

![Regression Plot](https://github.com/mmahin/mmahin.github.io/blob/9953c336a5c6dcf746498084bc1e49cda28edd20/_posts/stats/Rigression%20models%20and%20correlation%20analysis/000003.png)

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
![Residuals](https://github.com/mmahin/mmahin.github.io/blob/5dd2eeef66d506a612bb53ad6dc7c38ef0362e6f/_posts/stats/Rigression%20models%20and%20correlation%20analysis/000004.png)
