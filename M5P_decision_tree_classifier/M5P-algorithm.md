# M5P Algorithm with Manual Example

## Introduction to the M5P Algorithm

The **M5P algorithm** is a machine learning method used to create regression trees and model trees. It combines the structure of decision trees with linear regression models at the leaves, making it ideal for predicting continuous values. M5P builds intuitive and interpretable models by splitting the dataset and using linear regression to model outcomes in pure branches.

---

## How M5P Works

### 1. **Tree Building**
- The dataset is recursively split into subsets based on attributes that minimize the **standard deviation** of the target values.
- Attributes with the greatest ability to reduce variance are chosen at each step.

### 2. **Leaf Nodes**
- Instead of assigning fixed values to leaf nodes (as in standard decision trees), M5P assigns **linear regression models** to predict the target values.

### 3. **Pruning**
- Subtrees are replaced with simpler linear regression models if it improves overall performance.
- Pruning helps prevent overfitting and makes the model more generalizable.

---

## Manual Example: Predicting Exam Scores

### Problem
We want to predict a student’s **exam score** based on:
1. **Hours Studied** (continuous variable)
2. **Tutoring** (categorical variable: Yes or No)

### Dataset
| **Student** | **Hours Studied** | **Tutoring** | **Exam Score** |
|-------------|--------------------|--------------|----------------|
| 1           | 2                  | Yes          | 50             |
| 2           | 5                  | Yes          | 70             |
| 3           | 1                  | No           | 40             |
| 4           | 4                  | No           | 60             |
| 5           | 6                  | Yes          | 85             |
| 6           | 3                  | No           | 55             |

---

## Step 1: Splitting the Data

### Evaluate Attributes
The M5P algorithm splits the data to minimize the **standard deviation of Exam Scores**.

#### Split on Tutoring
- **Tutoring = Yes**:
  - Students: {1, 2, 5}
  - Exam Scores: {50, 70, 85}
  - Standard Deviation = 15

- **Tutoring = No**:
  - Students: {3, 4, 6}
  - Exam Scores: {40, 60, 55}
  - Standard Deviation = 10

#### Split on Hours Studied
We test thresholds such as \( \text{Hours Studied} \leq 3 \) and \( \text{Hours Studied} > 3 \):

- **Hours Studied ≤ 3**:
  - Students: {1, 3, 6}
  - Exam Scores: {50, 40, 55}
  - Standard Deviation = 7.64

- **Hours Studied > 3**:
  - Students: {2, 4, 5}
  - Exam Scores: {70, 60, 85}
  - Standard Deviation = 10.41

#### Choose the Split
- Splitting on **Hours Studied** minimizes the combined standard deviation, so it is chosen as the root split.

---

## Step 2: Build Subtrees and Regression Models

### Subtree 1: Hours Studied ≤ 3
- Students: {1, 3, 6}
- Exam Scores: {50, 40, 55}

Using linear regression:


\[
\text{Exam Score} = 35 + 5 \cdot \text{Hours Studied}
\]



### Subtree 2: Hours Studied > 3
- Students: {2, 4, 5}
- Exam Scores: {70, 60, 85}

Using linear regression:


\[
\text{Exam Score} = 45 + 6 \cdot \text{Hours Studied}
\]



---

## Step 3: Pruning

The algorithm evaluates whether replacing subtrees with a single linear regression model improves overall performance. In this case, the tree performs better than a single model, so pruning is not applied.

---

## Final Model Tree

```
Hours Studied? 
├── ≤ 3: Exam Score = 35 + 5 × Hours Studied 
└── > 3: Exam Score = 45 + 6 × Hours Studied
```

---

## Predictions Using the Tree

To predict a student's exam score:
1. Check the value of **Hours Studied**.
2. Use the corresponding regression model to calculate the predicted score.

### Example Predictions:
1. A student who studied **2 hours**:
   

$$
   \text{Exam Score} = 35 + 5 \cdot 2 = 45
$$



2. A student who studied **5 hours**:
   

$$ 
   \text{Exam Score} = 45 + 6 \cdot 5 = 75
$$



---

## Key Takeaways
- M5P combines decision trees with linear regression for robust prediction of continuous values.
- Splitting minimizes standard deviation, while pruning ensures simplicity and generalizability.
- The resulting model tree is intuitive and interpretable, making it easy to apply in practice.


