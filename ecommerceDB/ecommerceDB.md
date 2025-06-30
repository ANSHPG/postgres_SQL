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
\d orderItems
```

>[!NOTE]
> ```sql \d```
> To check all the tables have been initiated or not

## Entry: table

### Customer

```sql
INSERT INTO customer(
    fname,
    lname
)
VALUES
('Ira','Benin'),
('Florence','Light'),
('Valine','Rose'),
('Elina','Scotch'),
('Sofia','Valgaire')
;
```
```sql
SELECT * FROM customer;
```

### Customer

```sql
INSERT INTO product(
    p_name,
    mrp
)
VALUES
('CRUCIAL_SSD_240GB',1780),
('MSI_MOTHERBOARD',12520),
('SAMSUNG_DDR5_32GB_RAM',12660),
('INTEL_I7_14_GEN',41000),
('ASUS_SMPS_1000W',20219),
('LG_27_4K_MONITOR',48499),
('TP_LINK_WIFI_6_ROUTER',2999)
;
```
```sql
SELECT * FROM product;
```
>[!NOTE]
>We know it would have been better to add column for quantity, but for simplification we have decided to ommit that :)

### Bill
```sql
INSERT INTO bill(c_id)
VALUES
(1),(3),(5),(2),(5),(4),(3);
```
```sql
SELECT * FROM bill;
```
### OrderItems
```sql
INSERT INTO orderItems(
    b_id,
    p_id,
    quantity
)
VALUES
(1,1,2),
(1,3,3),
(2,6,1),
(3,7,4),
(3,5,2),
(3,1,2),
(4,3,2),
(5,6,2),
(6,2,1),
(6,4,2),
(7,2,1)
;
```
> [!WARNING]
> In case there is default serial input in column, then follow this code:
> ```sql 
>UPDATE orderItems
>SET i_id = i_id - 11
>WHERE i_id > 11 
>; 
>```

```sql
SELECT * FROM orderItems;
```

## Queries

### Join

```sql
SELECT 
b.b_id,
b.c_id,
p.p_name,
p.mrp
FROM orderItems i
JOIN bill b ON i.b_id = b.b_id
JOIN product p ON i.p_id = p.p_id
WHERE b.b_id = 3
;
```

### Join: all tables

```sql
SELECT 
c.c_id AS customer_id,
CONCAT_WS(' ',c.fname, c.lname) AS customer,
p.p_name AS product,
p.mrp AS mrp,
b.b_id AS bill_id,
o.i_Id AS item_id
FROM orderItems o
INNER JOIN  product p ON o.p_id = p.p_id
INNER JOIN  bill b ON o.b_id = b.b_id
INNER JOIN  customer c ON b.c_id = c.c_id
;
```

### GROUP BY

#### Filter: customers spent more than avg.
```sql
SELECT 
c.c_id AS customer_id,
CONCAT_WS(' ',c.fname, c.lname) AS customer,
COUNT(o.i_id) AS total_orders,
SUM(p.mrp) AS amount_spent
FROM orderItems o
INNER JOIN  product p ON o.p_id = p.p_id
INNER JOIN  bill b ON o.b_id = b.b_id
INNER JOIN  customer c ON b.c_id = c.c_id
GROUP BY c.c_id, customer
HAVING SUM(p.mrp) > (
SELECT AVG(spent) FROM(
SELECT AVG(p.mrp) AS spent
FROM orderItems o
INNER JOIN  product p ON o.p_id = p.p_id
INNER JOIN  bill b ON o.b_id = b.b_id
INNER JOIN  customer c ON b.c_id = c.c_id
GROUP BY c.c_id)
)
ORDER BY customer_id
;
```

#### Filter: Orders frequency & Sales
```sql
SELECT 
p.P_id AS product_id,
p.p_name AS product,
p.mrp AS mrp,
COUNT(o.i_id) AS orders,
SUM(p.mrp) AS sales
FROM orderItems o
INNER JOIN  product p ON o.p_id = p.p_id
INNER JOIN  bill b ON o.b_id = b.b_id
INNER JOIN  customer c ON b.c_id = c.c_id
GROUP BY p.p_id, p.p_name
ORDER BY p.p_id
;
```