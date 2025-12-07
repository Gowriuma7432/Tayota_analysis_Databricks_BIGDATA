# Tayota_analysis_Databricks_BIGDATA
Toyota Car Price &amp; Specs Analytics built in Databricks. Includes ingestion, cleaning, feature engineering, PySpark analysis, SQL insights, and an interactive dashboard visualizing model pricing, depreciation trends, fuel type vs transmission performance, and engine efficiency.


* **toyota.csv** (dataset)
* **Toyota_analysis_notebook.ipynb** (PySpark ingestion, cleaning, feature engineering, notebook analysis)
* **Toyota_analyisis_SQL_Queries.ipynb** (SQL analysis)
* **Toyota Car Price & Specs Dashboard.lvdash.json** (dashboard export, with full metadata including the SQL queries and visuals) 

# 📁 **Repository Contents (Flat Main Branch Structure)**

All files are uploaded directly into the main branch:

| File                                               | Description                                                                                |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **toyota.csv**                                     | Raw dataset containing Toyota car listings and attributes                                  |
| **Toyota_analysis_notebook.ipynb**                 | PySpark notebook for ingestion, cleaning, feature engineering, and notebook-level analysis |
| **Toyota_analyisis_SQL_Queries.ipynb**             | SQL queries powering insights and dashboard visuals                                        |
| **Toyota Car Price & Specs Dashboard.lvdash.json** | Databricks dashboard export including charts, KPIs, and filters                            |
| **README.md**                                      | Project documentation                                                                      |

---

# 🚗 **Dataset Description**

`toyota.csv` contains detailed car listing information such as:

* Model
* Year
* Price
* Mileage
* Fuel Type
* Transmission
* Tax
* MPG (Miles per Gallon)
* Engine Size

This dataset helps analyze:

* Depreciation trends
* Fuel type & transmission price effects
* Mileage vs price relationships
* Engine efficiency factors

---

# 🧵 **1. Data Ingestion (4.1)**

The dataset was uploaded to Databricks using **Create Table → Upload File**.

Inside the notebook:

```python
spark.sql("USE CATALOG workspace")
spark.sql("USE SCHEMA toyota_project")

df_raw = spark.table("toyota_raw")
```

This loads the dataset into Spark for cleaning and transformation.

---

# 🧼 **2. Data Cleaning & Feature Engineering (4.2)**

Performed in **Toyota_analysis_notebook.ipynb**.

### ✔ Cleaning Performed

* Selected essential columns
* Casted numeric values (year, mileage, price, engineSize, mpg, tax)
* Removed rows with missing or invalid values

### ✔ Engineered Features

To make the project richer and different from others, new fields were created:

* **car_age = 2020 – year**
* **price_per_mile = price ÷ mileage (safe divide)**
* **fuel_efficiency_index = mpg ÷ engineSize**
* **depreciation_score = price ÷ car_age**
* **mileage_band** (Low, Medium, High, Very High)

These features appear in multiple SQL queries and dashboard visuals.

---

# 💾 **3. Delta Table Storage (4.3)**

Cleaned data stored as:

```python
df.write.format("delta")
  .mode("overwrite")
  .saveAsTable("workspace.toyota_project.toyota_clean")
```

Delta format ensures fast, reliable SQL analytics.

---

# 📘 **4. Notebook Analysis (4.5)**

Inside **Toyota_analysis_notebook.ipynb**, several analyses were conducted:

* Summary statistics
* Price vs mileage trends
* Scatter plot visualization using `display()`
* Engine size vs efficiency comparisons

One main notebook chart was created:
**Price vs Mileage by Fuel Type** (using Databricks Plot tab)

This satisfies the professor’s requirement for notebook-based analysis.

---

# 🧮 **5. SQL Analysis (4.4)**

All SQL queries appear inside **Toyota_analyisis_SQL_Queries.ipynb** and are also referenced inside the dashboard metadata.
Below are the SQL insights extracted from the dashboard JSON (actual queries used). 

---

### ✔ **A. Top 10 Most Expensive Toyota Models**

```sql
SELECT model,
       ROUND(AVG(price), 2) AS avg_price,
       COUNT(*) AS num_cars
FROM toyota_clean
GROUP BY model
HAVING COUNT(*) >= 10
ORDER BY avg_price DESC
LIMIT 10;
```

---

### ✔ **B. Average Price by Fuel Type & Transmission**

```sql
SELECT fuelType,
       transmission,
       ROUND(AVG(price), 2) AS avg_price,
       COUNT(*) AS num_cars
FROM toyota_clean
GROUP BY fuelType, transmission
ORDER BY avg_price DESC;
```

---

### ✔ **C. Depreciation Trend: Price vs Car Age**

```sql
SELECT car_age,
       ROUND(AVG(price), 2) AS avg_price,
       ROUND(AVG(depreciation_score), 2) AS avg_depreciation_score
FROM toyota_clean
GROUP BY car_age
ORDER BY car_age;
```

---

### ✔ **D. Engine Size vs Fuel Efficiency Index**

```sql
SELECT engineSize,
       ROUND(AVG(fuel_efficiency_index), 3) AS avg_fuel_efficiency,
       ROUND(AVG(mpg), 2) AS avg_mpg,
       COUNT(*) AS num_cars
FROM toyota_clean
GROUP BY engineSize
ORDER BY engineSize;
```

---

### ✔ **E. Price Distribution (Histogram Query)**

```sql
SELECT price FROM toyota_clean;
```

---

### ✔ **F. KPI Query**

```sql
SELECT COUNT(*) AS total_cars,
       ROUND(AVG(price), 2) AS avg_price,
       MAX(price) AS max_price,
       ROUND(AVG(mileage),2) AS avg_mileage
FROM toyota_clean;
---

# 📊 **6. Dashboard (4.6)**

Dashboard: **Toyota Car Price & Specs Dashboard.lvdash.json**

Extracted from dashboard metadata: 

### ✔ Visuals Included

* **Bar Chart** → Top 10 most expensive Toyota models
* **Heatmap** → Price by fuel type & transmission
* **Line Chart** → Price vs car age (depreciation trend)
* **Scatter Plot** → Engine size vs fuel efficiency index
* **Histogram** → Car price distribution

### ✔ KPI Tiles

* Total Cars
* Average Mileage
* Highest Car Price

### ✔ Global Filters

* fuelType
* transmission
* model

These filters make the dashboard fully interactive.

---

# 🏁 **7. Summary**

This project demonstrates full mastery of the Databricks workflow:

* ✔ Data ingestion
* ✔ Cleaning & feature engineering
* ✔ Delta Lake storage
* ✔ Notebook analysis
* ✔ SQL analytics
* ✔ Interactive dashboard


