
# SQL Query Learning Sheet
## For Bangladesh Government IT Job Exams

---

## SAMPLE TABLES

### Table 1: Employees
```
emp_id | emp_name      | dept_id | salary | hire_date   | email
-------|---------------|---------|--------|-------------|------------------
1      | Rajib Ahmed   | 101     | 35000  | 2020-01-15  | rajib@company.com
2      | Fatema Khan   | 102     | 42000  | 2019-06-20  | fatema@company.com
3      | Karim Hassan  | 101     | 38000  | 2021-03-10  | karim@company.com
4      | Nadia Islam   | 103     | 45000  | 2018-11-05  | nadia@company.com
5      | Shakir Alam   | 102     | 40000  | 2020-09-12  | shakir@company.com
6      | Ayesha Siddiqui | 101   | 36000  | 2022-01-22  | ayesha@company.com
7      | Hasan Khan    | 103     | 50000  | 2017-07-30  | hasan@company.com
8      | Rina Das      | 104     | 33000  | 2021-05-18  | rina@company.com
```

### Table 2: Departments
```
dept_id | dept_name        | manager_id | location
--------|------------------|------------|----------
101     | IT              | 1          | Dhaka
102     | HR              | 2          | Dhaka
103     | Finance         | 4          | Chittagong
104     | Operations      | 5          | Khulna
105     | Marketing       | NULL       | Dhaka
```

---

## QUESTIONS AND ANSWERS

### Q1: SELECT All Employees
**Question:** Write a query to display all employees with all columns.
```sql
SELECT * FROM Employees;
```
**Output:** Returns all 8 employees with all columns.

---

### Q2: SELECT Specific Columns
**Question:** Display employee names and salaries only.
```sql
SELECT emp_name, salary FROM Employees;
```
**Output:** Shows names and salaries for all employees.

---

### Q3: WHERE Clause - Simple Condition
**Question:** Find employees whose salary is greater than 40000.
```sql
SELECT emp_name, salary FROM Employees WHERE salary > 40000;
```
**Output:** Fatema Khan (42000), Nadia Islam (45000), Hasan Khan (50000).

---

### Q4: WHERE with AND Operator
**Question:** Find employees in department 101 with salary above 35000.
```sql
SELECT emp_name, dept_id, salary FROM Employees 
WHERE dept_id = 101 AND salary > 35000;
```
**Output:** Karim Hassan (38000), Ayesha Siddiqui (36000).

---

### Q5: WHERE with OR Operator
**Question:** Find employees from department 101 or 103.
```sql
SELECT emp_name, dept_id FROM Employees 
WHERE dept_id = 101 OR dept_id = 103;
```
**Output:** 5 employees from departments 101 and 103.

---

### Q6: WHERE with IN Operator
**Question:** Find employees from departments 102 and 104.
```sql
SELECT emp_name, dept_id FROM Employees 
WHERE dept_id IN (102, 104);
```
**Output:** Fatema Khan, Shakir Alam, Rina Das.

---

### Q7: WHERE with NOT IN Operator
**Question:** Find employees NOT in department 101.
```sql
SELECT emp_name, dept_id FROM Employees 
WHERE dept_id NOT IN (101);
```
**Output:** All employees except those in department 101.

---

### Q8: WHERE with BETWEEN Operator
**Question:** Find employees with salary between 35000 and 45000.
```sql
SELECT emp_name, salary FROM Employees 
WHERE salary BETWEEN 35000 AND 45000;
```
**Output:** Rajib Ahmed, Fatema Khan, Karim Hassan, Nadia Islam, Shakir Alam, Ayesha Siddiqui.

---

### Q9: WHERE with LIKE Operator
**Question:** Find employees whose name starts with 'K'.
```sql
SELECT emp_name FROM Employees 
WHERE emp_name LIKE 'K%';
```
**Output:** Karim Hassan, Khan (if present).

---

### Q10: WHERE with LIKE (Contains Pattern)
**Question:** Find employees with 'Ahmed' in their name.
```sql
SELECT emp_name FROM Employees 
WHERE emp_name LIKE '%Ahmed%';
```
**Output:** Rajib Ahmed.

