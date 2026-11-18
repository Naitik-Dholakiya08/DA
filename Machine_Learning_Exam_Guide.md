# Machine Learning Practical Examination Guide

## Complete Exam-Sheet Coverage

The attached topics sheet requires the following areas:

1. Identification of input features and target variables.
2. Data preparation for machine learning.
3. Training and testing dataset split.
4. Model training.
5. Prediction generation.
6. Model performance evaluation.
7. Classification and regression.
8. Decision Tree, Random Forest, Linear Regression, Logistic Regression, KNN and SVM.

This guide includes every topic below. Use the notebook sections with `# 1` through `# 13` for executable code.

Use this file to identify the command for a machine-learning question. The complete executable examples are in [machine_learning.ipynb](F:\Nikk Project\Charusat\Sem - 3\DA\File\Python\machine_learning.ipynb).

## 1. Imports

| Question | Command | Use |
|---|---|---|
| Import data tools | `import pandas as pd`, `import numpy as np` | Work with tables and numeric arrays. |
| Plot charts | `import matplotlib.pyplot as plt`, `import seaborn as sns` | Visualize data and results. |
| Split dataset | `from sklearn.model_selection import train_test_split, cross_val_score` | Create train/test data and cross-validation. |
| Scale/encode | `from sklearn.preprocessing import StandardScaler, OneHotEncoder` | Transform numeric and categorical features. |
| Build pipeline | `from sklearn.pipeline import Pipeline` | Connect preprocessing and model steps. |
| Apply columns differently | `from sklearn.compose import ColumnTransformer` | Scale numeric and encode text columns separately. |
| Fill missing values | `from sklearn.impute import SimpleImputer` | Apply median or most-frequent values. |

## 2. Load and Prepare Target

| Question | Command | Use |
|---|---|---|
| Read cleaned CSV | `df = pd.read_csv("../Output/clean_merged_movie_bookings.csv")` | Load the merged cleaned table. |
| Standardize target text | `df["Booking_Status"].str.strip().str.lower()` | Remove spaces and capitalization differences. |
| Keep required classes | `df = df[df["Booking_Status"].isin(["confirmed", "cancelled"])].copy()` | Remove unknown status values. |
| Convert target to 0/1 | `df["target"] = df["Booking_Status"].map({"cancelled":0, "confirmed":1})` | Prepare binary classification label. |
| Select numeric features | `df[numeric_features]` | Select numeric inputs. |
| Select categorical features | `df[categorical_features]` | Select text inputs. |
| Check class balance | `df["target"].value_counts()` | See whether classes are equal or imbalanced. |
| Check missing target | `df["target"].isna().sum()` | Find records whose target could not be mapped. |

## 3. Train-Test Split

| Question | Command | Use |
|---|---|---|
| Split data | `train_test_split(X, y, test_size=0.2, random_state=42)` | Use 80% for training and 20% for testing. |
| Stratified split | `train_test_split(X, y, test_size=0.2, stratify=y, random_state=42)` | Preserve class proportions in each split. |
| View split shape | `X_train.shape, X_test.shape` | Verify row counts. |
| Assign training variables | `X_train, X_test, y_train, y_test = ...` | Store split outputs. |

## 4. Preprocessing

| Question | Command | Use |
|---|---|---|
| Fill numeric missing values | `SimpleImputer(strategy="median")` | Replace missing numeric values with median. |
| Fill text missing values | `SimpleImputer(strategy="most_frequent")` | Replace missing text values with mode. |
| Scale numeric values | `StandardScaler()` | Transform numeric features to mean 0 and deviation 1. |
| Encode categories | `OneHotEncoder(handle_unknown="ignore", sparse_output=False)` | Convert categories to dense 0/1 columns. |
| Build numeric pipeline | `Pipeline([("imputer", SimpleImputer(strategy="median")), ("scaler", StandardScaler())])` | Impute then scale numbers. |
| Build text pipeline | `Pipeline([("imputer", SimpleImputer(strategy="most_frequent")), ("encoder", OneHotEncoder(...))])` | Impute then encode text. |
| Apply both pipelines | `ColumnTransformer([("numeric", numeric_transformer, numeric_features), ("categorical", categorical_transformer, categorical_features)])` | Process numeric and text columns differently. |

