# Databse: Course

```sql
CREATE DATABASE dojo;
\c dojo
```
### windows
```sql
\! cls
```
### linux
```sql
ctrl + l
```

## Students
```sql
CREATE TABLE student(
    s_id SERIAL PRIMARY KEY,
    fname VARCHAR(150) NOT NULL,
    lname VARCHAR(150) NOT NULL,
    addmission DATE DEFAULT CURRENT_DATE
);

\d student

SELECT * FROM student ;
```

## Courses

```sql 
CREATE TABLE course(
    c_id SERIAL PRIMARY KEY,
    c_name VARCHAR(150) NOT NULL,
    fees DECIMAL(10,2) NOT NULL 
);

\d course

SELECT * FROM course ;
```

## Enrolment: history

```sql
CREATE TABLE enroll(
    e_id SERIAL PRIMARY KEY,
    s_id INT NOT NULL,
    c_id INT NOT NULL,
    e_date DATE DEFAULT CURRENT_DATE,
    FOREIGN KEY (s_id) REFERENCES student(s_id),
    FOREIGN KEY (c_id) REFERENCES course(c_id)
);

\d enroll

SELECT * FROM enroll ;
```

> [!NOTE]
> 🐧 On Linux, sometimes after running commands like `\d` or `\l` in `psql`, the output enters a *pager* mode (usually using `less`), where pressing `Enter` scrolls line by line and `Esc` doesn’t work.
>
> To exit this mode, simply press **`q`** (for *quit*).

## INSERT VALUES

#### Student

```sql
INSERT INTO student(fname, lname)
VALUES
('Ira','Benin'),
('Florence','Light'),
('Valine','Rose'),
('Elina','Scotch'),
('Sofia','Valgaire')
;
```
>[!WARNING]
> In VALUES for multiple vlaues , dont use parent common brackets

### Course

```sql
INSERT INTO course(c_name, fees)
VALUES
('Bioinformatics',750.00),
('Neurotechnology',910.00),
('Biomimetics',430.00),
('Genomics',690.00),
('Bioprinting',570.00)
;
```
### Enroll

```sql
INSERT INTO enroll(s_id,c_id)
VALUES
(1,2), -- Ira in Neurotechnology
(2,3), -- Florence in Biometics
(2,4), -- Florence in Genomics
(3,5), -- Valine in Bioprinting
(4,1), -- Elina in Neurotechnology
(4,3), -- Elina in Biometics
(5,3)  -- Sofia in Biometics
;
```
## Queries

### Join

```sql
SELECT
s.s_id,
CONCAT_WS(' ',s.fname,s.lname),
c.c_name,
c.fees,
e.e_id 
FROM enroll e
JOIN student s ON e.s_id = s.s_id
JOIN course c ON e.c_id = c.c_id
ORDER BY s.s_id
;
```
### GroupBy

```sql
SELECT
s.s_id,
CONCAT_WS(' ',s.fname,s.lname),
COUNT(c.c_id) AS courses,
SUM(c.fees) AS total_fees
FROM enroll e
JOIN student s ON e.s_id = s.s_id
JOIN course c ON e.c_id = c.c_id
GROUP BY s.s_id
ORDER BY s.s_id
;
```
### Having with GroupBy

```sql
SELECT
s.s_id,
CONCAT_WS(' ',s.fname,s.lname),
COUNT(c.c_id) AS courses,
SUM(c.fees) AS total_fees
FROM enroll e
JOIN student s ON e.s_id = s.s_id
JOIN course c ON e.c_id = c.c_id
GROUP BY s.s_id
HAVING COUNT(c.c_id) > 1
ORDER BY s.s_id
;
```
>[!NOTE]
>From here onwards you can take muliple scenarios and practice them 