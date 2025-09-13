The following steps provide examples of how I used SQL with filters to perform security-related
tasks.
In this section, I’ll walk through the exact steps I took during the investigation. I used SQL queries to filter data and spot patterns, while making sure the results were clear enough to support decision-making.

---

### Step 1: Failed logins after business hours
There was a potential security incident that occurred after business hours (after 18:00). All after
hours login attempts that failed could be a sign of suspicious behavior, and need to be investigated.
The following code demonstrates how I created a SQL query to filter for failed login attempts
that occurred after business hours.

Query:
```bash
SELECT * FROM log_in_attempts 
WHERE login_time > '18:00' AND success = FALSE;
```
![After hours failed login]()

The first part of the screenshot is my query, and the second part is a portion of the output. This
query filters for failed login attempts that occurred after **18:00**. First, I started by selecting all
data from the `log_in_attempts table`. Then, I used a `WHERE` clause with an `AND` operator to
filter my results to output only login attempts that occurred after `18:00` and were unsuccessful.
The first condition is `login_time > '18:00'`, which filters for the login attempts that
occurred after 18:00. The second condition is `success = FALSE`, which filters for the failed
login attempts.
> Result: I found 19 failed login attempts after business hours. These could point to either employees forgetting passwords late at night, or possibly an attacker testing credentials.

---

### Step 2: Retrieve login attempts on specific dates
A suspicious event occurred on `2022-05-09`. Any login activity that happened on `2022-05-09`
or on the day before needs to be investigated.

Query:
```bash
SELECT * FROM log_in_attempts 
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```
![Specific dates login attempts]()

The first part of the screenshot is my query, and the second part is a portion of the output. This
query returns all login attempts that occurred on **2022-05-09** or **2022-05-08**. First, I started by
selecting all data from the `log_in_attempts` table. Then, I used a `WHERE` clause with an `OR`
operator to filter my results to output only login attempts that occurred on either 2022-05-09
or 2022-05-08. The first condition is `login_date = '2022-05-09'`, which filters for logins
on 2022-05-09. The second condition is `login_date = '2022-05-08'`, which filters for
logins on 2022-05-08.
> Result: A total of 75 login attempts were made across those two days. Having this breakdown helped in correlating logs with reported issues.

---

### Step 3: Retrieve login attempts outside of Mexico
The company mainly operates in Mexico, so logins from other regions stood out as unusual. To focus on these, I excluded Mexico-based attempts.
The following code demonstrates how I created a SQL query to filter for login attempts that
occurred outside of Mexico.

Query:
```bash
SELECT * FROM log_in_attempts 
WHERE NOT country LIKE 'MEX%';
```
![Logs outside of Mexico]()

The first part of the screenshot is my query, and the second part is a portion of the output. This
query returns all login attempts that occurred in countries other than Mexico. First, I started by
selecting all data from the `log_in_attempts` table. Then, I used a `WHERE` clause with `NOT` to
filter for countries other than Mexico. I used `LIKE` with `MEX%` as the pattern to match because
the dataset represents Mexico as `MEX` and `MEXICO`. The percentage sign (`%`) represents any
number of unspecified characters when used with `LIKE`.
> Result: Found 144 login attempts coming from outside of Mexico. These required a closer look to confirm if they were legitimate remote workers or unauthorized access attempts.

---

### Step 4: Retrieve employees in Marketing (East building) 
My team wants to update the computers for certain employees in the Marketing department, but only those working in East offices.
To do this, I have to get information on which employee machines to update. The following code demonstrates how I created a SQL query to filter for employee machines from employees in the Marketing department in the East building.

Query:
```bash
SELECT * FROM employees 
WHERE department = 'Marketing' AND office LIKE 'East%';
```
![Employees in East building]()

The first part of the screenshot is my query, and the second part is a portion of the output. This
query returns all employees in the Marketing department in the East building. First, I started by
selecting all data from the `employees` table. Then, I used a `WHERE` clause with `AND` to filter for
employees who work in the Marketing department and in the East building. I used `LIKE` with
`East%` as the pattern to match because the data in the `office` column represents the East
building with the specific office number. The first condition is the `department =
'Marketing'` portion, which filters for employees in the Marketing department. The second
condition is the `office LIKE 'East%'` portion, which filters for employees in the East
building.
> Result: The query returned several employees, with the first being username = elarson.

---

### Step 5: Retrieve employees in Finance or Sales
The machines for employees in the **Finance** and **Sales** departments also need to be updated.
Since a different security update is needed, I have to get information on employees only from
these two departments.

Query:
```bash
SELECT * FROM employees 
WHERE department = 'Finance' OR department = 'Sales';
```
![Empolyees from finance or sales]()

The first part of the screenshot is my query, and the second part is a portion of the output. This
query returns all employees in the **Finance** and **Sales** departments. First, I started by selecting
all data from the `employees` table. Then, I used a `WHERE` clause with `OR` to filter for employees
who are in the Finance and Sales departments. I used the `OR` operator instead of `AND` because I
want all employees who are in either department. The first condition is `department =
'Finance'`, which filters for employees from the Finance department. The second condition is
`department = 'Sales'`, which filters for employees from the Sales department.
> Result: The query returned employees from both departments, and the first Sales employee listed was username = lrodriqu.

---

### Step 5: Retrieve all employees not in IT
My team needs to make one more security update on employees who are not in the
Information Technology department. To make the update, I first have to get information on
these employees.

Query:
```bash
SELECT * FROM employees 
WHERE NOT department = 'Information Technology';
```
![Employees not in IT]()

The first part of the screenshot is my query, and the second part is a portion of the output. The
query returns all employees not in the Information Technology department. First, I started by
selecting all data from the `employees` table. Then, I used a `WHERE` clause with `NOT` to filter for
employees not in this department.
> Result: Found 161 employees who still needed updates outside of IT.

---