## 5. Classification

| Question | Command | Use |
|---|---|---|
| Logistic regression | `LogisticRegression(max_iter=1000)` | Linear probability classification. |
| Decision tree | `DecisionTreeClassifier(random_state=42, max_depth=6)` | Rule-based classification. |
| Random forest | `RandomForestClassifier(n_estimators=200, random_state=42)` | Ensemble of many trees. |
| KNN | `KNeighborsClassifier(n_neighbors=7)` | Classify by nearest training records. |
| Naive Bayes | `GaussianNB()` | Probabilistic classifier using Gaussian assumptions. |
| Connect preprocessing and model | `Pipeline([("preprocessing", preprocessor), ("model", model)])` | Prevent preprocessing mistakes and leakage. |
| Fit model | `pipeline.fit(X_train, y_train)` | Learn relationships from training data. |
| Predict class | `pipeline.predict(X_test)` | Predict test-row labels. |
| Predict probability | `pipeline.predict_proba(X_test)[:, 1]` | Get probability of positive/confirmed class. |

## 6. Classification Metrics

| Question | Command | Use |
|---|---|---|
| Accuracy | `accuracy_score(y_test, pred)` | Correct predictions divided by total predictions. |
| Precision | `precision_score(y_test, pred, zero_division=0)` | Correct positive predictions divided by predicted positives. |
| Recall | `recall_score(y_test, pred, zero_division=0)` | Actual positives correctly found. |
| F1 score | `f1_score(y_test, pred, zero_division=0)` | Balance precision and recall. |
| Full report | `classification_report(y_test, pred, target_names=["Cancelled","Confirmed"])` | Show precision, recall, F1 and support. |
| Confusion matrix | `confusion_matrix(y_test, pred)` | Show true/false positives and negatives. |
| ROC-AUC | `roc_auc_score(y_test, probabilities)` | Measure class ranking ability. |
| Cross-validation | `cross_val_score(pipeline, X, y, cv=5, scoring="f1")` | Evaluate using five folds. |

## 7. Confusion Matrix Visualization

| Question | Command | Use |
|---|---|---|
| Create heatmap | `sns.heatmap(cm, annot=True, fmt="d", cmap="Blues")` | Display confusion matrix clearly. |
| Set x-axis label | `plt.xlabel("Predicted")` | Label predicted classes. |
| Set y-axis label | `plt.ylabel("Actual")` | Label actual classes. |
| Set chart title | `plt.title("Confusion Matrix")` | Add title. |
| Save/fit chart | `plt.tight_layout(); plt.show()` | Fit and display chart. |

## 8. Regression

| Question | Command | Use |
|---|---|---|
| Select target | `y_reg = df["Final_Amount"]` | Continuous target for prediction. |
| Remove null target | `df.dropna(subset=["Final_Amount"])` | Ensure target has no missing values. |
| Linear regression | `LinearRegression()` | Fit straight-line relationship. |
| Ridge regression | `Ridge(alpha=1.0)` | Linear regression with L2 regularization. |
| Decision tree regression | `DecisionTreeRegressor(random_state=42, max_depth=6)` | Rule-based numeric prediction. |
| Random forest regression | `RandomForestRegressor(n_estimators=200, random_state=42)` | Ensemble numeric prediction. |
| Predict number | `pipeline.predict(Xr_test)` | Produce continuous predictions. |

## 9. Regression Metrics

| Question | Command | Use |
|---|---|---|
| MAE | `mean_absolute_error(y_test, pred)` | Average absolute prediction error. |
| MSE | `mean_squared_error(y_test, pred)` | Average squared prediction error. |
| RMSE | `np.sqrt(mean_squared_error(y_test, pred))` | Error in original target units. |
| R-squared | `r2_score(y_test, pred)` | Percentage of target variance explained. |
| Residual | `y_test - pred` | Difference between actual and predicted. |

