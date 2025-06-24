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
