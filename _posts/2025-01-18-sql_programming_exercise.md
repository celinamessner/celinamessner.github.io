---
layout: post
title: "SQL Programming Exercise"
subtitle: "Building a company database with departments and employees in MySQL"
date: 2025-01-18
author: "Celina Messner"
categories: [sql, database]
tags: [sql, mysql, database, joins, aggregation]
---

### 🗃️ COMPANY1 Database

This SQL script creates a database called `COMPANY1` with two tables — `DEPT` (departments) and `EMP` (employees). The data spans department information (location, name, identifier) and employee information (name, job title, hire date, salary, commission, manager). The queries below answer specific business questions against this schema using filters, joins, and aggregations.

### Schema

The department table uses `DEPTNO` as the primary key. The employee table uses `EMPNO` as the primary key, and `DEPTNO` as a foreign key back to `DEPT`.

```sql
CREATE DATABASE COMPANY1;
USE COMPANY1;

CREATE TABLE DEPT (
    DEPTNO INT,
    DNAME  VARCHAR(15),
    LOC    VARCHAR(15),
    PRIMARY KEY (DEPTNO)
);

CREATE TABLE EMP (
    EMPNO    INT,
    ENAME    VARCHAR(15),
    JOB      VARCHAR(15),
    MGR      INT,
    HIREDATE DATE,
    SAL      DECIMAL(7,2),
    COMM     DECIMAL(7,2),
    DEPTNO   INT,
    PRIMARY KEY (EMPNO),
    FOREIGN KEY (DEPTNO) REFERENCES DEPT(DEPTNO)
);
```

### Seed data

Note: some `COMM` values are `NULL` rather than `0`, because the assignment data treats absence-of-commission as "not applicable" rather than literally zero.

```sql
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES
    (10, 'ACCOUNTING', 'NEW YORK'),
    (20, 'RESEARCH',   'DALLAS'),
    (30, 'SALES',      'CHICAGO');

INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
    (7369, 'SMITH',  'CLERK',     7902, '1980-12-17',  800, NULL, 20),
    (7499, 'ALLEN',  'SALESMAN',  7698, '1981-02-20', 1600,  300, 30),
    (7521, 'WARD',   'SALESMAN',  7698, '1981-02-22', 1250,  500, 30),
    (7566, 'JONES',  'MANAGER',   7839, '1981-04-02', 2975, NULL, 20),
    (7654, 'MARTIN', 'SALESMAN',  7698, '1981-09-28', 1250, 1400, 30),
    (7698, 'BLAKE',  'MANAGER',   7839, '1981-05-01', 2850, NULL, 30),
    (7782, 'CLARK',  'MANAGER',   7839, '1981-06-09', 2450, NULL, 10),
    (7788, 'SCOTT',  'ANALYST',   7566, '1987-04-19', 3000, NULL, 20),
    (7839, 'KING',   'PRESIDENT', NULL, '1981-11-17', 5000, NULL, 10),
    (7844, 'TURNER', 'SALESMAN',  7698, '1981-09-08', 1500, NULL, 30),
    (7876, 'ADAMS',  'CLERK',     7788, '1987-05-23', 1100, NULL, 20),
    (7900, 'JAMES',  'CLERK',     7698, '1981-12-03',  950, NULL, 30),
    (7902, 'FORD',   'ANALYST',   7566, '1981-12-03', 3000, NULL, 20),
    (7934, 'MILLER', 'CLERK',     7782, '1982-01-23', 1300, NULL, 10);
```

### Query 1 — Salary between 1,000 and 2,000 (exclusive of 2,000)

Joins `EMP` and `DEPT` to display employee name, department, and salary for all employees who earn more than 1,000 but not exactly 2,000.

```sql
SELECT E.ENAME AS Employee_name,
       D.DNAME AS Department,
       E.SAL   AS Salary
FROM   EMP E
JOIN   DEPT D ON E.DEPTNO = D.DEPTNO
WHERE  E.SAL > 1000
  AND  E.SAL <> 2000;
```

### Query 2 — Employees in department 30 with both salary and commission

Filters the `EMP` table for `DEPTNO = 30` and rows where neither salary nor commission is `NULL`, then counts the matching rows.

```sql
SELECT COUNT(*) AS Count_received_salary_and_commission
FROM   EMP
WHERE  DEPTNO = 30
  AND  SAL  IS NOT NULL
  AND  COMM IS NOT NULL;
```

### Query 3 — Name and salary of employees in Dallas earning ≥ 1,000

Joins the two tables on `DEPTNO` and applies both conditions (salary ≥ 1,000 *and* location is Dallas).

```sql
SELECT E.ENAME AS Employee_name,
       E.SAL   AS Salary,
       D.LOC   AS Location
FROM   EMP E
JOIN   DEPT D ON E.DEPTNO = D.DEPTNO
WHERE  E.SAL >= 1000
  AND  D.LOC = 'DALLAS';
```

### Query 4 — Departments with no current employees

A `LEFT JOIN` from `DEPT` to `EMP` keeps every department, even ones without employees. Filtering for `E.EMPNO IS NULL` isolates the departments that did not match any employee row.

```sql
SELECT D.DEPTNO,
       D.DNAME
FROM   DEPT D
LEFT JOIN EMP E ON D.DEPTNO = E.DEPTNO
WHERE  E.EMPNO IS NULL;
```

### Query 5 — Per-department average salary and headcount

Aggregates with `AVG()` and `COUNT()`, grouped by `DEPTNO`.

```sql
SELECT DEPTNO,
       AVG(SAL) AS Average_salary,
       COUNT(*) AS Num_employees
FROM   EMP
GROUP BY DEPTNO;
```

### Results

- **Query 1** — 12 employees match the salary criteria.
- **Query 2** — 4 employees in department 30 received both a salary and a commission.
- **Query 3** — 4 employees in Dallas earn ≥ 1,000.
- **Query 4** — Empty set: every department has at least one employee.
- **Query 5** — Average salary and headcount computed for each of the three departments.

### References

MySQL (2025) [15.1.12 CREATE DATABASE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-database.html).

W3 Schools (2025) [MySQL Tutorial](https://www.w3schools.com/mysql/mysql_sql.asp).
