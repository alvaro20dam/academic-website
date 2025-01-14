---
title: Linearregression
date: '2025-01-14'
---
```python
import numpy as np

X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)
```

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

plt.scatter(X,y)
```

    <matplotlib.collections.PathCollection at 0x2a90b29b5f0>

    
![png](output_1_1.png)
    

```python
X_b = np.c_[np.ones((100, 1)), X] # add x0 = 1 to each instance

theta_best = np.linalg.inv(X_b.T.dot(X_b)).dot(X_b.T).dot(y)
```

```python
theta_best
```

    array([[4.00204923],
           [3.04779344]])

```python
X_new = np.array([[0], [2]])
X_new_b = np.c_[np.ones((2, 1)), X_new] # add x0 = 1 to each instance
y_predict = X_new_b.dot(theta_best)
y_predict
```

    array([[ 4.00204923],
           [10.0976361 ]])

```python
plt.plot(X_new, y_predict, "r-")
plt.plot(X, y, "b.")
plt.axis([0, 2, 0, 15])
plt.show()
```

    
![png](output_5_0.png)
    

```python
from sklearn.linear_model import LinearRegression

lin_reg = LinearRegression()
lin_reg.fit(X, y)
lin_reg.intercept_, lin_reg.coef_
```

    (array([4.00204923]), array([[3.04779344]]))

```python
lin_reg.predict(X_new)
```

    array([[ 4.00204923],
           [10.0976361 ]])

```python
theta_best_svd, residuals, rank, s = np.linalg.lstsq(X_b, y, rcond=1e-6)
theta_best_svd
```

    array([[4.00204923],
           [3.04779344]])

```python
np.linalg.pinv(X_b).dot(y)
```

    array([[4.00204923],
           [3.04779344]])

```python
n_epochs = 50
t0, t1 = 5, 50 # learning schedule hyperparameters
m = len(X_b)

def learning_schedule(t):
    return t0 / (t + t1)
    
theta = np.random.randn(2,1) # random initialization

for epoch in range(n_epochs):
    for i in range(m):
        random_index = np.random.randint(m)
        xi = X_b[random_index:random_index+1]
        yi = y[random_index:random_index+1]
        gradients = 2 * xi.T.dot(xi.dot(theta) - yi)
        eta = learning_schedule(epoch * m + i)
        theta = theta - eta * gradients
```

```python
theta
```

    array([[4.01101645],
           [3.08194449]])

