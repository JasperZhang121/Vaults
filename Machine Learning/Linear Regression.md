
In [statistics](https://en.wikipedia.org/wiki/Statistics "Statistics"), **linear regression** is a [linear](https://en.wikipedia.org/wiki/Linearity "Linearity") approach for modelling the relationship between a [scalar](https://en.wikipedia.org/wiki/Scalar_(mathematics) "Scalar (mathematics)") response and one or more explanatory variables (also known as [dependent and independent variables](https://en.wikipedia.org/wiki/Dependent_and_independent_variables "Dependent and independent variables")). The case of one explanatory variable is called _[simple linear regression](https://en.wikipedia.org/wiki/Simple_linear_regression "Simple linear regression")_; for more than one, the process is called **multiple linear regression**.


Examples:

```python

# import and prepare
import pandas as pd
data=pd.read_excel('example.xlsx')
from sklearn.linear_model import LinearRegression

# create linear regression model
lr_model = LinearRegression()

# set training set
x = np.array(x)
y = np.array(y)
x = x.reshape(-1,1)
y = y.reshape(-1,1)

# train 
lr_model.fit(x,y)

# y_predict
y_predi = lr_model.predict(x)
```




