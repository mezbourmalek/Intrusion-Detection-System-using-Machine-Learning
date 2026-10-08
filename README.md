# Intrusion Detection System using Machine Learning

## Project Overview

This project was done as part of a Machine Learning exercise about building a simple **Intrusion Detection System (IDS)**.

The main goal was to build a complete machine learning pipeline, starting from the exploration of the dataset and ending with the evaluation of a classification model.

For this project, I used:

* **One-Hot Encoding** for categorical features
* **StandardScaler** for normalization
* **Random Forest Classifier** as the machine learning algorithm

The model is used to classify network connections as either **normal** or **attack**.

---

## Dataset

The dataset contains network connection information that can be used to detect suspicious network activity.

The original dataset contains **45,350 rows and 12 columns**.

The features include:

* `duration`
* `protocol_type`
* `service`
* `flag`
* `src_bytes`
* `dst_bytes`
* `count`
* `srv_count`
* `diff_srv_rate`
* `same_srv_rate`
* `dst_host_same_srv_rate`
* `class`

The target variable is:

* `normal`
* `attack`

### Dataset preprocessing

First, I checked the dataset for missing values. There were no missing values in the dataset.

I also checked for duplicated rows. There were **7,132 duplicated rows**, so I removed them before training the model.

After removing duplicates, the dataset contained **38,218 observations**.

The distribution of the target classes was:

* **Normal:** 21,401
* **Attack:** 16,817

---

## Machine Learning Pipeline

The project follows these main steps:

```text
Dataset
   ↓
Data Exploration
   ↓
Remove Duplicates
   ↓
Separate Features and Target
   ↓
One-Hot Encoding
   ↓
Train/Test Split
   ↓
StandardScaler
   ↓
Random Forest
   ↓
Prediction
   ↓
Evaluation
```

---

## 1. Data Exploration

The first step was to load the dataset using Pandas and inspect its structure.

I checked:

* Dataset dimensions
* Data types
* Missing values
* Duplicate rows
* Class distribution
* Categorical feature values
* Basic statistical information

For example, the categorical features in the dataset are:

```python
cat_cols = ['protocol_type', 'service', 'flag']
```

The numerical features were detected using their data types.

---

## 2. Removing Duplicates

The dataset contained duplicated observations.

I removed them using:

```python
df2 = df.drop_duplicates()
```

This reduced the dataset from **45,350 rows to 38,218 rows**.

This step was done before training the model to avoid having duplicated observations in the learning process.

---

## 3. One-Hot Encoding

The categorical features cannot be directly used by the Random Forest model, so I converted them into numerical features using **One-Hot Encoding**.

The categorical columns were:

```python
['protocol_type', 'service', 'flag']
```

I used Pandas:

```python
X_encoded = pd.get_dummies(
    X,
    columns=cat_cols,
    dtype=int
)
```

This transformed the categorical variables into binary columns.

After encoding, the dataset contained **91 features**.

---

## 4. Train/Test Split

The encoded dataset was divided into training and testing sets.

I used:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X_encoded,
    y,
    test_size=0.4,
    random_state=42
)
```

The test size was set to **40%**, while the remaining **60%** was used for training.

The `random_state=42` was used to make the split reproducible.

---

## 5. Data Normalization

After encoding, I applied **StandardScaler** to normalize the numerical representation of the features.

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

The scaler was fitted only on the training data and then applied to the test data.

---

## 6. Random Forest Classifier

For the machine learning part, I used a **Random Forest Classifier**.

The model was configured with **250 trees**:

```python
model = RandomForestClassifier(
    n_estimators=250,
    random_state=42
)

model.fit(X_train_scaled, y_train)
```

The trained model was then used to make predictions on the test set:

```python
y_pred = model.predict(X_test_scaled)
```

---

## 7. Model Evaluation

The model was evaluated using:

* Confusion Matrix
* Classification Report
* Accuracy

The confusion matrix obtained was:

```text
[[6710   36]
 [  12 8530]]
```

The classification results were approximately:

| Class  | Precision | Recall | F1-score |
| ------ | --------- | ------ | -------- |
| Attack | 1.00      | 0.99   | 1.00     |
| Normal | 1.00      | 1.00   | 1.00     |

The final accuracy was:

**99.69%**

```text
Accuracy: 0.9968602825745683
```

These results show that the Random Forest model performed very well on the test set.

---

## 8. Feature Importance

I also used the feature importance provided by the Random Forest model to look at the most important features for the classification.

The notebook includes a visualization of the **top 10 most important features**.

This gives an idea of which network characteristics contributed the most to the model's predictions.

---

## Technologies Used

* Python
* Jupyter Notebook
* Pandas
* NumPy
* Scikit-learn
* Matplotlib

---

## Project Structure

```text
IDS-RandomForest/
│
├── IDS_RandomForest.ipynb
├── IDS_resume.csv
└── README.md
```

> The dataset may not be included in the repository if it is subject to course or distribution restrictions.

---

## Conclusion

This project allowed me to implement a complete machine learning pipeline for a simple Intrusion Detection System.

The approach used in this work was:

**One-Hot Encoding + StandardScaler + Random Forest**

The Random Forest model achieved an accuracy of approximately **99.69%** on the test set.

The results are very good for this dataset, although a high accuracy on a specific dataset does not necessarily mean that the model would perform equally well on real-world network traffic. More testing with different datasets and validation methods would be needed for a real IDS system.

---

## Author

**Malek**

Computer Science / Data Science Student
