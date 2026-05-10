# Methods Taught in Data Mining from the PDF

Based on the PDF, the course covers **Business Intelligence, Data Warehousing, OLAP, ETL, DWH architectures, and Dimensional Fact Model (DFM)** rather than classic predictive “data mining” algorithms like decision trees or k-means [file:1].  
The methods and techniques explicitly taught in the document are listed below [file:1].

## 1) Main methods and techniques

- Business Intelligence concepts and definitions [file:1].
- Data Warehouse concepts and properties: subject-oriented, integrated, time-variant, non-volatile [file:1].
- DataMart design and use [file:1].
- OLAP analysis methods:
  - Roll-up [file:1].
  - Drill-down [file:1].
  - Slice-and-dice [file:1].
  - Pivot [file:1].
  - Drill-across [file:1].
  - Drill-through [file:1].
- ETL methods:
  - Extraction [file:1].
  - Cleansing [file:1].
  - Transformation [file:1].
  - Loading [file:1].
- Data inconsistency handling:
  - Dictionary-based techniques [file:1].
  - Approximate join [file:1].
  - Similarity approach / edit distance [file:1].
  - Ad-hoc business-rule-based algorithms [file:1].
- Data transformation techniques:
  - Type and format conversion [file:1].
  - String conversion [file:1].
  - Naming-convention transformation [file:1].
  - Enrichment and derived attributes [file:1].
  - Separation and concatenation [file:1].
  - Denormalization [file:1].
- Data warehouse architectures:
  - Single-layer architecture [file:1].
  - Two-layer architecture [file:1].
  - Three-layer architecture [file:1].
- DFM conceptual modeling:
  - Fact [file:1].
  - Measure [file:1].
  - Dimension [file:1].
  - Dimensional attribute [file:1].
  - Hierarchy [file:1].
  - Primary event and secondary event [file:1].
  - Additivity [file:1].
  - Aggregation operators: distributive, algebraic, holistic [file:1].
  - Advanced DFM concepts: descriptive attributes, cross-dimensional attributes, convergence, shared hierarchies, multiple arcs, optional arcs, incomplete hierarchies, recursive hierarchies [file:1].

## 2) Practice exercises in Python

Below are exam-style exercises you can do with only:
- Numpy
- Scipy
- pandas
- matplotlib
- seaborn
- scikit-learn

Each exercise includes a solution code block so you can study the logic.

---

### Exercise 1 — Roll-up with pandas

You have a sales table at product/month level. Compute a roll-up to category/year level.

```python
import pandas as pd

df = pd.DataFrame({
    "category": ["Food", "Food", "Food", "Tech", "Tech"],
    "product": ["Bread", "Milk", "Cheese", "Laptop", "Mouse"],
    "year": ,
    "revenue": 
})

rollup = df.groupby(["category", "year"], as_index=False)["revenue"].sum()
print(rollup)
```

---

### Exercise 2 — Drill-down

Starting from category/year aggregates, drill down to product/year.

```python
import pandas as pd

df = pd.DataFrame({
    "category": ["Food", "Food", "Tech", "Tech"],
    "product": ["Bread", "Milk", "Laptop", "Mouse"],
    "year": ,
    "revenue": 
})

drill_down = df.groupby(["category", "product", "year"], as_index=False)["revenue"].sum()
print(drill_down)
```

---

### Exercise 3 — Slice

Keep only rows for one category, then compute total profit by year.

```python
import pandas as pd

df = pd.DataFrame({
    "category": ["Food", "Food", "Tech", "Tech"],
    "year": ,
    "profit": 
})

slice_food = df[df["category"] == "Food"]
result = slice_food.groupby("year", as_index=False)["profit"].sum()
print(result)
```

---

### Exercise 4 — Pivot

Create a pivot table with rows as category and columns as year.

```python
import pandas as pd

df = pd.DataFrame({
    "category": ["Food", "Food", "Tech", "Tech"],
    "year": ,
    "revenue": 
})

pivot = df.pivot_table(index="category", columns="year", values="revenue", aggfunc="sum", fill_value=0)
print(pivot)
```

---

### Exercise 5 — Average with support count

In the PDF, average is an algebraic operator and requires a support measure like COUNT. Compute average revenue per category using sum and count.

```python
import pandas as pd

df = pd.DataFrame({
    "category": ["Food", "Food", "Food", "Tech", "Tech"],
    "revenue": 
})

agg = df.groupby("category").agg(
    total_revenue=("revenue", "sum"),
    count=("revenue", "count")
).reset_index()

agg["avg_revenue"] = agg["total_revenue"] / agg["count"]
print(agg)
```

---

### Exercise 6 — Clean missing values

Replace missing age values with the median age.

```python
import pandas as pd

df = pd.DataFrame({
    "customer": ["A", "B", "C", "D"],
    "age": [22, None, 35, None]
})

df["age"] = df["age"].fillna(df["age"].median())
print(df)
```

---

### Exercise 7 — Clean inconsistent text

Standardize city names to uppercase and strip spaces.

