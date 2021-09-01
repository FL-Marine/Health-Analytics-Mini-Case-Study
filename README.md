 ![image](https://user-images.githubusercontent.com/74512335/120242435-d903f280-c232-11eb-81f3-7fe8ef892153.png)

# Health-Analytics-Mini-Case-Study
Mini Case Study is from Danny Ma's 8 Week SQL Challenge

***The Objective of this case study is to debug SQL code and assist the General Manager of Health Analytics in answering questions for a board meeting.***

****Prior to answering the questions, SQL best practices encourage understanding the dataset and its fields.****

This dataset includes ***6*** columns which are named as **id, log_date, measure, measure_value, systolic, and diastolic.** 
How this was determined by running this query (See Below).
```sql
SELECT * FROM health.user_logs;
```
**Results:**
| id                                       | log\_date                | measure        | measure\_value | systolic | diastolic |
| ---------------------------------------- | ------------------------ | -------------- | -------------- | -------- | --------- |

Due to the large amount of data, I was initally unsure of how many measures were present, running the query below displayed ***3*** measures **blood_glucose, blood_pressure, weight.**
```sql
SELECT
  (DISTINCT meausure)
FROM
  health.user_logs;
  ```
  **Results:**
  | measure         |
| --------------- |
| blood\_glucose  |
| blood\_pressure |
| weight          |


Questions 1-9 will be displayed as screenshots of the the incorrect code followed by screenshots of the correct code.
 
 **Q.1** 

![image](https://user-images.githubusercontent.com/74512335/120242106-1a47d280-c232-11eb-9045-c4d39a2582bb.png)

This will not run properly due to user_id not being a field to COUNT. The following code will produce the correct answer
```sql
SELECT
  COUNT (DISTINCT id)
FROM
  health.user_logs;
  ```
**Results:**
   | count |
| ----- |
| 554   |

**Q.2-8 I created a temporary table***
```sql
DROP TABLE IF EXISTS user_measure_count;
CREATE TEMP TABLE user_measure_count AS
SELECT 
  id,
  COUNT(*) AS measure_count,
  COUNT (DISTINCT measure) AS unique_measures
FROM health.user_logs
GROUP BY 1;
```
To ensure duplicate tables are not made DROP TABLE IF EXISTS is used followed by a CREATE TEMP TABLE 

**Q.2**

![image](https://user-images.githubusercontent.com/74512335/120244806-34d17a00-c239-11eb-9d87-80977402dee2.png)

The above query is incorrect because the mean is calculated used the AVG function. It must be aliased as the mean_value.
```sql
SELECT
  ROUND (AVG(measure_count), 2) AS mean_value
FROM user_measure_count;
```
**Results:**
| mean\_value |
| ----------- |
| 79.23       |

**Q.3** 

![image](https://user-images.githubusercontent.com/74512335/120243644-2b92de00-c236-11eb-9658-3ffeb56337c7.png)

The question is asking median number of measurements **Per User** must order by measure_count

Median isnt a funciton in PostgresSQL Ordered Set Aggregate Functions is required to get the median value.
```sql
SELECT 
   PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_count) AS median_value 
FROM user_measure_count;
```
**Results:**
| median\_value |
| ----------- |
| 2       |

**Q.4**

![image](https://user-images.githubusercontent.com/74512335/120243732-639a2100-c236-11eb-9262-5a10971d3dcd.png)

This query will not work due to the HAVING clause which filters records from groups based on a specified condition. The correct clause to use is the WHERE clause because it filters data from a specific table.
```sql
SELECT COUNT(*)
FROM user_measure_count
WHERE measure_count >= 3;
```
**Results:**
| count |
| ----------- |
| 209   |

**Q.5**

![image](https://user-images.githubusercontent.com/74512335/120243753-6e54b600-c236-11eb-802b-776e6248295c.png)

Summing id values doesn't make sense because id values are individual per user and will not produce the intended results. The question is asking how many users which needs the COUNT clause.
```sql
SELECT COUNT(*)
FROM user_measure_count
WHERE measure_count >= 1000;
```
**Results:**
| count |
| ----------- |
| 5  |

**Q.6**

![image](https://user-images.githubusercontent.com/74512335/120243770-77de1e00-c236-11eb-9ae6-2f3a2acfca52.png)

COUNT DISTINCT needs to be inside () 'blood_sugar' needs to be replaced with 'blood_glucose'
```
SELECT 
  COUNT(DISTINCT id)
FROM health.user_logs
WHERE measure = 'blood_glucose';
```
**Results:**
| count |
| ----------- |
| 325  |

**Q.7**

![image](https://user-images.githubusercontent.com/74512335/120243784-81678600-c236-11eb-85ba-5b352451d57e.png)

COUNT(DISTINCT measures) does not exist because it was aliased using the AS clause to unique_measures.
```sql
SELECT 
  COUNT(*)
FROM user_measure_count
WHERE unique_measures >= 2;
```
**Results:**
| count |
| ----------- |
| 204  |

**Q.8**

![image](https://user-images.githubusercontent.com/74512335/120243801-8a585780-c236-11eb-8e18-5ab8ec53dd55.png)

This question is asking how many users have all 3 measurements. User was not fully spelled in the FROM clause.
```sql
SELECT
  COUNT(*)
FROM user_measure_count
WHERE unique_measures = 3;
```
**Results:**
| count |
| ----------- |
| 50  |

**Q.9**

![image](https://user-images.githubusercontent.com/74512335/120243821-93492900-c236-11eb-9911-3c8bdfbd4ab3.png)

This code will not run properly because GROUP clause is missing after each WITHIN clause. blood_pressure is a string and needs to be in single quotes 'blood_pressure'.
```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY systolic) AS median_systolic,
  PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY diastolic) AS median_diastolic
FROM health.user_logs
WHERE measure = 'blood_pressure';
```
**Results:**
| median\_systolic | median\_diastolic |
| ---------------- | ----------------- |
| 126              | 79                |

# Lessons Learned

Debugging Code

Summary statisitcs (mean & median)

Sorting & filtering Data

Dropping & Creating CTE's
