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
<h3>Problem Statements:</h3>

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

<h3> Findings</h3> 

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


<h3>1) Create Database</h3>

<pre>CREATE DATABASE hr;</pre>

<h3>2) Import Data to SQL Server</h3>
  
- Right-click on Human_Resources > Tasks > Import Data
- Verify that the import worked:

  
<pre>use hr;</pre>

<pre>SELECT *
FROM hr_data;</pre>

<h3>3) DATA CLEANING</h3>
The termdate was imported as nvarchar(50). This column contains termination dates, hence it needs to be converted to the date format.

<h5>Update date/time to date</h5>

![image](https://github.com/user-attachments/assets/f5a01e0b-6208-4d6d-ab04-90386c35f24c)

Update termdate date/time to date

- Convert dates to yyyy-MM-dd
- Create new column new_termdate
- Copy converted time values from termdate to new_termdate
  <br>
- Convert dates to yyyy-MM-dd
  
<pre>UPDATE hr_data
SET termdate = FORMAT(CONVERT(DATETIME, LEFT(termdate, 19), 120), 'yyyy-MM-dd');</pre>

- create new column new_termdate
  
<pre>ALTER TABLE hr_data
ADD new_termdate DATE;</pre>

- copy converted time values from termdate to new_termdate
  
<pre>UPDATE hr_data
SET new_termdate = CASE
 WHEN termdate IS NOT NULL AND ISDATE(termdate) = 1 THEN CAST(termdate AS DATETIME) ELSE NULL
 END;</pre>

- check results
  
<pre>SELECT new_termdate
FROM hr_data;</pre>

- create new column "age"
  
<pre>ALTER TABLE hr_data
ADD age nvarchar(50)</pre>

- populate new column with age
  
<pre>UPDATE hr_data
SET age = DATEDIFF(YEAR, birthdate, GETDATE());</pre>

# QUESTIONS TO ANSWER FROM THE DATA

<h3>1) What's the age distribution in the company?</h3>

- age distribution
  
<pre>SELECT
 MIN(age) AS youngest,
 MAX(age) AS OLDEST
FROM hr_data;</pre>

- age group count
  
<pre>SELECT age_group,
count(*) AS count
FROM
(SELECT 
 CASE
  WHEN age <= 21 AND age <= 30 THEN '21 to 30'
  WHEN age <= 31 AND age <= 40 THEN '31 to 40'
  WHEN age <= 41 AND age <= 50 THEN '41 to 50'
  ELSE '50+'
  END AS age_group
 FROM hr_data
 WHERE new_termdate IS NULL
 ) AS subquery
GROUP BY age_group
ORDER BY age_group;</pre>
                     
- Age group by gender
  
<pre>SELECT age_group,
gender,
count(*) AS count
FROM
(SELECT 
 CASE
  WHEN age <= 21 AND age <= 30 THEN '21 to 30'
  WHEN age <= 31 AND age <= 40 THEN '31 to 40'
  WHEN age <= 41 AND age <= 50 THEN '41 to 50'
  ELSE '50+'
  END AS age_group,
  gender
 FROM hr_data
 WHERE new_termdate IS NULL
 ) AS subquery
GROUP BY age_group, gender
ORDER BY age_group, gender;</pre>

<h3>2) What's the gender breakdown in the company?</h3>
  
<pre><>SELECT
 gender,
 COUNT(gender) AS count
FROM hr_data
WHERE new_termdate IS NULL
GROUP BY gender
ORDER BY gender ASC;</pre>

<h3>3) How does gender vary across departments and job titles?</h3>

<pre>SELECT 
department,
gender,
count(gender) AS count
FROM hr_data
WHERE new_termdate IS NULL
GROUP BY department, gender,
ORDER BY department, gender ASC;</pre>

- job titles
  
<pre>SELECT 
department, jobtitle,
gender,
count(gender) AS count
FROM hr_data
WHERE new_termdate IS NULL
GROUP BY department, jobtitle, gender
ORDER BY department, jobtitle, gender ASC;</pre>

<h3>4) What's the race distribution in the company?</h3>

<pre>SELECT
race,
count(*) AS count
FROM
hr_data
WHERE new_termdate IS NULL 
GROUP BY race
ORDER BY count DESC;</pre>

<h3>5) What's the average length of employment in the company?</h3>

<pre>SELECT 
AVG(DATEDIFF(year, hire_date, new_termdate)) AS tenure
FROM hr_data
WHERE new_termdate IS NOT NULL AND new_termdate <= GETDATE();</pre>

<h3>6) Which department has the highest turnover rate?</h3>
- get total count
- get terminated count
- terminated count/total count

<pre>SELECT
 department,
 total_count,
 terminated_count,
 (round((CAST(terminated_count AS FLOAT)/total_count), 2)) * 100 AS turnover_rate
 FROM
	(SELECT 
	 department,
	 count(*) AS total_count,
	 SUM(CASE
		WHEN new_termdate IS NOT NULL AND new_termdate <= GETDATE() THEN 1 ELSE 0
		END
		) AS terminated_count
	FROM hr_data
	GROUP BY department
	) AS subquery
ORDER BY turnover_rate DESC;</pre>

<h3>7) What is the tenure distribution for each department?</h3>

<pre>SELECT 
    department,
    AVG(DATEDIFF(year, hire_date, new_termdate)) AS tenure
FROM 
    hr_data
WHERE 
    new_termdate IS NOT NULL 
    AND new_termdate <= GETDATE()
GROUP BY 
    department;</pre>
    
<h3>8) How many employees work remotely for each department?</h3>

<pre>SELECT
 location,
 count(*) as count
FROM hr_data
WHERE new_termdate IS NULL
GROUP BY location;</pre>

<h3>9) What's the distribution of employees across different states?</h3>

<pre>SELECT 
 location_state,
 count(*) AS count
FROM hr_data
WHERE new_termdate IS NULL
GROUP BY location_state
ORDER BY count DESC;</pre>

<h3>10) How are job titles distributed in the company?</h3>

<pre>SELECT 
 jobtitle,
 count(*) AS count
 FROM hr_data
 WHERE new_termdate IS NULL
 GROUP BY jobtitle
 ORDER BY count DESC;</pre>
 
<h3>11) How have employee hire counts varied over time?</h3>

- calculate hires
- calculate terminations
- (hires-terminations)/hires percent hire change

<pre>SELECT
 hire_year,
 hires,
 terminations,
 hires - terminations AS net_change,
 (round(CAST(hires-terminations AS FLOAT)/hires, 2)) * 100 AS percent_hire_change
 FROM
	(SELECT 
	 YEAR(hire_date) AS hire_year,
	 count(*) AS hires,
	 SUM(CASE
			WHEN new_termdate is not null and new_termdate <= GETDATE() THEN 1 ELSE 0
			END
			) AS terminations
	FROM hr_data
	GROUP BY YEAR(hire_date)
	) AS subquery
ORDER BY percent_hire_change ASC;</pre>
