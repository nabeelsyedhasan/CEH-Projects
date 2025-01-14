# SQL Injection Writeup

## Overview
SQL Injection is a vulnerability that occurs when user inputs are directly embedded into SQL queries without proper validation. This allows an attacker to execute arbitrary SQL commands, potentially leading to data breaches, privilege escalation, or complete control of the database.

---

## **Steps Taken**

### 1. Identifying the Vulnerability
- Navigated to the SQL Injection module in DVWA.
- Entered a test payload into the input field:
  ```sql
  ' OR '1'='1
  ```
- Observed abnormal behavior, confirming the application was vulnerable to SQL Injection.

### 2. Automating Exploitation with `sqlmap`
Used `sqlmap`, a powerful tool for SQL Injection testing and automation, to enumerate the databases:
```bash
sqlmap -u "http://127.0.0.1/vulnerabilities/sqli/?id=1&Submit=Submit" \
       --cookie="PHPSESSID=<SESSION_ID>; security=low" --dbs
```

- The following steps were performed by `sqlmap`:
  1. Testing for vulnerabilities related to SQL Injection .
  2. Identifying the DBMS as MySQL.
  3. Fetching the list of databases.

### 3. Results
- Successfully enumerated the following databases:
  - `information_schema`
  - `dvwa`

Refer to the screenshots:
- ![SQLmap-Command-Execution](https://github.com/user-attachments/assets/eca722b8-387a-4a0b-8ea8-ccffd9907ebf)
<p align="center">SQLmap Command Execution</p>
* *
- ![SQLmap-Results](https://github.com/user-attachments/assets/f971f898-4c24-48f0-bcbd-274c193a59cd)
<p align="center">SQLmap-Results</p>

---

## **Analysis**
- **Impact:**
  - The attacker can extract sensitive information from the database.
  - Potential access to user credentials, financial data, or other sensitive records.
  - Possibility of escalating privileges to gain full control over the database.

- **Exploitability:**
  - Exploiting SQL Injection requires basic knowledge of SQL syntax and tools like `sqlmap`.

---

## **Mitigation Strategies**
1. **Use Parameterized Queries:**
   - Avoid directly embedding user inputs into SQL queries.
   - Use prepared statements to separate query logic from data inputs.

   Example (PHP):
   ```php
   $stmt = $pdo->prepare('SELECT * FROM users WHERE id = :id');
   $stmt->execute(['id' => $id]);
   ```

2. **Input Validation:**
   - Validate and sanitize all user inputs.
   - Reject inputs that do not match the expected format.

3. **Use an ORM:**
   - Use an Object-Relational Mapping (ORM) tool to abstract SQL queries.

4. **Least Privilege Principle:**
   - Ensure the database user has the minimum required privileges.

5. **Monitor and Audit Logs:**
   - Regularly monitor application and database logs for suspicious activity.

---

