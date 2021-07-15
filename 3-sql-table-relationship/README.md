
<p align="center">
    <h1 align="center">CRUD</h1>
    <p align="center">
        the syntax below is used to create, read, update or delete data on table.<br />
        <a href="../README.md"><strong>« back to menu</strong></a>
    </p>
    <br />
    <br />
</p>

<details close="close">
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#one-to-one">one to one</a></li>
    <li><a href="#one-to-many">one to many</a></li>
    <li><a href="#many-to-many">many to many</a></li>
  </ul>
</details>

## one to one
***example case:***
* first, create a _wallet table_
    ```
    CREATE TABLE wallets(
        wallet_id INT PRIMARY KEY AUTO_INCREMENT,
        user_id   VARCHAR(100),
        balance   INT
    ) Engine = InnoDB;
    ```
* second, create a _users table_
    ```
    create table users(
        user_id    VARCHAR(100) PRIMARY KEY,
        name       VARCHAR(100),
    ) Engine = InnoDB;
    ```
* third, add foreign key to _wallets table_ references to _users table_
    ```
    ALTER TABLE wallets 
    ADD CONSTRAINT fk_wallets_users 
    FOREIGN KEY (user_id) 
    REFERENCES users(user_id);
    ```
* fourth, make the user_id column in the _wallets table_ as unique value
    ```
    ALTER TABLE wallets 
    ADD CONSTRAINT userid_unique 
    UNIQUE (user_id);
    ```
    NOTE: *we can't add foreign key in column category_id at categories table because it's primary key for categories table*
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
    SELECT name,balance 
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
* first, create a _categories table_
    ```
    CREATE TABLE categorie (
        category_id varchar(100) PRIMARY KEY,
        name        varchar(100) NOT NULL
    ) ENGINE=InnoDB;
    ```
* second, create a _products table_
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
* third, add foreign key to _products table_ references to _categories table_
    ```
    ALTER TABLE products 
    ADD CONSTRAINT fk_prod_cat 
    FOREIGN KEY (category_id) 
    REFERENCES categories(category_id);
    ```
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

