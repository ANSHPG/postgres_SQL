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
```sql
\d customer
```
### Product
```sql
CREATE TABLE product(
    p_id SERIAL PRIMARY KEY,
    p_name VARCHAR(150) NOT NULL,
    mrp DECIMAL(10,2) NOT NULL,
    p_date DATE DEFAULT CURRENT_DATE
);
```
```sql 
\d product
```
