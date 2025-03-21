# Building a Decision Tree Using the J48 Algorithm

## Problem: Deciding Whether to Play Outside Based on Weather

### Dataset
| **Day** | **Outlook** | **Play Outside** |
|---------|-------------|------------------|
| 1       | Sunny       | No               |
| 2       | Sunny       | Yes              |
| 3       | Overcast    | Yes              |
| 4       | Rainy       | Yes              |
| 5       | Rainy       | No               |

The target variable is **Play Outside** (Yes/No), and we want to predict this based on the **Outlook** attribute.

---

## Step 1: Calculate Overall Entropy

The target attribute, **Play Outside**, has the following distribution:
- **Yes**: 3 instances
- **No**: 2 instances

The formula for entropy is:

$$
\text{Entropy} = -\sum p_i \cdot \log_2(p_i)
$$

For the dataset:

$$
p_{Yes} = \frac{3}{5}, \; p_{No} = \frac{2}{5}
$$

Substitute these values:

$$
\text{Entropy} = -\left(\frac{3}{5} \cdot \log_2\left(\frac{3}{5}\right)\right) - \left(\frac{2}{5} \cdot \log_2\left(\frac{2}{5}\right)\right)
$$

Using approximations for logarithms:
- $$ \log_2\left(\frac{3}{5}\right) = -0.737 $$
- $$ \log_2\left(\frac{2}{5}\right) = -1.322 $$

$$
\text{Entropy} = -\left(0.6 \cdot -0.737\right) - \left(0.4 \cdot -1.322\right)
$$

$$
\text{Entropy} = 0.442 + 0.529 = 0.971
$$

The overall entropy of the dataset is **0.971**.

---

## Step 2: Calculate Entropy for Splitting on **Outlook**

Now we calculate the entropy for each subset based on the attribute **Outlook**.

### Subset 1: Outlook = Sunny
- Instances: Day 1 (No), Day 2 (Yes)
- $$ p_{Yes} = \frac{1}{2}, \; p_{No} = \frac{1}{2} $$

$$
\text{Entropy}_{Sunny} = -\left(\frac{1}{2} \cdot \log_2\left(\frac{1}{2}\right)\right) - \left(\frac{1}{2} \cdot \log_2\left(\frac{1}{2}\right)\right)
$$

$$
\text{Entropy}_{Sunny} = -\left(0.5 \cdot -1\right) - \left(0.5 \cdot -1\right)
$$

$$
\text{Entropy}_{Sunny} = 0.5 + 0.5 = 1.0
$$

### Subset 2: Outlook = Overcast
- Instances: Day 3 (Yes)
- $$ p_{Yes} = 1, \; p_{No} = 0 $$

$$
\text{Entropy}_{Overcast} = -\left(\frac{1}{1} \cdot \log_2\left(\frac{1}{1}\right)\right) = 0
$$

### Subset 3: Outlook = Rainy
- Instances: Day 4 (Yes), Day 5 (No)
- $$ p_{Yes} = \frac{1}{2}, \; p_{No} = \frac{1}{2} $$

$$
\text{Entropy}_{Rainy} = -\left(\frac{1}{2} \cdot \log_2\left(\frac{1}{2}\right)\right) - \left(\frac{1}{2} \cdot \log_2\left(\frac{1}{2}\right)\right)
$$

$$
\text{Entropy}_{Rainy} = 0.5 + 0.5 = 1.0
$$

---

## Step 3: Calculate Weighted Entropy for the Split

The formula for weighted entropy is:

$$
\text{Weighted Entropy} = \sum \left(\frac{\text{Subset Size}}{\text{Total Size}} \cdot \text{Entropy of Subset}\right)
$$

- **Sunny**: $$ \frac{2}{5} \cdot 1.0 = 0.4 $$
- **Overcast**: $$ \frac{1}{5} \cdot 0 = 0.0 $$
- **Rainy**: $$ \frac{2}{5} \cdot 1.0 = 0.4 $$

$$
\text{Weighted Entropy} = 0.4 + 0.0 + 0.4 = 0.8
$$

---

## Step 4: Calculate Information Gain for **Outlook**

The formula for Information Gain (IG) is:

$$
\text{IG} = \text{Entropy of Parent Node} - \text{Weighted Entropy of Child Nodes}
$$

For the split on **Outlook**:

$$
\text{IG}_{Outlook} = 0.971 - 0.8 = 0.171
$$

---

## Step 5: Build the Decision Tree

### Subtree: **Outlook = Sunny**
| **Day** | **Temperature** | **Play Outside** |
|---------|-----------------|------------------|
| 1       | Hot             | No               |
| 2       | Mild            | Yes              |

Split on **Temperature**:
- **Hot** → Play Outside = No (pure subset)
- **Mild** → Play Outside = Yes (pure subset)

### Subtree: **Outlook = Overcast**
- This subset is pure → Play Outside = Yes.

### Subtree: **Outlook = Rainy**
| **Day** | **Wind** | **Play Outside** |
|---------|----------|------------------|
| 4       | Weak     | Yes              |
| 5       | Strong   | No               |

Split on **Wind**:
- **Weak** → Play Outside = Yes (pure subset)
- **Strong** → Play Outside = No (pure subset)

---

## Final Decision Tree
```
Outlook?
├── Sunny
│   ├── Temperature = Hot: No
│   └── Temperature = Mild: Yes
├── Overcast: Yes
└── Rainy
    ├── Wind = Weak: Yes
    └── Wind = Strong: No
```