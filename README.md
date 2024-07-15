# HR DATA ANALYSIS - SQL SERVER 2022 / POWER BI

This project dives deep into the realm of data analysis using SQL and Power BI to uncover important human resource insights that can greatly benefit the company. Featuring eye-catching dashboards offer crucial HR metrics like employee turnover, diversity, recruitment efficacy and performance evaluations. These help HR professionals make informed decisions and strategic workforce planning.

# Source Data:

The source data contained Human Resource 22000 records from 2000 to 2020. This is included in the repository.

# Data Cleaning & Analysis:

This was done on SQL server 2022 involving

- Data loading & inspection
- Handling missing values
- Data cleaning and analysis

# Data Visualization:

![image](https://github.com/user-attachments/assets/d327e5f3-9377-477a-ad96-6bc2df3aae73)

![image](https://github.com/user-attachments/assets/51f8b13f-9f71-4bee-a183-0d6e9df9509c)

# Exploratory Data Analysis
#Questions:

- What's the age distribution in the company?
- What's the gender breakdown in the company?
- How does gender vary across departments and job titles?
- What's the race distribution in the company?
- What's the average length of employment in the company?
- Which department has the highest turnover rate?
- What is the tenure distribution for each department?
- How many employees work remotely for each department?
- What's the distribution of employees across different states?
- How are job titles distributed in the company?
- How have employee hire counts varied over time?

# Findings 

- There are more male employees than female or non-conforming employees.
- The genders are fairly evenly distributed across departments. There are slightly more male employees overall.
- Employees 21-30 years old are the fewest in the company. Most employees are 31-50 years old. Surprisingly, the age group 50+ have the most employees in the company.
- Caucasian employees are the majority in the company, followed by mixed race, black, Asian, Hispanic, and native Americans.
- The average length of employment is 7 years.
- Auditing has the highest turnover rate, followed by Legal, Research & Development and Training. Business Development & Marketing have the lowest turnover rates.
- Employees tend to stay with the company for 6-8 years. Tenure is quite evenly distributed across departments.
- About 25% of employees work remotely.
- Most employees are in Ohio (14,788) followed distantly by Pennsylvania (930) and Illinois (730), Indiana (572), Michigan (569), Kentucky (375) and Wisconsin (321).
- There are 182 job titles in the company, with Research Assistant II taking most of the employees (634) and Assistant Professor, - Marketing Manager, Office Assistant IV, Associate Professor and VP of Training and Development taking the just 1 employee each.
- Employee hire counts have increased over the years.


# 1) Create Database

CREATE DATABASE hr;

# 2) Import Data to SQL Server
- Right-click on Human_Resources > Tasks > Import Data
- Verify that the import worked:

<pre>use hr;</pre>

<pre>SELECT *
FROM hr_data;</pre>