```python
import pandas as pd

df = pd.DataFrame({
    "city": [" Bologna", "bologna ", "MILAN", "milan "]
})

df["city_clean"] = df["city"].str.strip().str.upper()
print(df)
```

---

### Exercise 8 — Detect duplicate records

Find repeated customer rows.

```python
import pandas as pd

df = pd.DataFrame({
    "customer_id":,[1]
    "name": ["Ana", "Bob", "Bob", "Cara", "Dan", "Dan", "Dan"]
})

duplicates = df[df.duplicated(subset=["customer_id", "name"], keep=False)]
print(duplicates)
```

---

### Exercise 9 — Approximate string matching

Use Levenshtein distance from SciPy to compare surnames.

```python
from scipy.spatial.distance import levenshtein

a = "Turicchia"
b = "Turricchia"

distance = levenshtein(a, b)
print(distance)
```

If your SciPy version does not include that function, use this fallback:

```python
def edit_distance(s1, s2):
    m, n = len(s1), len(s2)
    dp = [ * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i] = i
    for j in range(n + 1):
        dp[j] = j
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            cost = 0 if s1[i - 1] == s2[j - 1] else 1
            dp[i][j] = min(
                dp[i - 1][j] + 1,
                dp[i][j - 1] + 1,
                dp[i - 1][j - 1] + cost
            )
    return dp[m][n]

print(edit_distance("Turicchia", "Turricchia"))
```

---

### Exercise 10 — One-hot encode a dimension

Transform a categorical dimension into dummy variables.

```python
import pandas as pd

df = pd.DataFrame({
    "product": ["Bread", "Milk", "Laptop", "Mouse"]
})

encoded = pd.get_dummies(df, columns=["product"])
print(encoded)
```

---

### Exercise 11 — Normalize numeric features

Use scikit-learn to standardize revenue and profit.

```python
import pandas as pd
from sklearn.preprocessing import StandardScaler

df = pd.DataFrame({
    "revenue": ,
    "profit": 
})

scaler = StandardScaler()
scaled = scaler.fit_transform(df)

result = pd.DataFrame(scaled, columns=df.columns)
print(result)
```

---

### Exercise 12 — K-means clustering

Cluster customers using income and spending score.

```python
import pandas as pd
from sklearn.cluster import KMeans

df = pd.DataFrame({
    "income": ,
    "spending": 
})

model = KMeans(n_clusters=2, random_state=42, n_init=10)
df["cluster"] = model.fit_predict(df[["income", "spending"]])
print(df)
```

---

### Exercise 13 — Classification with train/test split

Predict whether a customer buys a product.

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

df = pd.DataFrame({
    "age": ,
    "income": ,
    "buy":[1]
})

X = df[["age", "income"]]
y = df["buy"]

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)

clf = DecisionTreeClassifier(random_state=42)
clf.fit(X_train, y_train)

pred = clf.predict(X_test)
print("Accuracy:", accuracy_score(y_test, pred))
```

---

### Exercise 14 — Correlation analysis

Compute the correlation between quantity sold and revenue.

```python
import pandas as pd

df = pd.DataFrame({
    "qty_sold":,[1]
    "revenue": 
})

corr = df["qty_sold"].corr(df["revenue"])
print(corr)
```

---

### Exercise 15 — Visualization of aggregates

Plot total revenue by category.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.DataFrame({
    "category": ["Food", "Food", "Tech", "Tech"],
    "revenue": 
})

agg = df.groupby("category", as_index=False)["revenue"].sum()

sns.barplot(data=agg, x="category", y="revenue")
plt.title("Total Revenue by Category")
plt.show()
```

---

### Exercise 16 — Build a mini DWH-style table

Create a denormalized sales table by joining fact and dimension tables.

```python
import pandas as pd

fact = pd.DataFrame({
    "date_key":,[1]
    "product_key": ,
    "qty_sold": ,
    "revenue": 
})

dim_date = pd.DataFrame({
    "date_key":,[1]
    "year": ,
    "month": ["Jan", "Feb"]
})

dim_product = pd.DataFrame({
    "product_key": ,
    "product_name": ["Milk", "Bread"],
    "category": ["Food", "Food"]
})

merged = fact.merge(dim_date, on="date_key").merge(dim_product, on="product_key")
print(merged)
```

---

## 3) Exam-focused tips

- Practice reading a table and deciding whether the correct operation is roll-up, drill-down, slice, or pivot [file:1].
- Be able to explain why average needs both sum and count in DFM terms [file:1].
- Memorize the ETL phases and what each one does [file:1].
- Know the difference between normalized and denormalized schemas [file:1].
- For Python, be comfortable with `groupby`, `pivot_table`, `merge`, `fillna`, and basic scikit-learn workflows because they match the exam topics well.

## 4) Suggested study order

1. Data Warehouse basics [file:1].
2. OLAP operators [file:1].
3. ETL and data cleaning [file:1].
4. DFM concepts and additivity [file:1].
5. Python practice with pandas and scikit-learn.

Would you like me to turn this into a cleaner **study sheet** with only the exercise statements first, then a separate **solutions section**?
