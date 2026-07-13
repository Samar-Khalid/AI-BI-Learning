# Breast Cancer Prediction

## Objective
Build a machine learning model to predict whether a breast cancer tumor is malignant or benign.

## Dataset
- Features: 30 numeric features (radius, texture, perimeter, area, etc.)
- Target: Diagnosis (M = Malignant, B = Benign)
- Samples: 569

## Steps

### 1. Load Data
```python
import pandas as pd
from sklearn.datasets import load_breast_cancer

data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target
```

### 2. Explore Data
```python
df.info()
df.describe()
df['target'].value_counts()
```

### 3. Preprocess
```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

X = df.drop('target', axis=1)
y = df['target']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### 4. Train Model
```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(max_iter=1000)
model.fit(X_train_scaled, y_train)
```

### 5. Evaluate
```python
from sklearn.metrics import accuracy_score, classification_report

y_pred = model.predict(X_test_scaled)
print(f"Accuracy: {accuracy_score(y_test, y_pred):.2f}")
print(classification_report(y_test, y_pred))
```

## Expected Results
- Accuracy: ~96-98%
- Precision: ~97%
- Recall: ~98%

## Exercises
1. Try different algorithms (Random Forest, SVM, KNN)
2. Perform feature selection
3. Tune hyperparameters
4. Create a confusion matrix visualization