## 10. Clustering

| Question | Command | Use |
|---|---|---|
| Choose cluster columns | `cluster_data = df[["Customer_Age","Tickets","Final_Amount"]]` | Select values for grouping. |
| Remove null rows | `dropna()` | Avoid invalid K-Means input. |
| Scale cluster inputs | `StandardScaler().fit_transform(cluster_data)` | Give columns comparable scale. |
| Create K-Means model | `KMeans(n_clusters=3, random_state=42, n_init=10)` | Create three customer clusters. |
| Fit and predict clusters | `kmeans.fit_predict(cluster_scaled)` | Assign each row to a cluster. |
| View cluster profiles | `cluster_data.groupby("Cluster").mean()` | Interpret cluster averages. |
| Elbow method | `km.inertia_` for several `k` values | Choose suitable number of clusters. |
| Plot elbow chart | `plt.plot(k_values, inertia, marker="o")` | Visualize inertia versus cluster count. |

## 11. PCA

| Question | Command | Use |
|---|---|---|
| Create PCA | `PCA(n_components=2)` | Reduce features to two components. |
| Transform data | `pca.fit_transform(cluster_scaled)` | Obtain PCA coordinates. |
| Plot components | `plt.scatter(components[:,0], components[:,1], c=labels)` | Visualize clusters in 2D. |
| Explained variance | `pca.explained_variance_ratio_` | Measure information retained by each component. |
| Total explained variance | `pca.explained_variance_ratio_.sum()` | Measure combined retained information. |

## 12. Feature Importance and Correlation

| Question | Command | Use |
|---|---|---|
| Obtain RF feature names | `rf_pipeline.named_steps["preprocessing"].get_feature_names_out()` | Get names after encoding. |
| Obtain importance values | `rf_model.feature_importances_` | Measure how much each feature is used. |
| Create importance table | `pd.Series(rf_model.feature_importances_, index=feature_names)` | Convert values to labeled Series. |
| Sort importance | `.sort_values(ascending=False)` | Highest importance first. |
| Plot importance | `.plot(kind="barh")` | Visualize important features. |
| Numeric correlation | `df.select_dtypes(include=np.number).corr()` | Find pairwise numeric relationships. |
| Correlation heatmap | `sns.heatmap(correlation, annot=True, cmap="coolwarm")` | Visualize correlation matrix. |

## 13. Save and Load Models

| Question | Command | Use |
|---|---|---|
| Import joblib | `import joblib` | Save/load Python objects. |
| Save model | `joblib.dump(model, "../Output/model.joblib")` | Save trained pipeline. |
| Load model | `model = joblib.load("../Output/model.joblib")` | Restore model later. |

## 14. Viva Answers and Question Matching

| Practical question | What to use |
|---|---|
| Predict Confirmed/Cancelled | Binary classification using Logistic Regression or Random Forest. |
| Predict Final Amount | Regression using Linear/Ridge/Random Forest Regressor. |
| Group customer types | K-Means clustering. |
| Reduce many features for visualization | PCA. |
| Compare classification models | Accuracy, precision, recall, F1 and confusion matrix. |
| Compare regression models | MAE, RMSE and R-squared. |
| Find important booking factors | Random Forest feature importance. |
| Check overfitting | Compare training/testing scores and use cross-validation. |
| Identify missing values | `isna().sum()` and `SimpleImputer`. |
| Encode movie genres/cities | `OneHotEncoder` or `get_dummies`. |
| Scale customer age/amount | `StandardScaler`. |

## 15. Quick Definitions

