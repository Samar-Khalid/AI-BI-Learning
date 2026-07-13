# Startups Profit Prediction

## Objective
Build a multiple linear regression model to predict startup profits based on spending patterns.

## Dataset
- Features: R&D Spend, Administration, Marketing Spend, State
- Target: Profit
- Samples: 50

## Steps

### 1. Load Data
```python
import pandas as pd

df = pd.read_csv('50_Startups.csv')
df.head()
```

### 2. Explore Data
```python
df.info()
df.describe()
df['State'].value_counts()
```

### 3. Preprocess
```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder

# Encode State
le = LabelEncoder()
df['State'] = le.fit_transform(df['State'])

X = df.drop('Profit', axis=1)
y = df['Profit']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

### 4. Train Model
```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)
```

### 5. Evaluate
```python
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np

y_pred = model.predict(X_test)
print(f"RMSE: {np.sqrt(mean_squared_error(y_test, y_pred)):.2f}")
print(f"R2 Score: {r2_score(y_test, y_pred):.2f}")
```

### 6. Interpret Coefficients
```python
coefficients = pd.DataFrame({
    'Feature': X.columns,
    'Coefficient': model.coef_
})
print(coefficients)
```

## Expected Results
- R2 Score: ~0.90-0.95
- Most important feature: R&D Spend

## Exercises
1. Remove less important features
2. Try Ridge/Lasso regression
3. Add polynomial features
4. Visualize feature importance
