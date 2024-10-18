# Anomaly Detection Methodologies in Python

Anomaly detection involves identifying data points that deviate significantly from the majority of a dataset. This document summarizes common anomaly detection methods available in Python, along with sample code snippets for implementation.

---

## Table of Contents

- [1. Statistical Methods](#1-statistical-methods)
  - [1.1. Z-Score Method](#11-z-score-method)
  - [1.2. Interquartile Range (IQR) Method](#12-interquartile-range-iqr-method)
- [2. Distance-Based Methods](#2-distance-based-methods)
  - [2.1. K-Nearest Neighbors (KNN)](#21-k-nearest-neighbors-knn)
  - [2.2. Local Outlier Factor (LOF)](#22-local-outlier-factor-lof)
- [3. Clustering-Based Methods](#3-clustering-based-methods)
  - [3.1. DBSCAN](#31-dbscan)
  - [3.2. Isolation Forest](#32-isolation-forest)
- [4. Machine Learning Methods](#4-machine-learning-methods)
  - [4.1. One-Class SVM](#41-one-class-svm)
  - [4.2. Autoencoders](#42-autoencoders)
- [5. Time-Series Specific Methods](#5-time-series-specific-methods)
  - [5.1. ARIMA Models](#51-arima-models)
  - [5.2. Facebook Prophet](#52-facebook-prophet)
- [6. Probabilistic Models](#6-probabilistic-models)
  - [6.1. Gaussian Mixture Models (GMM)](#61-gaussian-mixture-models-gmm)
- [7. Ensemble Methods](#7-ensemble-methods)

---

## 1. Statistical Methods

### 1.1. Z-Score Method

- **Description**: Calculates the standard score to measure how many standard deviations a data point is from the mean.
- **Use Case**: Effective for normally distributed data.

```python
import numpy as np
from scipy import stats

data = np.random.randn(100)
z_scores = np.abs(stats.zscore(data))
threshold = 3
anomalies = np.where(z_scores > threshold)
```

### 12-interquartile-range-iqr-method

- Description: Uses the IQR to detect outliers by defining thresholds based on quartiles.
- Use Case: Suitable for skewed distributions.

```python
import numpy as np

data = np.random.randn(100)
Q1, Q3 = np.percentile(data, [25, 75])
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 _ IQR
upper_bound = Q3 + 1.5 _ IQR
anomalies = np.where((data < lower_bound) | (data > upper_bound))
```

### 21-k-nearest-neighbors-knn

- Description: Identifies anomalies based on the distance to the k-th nearest neighbor.
- Use Case: Effective when anomalies are isolated points.

```python
from sklearn.neighbors import NearestNeighbors
import numpy as np

data = np.random.randn(100, 2)
neigh = NearestNeighbors(n*neighbors=5)
neigh.fit(data)
distances, * = neigh.kneighbors(data)
anomaly_scores = distances.mean(axis=1)
```

### 22-local-outlier-factor-lof

- Description: Measures the local density deviation of a data point compared to its neighbors.
- Use Case: Suitable for datasets with varying density.

```python
from sklearn.neighbors import LocalOutlierFactor

data = np.random.randn(100, 2)
lof = LocalOutlierFactor(n_neighbors=20)
predictions = lof.fit_predict(data)
anomalies = data[predictions == -1]
```

### 31-dbscan

- Description: Clusters data points based on density, marking outliers as noise.
- Use Case: Effective when anomalies do not belong to any cluster.

```python
from sklearn.cluster import DBSCAN

data = np.random.randn(100, 2)
dbscan = DBSCAN(eps=0.5, min_samples=5)
clusters = dbscan.fit_predict(data)
anomalies = data[clusters == -1]
```

### 32-isolation-forest

- Description: Uses random forests to isolate anomalies with fewer splits.
- Use Case: Handles high-dimensional data well.

```python
from sklearn.ensemble import IsolationForest

data = np.random.randn(100, 2)
iso_forest = IsolationForest(contamination=0.1)
iso_forest.fit(data)
predictions = iso_forest.predict(data)
anomalies = data[predictions == -1]
```

### 41-one-class-svm

- Description: Learns the boundary of normal data in feature space.
- Use Case: Effective when only normal data is available.

```python
from sklearn.svm import OneClassSVM

data = np.random.randn(100, 2)
oc_svm = OneClassSVM(nu=0.1, kernel="rbf", gamma=0.1)
oc_svm.fit(data)
predictions = oc_svm.predict(data)
anomalies = data[predictions == -1]
```

### 42-autoencoders

- Description: Neural networks trained to reconstruct input data; high reconstruction errors indicate anomalies.
- Use Case: Suitable for complex, high-dimensional data.

```python
import numpy as np
from tensorflow.keras.models import Model
from tensorflow.keras.layers import Input, Dense

data = np.random.randn(1000, 20)
input_dim = data.shape[1]
encoding_dim = 10

input_layer = Input(shape=(input_dim,))
encoder = Dense(encoding_dim, activation='relu')(input_layer)
decoder = Dense(input_dim, activation='linear')(encoder)
autoencoder = Model(inputs=input_layer, outputs=decoder)

autoencoder.compile(optimizer='adam', loss='mse')
autoencoder.fit(data, data, epochs=50, batch_size=32, shuffle=True)

reconstructions = autoencoder.predict(data)
mse = np.mean(np.power(data - reconstructions, 2), axis=1)
threshold = np.percentile(mse, 95)
anomalies = data[mse > threshold]
```

### 51-arima-models

- Description: Models time-series data to detect anomalies based on forecasting errors.
- Use Case: Suitable for temporal data with patterns.

```python
import pandas as pd
import statsmodels.api as sm
import numpy as np

data = pd.Series(np.random.randn(100))
model = sm.tsa.ARIMA(data, order=(1, 1, 1))
results = model.fit()
residuals = results.resid
anomalies = residuals[np.abs(residuals) > 3 * residuals.std()]
```

### 52-facebook-prophet

- Description: Forecasting tool that models seasonality and trend components.
- Use Case: Identifies anomalies as deviations from predicted trends.

```python
from prophet import Prophet
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'ds': pd.date_range(start='2020-01-01', periods=100, freq='D'),
    'y': np.random.randn(100)
})
model = Prophet()
model.fit(df)
forecast = model.predict(df)
df['anomaly'] = df['y'] - forecast['yhat']
anomalies = df[np.abs(df['anomaly']) > 3 * df['anomaly'].std()]
```

### 61-gaussian-mixture-models-gmm

- Description: Models data as a mixture of multiple Gaussian distributions.
- Use Case: Detects anomalies based on low probability under the model.

```python
from sklearn.mixture import GaussianMixture

data = np.random.randn(100, 2)
gmm = GaussianMixture(n_components=2)
gmm.fit(data)
scores = gmm.score_samples(data)
threshold = np.percentile(scores, 5)
anomalies = data[scores < threshold]
```

### 7-ensemble-methods

- Description: Combines multiple algorithms to improve detection accuracy.
- Use Case: Useful when no single method is sufficient.

```python
from pyod.models.knn import KNN
from pyod.models.iforest import IForest
from pyod.models.auto_encoder import AutoEncoder
from pyod.utils.data import generate_data, evaluate_print

X_train, y_train, X_test, y_test = generate_data(n_train=200, n_test=100)
classifiers = {
    'KNN': KNN(),
    'Isolation Forest': IForest(),
    'AutoEncoder': AutoEncoder()
}

for clf_name, clf in classifiers.items():
    clf.fit(X_train)
    y_pred = clf.predict(X_test)
    evaluate_print(clf_name, y_test, y_pred)
```
