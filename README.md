# credit_financial_dash
Power BI Dash
Project Objective:

To develop a comprehensive credit card weekly dashboard that provides real time insights into key performace metrics and trends enabling stakeholders
to monitor and analyze credit card operations effectively.

For My SQL the Server Comes as 127.0.0.1:3306
Database name


Queries To Extract take data from SQL and then linked it to Power BI
Create a database 
CREATE DATABASE ccdb;

-- 1. Create cc_detail table

CREATE TABLE cc_detail (
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);


-- 2. Create cc_detail table

CREATE TABLE cust_detail (
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
);


-- 3. Copy csv data into SQL (remember to update the file name and file location in below query)

-- copy cc_detail table

COPY cc_detail
FROM 'D:\credit_card.csv' 
DELIMITER ',' 
CSV HEADER;


-- copy cust_detail table

COPY cust_detail
FROM 'D:\customer.csv' 
DELIMITER ',' 
CSV HEADER;



-- If you are getting below error, then use the below point:  
   -- ERROR:  date/time field value out of range: "0"
   -- HINT:  Perhaps you need a different "datestyle" setting.

-- Check the Data in Your CSV File: Ensure date column values are formatted correctly and are in a valid format that PostgreSQL can recognize (e.g., YYYY-MM-DD). And correct any incorrect or missing date values in the CSV file. 
   -- or
-- Update the Datestyle Setting: Set the datestyle explicitly for your session using the following command:
SET datestyle TO 'ISO, DMY';

-- Now, try to COPY the csv files!


-- 4. Insert additional data into SQL, using same COPY function

-- copy additional data (week-53) in cc_detail table

COPY cc_detail
FROM 'D:\cc_add.csv' 
DELIMITER ',' 
CSV HEADER;


-- copy additional data (week-53) in cust_detail table (remember to update the file name and file location in below query)

COPY cust_detail
FROM 'D:\cust_add.csv' 
DELIMITER ',' 
CSV HEADER;

-Created 2 Columns using SWITCH DAX functions to create 1. AgeGroup and 2. IncomeGroup in cust_detail.
- Created column Revenue  by adding 1. Annual Fees 2. Total transaction amount 3. Interest Earned
-Created WeekNumber column as the initial column was not getting sorted, so created a seperate column named Week_Num2 using WEEKNUM DAX functions.
- Created a column Current_Week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[Week_Num2] = MAX('public cc_detail'[Week_Num2]))), current week revenue where the CALCULATE function will take the SUM of Revenue
and filter ALL contents from the cc_detail table basis the condition that the Weeknum2 = Max of WeekNum.

Designed the Dashboard

Project Insights:

Wow Change:
1.Revenue increased by 28.8%.
Overview YTD:
1. Overall revenue is 57M
2. Total interest is 7.84 M
3. Total transaction amount is 46 M
4. Male customers are contributing more in revenue 31 M, female 26 M.
5. Blue, Silver credit card are contributing to 93 % of overall transaction.
6. TX, NY and CA is contributing to 68 %.
7. Overall Activation rate is 57.5 %.
8. Overall deliquent rate is 6.06 %.