---

### Q11: ORDER BY Ascending
**Question:** Display employees sorted by salary in ascending order.
```sql
SELECT emp_name, salary FROM Employees 
ORDER BY salary ASC;
```
**Output:** Employees from lowest to highest salary.

---

### Q12: ORDER BY Descending
**Question:** Display employees sorted by salary in descending order.
```sql
SELECT emp_name, salary FROM Employees 
ORDER BY salary DESC;
```
**Output:** Employees from highest to lowest salary.

---

### Q13: ORDER BY Multiple Columns
**Question:** Sort by department ID, then by salary descending.
```sql
SELECT emp_name, dept_id, salary FROM Employees 
ORDER BY dept_id ASC, salary DESC;
```
**Output:** Organized by department, then by salary within each department.

---

### Q14: DISTINCT Keyword
**Question:** Find all unique department IDs.
```sql
SELECT DISTINCT dept_id FROM Employees;
```
**Output:** 101, 102, 103, 104 (4 unique departments).

---

### Q15: DISTINCT with Multiple Columns
**Question:** Find unique combinations of department and salary ranges.
```sql
SELECT DISTINCT dept_id, salary FROM Employees 
ORDER BY dept_id;
```
**Output:** Unique department-salary combinations.

---

### Q16: COUNT Function
**Question:** Count total number of employees.
```sql
SELECT COUNT(*) as total_employees FROM Employees;
```
**Output:** 8

---

### Q17: COUNT with WHERE
**Question:** Count employees in department 101.
```sql
SELECT COUNT(*) as dept_101_count FROM Employees 
WHERE dept_id = 101;
```
**Output:** 3

---

### Q18: SUM Function
**Question:** Calculate total salary expense.
```sql
SELECT SUM(salary) as total_salary FROM Employees;
```
**Output:** 309000

---

### Q19: AVG Function
**Question:** Find average salary of all employees.
```sql
SELECT AVG(salary) as average_salary FROM Employees;
```
**Output:** 38625

---

### Q20: MAX Function
**Question:** Find the highest salary.
```sql
SELECT MAX(salary) as highest_salary FROM Employees;
```
**Output:** 50000

---

### Q21: MIN Function
**Question:** Find the lowest salary.
```sql
SELECT MIN(salary) as lowest_salary FROM Employees;
```
**Output:** 33000

---

### Q22: GROUP BY Simple
**Question:** Count employees in each department.
```sql
SELECT dept_id, COUNT(*) as employee_count FROM Employees 
GROUP BY dept_id;
```
**Output:**
```
dept_id | employee_count
101     | 3
102     | 2
103     | 2
104     | 1
```

---

### Q23: GROUP BY with SUM
**Question:** Find total salary expense for each department.
```sql
SELECT dept_id, SUM(salary) as dept_salary FROM Employees 
GROUP BY dept_id;
```
**Output:** Total salary by department.

---

### Q24: GROUP BY with AVG
**Question:** Find average salary per department.
```sql
SELECT dept_id, AVG(salary) as avg_salary FROM Employees 
GROUP BY dept_id;
```
**Output:** Average salary for each department.

---

### Q25: GROUP BY with HAVING
**Question:** Find departments with average salary above 40000.
```sql
SELECT dept_id, AVG(salary) as avg_salary FROM Employees 
GROUP BY dept_id 
HAVING AVG(salary) > 40000;
```
**Output:** Departments 102, 103, 104.

---

### Q26: GROUP BY with ORDER BY
**Question:** Count employees per department and sort by count descending.
```sql
SELECT dept_id, COUNT(*) as emp_count FROM Employees 
GROUP BY dept_id 
ORDER BY emp_count DESC;
```
**Output:** Departments sorted by employee count.

---

### Q27: INNER JOIN
**Question:** Display employee names with their department names.
```sql
SELECT e.emp_name, d.dept_name FROM Employees e 
INNER JOIN Departments d ON e.dept_id = d.dept_id;
```
**Output:** 8 rows with employee and department names.

---

