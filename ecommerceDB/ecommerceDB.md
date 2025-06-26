# EcommerceDB on PostgresSQL

## DATABASE: store
```sql
CREATE DATABASE store;
```

## TABLE INTIALISATION

### Customer
```sql
CREATE TABLE customer(
    c_id SERIAL PRIMARY KEY,
    fname VARCHAR(150) NOT NULL,
    lname VARCHAR(150) NOT NULL,
    c_date DATE DEFAULT CURRENT_DATE
);
```
```sql example-good
\d customer
```
