 ![image](https://user-images.githubusercontent.com/74512335/120242435-d903f280-c232-11eb-81f3-7fe8ef892153.png)

# Health-Analytics-Mini-Case-Study
Mini Case Study is from Danny Ma's 8 Week SQL Challenge

***The Objective of this case study is to debug SQL code and assist the General Manager of Health Analytics in answering questions for a board meeting.***

****Prior to answering the questions, SQL best practices encourage understanding the dataset and its fields.****

This dataset includes ***6*** columns which are named as **id, log_date, measure, measure_value, systolic, and diastolic.** 
How this was determined by running this query (See Below).

![image](https://user-images.githubusercontent.com/74512335/120244619-b5dc4180-c238-11eb-91fa-ddb4a6e52b78.png)

Due to the large amount of data, I was initally unsure of how many measures were present, running the query below displayed ***3*** measures **blood_glucose, blood_pressure, weight.**

![image](https://user-images.githubusercontent.com/74512335/120244560-86c5d000-c238-11eb-80d0-cae87d81026c.png)

Questions 1-9 will be displayed as screenshots of the the incorrect code followed by screenshots of the correct code.
 
 Q.1 

![image](https://user-images.githubusercontent.com/74512335/120242106-1a47d280-c232-11eb-9045-c4d39a2582bb.png)

This will not run properly due to user_id not being a field to COUNT. The following code will produce the correct answer

![image](https://user-images.githubusercontent.com/74512335/120315405-54ed5180-c2aa-11eb-8a0c-a5491221f482.png)


Q.2-8 

![image](https://user-images.githubusercontent.com/74512335/120242853-ef5e7e00-c233-11eb-94f6-c594cd501850.png)

![image](https://user-images.githubusercontent.com/74512335/120316235-320f6d00-c2ab-11eb-931d-02be774f64e4.png)

To ensure duplicate tables are not made DROP TABLE IF EXISTS is used followed by a CREATE TEMP TABLE 

Q.2

![image](https://user-images.githubusercontent.com/74512335/120244806-34d17a00-c239-11eb-9d87-80977402dee2.png)

The above query is incorrect because the mean is calculated used the AVG function. It must be aliased as the mean_value.

![image](https://user-images.githubusercontent.com/74512335/120317177-50c23380-c2ac-11eb-9104-a85d91606dc1.png)

Q.3 

![image](https://user-images.githubusercontent.com/74512335/120243644-2b92de00-c236-11eb-9658-3ffeb56337c7.png)

The question is asking median number of measurements **Per User** must order by measure_count

Median isnt a funciton in PostgresSQL Ordered Set Aggregate Functions is required to get the median value.

![image](https://user-images.githubusercontent.com/74512335/120318572-03df5c80-c2ae-11eb-8a70-bf369a10e1ed.png)

Q.4

![image](https://user-images.githubusercontent.com/74512335/120243732-639a2100-c236-11eb-9262-5a10971d3dcd.png)

This query will not work due to the HAVING clause which filters records from groups based on a specified condition. The correct clause to use is the WHERE clause because it filters data from a specific table.

![image](https://user-images.githubusercontent.com/74512335/120454445-e370da00-c361-11eb-9948-438f472d1f44.png)

Q.5

![image](https://user-images.githubusercontent.com/74512335/120243753-6e54b600-c236-11eb-802b-776e6248295c.png)

Summing id values doesn't make sense because id values are individual per user and will not produce the intended results. The question is asking how many users which needs the COUNT clause.

![image](https://user-images.githubusercontent.com/74512335/120455673-f7690b80-c362-11eb-9125-ef8516d4a983.png)

Q.6

![image](https://user-images.githubusercontent.com/74512335/120243770-77de1e00-c236-11eb-9ae6-2f3a2acfca52.png)

COUNT DISTINCT needs to be inside () 'blood_sugar' needs to be replaced with 'blood_glucose'

![image](https://user-images.githubusercontent.com/74512335/120456531-b4f3fe80-c363-11eb-9875-a72009d7b3ae.png)

Q.7

![image](https://user-images.githubusercontent.com/74512335/120243784-81678600-c236-11eb-85ba-5b352451d57e.png)

COUNT(DISTINCT measures) does not exist because it was aliased using the AS clause to unique_measures.

![image](https://user-images.githubusercontent.com/74512335/120457757-b540c980-c364-11eb-958f-14325ac36993.png)

Q.8

![image](https://user-images.githubusercontent.com/74512335/120243801-8a585780-c236-11eb-8e18-5ab8ec53dd55.png)

This question is asking how many users have all 3 measurements. User was not fully spelled in the FROM clause.

![image](https://user-images.githubusercontent.com/74512335/120567537-0d68e180-c3e0-11eb-943f-67d6516ace0f.png)


Q.9

![image](https://user-images.githubusercontent.com/74512335/120243821-93492900-c236-11eb-9911-3c8bdfbd4ab3.png)

This code will not run properly because GROUP clause is missing after each WITHIN clause. blood_pressure is a string and needs to be in single quotes 'blood_pressure'.

![image](https://user-images.githubusercontent.com/74512335/120615587-6d827680-c426-11eb-9ad7-486213d759a5.png)
