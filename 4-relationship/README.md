
<p align="center">
    <h1 align="center">RELATIONSHIP</h1>
    <p align="center">
        WARNING: make sure you understand how to create a foreignkey and join data in table, Click <a href="../1-ddl/README.md/#foreign-key">here</a> if you don't understand how to create a foreignkey and <a href="../3-select/README.md/#join">here</a> for understanding how to join data.<br/>
        <a href="../README.md"><strong>« back to menu</strong></a>
    </p>
    <br />
    <br />
</p>

<details open="open">
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#one-to-one">one to one</a></li>
    <li><a href="#one-to-many">one to many</a></li>
    <li><a href="#many-to-many">many to many</a></li>
  </ul>
</details>

## one to one
***example case:***
* first, create a ***wallet table***
    ```
    CREATE TABLE wallets(
        wallet_id INT PRIMARY KEY AUTO_INCREMENT,
        user_id   VARCHAR(100),
        balance   INT
    ) Engine = InnoDB;
    ```
* second, create a ***users table***
    ```
    create table users(
        user_id    VARCHAR(100) PRIMARY KEY,
        name       VARCHAR(100),
    ) Engine = InnoDB;
    ```
* third, add foreign key to ***wallets table*** references to ***users table***
    ```
    ALTER TABLE wallets 
    ADD CONSTRAINT fk_wallets_users 
    FOREIGN KEY (user_id) 
    REFERENCES users(user_id);
    ```
* fourth, make the user_id column in the **wallets table** as unique value
    ```
    ALTER TABLE wallets 
    ADD CONSTRAINT userid_unique 
    UNIQUE (user_id);
    ```
    NOTE: _This step is very important because it only allows one row in the wallets column to be connected to one row in the users column_
* insert data
    ```
    INSERT INTO users(user_id,name) 
    VALUES ('U001','Johan'),
           ('U002','Zaki'),
           ('U003','Maryam'),
           ('U004','Jordan');
    ```
    ```
    INSERT INTO wallets(user_id,balance) 
    VALUES ('U001',1000000),
           ('U003',2300000);
    ```
* how to use?
    ```
    SELECT name,balance 
    FROM wallets 
    JOIN users 
    ON (wallets.user_id = users.user_id);
    ```
    _or_
    <br/>

    ```
    ```
    SELECT u.name,w.balance 
    FROM wallets AS w 
    JOIN users   AS u 
    ON (w.user_id = u.user_id);
    ```
    | name   | balance |
    | :--: | :--: |
    | Johan  | 1000000 |
    | Maryam | 2300000 |
<br/>

## one to many
***example case:***
* first, create a ***categories table***
    ```
    CREATE TABLE categorie (
        category_id varchar(100) PRIMARY KEY,
        name        varchar(100) NOT NULL
    ) ENGINE=InnoDB;
    ```
* second, create a ***products table***
    ```
    CREATE TABLE product (
        product_id  varchar(100) PRIMARY KEY,
        name        varchar(100) NOT NULL,
        price       int(11)      NOT NULL DEFAULT 0,
        quantity    int(11)      NOT NULL DEFAULT 0,
        category_id varchar(100),
        created_at  timestamp    NOT NULL DEFAULT current_timestamp()
    ) ENGINE=InnoDB;
    ```
* third, add foreign key to ***products table*** references to ***categories table***
    ```
    ALTER TABLE products 
    ADD CONSTRAINT fk_prod_cat 
    FOREIGN KEY (category_id) 
    REFERENCES categories(category_id);
    ```
    NOTE: *we can't add foreign key in column category_id at categories table because it's primary key for categories table*
* insert data
    ```
    INSERT INTO categories 
    VALUES ('C10','spicy food'),
           ('C20','fresh drink');
    ```
    ```
    INSERT INTO products(product_id,name,price,quantity,category_id) 
    VALUES ('P0011','Spicy grilled chicken',15000,12,'C10'),
           ('P0012','spicy burger'         ,9000 ,2 ,'C10'),
           ('P0013','grapefruit ice'       ,5000 ,20,'C20'),
           ('P0014','Mango juice'          ,5000 ,11,'C20');
    ```
* how to use?
    ```
    SELECT categories.name AS category_name,
           products.name   AS product_name 
    FROM categories 
    JOIN products 
    ON (categories.category_id = products.category_id);
    ```
    | category_name   | product_name              |
    | :--: | :--: |
    | spicy food      | Spicy grilled chicken     |
    | spicy food      | spicy burger              |
    | fresh drink     | grapefruit ice            |
    | fresh drink     | Mango juice               |
<br/>

## many to many
many to many is a combination of two or more one to many. in this case, orders to orders_detail is one to many. Products to orders_detail is one to many. Users to orders_detail is one to many.
<br/>


***example case:***
* first, create a ***users table***
    ```
    create table users(
        user_id    VARCHAR(100) PRIMARY KEY,
        name       VARCHAR(100),
    ) Engine = InnoDB;
    ```
* second, create a ***products table***
    ```
    CREATE TABLE product (
        product_id  varchar(100) PRIMARY KEY,
        name        varchar(100) NOT NULL,
        price       int(11)      NOT NULL DEFAULT 0,
        quantity    int(11)      NOT NULL DEFAULT 0,
        category_id varchar(100),
        created_at  timestamp    NOT NULL DEFAULT current_timestamp()
    ) ENGINE=InnoDB;
    ```
* third, create a ***orders table***
    ```
    CREATE TABLE orders(
        order_id   VARCHAR(100) PRIMARY KEY,
        created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
    ) Engine = InnoDB;

    ```
* fourth, create a ***orders_detail table***
    ```
    CREATE TABLE orders_detail(
        order_id   VARCHAR(100),
        product_id VARCHAR(100),
        user_id    VARCHAR(100),
        quantity   INT
    ) Engine = InnoDB;
    ```
* insert data
    ```
    INSERT INTO orders(order_id) 
    VALUES ('ORDER1')
           ('ORDER2')
           ('ORDER3')
           ('ORDER4');
    ```
    ```
    INSERT INTO orders_detail(order_id,product_id,user_id,quantity) 
    VALUES ('ORDER1','P0001','U001',3),
           ('ORDER1','P0002','U001',2),
           ('ORDER1','P0003','U001',5),
           ('ORDER1','P0004','U001',1),
           ('ORDER2','P0001','U004',10),
           ('ORDER3','P0002','U003',6),
           ('ORDER4','P0003','U002',9),
           ('ORDER4','P0004','U001',9);
    ```
* how to use?
    ```
    SELECT o.order_id,
           u.name AS 'user name',
           p.name AS 'product name',
           od.quantity 
    FROM orders_detail AS od 
    JOIN orders        AS o ON (o.order_id   = od.order_id) 
    JOIN users         AS u ON (u.user_id    = od.user_id) 
    JOIN products      AS p ON (p.product_id = od.product_id);
    ```
    | order_id | user name   | product name | quantity |
    | :--: | :--: | :--: | :--: |
    | ORDER1   | Johan  | Popcorn Creamello 150gram |        3 |
    | ORDER1   | Johan  | Tricks backed chips       |        2 |
    | ORDER1   | Johan  | Rondoleti wafer           |        5 |
    | ORDER1   | Johan  | Kripik pisang sale        |        1 |
    | ORDER4   | Johan  | Kripik pisang sale        |        9 |
    | ORDER4   | Zaki   | Rondoleti wafer           |        9 |
    | ORDER3   | Maryam | Tricks backed chips       |        6 |
    | ORDER2   | Jordan | Popcorn Creamello 150gram |       10 |
