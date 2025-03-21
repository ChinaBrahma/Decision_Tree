# J48 Decision Tree Algorithm - Manual Example

## Example: Deciding Whether to Play Outside Based on Weather

### Dataset
We have a small dataset that describes whether to play outside, based on weather conditions:

| **Day** | **Outlook** | **Temperature** | **Humidity** | **Wind** | **Play Outside** |
|---------|-------------|-----------------|--------------|----------|------------------|
| 1       | Sunny       | Hot             | High         | Weak     | No               |
| 2       | Sunny       | Hot             | High         | Strong   | No               |
| 3       | Overcast    | Hot             | High         | Weak     | Yes              |
| 4       | Rainy       | Mild            | High         | Weak     | Yes              |
| 5       | Rainy       | Cool            | Normal       | Weak     | Yes              |
| 6       | Rainy       | Cool            | Normal       | Strong   | No               |
| 7       | Overcast    | Cool            | Normal       | Strong   | Yes              |
| 8       | Sunny       | Mild            | High         | Weak     | No               |
| 9       | Sunny       | Cool            | Normal       | Weak     | Yes              |
| 10      | Rainy       | Mild            | Normal       | Weak     | Yes              |
| 11      | Sunny       | Mild            | Normal       | Strong   | Yes              |
| 12      | Overcast    | Mild            | High         | Strong   | Yes              |
| 13      | Overcast    | Hot             | Normal       | Weak     | Yes              |
| 14      | Rainy       | Mild            | High         | Strong   | No               |

The target attribute is **Play Outside (Yes/No)**, and we aim to predict this using the other attributes.

---

### Step-by-Step Process

#### 1. **Calculate Entropy (Measure of Uncertainty)**
Entropy indicates how pure (or impure) a dataset is. For the target attribute `Play Outside`, calculate the overall entropy:

The formula for entropy is:
$$ \text{Entropy} = -\sum p_i \cdot \log_2(p_i) $$

In our dataset:
- Total instances = 14
- `Yes`: 9
- `No`: 5

Overall entropy:
$$ E = -\frac{9}{14} \cdot \log_2(\frac{9}{14}) - \frac{5}{14} \cdot \log_2(\frac{5}{14}) = 0.94 $$

---

#### 2. **Calculate Information Gain (Split Quality)**
Information Gain (IG) measures how much an attribute reduces uncertainty. We calculate IG for each attribute.

**For `Outlook`:**

- **Sunny**:
  - `Yes`: 2, `No`: 3
  - Entropy = $$ -\frac{2}{5} \cdot \log_2(\frac{2}{5}) - \frac{3}{5} \cdot \log_2(\frac{3}{5}) = 0.97 $$

- **Overcast**:
  - `Yes`: 4, `No`: 0
  - Entropy = $$ -\frac{4}{4} \cdot \log_2(\frac{4}{4}) = 0 $$

- **Rainy**:
  - `Yes`: 3, `No`: 2
  - Entropy = $$ -\frac{3}{5} \cdot \log_2(\frac{3}{5}) - \frac{2}{5} \cdot \log_2(\frac{2}{5}) = 0.97 $$

Weighted Entropy for `Outlook`:
$$ E_{Outlook} = \frac{5}{14} \cdot 0.97 + \frac{4}{14} \cdot 0 + \frac{5}{14} \cdot 0.97 = 0.69 $$

**Information Gain for `Outlook`:**
$$ IG = E - E_{Outlook} = 0.94 - 0.69 = 0.25 $$

Repeat this process for the other attributes.

---

#### 3. **Select the Best Attribute to Split**
Choose the attribute with the highest IG as the root of the decision tree. In this example, assume `Outlook` has the highest IG, so it becomes the root node.

---

#### 4. **Split the Data and Repeat**
Partition the dataset based on the selected attribute (`Outlook`):
- Subset for `Sunny`
- Subset for `Overcast`
- Subset for `Rainy`

For each subset:
- Recalculate entropy and IG for the remaining attributes.
- Select the next best attribute to split.

---

#### 5. **Stopping Conditions**
The process stops when:
- All data in a subset belongs to a single class.
- No more attributes are left.
- A predefined maximum depth is reached.

---

### Resulting Tree
After following this process, the resulting decision tree might look like this:
```
Outlook
├── Sunny
│   ├── Humidity = High: No
│   └── Humidity = Normal: Yes
├── Overcast: Yes
└── Rainy
    ├── Wind = Weak: Yes
    └── Wind = Strong: No
```


This decision tree can now classify new instances by following the paths based on attribute values.

---

### Why These Steps Are Necessary
- **Entropy** is calculated to measure uncertainty and decide the best split.
- **Information Gain** ensures we choose the attribute that provides the most clarity in classification.
- **Splitting** organizes data into smaller, homogeneous subsets, making predictions easier.
- **Stopping Conditions** prevent overfitting and keep the tree interpretable.

---
# Understanding \( p_i \) in Entropy

## What is Entropy?

Entropy measures the uncertainty or randomness in a dataset. It is commonly used in decision trees to evaluate how "pure" or "impure" a dataset is. The formula for entropy is:

$$
\text{Entropy} = -\sum p_i \cdot \log_2(p_i)
$$

Where:
- \( p_i \) represents the **probability of the \( i \)-th class** in the dataset.

---

## What Does \( p_i \) Represent?

In the context of entropy:
- \( p_i \) is the proportion of instances belonging to class \( i \).
- It is calculated using the formula:

$$
p_i = \frac{\text{Count of instances in class } i}{\text{Total number of instances}}
$$

This ensures that the entropy calculation reflects the distribution of classes in the dataset.

---

## Example Dataset

Consider the following dataset:

| **Day** | **Play Outside** |
|---------|------------------|
| 1       | Yes              |
| 2       | No               |
| 3       | Yes              |
| 4       | Yes              |
| 5       | No               |

---

## Step-by-Step Calculation

### Step 1: Calculate \( p_i \) for Each Class

- Total examples = 5
- `Yes`: 3 instances → $$ p_{Yes} = \frac{3}{5} = 0.6 $$
- `No`: 2 instances → $$ p_{No} = \frac{2}{5} = 0.4 $$

---

### Step 2: Plug \( p_i \) Values Into the Entropy Formula

The entropy formula is:

$$
\text{Entropy} = -\sum p_i \cdot \log_2(p_i)
$$

Substitute \( p_{Yes} = 0.6 \) and \( p_{No} = 0.4 \):

$$
\text{Entropy} = -\left(0.6 \cdot \log_2(0.6)\right) - \left(0.4 \cdot \log_2(0.4)\right)
$$

Using logarithm values:
- \( \log_2(0.6) = -0.737 \)
- \( \log_2(0.4) = -1.322 \)

$$
\text{Entropy} = -\left(0.6 \cdot -0.737\right) - \left(0.4 \cdot -1.322\right)
$$

$$
\text{Entropy} = 0.442 + 0.529 = 0.971
$$

---

## Interpretation of Results

- The entropy of 0.971 indicates that the dataset has a moderate level of uncertainty.
- If all examples belonged to one class, entropy would be 0 (no uncertainty).
- If the examples were evenly distributed across classes, entropy would be at its maximum value.

---

## Summary

- \( p_i \) represents the probability of each class in the data.
- Entropy uses \( p_i \) to measure uncertainty in the dataset.
- Decision trees like J48 minimize entropy by splitting data into purer subsets.
