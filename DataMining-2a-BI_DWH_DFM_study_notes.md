# DataMining-2a-BI DWH DFM — Study Notes

### Course: Machine Learning and Data Mining | University of Bologna

***

## Table of Contents

- [1. Scope and context](#1-scope-and-context)
- [2. Business Intelligence](#2-business-intelligence)
- [3. Data Warehouse](#3-data-warehouse)
- [4. Data Warehouse Architecture](#4-data-warehouse-architecture)
- [5. DFM: Dimensional Fact Model](#5-dfm-dimensional-fact-model)
- [6. Measures, dimensions, and hierarchies](#6-measures-dimensions-and-hierarchies)
- [7. Designing a dimensional model](#7-designing-a-dimensional-model)
- [8. Typical examples](#8-typical-examples)
- [9. Common mistakes and exam traps](#9-common-mistakes-and-exam-traps)
- [10. Mathematical and analytical exercises](#10-mathematical-and-analytical-exercises)
- [11. Summary checklist](#11-summary-checklist)
- [12. 📝 Review Questions (30 Questions)](#12--review-questions-30-questions)

## 1. Scope and context

These notes cover the Business Intelligence, Data Warehouse, and Dimensional Fact Model topics commonly taught inside the Machine Learning and Data Mining course at the University of Bologna. The official course description includes data warehouse and data lake concepts among the core contents, together with data mining process topics and analytical methods.[cite:1]

A **data warehouse** is not the same thing as a transactional database. Transactional systems are designed to record day-to-day operations efficiently, while analytical systems are designed to support reporting, aggregation, historical analysis, and decision making.[cite:1]

> ⚠️ **Exam Tip:** A very common exam question is to ask for the difference between operational systems and analytical systems. Be ready to explain not only *what* they are, but also *why* they are designed differently.

> 💡 **Additional Context (from assistant):** Think of an online store. The checkout database must quickly record a single purchase, while the warehouse must answer questions like “How did quarterly revenue change by region, product category, and customer segment over the last three years?”

## 2. Business Intelligence

**Business Intelligence (BI)** is the set of methods, architectures, and tools used to transform raw organizational data into useful information for decision support. In practice, BI supports managers and analysts through dashboards, reports, OLAP-style exploration, trend analysis, and performance monitoring.[cite:1]

BI is usually built on top of integrated historical data rather than on isolated operational tables. This is why the data warehouse plays a central role: it collects data from multiple sources, cleans and integrates it, and stores it in a form suitable for analysis.[cite:1]

### BI goals

- Support strategic and tactical decisions.
- Provide a unified and consistent view of organizational data.
- Enable historical and comparative analysis over time.
- Reduce ambiguity caused by inconsistent source systems.

> ⚠️ **Exam Tip:** When defining BI, include both the *decision-support goal* and the *integration of historical data*. A short definition without these two parts is usually incomplete.

## 3. Data Warehouse

A **Data Warehouse (DWH)** is a repository designed for query and analysis rather than transaction processing. A classical definition describes it as subject-oriented, integrated, time-variant, and non-volatile.[cite:1]

### Core properties

- **Subject-oriented:** organized around major analytical subjects such as sales, customers, claims, or shipments.
- **Integrated:** data from multiple operational sources is standardized and reconciled.
- **Time-variant:** historical data is preserved, so analysis over time is possible.
- **Non-volatile:** data is mainly loaded and queried, not continuously updated like in OLTP systems.

### OLTP vs DWH

| Aspect | OLTP system | Data Warehouse |
|---|---|---|
| Main goal | Execute daily operations | Support analysis and decisions |
| Data scope | Current, detailed transactions | Historical, integrated data |
| Workload | Many short insert/update/delete operations | Complex read-heavy queries |
| Schema style | Highly normalized | Often dimensional / denormalized |
| Users | Clerks, applications, operational services | Analysts, managers, data scientists |

> ⚠️ **Exam Tip:** The phrase “read-heavy, historical, integrated, analytical” is a strong compact description of a warehouse. Memorize it.

> 💡 **Additional Context (from assistant):** If OLTP is the “cash register,” the warehouse is the “management control room.”

## 4. Data Warehouse Architecture

A warehouse architecture usually includes data sources, an ETL or ELT layer, a central repository, and analytical access tools. The course description explicitly places data warehouse and data lake concepts inside the broader ML and data mining curriculum, which is why architecture is usually taught together with modeling.[cite:1]

### Main components

1. **Source systems:** ERP, CRM, web logs, sensors, spreadsheets, external datasets.
2. **ETL/ELT:** extraction, cleaning, transformation, integration, and loading.
3. **Warehouse storage:** central historical repository.
4. **Data marts:** departmental or subject-specific analytical subsets.
5. **Analysis layer:** reporting, dashboards, OLAP, ad hoc queries, and mining.

### ETL

**ETL** stands for Extract, Transform, Load.

- **Extract:** collect data from source systems.
- **Transform:** clean errors, resolve inconsistencies, standardize formats, derive attributes, and integrate sources.
- **Load:** insert transformed data into the warehouse according to the target schema.

> ⚠️ **Exam Tip:** If the exam asks why ETL is necessary, do not answer only “to move data.” The key idea is *data quality and integration for analysis*.

## 5. DFM: Dimensional Fact Model

The **Dimensional Fact Model (DFM)** is a conceptual model used to represent analytical data in a multidimensional way. It focuses on **facts**, **measures**, **dimensions**, and **hierarchies**, making it easier to reason about aggregation and analysis.

A **fact** represents an event or phenomenon to analyze, such as a sale, shipment, booking, or claim. A **measure** is a numerical quantity associated with the fact, such as quantity sold, revenue, discount, or cost.

A **dimension** provides the perspectives from which the fact is analyzed, such as time, product, customer, or store. A **hierarchy** organizes dimension attributes across levels of granularity, such as Day → Month → Quarter → Year.

### Core elements

- **Fact:** central analytical event.
- **Measures:** numeric values to aggregate.
- **Dimensions:** analysis axes.
- **Hierarchies:** roll-up paths across granularity levels.
- **Attributes:** descriptive properties within dimensions.

> ⚠️ **Exam Tip:** Students often confuse a fact with a table and a measure with any numeric field. A numeric attribute is a *measure* only if it is meaningful for analysis and aggregation in the modeled fact.

## 6. Measures, dimensions, and hierarchies

### Facts

A good fact usually has many occurrences, is relevant to business analysis, and is associated with quantitative measures. “Sale” is a strong fact because it occurs often and supports many analytical questions.

Bad choices for facts are often very static entities such as “Product” or “Customer,” unless the analytical focus is on events involving them. These are usually better modeled as dimensions.

### Measures

Measures are numerical quantities summarized through aggregation functions such as SUM, COUNT, AVG, MIN, or MAX. Not every measure supports every aggregation correctly.

#### Additivity

- **Additive:** can be summed across all dimensions, for example sales amount.
- **Semi-additive:** can be summed along some dimensions but not all, for example account balance across customers but not across time.
- **Non-additive:** cannot be meaningfully summed, for example ratios or percentages.

> ⚠️ **Exam Tip:** Additivity is a favorite exam topic. Be ready to classify measures and justify the classification.

### Dimensions

Dimensions answer questions like:

- When did it happen?
- Where did it happen?
- Which product was involved?
- Which customer segment generated it?

### Hierarchies

Hierarchies support **roll-up** and **drill-down** operations.

- **Roll-up:** aggregate from finer to coarser detail, for example Day → Month.
- **Drill-down:** move from summary to more detailed levels, for example Region → City → Store.

A hierarchy must represent a meaningful path of aggregation. If the path is ambiguous or many-to-many, aggregation may become problematic.

> 💡 **Additional Context (from assistant):** A hierarchy behaves like a map zoom. Zooming out gives broader summaries; zooming in gives detail.

## 7. Designing a dimensional model

Dimensional design usually starts from business requirements. The designer identifies the processes to analyze, the grain of the fact, the candidate measures, and the relevant dimensions.

### Recommended design steps

1. Identify the business process.
2. Choose the **grain**, meaning the level of detail represented by one fact occurrence.
3. Identify the measures.
4. Identify dimensions and hierarchies.
5. Verify summarizability and aggregation correctness.
6. Translate the conceptual model into a logical star or snowflake schema.

### Grain

The **grain** is one of the most important modeling choices. It defines exactly what one row in the fact table means.

For example:

- “One row per sold product per receipt line” is a fine grain.
- “One row per store per month per product category” is a much coarser grain.

Changing the grain changes the possible analyses, the detail available, and the meaning of the measures.

> ⚠️ **Exam Tip:** If the exam asks you to design a fact, define the grain explicitly before listing measures and dimensions.

### Star and snowflake

A **star schema** places the fact table in the center and connects it directly to denormalized dimension tables. A **snowflake schema** normalizes some dimension tables into additional linked tables.

In teaching and conceptual modeling, the star idea is usually easier to understand because it highlights the multidimensional structure more clearly. Snowflaking can reduce redundancy but often increases join complexity.

## 8. Typical examples

### Sales example

Consider a retail company that wants to analyze sales.

- **Fact:** Sale
- **Grain:** one sold product in one receipt line
- **Measures:** quantity, gross amount, discount amount, net amount
- **Dimensions:** Time, Product, Store, Customer, Promotion

Possible hierarchies:

- **Time:** Day → Month → Quarter → Year
- **Product:** Product → Category → Department
- **Store:** Store → City → Region → Country
- **Customer:** Customer → Segment

Typical queries:

- Total net sales by quarter and region.
- Average discount by product category.
- Quantity sold by promotion and month.

### Shipment example

- **Fact:** Shipment
- **Grain:** one shipment event
- **Measures:** shipped quantity, shipping cost, delay in days
- **Dimensions:** Time, Warehouse, Destination, Carrier, Product

### Hospital example

- **Fact:** Admission
- **Grain:** one patient admission
- **Measures:** stay length, total cost
- **Dimensions:** Time, Ward, Diagnosis, Patient, Physician

> ⚠️ **Exam Tip:** When inventing examples, ensure the dimensions are perspectives of analysis and the measures are consistent with the grain.

## 9. Common mistakes and exam traps

### Mistake 1: confusing facts and dimensions

“Product” is usually a dimension, not a fact. “Product sale” is a fact because it represents an event to analyze.

### Mistake 2: forgetting the grain

Without a precise grain, the meaning of measures becomes unclear. For example, “sales amount” means something different if the row represents a receipt line, a daily store summary, or a monthly regional summary.

### Mistake 3: invalid aggregation

A percentage such as conversion rate is generally non-additive. Summing percentages across rows usually produces meaningless results.

### Mistake 4: weak hierarchies

If the hierarchy is not strict or complete, aggregation may double count or omit values. Good dimensional design checks whether roll-up operations are semantically valid.

### Mistake 5: mixing operational and analytical requirements

A warehouse is not designed with the same priorities as a transactional database. Analytical flexibility, integration, and historical consistency are central goals.

> ⚠️ **Exam Tip:** In conceptual questions, always connect modeling choices to analytical requirements. Professors often reward justified design decisions more than raw definitions.

## 10. Mathematical and analytical exercises

This topic is more conceptual than formula-heavy, but numerical exercises still appear, especially on aggregation and measure interpretation.

### Exercise 1: additive vs non-additive

A fact table stores the following monthly data for two stores:

| Store | Month | Revenue | Margin % |
|---|---|---:|---:|
| A | Jan | 1000 | 20% |
| B | Jan | 2000 | 10% |

#### Question

Can both Revenue and Margin % be summed across stores?

#### Solution

- Revenue is additive, so total revenue is 1000 + 2000 = 3000.
- Margin % is non-additive, so 20% + 10% = 30% is **not** a valid combined margin.
- To compute an overall margin percentage, the underlying absolute margin values are needed.
- If absolute margins are available: store A margin = 200, store B margin = 200, total margin = 400.
- Overall margin percentage = 400 / 3000 = 13.33%.

> ⚠️ **Exam Tip:** If a ratio is given, first ask whether the numerator and denominator are available. If yes, recompute the ratio after aggregation; do not sum the ratios.

### Three similar examples

#### Example 1

A branch has profit margins 25% on 400 euros of sales and 15% on 600 euros of sales. Compute the overall margin.

**Step 1:** Compute absolute margins.

- First part: 0.25 × 400 = 100
- Second part: 0.15 × 600 = 90

**Step 2:** Aggregate margins and sales.

- Total margin = 100 + 90 = 190
- Total sales = 400 + 600 = 1000

**Step 3:** Recompute the ratio.

- Overall margin % = 190 / 1000 = 19%

#### Example 2

Two regions have defect rates 2% over 500 items and 5% over 100 items. Compute the overall defect rate.

**Step 1:** Compute defective items.

- Region 1: 0.02 × 500 = 10
- Region 2: 0.05 × 100 = 5

**Step 2:** Aggregate.

- Total defective items = 15
- Total items = 600

**Step 3:** Ratio.

- Overall defect rate = 15 / 600 = 2.5%

#### Example 3

Class A has pass rate 80% over 20 students, class B has pass rate 50% over 10 students. Compute the overall pass rate.

**Step 1:** Passed students.

- Class A: 0.80 × 20 = 16
- Class B: 0.50 × 10 = 5

**Step 2:** Aggregate.

- Total passed = 21
- Total students = 30

**Step 3:** Ratio.

- Overall pass rate = 21 / 30 = 70%

### Exercise 2: roll-up aggregation

Daily sales for a store are:

| Day | Sales |
|---|---:|
| 1 | 120 |
| 2 | 150 |
| 3 | 130 |
| 4 | 100 |

#### Question

Roll up the data to monthly total and daily average.

#### Solution

- Monthly total = 120 + 150 + 130 + 100 = 500
- Daily average = 500 / 4 = 125

This shows that different aggregate functions answer different analytical questions.

### Three similar examples

#### Example 1

Values: 10, 20, 30

- Sum = 10 + 20 + 30 = 60
- Average = 60 / 3 = 20

#### Example 2

Values: 8, 12, 16, 24

- Sum = 8 + 12 + 16 + 24 = 60
- Average = 60 / 4 = 15

#### Example 3

Values: 200, 100

- Sum = 300
- Average = 150

### Exercise 3: choosing the grain

A company wants to analyze orders. Possible choices are:

1. One row per order.
2. One row per order line.
3. One row per customer per month.

#### Solution

- If the goal is to analyze products within orders, **one row per order line** is the best grain.
- If the goal is only order-level totals, **one row per order** may be sufficient.
- If the goal is monthly customer summaries, **one row per customer per month** may be acceptable, but detailed product-level analysis is lost.

The best grain is the finest level that supports the required analyses without pre-aggregating away useful detail.

### Three similar examples

#### Example 1

Goal: analyze website visits by page and device.

Best grain: one row per page visit event.

Reason: a coarser grain such as one row per day would lose page-level detail.

#### Example 2

Goal: analyze medical prescriptions by drug.

Best grain: one row per prescribed drug item.

Reason: one row per patient visit would merge multiple drugs and reduce analytical precision.

#### Example 3

Goal: analyze airline bookings by route and booking date.

Best grain: one row per booking.

Reason: this preserves the event-level information needed for flexible aggregation.

## 11. Summary checklist

Before the exam, make sure these points are clear:

- Define BI in terms of decision support and integrated historical analysis.
- State the four classical DWH properties: subject-oriented, integrated, time-variant, non-volatile.
- Distinguish OLTP from DWH.
- Define fact, measure, dimension, hierarchy, and grain.
- Explain additive, semi-additive, and non-additive measures.
- Describe roll-up and drill-down.
- Build a small dimensional schema from a business scenario.
- Justify design choices, not only definitions.

> ⚠️ **Exam Tip:** If you can correctly identify the fact, set the grain, choose valid measures, and propose coherent dimensions with hierarchies, you can solve most modeling questions.

## 12. 📝 Review Questions (30 Questions)

### 1. What is Business Intelligence?

Business Intelligence is the collection of methods and tools used to convert organizational data into information that supports decision making. It focuses on analysis, reporting, and strategic insight rather than transaction execution.

### 2. Why is BI different from operational data processing?

Operational processing records business events efficiently, while BI analyzes data across time and organizational perspectives. BI needs integrated and historical data, not just current transaction records.

### 3. Define a Data Warehouse.

A data warehouse is a repository designed for query and analysis. It is classically described as subject-oriented, integrated, time-variant, and non-volatile.

### 4. ⚠️ What does “subject-oriented” mean?

It means the data is organized around major analytical subjects such as sales, customers, or shipments rather than around specific applications or transactions. This supports business analysis more directly.

### 5. What does “integrated” mean in a data warehouse?

Data from multiple sources is cleaned and standardized so that names, codes, units, and formats become consistent. This allows reliable cross-source analysis.

### 6. What does “time-variant” mean?

The warehouse stores historical data over long periods. This makes trend analysis and time-based comparison possible.

### 7. What does “non-volatile” mean?

Warehouse data is mainly loaded and queried rather than continuously updated by operational transactions. Stability supports consistent analytical results.

### 8. ⚠️ Compare OLTP and DWH.

OLTP supports many short operational transactions, often on current normalized data. DWH supports complex read-heavy analytical queries on integrated historical data.

### 9. What is ETL?

ETL means Extract, Transform, Load. It is the pipeline that acquires source data, cleans and integrates it, and loads it into the analytical repository.

### 10. Why is ETL important?

Because source systems are often inconsistent, incomplete, or heterogeneous. ETL improves data quality and prepares data for correct analysis.

### 11. What is a fact in the DFM?

A fact is the central event or phenomenon being analyzed, such as a sale or shipment. It is the core of the multidimensional model.

### 12. What is a measure?

A measure is a numerical quantity associated with a fact and used for analysis. Examples include amount, quantity, and cost.

### 13. What is a dimension?

A dimension is an analysis perspective such as time, product, or store. It answers “by what viewpoint do we analyze the fact?”

### 14. What is a hierarchy?

A hierarchy is an ordered structure of levels inside a dimension, such as Day → Month → Year. It supports aggregation across granularities.

### 15. ⚠️ What is the grain of a fact?

The grain is the precise meaning of one fact occurrence or one fact table row. It defines the level of detail represented in the model.

### 16. Why is the grain so important?

Because measures, dimensions, and allowed queries all depend on it. A vague grain causes ambiguous or incorrect analysis.

### 17. What is an additive measure?

It is a measure that can be summed meaningfully across all dimensions. Sales amount is a standard example.

### 18. What is a semi-additive measure?

It can be summed across some dimensions but not all. Account balance is usually semi-additive because summing it across time is problematic.

### 19. What is a non-additive measure?

It cannot be meaningfully summed. Ratios and percentages are common examples.

### 20. ⚠️ Why can’t percentages usually be summed?

Because each percentage depends on its own denominator. The correct overall percentage must be recomputed from aggregated numerators and denominators.

### 21. What is roll-up?

Roll-up is aggregation from a finer level to a coarser level in a hierarchy. For example, moving from daily sales to monthly sales.

### 22. What is drill-down?

Drill-down is the reverse operation: moving from a summary level to more detail. For example, from region to city to store.

### 23. Give an example of a sales fact model.

Fact: Sale. Measures: quantity, net amount. Dimensions: Time, Product, Store, Customer. Grain: one sold product in one receipt line.

### 24. ⚠️ How do you choose a good fact?

Choose a business event that occurs frequently, is important for analysis, and has meaningful quantitative measures. It must support the analytical goals.

### 25. How do you distinguish a fact from a dimension?

A fact is usually an event to analyze, while a dimension is a descriptive viewpoint used to analyze that event. “Sale” is a fact; “Product” is a dimension.

### 26. What is the difference between star and snowflake schema?

A star schema has one central fact table linked directly to denormalized dimensions. A snowflake schema normalizes some dimensions into multiple linked tables.

### 27. Why is the star schema often preferred for teaching and BI?

Because it is simpler to understand and usually easier for analytical querying. It makes multidimensional reasoning explicit.

### 28. Solve this: revenues are 100 and 300 for two units. What is the total?

Step 1: identify the aggregation. Revenue is additive.

Step 2: sum the values.

100 + 300 = 400.

Therefore, total revenue is 400.

### 29. Solve this: defect rates are 10% over 50 items and 20% over 150 items. What is the overall defect rate?

Step 1: compute defective items.

- First group: 0.10 × 50 = 5
- Second group: 0.20 × 150 = 30

Step 2: aggregate.

- Total defects = 35
- Total items = 200

Step 3: compute the overall rate.

35 / 200 = 17.5%

Therefore, the overall defect rate is 17.5%.

### 30. ⚠️ Design a basic dimensional schema for hotel bookings.

**Step 1:** Identify the fact: Booking.

**Step 2:** Choose the grain: one row per booking.

**Step 3:** Choose measures: nights, booking amount, discount amount.

**Step 4:** Choose dimensions: Time, Hotel, Room Type, Customer, Channel.

**Step 5:** Add hierarchies, for example Time as Day → Month → Year and Hotel as Hotel → City → Country.

This schema supports analysis by time, place, customer type, and booking source.
