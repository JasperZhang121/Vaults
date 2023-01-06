
Examples:

```python

# import and prepare
import pandas as pd
data=pd.read_excel('example')
data.head()
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

