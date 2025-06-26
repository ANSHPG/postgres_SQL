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

### Bill

```sql
CREATE TABLE bill(
    b_id SERIAL PRIMARY KEY,
    c_id INT NOT NULL,
    b_date DATE DEFAULT CURRENT_DATE,

    FOREIGN KEY(c_id) REFERENCES customer(c_id) 
);
```
```sql
\d bill
```

### Order_Items

```sql
CREATE TABLE orderItems(
    i_id SERIAL PRIMARY KEY,
    b_id INT NOT NULL,
    p_id INT NOT NULL,
    quantity INT DEFAULT 1,

    FOREIGN KEY(b_id) REFERENCES bill(b_id),
    FOREIGN KEY(p_id) REFERENCES product(p_id)
);
```
```sql
\d bill
```

>[!NOTE]
> ```sql \d```
> To check all the tables have been initiated or not