### Q28: LEFT JOIN
**Question:** Show all departments with their employees (include empty departments).
```sql
SELECT d.dept_name, e.emp_name FROM Departments d 
LEFT JOIN Employees e ON d.dept_id = e.dept_id;
```
**Output:** All departments, including Marketing (which has no employees).

---

### Q29: RIGHT JOIN
**Question:** Show all employees with their department information.
```sql
SELECT e.emp_name, d.dept_name FROM Employees e 
RIGHT JOIN Departments d ON e.dept_id = d.dept_id;
```
**Output:** All employees with department names.

---

### Q30: FULL OUTER JOIN (UNION method)
**Question:** Get all employees and departments (even unmatched).
```sql
SELECT e.emp_name, d.dept_name FROM Employees e 
FULL OUTER JOIN Departments d ON e.dept_id = d.dept_id;
```
**Note:** Use UNION for databases that don't support FULL OUTER JOIN.

---

### Q31: Self JOIN
**Question:** Find employees managed by managers (assuming manager_id exists in Employees).
```sql
SELECT e.emp_name, m.emp_name as manager_name FROM Employees e 
LEFT JOIN Employees m ON e.emp_id = m.emp_id;
```
**Output:** Shows employee-manager relationships.

---

### Q32: Multiple JOINs
**Question:** Get employee, department, and manager information.
```sql
SELECT e.emp_name, d.dept_name, m.emp_name as manager FROM Employees e 
INNER JOIN Departments d ON e.dept_id = d.dept_id 
LEFT JOIN Employees m ON d.manager_id = m.emp_id;
```
**Output:** Complete employee information with department and manager.

---

### Q33: UNION Operator
**Question:** Combine employees and department manager names.
```sql
SELECT emp_name FROM Employees 
UNION 
SELECT emp_name FROM Employees WHERE emp_id IN (1,2,4,5);
```
**Output:** Unique names (duplicates removed).

---

### Q34: UNION ALL Operator
**Question:** Combine with duplicates allowed.
```sql
SELECT emp_name FROM Employees 
UNION ALL 
SELECT emp_name FROM Employees WHERE dept_id = 101;
```
**Output:** All names including duplicates.

---

### Q35: Subquery in WHERE
**Question:** Find employees earning more than average salary.
```sql
SELECT emp_name, salary FROM Employees 
WHERE salary > (SELECT AVG(salary) FROM Employees);
```
**Output:** 4 employees above average.

---

### Q36: Subquery with IN
**Question:** Find employees in departments that have more than 2 employees.
```sql
SELECT emp_name, dept_id FROM Employees 
WHERE dept_id IN (SELECT dept_id FROM Employees GROUP BY dept_id HAVING COUNT(*) > 2);
```
**Output:** Employees from department 101.

---

### Q37: Subquery with COUNT
**Question:** Find total employees in the company.
```sql
SELECT (SELECT COUNT(*) FROM Employees) as total_emp;
```
**Output:** 8

---

### Q38: Correlated Subquery
**Question:** Find employees earning more than their department's average.
```sql
SELECT emp_name, salary FROM Employees e1 
WHERE salary > (SELECT AVG(salary) FROM Employees e2 WHERE e2.dept_id = e1.dept_id);
```
**Output:** Employees above their department average.

---

### Q39: EXISTS Operator
**Question:** Find departments that have employees.
```sql
SELECT dept_name FROM Departments d 
WHERE EXISTS (SELECT 1 FROM Employees e WHERE e.dept_id = d.dept_id);
```
**Output:** Departments 101, 102, 103, 104.

---

### Q40: NOT EXISTS
**Question:** Find departments with no employees.
```sql
SELECT dept_name FROM Departments d 
WHERE NOT EXISTS (SELECT 1 FROM Employees e WHERE e.dept_id = d.dept_id);
```
**Output:** Marketing department.

---

### Q41: CASE Statement
**Question:** Categorize employees by salary range.
```sql
SELECT emp_name, salary,
CASE 
  WHEN salary < 35000 THEN 'Low'
  WHEN salary BETWEEN 35000 AND 42000 THEN 'Medium'
  ELSE 'High'
END as salary_category
FROM Employees;
```
**Output:** Salary categories for all employees.

---

