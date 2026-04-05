# SQL Injection — DVWA (Low Security)

## Objective
 
Test the DVWA "SQL Injection" module using injection techniques — from basic data extraction to discovering the database structure, dumping passwords, and identifying the database version.

## Evironment

 
| Machine | OS | Role |
|---|---|---|
| Kali Linux | Kali | Attack platform |
| Metasploitable 2 | Ubuntu Linux | Hosts DVWA web application |

**Target application:** Damn Vulnerable Web Application (DVWA)

**DVWA security level:** Low

**Network:** Isolated host-only network


## Information

An SQL injection occurs when an application passes a user input directily into an SQL query. If the input field does no filter out special characters such as quotes or logical SQL operators, attackers can use these inputs to return data that they shouldn't have access to. They could even potentially also add, modify or delete data records completly.

DVWA's SQL Injection module can simulate this by providing a simple "User ID" lookup form that is backed by a database query. The original query that is running behind the form should look like:

```sql
SELECT first_name, last_name FROM users WHERE user_id = '$input';
```

All injections bellow takes advantage of the 'input' portion of the code and appending additional SQL logic, beyond what is expected.

## Attack 1: Dumping All Users

**SQL Query:**
```
1' OR 1=1#
```





