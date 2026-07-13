# Car Price Prediction

## Objective
Build a regression model to predict car prices based on various features.

## Dataset
- Features: year, selling_price, km_driven, fuel, seller_type, transmission, owner
- Target: selling_price
- Samples: ~300

## Steps

### 1. Load Data
```python
import pandas as pd

df = pd.read_csv('car data.csv')
df.head()
```

### 2. Explore Data
```python
df.info()
df.describe()
df['Fuel_Type'].value_counts()
```

### 3. Preprocess
```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder

# Encode categorical variables
le = LabelEncoder()
df['Fuel_Type'] = le.fit_transform(df['Fuel_Type'])
df['Seller_Type'] = le.fit_transform(df['Seller_Type'])
df['Transmission'] = le.fit_transform(df['Transmission'])

X = df.drop('Selling_Price', axis=1)
y = df['Selling_Price']

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

## Expected Results
- R2 Score: ~0.85-0.90
- RMSE: varies

## Exercises
1. Try polynomial features
2. Use Random Forest Regressor
3. Perform feature engineering
4. Visualize predictions vs actual