### Q42: CASE with Multiple Conditions
**Question:** Assign bonus based on department.
```sql
SELECT emp_name, dept_id,
CASE 
  WHEN dept_id = 101 THEN 'Bonus: 5%'
  WHEN dept_id = 102 THEN 'Bonus: 7%'
  WHEN dept_id = 103 THEN 'Bonus: 10%'
  ELSE 'Bonus: 3%'
END as bonus
FROM Employees;
```
**Output:** Bonus assignments by department.

---

### Q43: LIMIT Clause
**Question:** Get top 3 highest-paid employees.
```sql
SELECT emp_name, salary FROM Employees 
ORDER BY salary DESC 
LIMIT 3;
```
**Output:** Top 3 employees by salary.

---

### Q44: OFFSET with LIMIT
**Question:** Get employees ranked 4-6 by salary.
```sql
SELECT emp_name, salary FROM Employees 
ORDER BY salary DESC 
LIMIT 3 OFFSET 3;
```
**Output:** 4th to 6th highest-paid employees.

---

### Q45: String Functions - CONCAT
**Question:** Display employee email and name together.
```sql
SELECT CONCAT(emp_name, ' (', email, ')') as emp_email FROM Employees;
```
**Output:** Names with emails in parentheses.

---

### Q46: String Functions - UPPER/LOWER
**Question:** Display employee names in uppercase.
```sql
SELECT UPPER(emp_name) as upper_name, LOWER(email) as lower_email FROM Employees;
```
**Output:** Names in uppercase, emails in lowercase.

---

### Q47: String Functions - LENGTH
**Question:** Find employees with name length greater than 10 characters.
```sql
SELECT emp_name FROM Employees 
WHERE LENGTH(emp_name) > 10;
```
**Output:** Longer names only.

---

### Q48: Date Functions
**Question:** Find employees hired in 2020.
```sql
SELECT emp_name, hire_date FROM Employees 
WHERE YEAR(hire_date) = 2020;
```
**Output:** Employees hired in 2020.

---

### Q49: BETWEEN with Dates
**Question:** Find employees hired between 2019 and 2020.
```sql
SELECT emp_name, hire_date FROM Employees 
WHERE hire_date BETWEEN '2019-01-01' AND '2020-12-31';
```
**Output:** Employees from 2019-2020 period.

---

### Q50: Complex Query - Combination
**Question:** Find department names with employee count, average salary, and list of employees earning above department average.
```sql
SELECT 
  d.dept_name,
  COUNT(e.emp_id) as employee_count,
  ROUND(AVG(e.salary), 2) as avg_salary,
  SUM(e.salary) as total_salary
FROM Departments d 
LEFT JOIN Employees e ON d.dept_id = e.dept_id 
GROUP BY d.dept_id, d.dept_name 
HAVING COUNT(e.emp_id) > 0
ORDER BY total_salary DESC;
```
**Output:** Comprehensive department analysis.

---

## KEY TECHNIQUES COVERED

1. **Basic SELECT** - Columns, WHERE clauses
2. **Operators** - AND, OR, IN, NOT IN, BETWEEN, LIKE
3. **Sorting** - ORDER BY ASC/DESC
4. **Aggregation** - COUNT, SUM, AVG, MAX, MIN
5. **Grouping** - GROUP BY, HAVING
6. **JOINs** - INNER, LEFT, RIGHT, FULL, Self, Multiple
7. **Set Operations** - UNION, UNION ALL
8. **Subqueries** - WHERE, IN, EXISTS, Correlated
9. **Conditionals** - CASE statements
10. **Pagination** - LIMIT, OFFSET
11. **String Functions** - CONCAT, UPPER, LOWER, LENGTH
12. **Date Functions** - YEAR, BETWEEN for dates

---

## TIPS FOR EXAM SUCCESS

- Always use table aliases (e.g., e, d) in JOINs for clarity
- Start with simpler queries before combining complex logic
- Use meaningful column names in output (aliases)
- Test WHERE conditions before adding JOINs
- Remember: WHERE filters rows before GROUP BY; HAVING filters groups after
- Practice writing queries without looking at examples
- Understand the difference between COUNT(*) and COUNT(column_name)
