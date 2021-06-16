# Handling categorical values

When we are working with rigression models we need numerical values. So we need to convert the numeric values into some kind of number form. One method is one hot encoding. 
In this method for every value in a categorical variable a new variable is created. The rows in the previous variable, where the value was present it will set 1 or else it will be 0. 
One problem of this method is, if we have too many categories or too many categorical variables, number of variables will increase drastically, and our model will suffer from curse of
dimntionality. One exmple is given below:

```python
# Create the dummis
dummies = pd.get_dummies(data['variable name'])

# Add the dummies to old dataframe
data = data.join(dummies)

# Remove the old variable. We already have the variable in form of dummies
X = data.drop(['variable name'],axis =1).values
```
