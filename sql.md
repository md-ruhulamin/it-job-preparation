# 📚 SQL Learning Cheat Sheet

A comprehensive guide to mastering SQL queries with practical examples using aggregate functions, joins, GROUP BY, HAVING, and more!

---

## 📋 Table of Contents
- [Database Setup](#database-setup)
- [Table Structures](#table-structures)
- [Sample Data](#sample-data)
- [SQL Queries & Examples](#sql-queries--examples)
  - [Basic SELECT Queries](#basic-select-queries)
  - [Aggregate Functions](#aggregate-functions)
  - [GROUP BY Clause](#group-by-clause)
  - [HAVING Clause](#having-clause)
  - [JOIN Operations](#join-operations)
  - [Advanced Queries](#advanced-queries)

---

## 🗄️ Database Setup

### Table Structures

#### 1. EmployDetails Table
```sql
CREATE TABLE EmployDetails (
    EmpId INT PRIMARY KEY,
    FullName VARCHAR(100),
    Country VARCHAR(50),
    ManagerId INT,
    DateOfJoining DATE
);
```

#### 2. EmploySalary Table
```sql
CREATE TABLE EmploySalary (
    EmpId INT,
    ProjectName VARCHAR(100),
    Salary DECIMAL(10,2),
    FOREIGN KEY (EmpId) REFERENCES EmployDetails(EmpId)
);
```

---

## 📊 Sample Data

### EmployDetails Table

| EmpId | FullName | Country | ManagerId | DateOfJoining |
|-------|----------|---------|-----------|---------------|
| 1 | Ruhul Amin | Bangladesh | 101 | 2022-01-15 |
| 2 | John Smith | USA | 102 | 2021-07-10 |
| 3 | Maria Garcia | Spain | 103 | 2020-03-25 |
| 4 | Chen Wei | China | 104 | 2023-05-05 |
| 5 | Md.Ruhul Amin | Brazil | 104 | 2022-03-25 |
| 6 | John Smith | Bangladesh | 102 | 2021-07-10 |
| 7 | Jhon Snow | Spain | 104 | 2020-03-25 |
| 8 | Chen Chung | Spain | 101 | 2023-05-05 |

### EmploySalary Table

| EmpId | ProjectName | Salary |
|-------|-------------|--------|
| 1 | Project Alpha | 75000 |
| 2 | Project Beta | 68000 |
| 3 | Project Gamma | 72000 |
| 4 | Project Delta | 80000 |
| 1 | Project Epsilon | 76000 |
| 2 | Project Zeta | 69000 |
| 2 | Project Alpha | 71000 |

---

## 🔍 SQL Queries & Examples

### Basic SELECT Queries

#### Q1: Get all employees from Spain

```sql
SELECT * FROM EmployDetails
WHERE Country = 'Spain';
```

**Result:**

| EmpId | FullName | Country | ManagerId | DateOfJoining |
|-------|----------|---------|-----------|---------------|
| 3 | Maria Garcia | Spain | 103 | 2020-03-25 |
| 7 | Jhon Snow | Spain | 104 | 2020-03-25 |
| 8 | Chen Chung | Spain | 101 | 2023-05-05 |

---

### Aggregate Functions

#### Q2: Count total number of employees

```sql
SELECT COUNT(*) AS TotalEmployees 
FROM EmployDetails;
```

**Result:**

| TotalEmployees |
|----------------|
| 8 |

---

#### Q3: Find the maximum salary

```sql
SELECT MAX(Salary) AS MaxSalary 
FROM EmploySalary;
```

**Result:**

| MaxSalary |
|-----------|
| 80000 |

---

#### Q4: Calculate average salary across all projects

```sql
SELECT AVG(Salary) AS AverageSalary 
FROM EmploySalary;
```

**Result:**

| AverageSalary |
|---------------|
| 73000 |

---

### GROUP BY Clause

#### Q5: Count employees by country

```sql
SELECT Country, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY Country
ORDER BY EmployeeCount DESC;
```

**Result:**

| Country | EmployeeCount |
|---------|---------------|
| Spain | 3 |
| Bangladesh | 2 |
| USA | 1 |
| China | 1 |
| Brazil | 1 |

---

#### Q6: Calculate total salary per employee

```sql
SELECT EmpId, SUM(Salary) AS TotalSalary
FROM EmploySalary
GROUP BY EmpId
ORDER BY TotalSalary DESC;
```

**Result:**

| EmpId | TotalSalary |
|-------|-------------|
| 2 | 208000 |
| 1 | 151000 |
| 4 | 80000 |
| 3 | 72000 |

---

#### Q7: Count projects per employee

```sql
SELECT EmpId, COUNT(*) AS ProjectCount
FROM EmploySalary
GROUP BY EmpId
ORDER BY ProjectCount DESC;
```

**Result:**

| EmpId | ProjectCount |
|-------|--------------|
| 2 | 3 |
| 1 | 2 |
| 3 | 1 |
| 4 | 1 |

---

### HAVING Clause

#### Q8: Find employees working on more than 1 project

```sql
SELECT EmpId, COUNT(*) AS ProjectCount
FROM EmploySalary
GROUP BY EmpId
HAVING COUNT(*) > 1;
```

**Result:**

| EmpId | ProjectCount |
|-------|--------------|
| 1 | 2 |
| 2 | 3 |

---

#### Q9: Find employees with total salary greater than 150000

```sql
SELECT EmpId, SUM(Salary) AS TotalSalary
FROM EmploySalary
GROUP BY EmpId
HAVING SUM(Salary) > 150000;
```

**Result:**

| EmpId | TotalSalary |
|-------|-------------|
| 1 | 151000 |
| 2 | 208000 |

---

#### Q10: Countries with more than 1 employee

```sql
SELECT Country, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY Country
HAVING COUNT(*) > 1;
```

**Result:**

| Country | EmployeeCount |
|---------|---------------|
| Spain | 3 |
| Bangladesh | 2 |

---

### JOIN Operations

#### Q11: Get employee details with their salaries (INNER JOIN)

```sql
SELECT e.EmpId, e.FullName, e.Country, s.ProjectName, s.Salary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
ORDER BY e.EmpId;
```

**Result:**

| EmpId | FullName | Country | ProjectName | Salary |
|-------|----------|---------|-------------|--------|
| 1 | Ruhul Amin | Bangladesh | Project Alpha | 75000 |
| 1 | Ruhul Amin | Bangladesh | Project Epsilon | 76000 |
| 2 | John Smith | USA | Project Beta | 68000 |
| 2 | John Smith | USA | Project Zeta | 69000 |
| 2 | John Smith | USA | Project Alpha | 71000 |
| 3 | Maria Garcia | Spain | Project Gamma | 72000 |
| 4 | Chen Wei | China | Project Delta | 80000 |

---

#### Q12: Get all employees and their projects (LEFT JOIN)

```sql
SELECT e.EmpId, e.FullName, e.Country, s.ProjectName, s.Salary
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId
ORDER BY e.EmpId;
```

**Result:**

| EmpId | FullName | Country | ProjectName | Salary |
|-------|----------|---------|-------------|--------|
| 1 | Ruhul Amin | Bangladesh | Project Alpha | 75000 |
| 1 | Ruhul Amin | Bangladesh | Project Epsilon | 76000 |
| 2 | John Smith | USA | Project Beta | 68000 |
| 2 | John Smith | USA | Project Zeta | 69000 |
| 2 | John Smith | USA | Project Alpha | 71000 |
| 3 | Maria Garcia | Spain | Project Gamma | 72000 |
| 4 | Chen Wei | China | Project Delta | 80000 |
| 5 | Md.Ruhul Amin | Brazil | NULL | NULL |
| 6 | John Smith | Bangladesh | NULL | NULL |
| 7 | Jhon Snow | Spain | NULL | NULL |
| 8 | Chen Chung | Spain | NULL | NULL |

---

### Advanced Queries

#### Q13: Get employee name and their total earnings

```sql
SELECT e.FullName, e.Country, SUM(s.Salary) AS TotalEarnings
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.EmpId, e.FullName, e.Country
ORDER BY TotalEarnings DESC;
```

**Result:**

| FullName | Country | TotalEarnings |
|----------|---------|---------------|
| John Smith | USA | 208000 |
| Ruhul Amin | Bangladesh | 151000 |
| Chen Wei | China | 80000 |
| Maria Garcia | Spain | 72000 |

---

#### Q14: Find employees earning more than average salary

```sql
SELECT e.FullName, s.ProjectName, s.Salary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE s.Salary > (SELECT AVG(Salary) FROM EmploySalary)
ORDER BY s.Salary DESC;
```

**Result:**

| FullName | ProjectName | Salary |
|----------|-------------|--------|
| Chen Wei | Project Delta | 80000 |
| Ruhul Amin | Project Epsilon | 76000 |
| Ruhul Amin | Project Alpha | 75000 |

---

#### Q15: Count employees by manager with country info

```sql
SELECT e.ManagerId, COUNT(DISTINCT e.EmpId) AS EmployeeCount
FROM EmployDetails e
GROUP BY e.ManagerId
ORDER BY EmployeeCount DESC;
```

**Result:**

| ManagerId | EmployeeCount |
|-----------|---------------|
| 104 | 3 |
| 102 | 2 |
| 101 | 2 |
| 103 | 1 |

---

#### Q16: Get employees with highest salary per country

```sql
SELECT e.Country, e.FullName, MAX(s.Salary) AS HighestSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country, e.FullName
ORDER BY HighestSalary DESC;
```

**Result:**

| Country | FullName | HighestSalary |
|---------|----------|---------------|
| China | Chen Wei | 80000 |
| Bangladesh | Ruhul Amin | 76000 |
| Spain | Maria Garcia | 72000 |
| USA | John Smith | 71000 |

---

#### Q17: Find employees who joined in 2022 or later with their projects

```sql
SELECT e.FullName, e.DateOfJoining, s.ProjectName, s.Salary
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE YEAR(e.DateOfJoining) >= 2022
ORDER BY e.DateOfJoining;
```

**Result:**

| FullName | DateOfJoining | ProjectName | Salary |
|----------|---------------|-------------|--------|
| Ruhul Amin | 2022-01-15 | Project Alpha | 75000 |
| Ruhul Amin | 2022-01-15 | Project Epsilon | 76000 |
| Md.Ruhul Amin | 2022-03-25 | NULL | NULL |
| Chen Wei | 2023-05-05 | Project Delta | 80000 |
| Chen Chung | 2023-05-05 | NULL | NULL |

---

### COUNT Function - Advanced Examples

#### Q18: Count distinct countries in employee table

```sql
SELECT COUNT(DISTINCT Country) AS UniqueCountries
FROM EmployDetails;
```

**Result:**

| UniqueCountries |
|-----------------|
| 5 |

---

#### Q19: Count employees who have projects vs those who don't

```sql
SELECT 
    COUNT(DISTINCT CASE WHEN s.EmpId IS NOT NULL THEN e.EmpId END) AS WithProjects,
    COUNT(DISTINCT CASE WHEN s.EmpId IS NULL THEN e.EmpId END) AS WithoutProjects
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId;
```

**Result:**

| WithProjects | WithoutProjects |
|--------------|-----------------|
| 4 | 4 |

---

#### Q20: Count distinct project names

```sql
SELECT COUNT(DISTINCT ProjectName) AS TotalUniqueProjects
FROM EmploySalary;
```

**Result:**

| TotalUniqueProjects |
|---------------------|
| 6 |

---

#### Q21: Count employees by each manager

```sql
SELECT ManagerId, COUNT(*) AS EmployeesUnderManager
FROM EmployDetails
GROUP BY ManagerId
ORDER BY EmployeesUnderManager DESC;
```

**Result:**

| ManagerId | EmployeesUnderManager |
|-----------|-----------------------|
| 104 | 3 |
| 102 | 2 |
| 101 | 2 |
| 103 | 1 |

---

#### Q22: Count total salary records in EmploySalary table

```sql
SELECT COUNT(*) AS TotalSalaryRecords
FROM EmploySalary;
```

**Result:**

| TotalSalaryRecords |
|--------------------|
| 7 |

---

#### Q23: Count employees who joined each year

```sql
SELECT YEAR(DateOfJoining) AS JoiningYear, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY YEAR(DateOfJoining)
ORDER BY JoiningYear;
```

**Result:**

| JoiningYear | EmployeeCount |
|-------------|---------------|
| 2020 | 2 |
| 2021 | 2 |
| 2022 | 2 |
| 2023 | 2 |

---

#### Q24: Count how many times each project appears

```sql
SELECT ProjectName, COUNT(*) AS AssignmentCount
FROM EmploySalary
GROUP BY ProjectName
ORDER BY AssignmentCount DESC;
```

**Result:**

| ProjectName | AssignmentCount |
|-------------|-----------------|
| Project Alpha | 2 |
| Project Beta | 1 |
| Project Gamma | 1 |
| Project Delta | 1 |
| Project Epsilon | 1 |
| Project Zeta | 1 |

---

#### Q25: Count employees per country with more than 1 employee

```sql
SELECT Country, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY Country
HAVING COUNT(*) > 1
ORDER BY EmployeeCount DESC;
```

**Result:**

| Country | EmployeeCount |
|---------|---------------|
| Spain | 3 |
| Bangladesh | 2 |

---

#### Q26: Count distinct employees working on projects

```sql
SELECT COUNT(DISTINCT EmpId) AS EmployeesWithProjects
FROM EmploySalary;
```

**Result:**

| EmployeesWithProjects |
|-----------------------|
| 4 |

---

#### Q27: Count employees by country and manager

```sql
SELECT Country, ManagerId, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY Country, ManagerId
ORDER BY Country, EmployeeCount DESC;
```

**Result:**

| Country | ManagerId | EmployeeCount |
|---------|-----------|---------------|
| Bangladesh | 101 | 1 |
| Bangladesh | 102 | 1 |
| Brazil | 104 | 1 |
| China | 104 | 1 |
| Spain | 104 | 2 |
| Spain | 101 | 1 |
| USA | 102 | 1 |

---
### SUM Function - Advanced Examples

#### Q28: Calculate total salary of all employees
```sql
SELECT SUM(Salary) AS TotalSalary
FROM EmploySalary;
```
**Result:**
| TotalSalary |
|-------------|
| 385000 |

---

#### Q29: Calculate sum of salaries by country
```sql
SELECT e.Country, SUM(s.Salary) AS TotalSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country;
```
**Result:**
| Country | TotalSalary |
|---------|-------------|
| Bangladesh | 120000 |
| USA | 95000 |
| Spain | 85000 |
| China | 85000 |

---

#### Q30: Calculate sum of salaries for employees managed by specific manager
```sql
SELECT e.ManagerId, SUM(s.Salary) AS TotalSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE e.ManagerId = 104
GROUP BY e.ManagerId;
```
**Result:**
| ManagerId | TotalSalary |
|-----------|-------------|
| 104 | 180000 |

---

#### Q31: Calculate sum of salaries for employees joined after 2021
```sql
SELECT SUM(s.Salary) AS TotalSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE e.DateOfJoining > '2021-12-31';
```
**Result:**
| TotalSalary |
|-------------|
| 180000 |

---

#### Q32: Calculate sum of salaries with CASE statement (bonus calculation)
```sql
SELECT 
    SUM(s.Salary) AS TotalSalary,
    SUM(CASE WHEN s.Salary > 50000 THEN s.Salary * 0.10 ELSE s.Salary * 0.05 END) AS TotalBonus
FROM EmploySalary s;
```
**Result:**
| TotalSalary | TotalBonus |
|-------------|------------|
| 385000 | 32500.00 |

---

### MAX Function - Advanced Examples

#### Q33: Find maximum salary in the company
```sql
SELECT MAX(Salary) AS MaxSalary
FROM EmploySalary;
```
**Result:**
| MaxSalary |
|-----------|
| 95000 |

---

#### Q34: Find maximum salary by country
```sql
SELECT e.Country, MAX(s.Salary) AS MaxSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country;
```
**Result:**
| Country | MaxSalary |
|---------|-----------|
| Bangladesh | 65000 |
| USA | 95000 |
| Spain | 50000 |
| China | 85000 |

---

#### Q35: Find employee with maximum salary using subquery
```sql
SELECT e.FullName, s.Salary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE s.Salary = (SELECT MAX(Salary) FROM EmploySalary);
```
**Result:**
| FullName | Salary |
|----------|--------|
| John Smith | 95000 |

---

#### Q36: Find maximum date of joining by country
```sql
SELECT Country, MAX(DateOfJoining) AS LatestJoining
FROM EmployDetails
GROUP BY Country;
```
**Result:**
| Country | LatestJoining |
|---------|---------------|
| Bangladesh | 2022-01-15 |
| USA | 2021-07-10 |
| Spain | 2023-05-05 |
| China | 2023-05-05 |

---

#### Q37: Find maximum salary for each manager
```sql
SELECT e.ManagerId, MAX(s.Salary) AS MaxSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.ManagerId;
```
**Result:**
| ManagerId | MaxSalary |
|-----------|-----------|
| 101 | 65000 |
| 102 | 95000 |
| 103 | 50000 |
| 104 | 85000 |

---

### AVG Function - Advanced Examples

#### Q38: Calculate average salary of all employees
```sql
SELECT AVG(Salary) AS AverageSalary
FROM EmploySalary;
```
**Result:**
| AverageSalary |
|---------------|
| 55000.00 |

---

#### Q39: Calculate average salary by country
```sql
SELECT e.Country, AVG(s.Salary) AS AverageSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country;
```
**Result:**
| Country | AverageSalary |
|---------|---------------|
| Bangladesh | 60000.00 |
| USA | 95000.00 |
| Spain | 42500.00 |
| China | 85000.00 |

---

#### Q40: Find employees with salary above average
```sql
SELECT e.FullName, s.Salary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE s.Salary > (SELECT AVG(Salary) FROM EmploySalary);
```
**Result:**
| FullName | Salary |
|----------|--------|
| Ruhul Amin | 65000 |
| John Smith | 95000 |
| Chen Wei | 85000 |

---

#### Q41: Calculate average salary by manager with employee count
```sql
SELECT e.ManagerId, AVG(s.Salary) AS AverageSalary, COUNT(e.EmpId) AS EmployeeCount
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.ManagerId;
```
**Result:**
| ManagerId | AverageSalary | EmployeeCount |
|-----------|---------------|---------------|
| 101 | 57500.00 | 2 |
| 102 | 75000.00 | 2 |
| 103 | 50000.00 | 1 |
| 104 | 60000.00 | 3 |

---

#### Q42: Calculate average salary for employees joined in each year
```sql
SELECT YEAR(e.DateOfJoining) AS JoiningYear, AVG(s.Salary) AS AverageSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY YEAR(e.DateOfJoining);
```
**Result:**
| JoiningYear | AverageSalary |
|-------------|---------------|
| 2020 | 42500.00 |
| 2021 | 75000.00 |
| 2022 | 52500.00 |
| 2023 | 67500.00 |

---

### Combined Aggregate Functions

#### Q43: Show SUM, MAX, AVG salary together
```sql
SELECT 
    SUM(Salary) AS TotalSalary,
    MAX(Salary) AS MaxSalary,
    AVG(Salary) AS AverageSalary
FROM EmploySalary;
```
**Result:**
| TotalSalary | MaxSalary | AverageSalary |
|-------------|-----------|---------------|
| 385000 | 95000 | 55000.00 |

---

#### Q44: Calculate salary statistics by country
```sql
SELECT 
    e.Country,
    SUM(s.Salary) AS TotalSalary,
    MAX(s.Salary) AS MaxSalary,
    AVG(s.Salary) AS AverageSalary,
    COUNT(e.EmpId) AS EmployeeCount
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country;
```
**Result:**
| Country | TotalSalary | MaxSalary | AverageSalary | EmployeeCount |
|---------|-------------|-----------|---------------|---------------|
| Bangladesh | 120000 | 65000 | 60000.00 | 2 |
| USA | 95000 | 95000 | 95000.00 | 1 |
| Spain | 85000 | 50000 | 42500.00 | 2 |
| China | 85000 | 85000 | 85000.00 | 1 |

---

#### Q45: Find countries where average salary is above overall average
```sql
SELECT e.Country, AVG(s.Salary) AS AverageSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country
HAVING AVG(s.Salary) > (SELECT AVG(Salary) FROM EmploySalary);
```
**Result:**
| Country | AverageSalary |
|---------|---------------|
| Bangladesh | 60000.00 |
| USA | 95000.00 |
| China | 85000.00 |

---

#### Q46: Calculate percentage of total salary by country
```sql
SELECT 
    e.Country,
    SUM(s.Salary) AS CountrySalary,
    (SUM(s.Salary) * 100.0 / (SELECT SUM(Salary) FROM EmploySalary)) AS PercentageOfTotal
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country;
```
**Result:**
| Country | CountrySalary | PercentageOfTotal |
|---------|---------------|-------------------|
| Bangladesh | 120000 | 31.17 |
| USA | 95000 | 24.68 |
| Spain | 85000 | 22.08 |
| China | 85000 | 22.08 |

---

#### Q47: Find difference between each employee's salary and average salary
```sql
SELECT 
    e.FullName,
    s.Salary,
    (SELECT AVG(Salary) FROM EmploySalary) AS AvgSalary,
    (s.Salary - (SELECT AVG(Salary) FROM EmploySalary)) AS DifferenceFromAvg
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId;
```
**Result:**
| FullName | Salary | AvgSalary | DifferenceFromAvg |
|----------|--------|-----------|-------------------|
| Ruhul Amin | 65000 | 55000.00 | 10000.00 |
| John Smith | 95000 | 55000.00 | 40000.00 |
| Maria Garcia | 50000 | 55000.00 | -5000.00 |
| Chen Wei | 85000 | 55000.00 | 30000.00 |
| Md.Ruhul Amin | 40000 | 55000.00 | -15000.00 |
| John Smith | 55000 | 55000.00 | 0.00 |
| Jhon Snow | 35000 | 55000.00 | -20000.00 |

---
### Finding Highest, Second Highest, Third Highest Salaries

#### Q48: Find the highest salary (alternative method)
```sql
SELECT Salary AS HighestSalary
FROM EmploySalary
ORDER BY Salary DESC
LIMIT 1;
```
**Result:**
| HighestSalary |
|---------------|
| 95000 |

---

#### Q49: Find the second highest salary
```sql
SELECT MAX(Salary) AS SecondHighestSalary
FROM EmploySalary
WHERE Salary < (SELECT MAX(Salary) FROM EmploySalary);
```
**Result:**
| SecondHighestSalary |
|---------------------|
| 85000 |

---

#### Q50: Find the third highest salary
```sql
SELECT DISTINCT Salary AS ThirdHighestSalary
FROM EmploySalary
ORDER BY Salary DESC
LIMIT 1 OFFSET 2;
```
**Result:**
| ThirdHighestSalary |
|--------------------|
| 65000 |

---

#### Q51: Find top 3 highest salaries with employee names
```sql
SELECT e.FullName, s.Salary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
ORDER BY s.Salary DESC
LIMIT 3;
```
**Result:**
| FullName | Salary |
|----------|--------|
| John Smith | 95000 |
| Chen Wei | 85000 |
| Ruhul Amin | 65000 |

---

#### Q52: Find Nth highest salary using subquery (N=4)
```sql
SELECT DISTINCT Salary
FROM EmploySalary e1
WHERE 3 = (
    SELECT COUNT(DISTINCT Salary)
    FROM EmploySalary e2
    WHERE e2.Salary > e1.Salary
);
```
**Result:**
| Salary |
|--------|
| 55000 |

---

### Duplicate and Distinct Values

#### Q53: Find duplicate employee names
```sql
SELECT FullName, COUNT(*) AS Count
FROM EmployDetails
GROUP BY FullName
HAVING COUNT(*) > 1;
```
**Result:**
| FullName | Count |
|----------|-------|
| John Smith | 2 |

---

#### Q54: Find employees with duplicate names and show their details
```sql
SELECT e.EmpId, e.FullName, e.Country, e.DateOfJoining
FROM EmployDetails e
WHERE e.FullName IN (
    SELECT FullName
    FROM EmployDetails
    GROUP BY FullName
    HAVING COUNT(*) > 1
);
```
**Result:**
| EmpId | FullName | Country | DateOfJoining |
|-------|----------|---------|---------------|
| 2 | John Smith | USA | 2021-07-10 |
| 6 | John Smith | Bangladesh | 2021-07-10 |

---

#### Q55: Find distinct countries with employee count
```sql
SELECT DISTINCT Country, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY Country;
```
**Result:**
| Country | EmployeeCount |
|---------|---------------|
| Bangladesh | 2 |
| USA | 1 |
| Spain | 2 |
| China | 1 |
| Brazil | 1 |

---

#### Q56: Find employees who joined on the same date
```sql
SELECT DateOfJoining, COUNT(*) AS EmployeeCount, GROUP_CONCAT(FullName) AS Employees
FROM EmployDetails
GROUP BY DateOfJoining
HAVING COUNT(*) > 1;
```
**Result:**
| DateOfJoining | EmployeeCount | Employees |
|---------------|---------------|-----------|
| 2021-07-10 | 2 | John Smith,John Smith |
| 2020-03-25 | 2 | Maria Garcia,Jhon Snow |
| 2023-05-05 | 2 | Chen Wei,Chen Chung |

---

#### Q57: Find distinct managers and their employee count
```sql
SELECT DISTINCT ManagerId, COUNT(*) AS EmployeeCount
FROM EmployDetails
GROUP BY ManagerId
ORDER BY EmployeeCount DESC;
```
**Result:**
| ManagerId | EmployeeCount |
|-----------|---------------|
| 104 | 3 |
| 102 | 2 |
| 101 | 2 |
| 103 | 1 |

---

### Advanced JOIN Operations

#### Q58: INNER JOIN - Show employees with their salaries and manager details
```sql
SELECT 
    e.EmpId,
    e.FullName AS EmployeeName,
    e.Country,
    s.Salary,
    m.FullName AS ManagerName
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
INNER JOIN EmployDetails m ON e.ManagerId = m.EmpId;
```
**Result:**
| EmpId | EmployeeName | Country | Salary | ManagerName |
|-------|--------------|---------|--------|-------------|
| 1 | Ruhul Amin | Bangladesh | 65000 | (Manager Data) |
| 2 | John Smith | USA | 95000 | (Manager Data) |

---

#### Q59: LEFT JOIN - Show all employees with salary (including those without salary records)
```sql
SELECT 
    e.EmpId,
    e.FullName,
    e.Country,
    COALESCE(s.Salary, 0) AS Salary
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId;
```
**Result:**
| EmpId | FullName | Country | Salary |
|-------|----------|---------|--------|
| 1 | Ruhul Amin | Bangladesh | 65000 |
| 2 | John Smith | USA | 95000 |
| 3 | Maria Garcia | Spain | 50000 |
| 4 | Chen Wei | China | 85000 |
| 5 | Md.Ruhul Amin | Brazil | 0 |
| 6 | John Smith | Bangladesh | 0 |
| 7 | Jhon Snow | Spain | 0 |
| 8 | Chen Chung | Spain | 0 |

---

#### Q60: RIGHT JOIN - Show all salary records with employee details
```sql
SELECT 
    s.EmpId,
    e.FullName,
    e.Country,
    s.Salary
FROM EmployDetails e
RIGHT JOIN EmploySalary s ON e.EmpId = s.EmpId;
```
**Result:**
| EmpId | FullName | Country | Salary |
|-------|----------|---------|--------|
| 1 | Ruhul Amin | Bangladesh | 65000 |
| 2 | John Smith | USA | 95000 |
| 3 | Maria Garcia | Spain | 50000 |
| 4 | Chen Wei | China | 85000 |

---

#### Q61: SELF JOIN - Find employees from the same country
```sql
SELECT 
    e1.FullName AS Employee1,
    e2.FullName AS Employee2,
    e1.Country
FROM EmployDetails e1
INNER JOIN EmployDetails e2 ON e1.Country = e2.Country AND e1.EmpId < e2.EmpId;
```
**Result:**
| Employee1 | Employee2 | Country |
|-----------|-----------|---------|
| Ruhul Amin | John Smith | Bangladesh |
| Maria Garcia | Jhon Snow | Spain |
| Maria Garcia | Chen Chung | Spain |
| Jhon Snow | Chen Chung | Spain |

---

#### Q62: CROSS JOIN - Show all possible employee-manager combinations
```sql
SELECT 
    e.FullName AS Employee,
    m.FullName AS PossibleManager
FROM EmployDetails e
CROSS JOIN EmployDetails m
WHERE e.EmpId != m.EmpId
LIMIT 10;
```
**Result:**
| Employee | PossibleManager |
|----------|-----------------|
| Ruhul Amin | John Smith |
| Ruhul Amin | Maria Garcia |
| Ruhul Amin | Chen Wei |
| Ruhul Amin | Md.Ruhul Amin |
| John Smith | Ruhul Amin |
| John Smith | Maria Garcia |
| John Smith | Chen Wei |
| John Smith | Md.Ruhul Amin |
| Maria Garcia | Ruhul Amin |
| Maria Garcia | John Smith |

---

#### Q63: Multiple JOINs - Employee, Salary, and Project information
```sql
SELECT 
    e.FullName,
    e.Country,
    s.Salary,
    p.ProjectName
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
INNER JOIN EmployProject p ON e.EmpId = p.EmpId;
```
**Result:**
| FullName | Country | Salary | ProjectName |
|----------|---------|--------|-------------|
| Ruhul Amin | Bangladesh | 65000 | Project Alpha |
| John Smith | USA | 95000 | Project Beta |
| Maria Garcia | Spain | 50000 | Project Gamma |
| Chen Wei | China | 85000 | Project Delta |

---

#### Q64: JOIN with aggregate - Count employees per country with average salary
```sql
SELECT 
    e.Country,
    COUNT(e.EmpId) AS TotalEmployees,
    COUNT(s.EmpId) AS EmployeesWithSalary,
    AVG(s.Salary) AS AverageSalary
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY e.Country;
```
**Result:**
| Country | TotalEmployees | EmployeesWithSalary | AverageSalary |
|---------|----------------|---------------------|---------------|
| Bangladesh | 2 | 1 | 65000.00 |
| USA | 1 | 1 | 95000.00 |
| Spain | 2 | 1 | 50000.00 |
| China | 1 | 1 | 85000.00 |
| Brazil | 1 | 0 | NULL |

---

#### Q65: JOIN with subquery - Employees earning more than their country average
```sql
SELECT 
    e.FullName,
    e.Country,
    s.Salary,
    ca.AvgSalary AS CountryAvgSalary
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId
INNER JOIN (
    SELECT e2.Country, AVG(s2.Salary) AS AvgSalary
    FROM EmployDetails e2
    INNER JOIN EmploySalary s2 ON e2.EmpId = s2.EmpId
    GROUP BY e2.Country
) ca ON e.Country = ca.Country
WHERE s.Salary > ca.AvgSalary;
```
**Result:**
| FullName | Country | Salary | CountryAvgSalary |
|----------|---------|--------|------------------|
| Ruhul Amin | Bangladesh | 65000 | 60000.00 |
| John Smith | USA | 95000 | 95000.00 |
| Maria Garcia | Spain | 50000 | 42500.00 |
| Chen Wei | China | 85000 | 85000.00 |

---

#### Q66: Find employees without salary records using LEFT JOIN
```sql
SELECT 
    e.EmpId,
    e.FullName,
    e.Country
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId
WHERE s.EmpId IS NULL;
```
**Result:**
| EmpId | FullName | Country |
|-------|----------|---------|
| 5 | Md.Ruhul Amin | Brazil |
| 6 | John Smith | Bangladesh |
| 7 | Jhon Snow | Spain |
| 8 | Chen Chung | Spain |

---

#### Q67: UNION - Combine employees from specific countries
```sql
SELECT FullName, Country, 'Asia' AS Region
FROM EmployDetails
WHERE Country IN ('Bangladesh', 'China')
UNION
SELECT FullName, Country, 'Europe' AS Region
FROM EmployDetails
WHERE Country = 'Spain';
```
**Result:**
| FullName | Country | Region |
|----------|---------|--------|
| Ruhul Amin | Bangladesh | Asia |
| Chen Wei | China | Asia |
| John Smith | Bangladesh | Asia |
| Maria Garcia | Spain | Europe |
| Jhon Snow | Spain | Europe |
| Chen Chung | Spain | Europe |

---
### DROP, UPDATE, DELETE, TRUNCATE Operations with Theory

---

## Theory Overview

### DROP vs DELETE vs TRUNCATE

| Feature | DROP | DELETE | TRUNCATE |
|---------|------|--------|----------|
| **Purpose** | Removes entire table structure | Removes specific rows | Removes all rows but keeps structure |
| **Rollback** | Cannot rollback (DDL) | Can rollback (DML) | Cannot rollback (DDL) |
| **WHERE Clause** | Not applicable | Supported | Not supported |
| **Speed** | Fast | Slower (row by row) | Fastest |
| **Auto Increment** | Resets | Doesn't reset | Resets |
| **Triggers** | Not fired | Fired | Not fired |

---

### DROP Operations

#### Q68: Drop a specific table
```sql
DROP TABLE EmploySalary;
```
**Result:**
```
Table 'EmploySalary' has been dropped successfully.
The table structure and all data are permanently removed.
```

---

#### Q69: Drop table if exists (safe drop)
```sql
DROP TABLE IF EXISTS EmployBackup;
```
**Result:**
```
Query executed successfully.
If table exists, it will be dropped. Otherwise, no error is thrown.
```

---

#### Q70: Drop multiple tables at once
```sql
DROP TABLE IF EXISTS EmploySalary, EmployProject, EmployBackup;
```
**Result:**
```
All specified tables are dropped.
Multiple tables can be dropped in a single statement.
```

---

#### Q71: Drop temporary table
```sql
DROP TEMPORARY TABLE IF EXISTS TempEmploy;
```
**Result:**
```
Temporary table dropped successfully.
Only drops temporary tables, not permanent ones.
```

---

### DELETE Operations

#### Q72: Delete specific employee by ID
```sql
DELETE FROM EmployDetails
WHERE EmpId = 5;
```
**Result:**
```
1 row affected.
Employee with EmpId 5 (Md.Ruhul Amin) has been deleted.
```
**Remaining Data:**
| EmpId | FullName | Country |
|-------|----------|---------|
| 1 | Ruhul Amin | Bangladesh |
| 2 | John Smith | USA |
| 3 | Maria Garcia | Spain |
| 4 | Chen Wei | China |
| 6 | John Smith | Bangladesh |
| 7 | Jhon Snow | Spain |
| 8 | Chen Chung | Spain |

---

#### Q73: Delete employees from specific country
```sql
DELETE FROM EmployDetails
WHERE Country = 'Spain';
```
**Result:**
```
3 rows affected.
All employees from Spain have been deleted.
```
**Remaining Data:**
| EmpId | FullName | Country |
|-------|----------|---------|
| 1 | Ruhul Amin | Bangladesh |
| 2 | John Smith | USA |
| 4 | Chen Wei | China |
| 5 | Md.Ruhul Amin | Brazil |
| 6 | John Smith | Bangladesh |

---

#### Q74: Delete employees with salary less than specific amount
```sql
DELETE FROM EmploySalary
WHERE Salary < 50000;
```
**Result:**
```
2 rows affected.
Employees with salary below 50000 have been removed from salary table.
```

---

#### Q75: Delete using JOIN condition
```sql
DELETE s
FROM EmploySalary s
INNER JOIN EmployDetails e ON s.EmpId = e.EmpId
WHERE e.Country = 'Bangladesh';
```
**Result:**
```
2 rows affected.
Salary records for Bangladesh employees have been deleted.
```

---

#### Q76: Delete duplicate records keeping one
```sql
DELETE e1 FROM EmployDetails e1
INNER JOIN EmployDetails e2
WHERE e1.EmpId > e2.EmpId
AND e1.FullName = e2.FullName
AND e1.DateOfJoining = e2.DateOfJoining;
```
**Result:**
```
1 row affected.
Duplicate employee record has been removed, keeping the one with lower EmpId.
```

---

#### Q77: Delete all records (without dropping table)
```sql
DELETE FROM EmployDetails;
```
**Result:**
```
8 rows affected.
All data deleted but table structure remains.
Auto increment continues from last value.
```

---

### TRUNCATE Operations

#### Q78: Truncate table to remove all data
```sql
TRUNCATE TABLE EmploySalary;
```
**Result:**
```
Table truncated successfully.
All rows removed, table structure intact.
Auto increment reset to 1.
Faster than DELETE for removing all rows.
```

---

#### Q79: Difference demonstration - DELETE vs TRUNCATE
```sql
-- Using DELETE
DELETE FROM EmployBackup;
-- New insert will continue from last auto increment value

-- Using TRUNCATE
TRUNCATE TABLE EmployBackup;
-- New insert will start from 1
```
**Result:**
```
DELETE: Maintains auto increment counter
TRUNCATE: Resets auto increment to 1
```

---

### UPDATE Operations

#### Q80: Update employee country
```sql
UPDATE EmployDetails
SET Country = 'UK'
WHERE EmpId = 2;
```
**Result:**
```
1 row affected.
John Smith's country changed from USA to UK.
```
**Updated Data:**
| EmpId | FullName | Country |
|-------|----------|---------|
| 2 | John Smith | UK |

---

#### Q81: Update salary for specific employee
```sql
UPDATE EmploySalary
SET Salary = 70000
WHERE EmpId = 1;
```
**Result:**
```
1 row affected.
Salary updated from 65000 to 70000.
```
**Updated Data:**
| EmpId | Salary |
|-------|--------|
| 1 | 70000 |

---

#### Q82: Update salary with percentage increase
```sql
UPDATE EmploySalary
SET Salary = Salary * 1.10
WHERE Salary < 60000;
```
**Result:**
```
3 rows affected.
10% salary increase applied to employees earning below 60000.
```
**Example:**
| EmpId | Old Salary | New Salary |
|-------|------------|------------|
| 3 | 50000 | 55000 |
| 5 | 40000 | 44000 |
| 7 | 35000 | 38500 |

---

#### Q83: Update multiple columns at once
```sql
UPDATE EmployDetails
SET Country = 'Canada', ManagerId = 105
WHERE EmpId = 6;
```
**Result:**
```
1 row affected.
Employee 6: Country changed to Canada and ManagerId updated to 105.
```

---

#### Q84: Update using JOIN
```sql
UPDATE EmploySalary s
INNER JOIN EmployDetails e ON s.EmpId = e.EmpId
SET s.Salary = s.Salary * 1.15
WHERE e.Country = 'Spain';
```
**Result:**
```
2 rows affected.
15% salary increase for all Spain-based employees.
```

---

#### Q85: Update with CASE statement (conditional update)
```sql
UPDATE EmploySalary
SET Salary = CASE
    WHEN Salary < 50000 THEN Salary * 1.20
    WHEN Salary BETWEEN 50000 AND 70000 THEN Salary * 1.15
    ELSE Salary * 1.10
END;
```
**Result:**
```
All rows affected.
Different percentage increases based on current salary range:
- Below 50000: 20% increase
- 50000-70000: 15% increase
- Above 70000: 10% increase
```

---

#### Q86: Update using subquery
```sql
UPDATE EmploySalary
SET Salary = (SELECT AVG(Salary) FROM EmploySalary)
WHERE EmpId = 5;
```
**Result:**
```
1 row affected.
Employee 5's salary set to company average salary.
```

---

### IF Statement and Conditional Logic

#### Q87: Using IF in SELECT statement
```sql
SELECT 
    EmpId,
    FullName,
    Country,
    IF(Country = 'Bangladesh', 'Local', 'International') AS EmployeeType
FROM EmployDetails;
```
**Result:**
| EmpId | FullName | Country | EmployeeType |
|-------|----------|---------|--------------|
| 1 | Ruhul Amin | Bangladesh | Local |
| 2 | John Smith | USA | International |
| 3 | Maria Garcia | Spain | International |
| 4 | Chen Wei | China | International |
| 6 | John Smith | Bangladesh | Local |

---

#### Q88: Nested IF statements
```sql
SELECT 
    e.FullName,
    s.Salary,
    IF(s.Salary >= 80000, 'High',
        IF(s.Salary >= 50000, 'Medium', 'Low')
    ) AS SalaryGrade
FROM EmployDetails e
INNER JOIN EmploySalary s ON e.EmpId = s.EmpId;
```
**Result:**
| FullName | Salary | SalaryGrade |
|----------|--------|-------------|
| Ruhul Amin | 65000 | Medium |
| John Smith | 95000 | High |
| Maria Garcia | 50000 | Medium |
| Chen Wei | 85000 | High |
| Md.Ruhul Amin | 40000 | Low |

---

#### Q89: CASE statement (alternative to IF)
```sql
SELECT 
    EmpId,
    FullName,
    CASE Country
        WHEN 'Bangladesh' THEN 'South Asia'
        WHEN 'China' THEN 'East Asia'
        WHEN 'Spain' THEN 'Europe'
        WHEN 'USA' THEN 'North America'
        ELSE 'Other'
    END AS Region
FROM EmployDetails;
```
**Result:**
| EmpId | FullName | Region |
|-------|----------|--------|
| 1 | Ruhul Amin | South Asia |
| 2 | John Smith | North America |
| 3 | Maria Garcia | Europe |
| 4 | Chen Wei | East Asia |
| 5 | Md.Ruhul Amin | Other |

---

#### Q90: IF with aggregate functions
```sql
SELECT 
    Country,
    COUNT(*) AS TotalEmployees,
    SUM(IF(s.Salary > 60000, 1, 0)) AS HighEarners,
    SUM(IF(s.Salary <= 60000, 1, 0)) AS LowEarners
FROM EmployDetails e
LEFT JOIN EmploySalary s ON e.EmpId = s.EmpId
GROUP BY Country;
```
**Result:**
| Country | TotalEmployees | HighEarners | LowEarners |
|---------|----------------|-------------|------------|
| Bangladesh | 2 | 1 | 1 |
| USA | 1 | 1 | 0 |
| Spain | 2 | 0 | 2 |
| China | 1 | 1 | 0 |
| Brazil | 1 | 0 | 1 |

---

### Important Notes:

**⚠️ Safety Tips:**
1. Always use `WHERE` clause with DELETE and UPDATE
2. Test with SELECT before running DELETE/UPDATE
3. Backup data before DROP operations
4. Use transactions for critical operations
5. TRUNCATE cannot be rolled back - use with caution

**Best Practices:**
- Use `IF EXISTS` with DROP to avoid errors
- Always use WHERE clause to prevent accidental full table updates
- Prefer DELETE over TRUNCATE if you need to rollback
- Use CASE for complex conditional logic
## 🎯 Key SQL Concepts Covered

- ✅ **SELECT** - Retrieve data from tables
- ✅ **WHERE** - Filter rows based on conditions
- ✅ **COUNT()** - Count number of rows
- ✅ **SUM()** - Calculate total of numeric values
- ✅ **AVG()** - Calculate average of numeric values
- ✅ **MAX()** - Find maximum value
- ✅ **MIN()** - Find minimum value
- ✅ **GROUP BY** - Group rows that have same values
- ✅ **HAVING** - Filter groups based on conditions
- ✅ **INNER JOIN** - Return matching rows from both tables
- ✅ **LEFT JOIN** - Return all rows from left table and matching rows from right
- ✅ **ORDER BY** - Sort results
- ✅ **Subqueries** - Nested queries for complex logic

---

## 📝 Notes

- All queries are written in standard SQL syntax
- Results may vary slightly depending on your SQL database system (MySQL, PostgreSQL, SQL Server, etc.)
- Practice these queries to master SQL concepts!

---

## 🤝 Contributing

Feel free to add more examples and queries to expand this cheat sheet!

---

## 📄 License

Free to use for learning purposes.

---

**Happy Learning! 🚀**