- **Supervised learning:** Training uses known target values.
- **Classification:** Predicts category such as Confirmed/Cancelled.
- **Regression:** Predicts a continuous value such as Final Amount.
- **Clustering:** Finds patterns/groups without a target.
- **Training set:** Data used to learn model parameters.
- **Test set:** Unseen data used to evaluate performance.
- **Overfitting:** Model learns training noise and performs poorly on new data.
- **Underfitting:** Model is too simple to learn patterns.
- **Data leakage:** Test or future information accidentally reaches training.
- **Cross-validation:** Repeated train/test evaluation on folds.
- **Precision:** Positive predictions that are correct.
- **Recall:** Actual positives that are found.
- **F1:** Balance of precision and recall.

## 16. Feature, Target and Prediction Identification

| Practical question | Command or approach |
|---|---|
| Identify input features | Create `X = df[["Customer_Age", "Tickets", "Genre", "City", "Rating"]]` |
| Identify classification target | Create `y = df["target"]` where Confirmed = 1 and Cancelled = 0 |
| Identify regression target | Create `y = df["Final_Amount"]` for continuous amount prediction |
| Identify cluster target | For clustering, do not create `y`; use `X` only |
| Select numeric features | `numeric_features = df.select_dtypes(include=np.number).columns.drop("target").tolist()` |
| Select text features | `categorical_features = df.select_dtypes(include=["object", "string"]).columns.tolist()` |
| Prepare model input | `X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)` |
| Generate predictions | `prediction = model.predict(X_test)` |
| Generate probabilities | `probability = model.predict_proba(X_test)[:, 1]` |
| Compare actual and predicted | `pd.DataFrame({"Actual": y_test, "Predicted": prediction})` |
| Predict one new booking | `model.predict(pd.DataFrame([new_booking]))` |

Use: selecting the wrong target or accidentally including the target inside `X` causes leakage or invalid training.

## 17. Support Vector Machine (SVM)

Support Vector Machine is an additional exam-sheet algorithm. Import it with:

```python
from sklearn.svm import SVC, SVR
```

### SVM Classification

```python
from sklearn.svm import SVC

svm_classifier = Pipeline([
    ("preprocessing", preprocessor),
    ("model", SVC(kernel="rbf", probability=True, random_state=42))
])

svm_classifier.fit(X_train, y_train)
svm_prediction = svm_classifier.predict(X_test)
```

| SVM parameter | Meaning |
|---|---|
| `SVC(...)` | Support Vector Classifier; predicts categories. |
| `kernel="linear"` | Uses a straight linear boundary. |
| `kernel="rbf"` | Uses a nonlinear radial basis function boundary. |
| `kernel="poly"` | Uses a polynomial boundary. |
| `C=1.0` | Controls penalty for misclassification. |
| `gamma="scale"` | Controls influence of one training record for RBF/poly kernels. |
| `probability=True` | Enables `predict_proba()`. |
| `random_state=42` | Makes a probabilistic run reproducible. |

Evaluate SVM classification:

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

print("Accuracy:", accuracy_score(y_test, svm_prediction))
print("Precision:", precision_score(y_test, svm_prediction, zero_division=0))
print("Recall:", recall_score(y_test, svm_prediction, zero_division=0))
print("F1:", f1_score(y_test, svm_prediction, zero_division=0))
```

### SVM Regression

```python
from sklearn.svm import SVR

svm_regressor = Pipeline([
    ("preprocessing", reg_preprocessor),
    ("model", SVR(kernel="rbf", C=100, epsilon=0.1))
])

svm_regressor.fit(Xr_train, yr_train)
svm_regression_prediction = svm_regressor.predict(Xr_test)
```

`SVR` is Support Vector Regression. Use it only when the target is continuous, such as `Final_Amount`.

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

print("MAE:", mean_absolute_error(yr_test, svm_regression_prediction))
print("RMSE:", np.sqrt(mean_squared_error(yr_test, svm_regression_prediction)))
print("R2:", r2_score(yr_test, svm_regression_prediction))
```

SVM needs feature scaling because SVM distance and margin calculations are sensitive to units. Therefore use `StandardScaler` inside the pipeline.
