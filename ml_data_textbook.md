# The Nature of Data in Machine Learning

*Adapted from the lectures of Claudio Sartori (DISI - Department of Computer Science and Engineering, University of Bologna).*

---

## Introduction: The Reality of Data

In the theoretical world of algorithms, data is often assumed to be a clean, perfectly structured matrix of numbers. In reality, data is messy, complicated, and rarely perfect. Real-world datasets come in a variety of formats—quantitative, qualitative, structured, unstructured, texts, images, and graphs. 

Before we can even begin to apply Machine Learning techniques, we must confront a fundamental truth of computer science: **Garbage-in, garbage-out**. If the quality of the data is poor—riddled with missing values, inconsistencies, duplications, or unhandled errors—the resulting models will be equally poor. Therefore, understanding the nature of your data and executing rigorous pre-processing activities to transform it is just as crucial as choosing the right machine learning algorithm.

---

## Chapter 1: Anatomy of Data Types

To analyze data, we must first understand what *type* of data we are looking at. According to the classic typology (Stevens, 1946), data is broadly split into **Categorical** and **Numerical** types, which are further broken down into four foundational scales:

### Categorical Data
1. **Nominal Data:** The values are merely labels used to distinguish one entity from another. 
   * *Examples:* Zip codes, eye color, gender, ID numbers.
   * *Operators:* You can only test for equality ($=$ or $\neq$). 
   * *Allowed Transformations:* Any one-to-one correspondence (e.g., scrambling or masking IDs).
2. **Ordinal Data:** The values provide enough information to impose a total ordering, but the exact difference between values is not known or meaningful.
   * *Examples:* Hardness of minerals, or non-numerical quality evaluations (Bad, Fair, Good, Excellent).
   * *Operators:* You can test for order ($<, >$).
   * *Allowed Transformations:* Any order-preserving (monotonic) transformation. (e.g., mapping "Bad, Fair, Good" to 1, 2, 3).

### Numerical Data
3. **Interval Data:** The differences between values are meaningful, but there is no true, absolute zero point.
   * *Examples:* Calendar dates, Celsius or Fahrenheit temperatures, IQ test scores.
   * *Operators:* You can add or subtract ($+, -$).
   * *Allowed Transformations:* Linear functions ($new = a + b \times old$). 
4. **Ratio Data:** There is a univocal, true definition of zero. 
   * *Examples:* Kelvin temperatures, mass, length, counts of items, ages.
   * *Operators:* All mathematical operations are allowed, including multiplication and division.
   * *Allowed Transformations:* Any mathematical function, including standardization and percentage variations.

> **Author's Note: The "Twice as Hot" Fallacy**
> The distinction between Interval and Ratio data often trips people up. Think about temperature. If it is $10^\circ C$ today and $20^\circ C$ tomorrow, is tomorrow *twice as hot*? No. Because $0^\circ C$ is an arbitrary point (the freezing point of water), not the absolute absence of heat. If you convert those same temperatures to Fahrenheit ($50^\circ F$ and $68^\circ F$), the ratio is completely different. However, Kelvin is a *Ratio* scale because $0 K$ is absolute zero. A substance at $200 K$ truly has twice the thermal energy of a substance at $100 K$. This is why percentage variations are meaningless for Interval data, but valid for Ratio data!

---

## Chapter 2: Granularity and Symmetry

Beyond the four basic types, we must also consider how the data behaves structurally:

* **Discrete vs. Continuous Domains:** * *Discrete:* Variables that allow a finite (or infinitely countable) number of values, such as codes or counts. Binary attributes (0 or 1) are a special case of discrete data. Nominal and ordinal data are inherently discrete.
  * *Continuous:* Variables represented as floating-point numbers. Interval and ratio data are generally continuous (though often recorded with some approximation).
* **Asymmetric Attributes:** Sometimes, only the *presence* of a feature is important. For example, consider a university dataset recording every exam offered. Most students will only have passed a few, leaving the rest empty or zero. The zeros don't carry much information; only the non-null values (the passed exams) are interesting. This is highly relevant in market basket analysis and the discovery of association rules.

---

## Chapter 3: The Shape of Datasets

Datasets come in many shapes, characterized by three major overarching properties:
1. **Dimensionality:** The number of attributes. Having hundreds or thousands of attributes isn't just a quantitative challenge; it changes the nature of the problem (a phenomenon often called the *curse of dimensionality*).
2. **Sparsity:** When a dataset is mostly empty (composed of many zeros or nulls).
3. **Resolution:** The scale at which data is collected. Analyzing data that is too detailed can drown your model in noise, while data that is too highly aggregated can obscure the very patterns you are trying to find.

### Common Data Formats
* **Relational Tables:** The standard format where a set of attributes is the same for all records.
* **Data Matrix:** A table consisting only of numeric attributes of the same type. In this format, each row represents a point in an $N$-dimensional vector space.
* **Document Representation:** Used in text mining. Each row represents a document, each column represents a specific word (term), and the cell contains the frequency of that word in the document. Notice that the *sequence* of terms is completely lost in this format.
* **Transactional Data:** Used for market basket analysis. Each record is simply a set of items bought together, with varying lengths (e.g., Transaction 1: Bread, Milk. Transaction 2: Beer, Diapers, Bread).
* **Graph and Ordered Data:** Some data relies heavily on relationships and sequences, such as web page links, molecular structures, spatial maps, temporal events, or genetic sequences.

---

## Chapter 4: Data Quality - Hunting for Gremlins

"Data cleaning" is the difficult but mandatory process of dealing with flawed data. Here are the primary offenders:

### 1. Noise and Outliers
* **Noise:** The random modification of original values, often due to transmission errors or mixed signals (e.g., web crawler traffic mixed in with real human clicks on a website).
* **Outliers:** Data points with characteristics considerably different from the vast majority of the dataset. They can be caused by noise, or they might represent a genuinely rare event (which is often the most valuable insight!).

> **Author's Note: Catching Outliers with the Boxplot**
> A simple, highly effective way to detect numeric outliers is using descriptive statistics, specifically the **InterQuartile Range (IQR)**. 
> Imagine ordering all your data from smallest to largest and splitting it into four equal chunks (quartiles). The first quartile (Q1) is the 25% mark, and the third quartile (Q3) is the 75% mark. The IQR is the distance between Q3 and Q1 (the middle 50% of your data). 
> A standard rule of thumb is to calculate the "whiskers" of your data: anything lower than $(Q1 - 1.5 \times IQR)$ or higher than $(Q3 + 1.5 \times IQR)$ is considered an outlier. Visualizing this creates a "Boxplot," making anomalous points instantly visible.

### 2. Missing Values
Why is data missing? Sometimes it wasn't collected (e.g., people refusing to share their income), and sometimes it simply isn't applicable (e.g., asking a child for their annual salary). 

Handling them is tricky:
* You could ignore or discard objects with missing values (often a bad idea, as you might lose too much data).
* You could estimate or insert a default value (like the mean).
* You could insert possible values weighted with probabilities.

*Beware the nulls in disguise!* A widespread bad habit in database management is storing a '0' or a special code (like '999') when information is unavailable. If a machine learning algorithm reads a missing age as '0', it will severely skew the results.

### 3. Duplicated Data
When merging data from different systems (e.g., two different company databases after a merger), you will inevitably encounter duplicate or "almost duplicated" objects. Identifying and reconciling these entities is a major hurdle in data pre-processing.
