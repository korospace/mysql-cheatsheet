
<p align="center">
    <h1 align="center">DDL</h1>
    <p align="center">
        the syntax below is used to create or change table's schema.<br />
        <a href="../README.md"><strong>« back to menu</strong></a>
    </p>
    <br />
    <br />
</p>

<details open="open">
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#show-databases">show databases</a></li>
    <li><a href="#create-database">create database</a></li>
    <li><a href="#delete-database">delete database</a></li>
    <li><a href="#use-database">use database</a></li>
    <li><a href="#show-engines">show engines</a></li>
    <li><a href="#show-tables">show tables</a></li>
    <li><a href="#create-table">create table</a></li>
    <li><a href="#delete-table">delete table</a></li>
    <li><a href="#truncate-table">truncate table</a></li>
    <li><a href="#desc-table">desc table</a></li>
    <li><a href="#rename-table">rename table</a></li>
    <li><a href="#edit-column">edit column</a></li>
    <li><a href="#primary-key">primary key</a></li>
    <li><a href="#foreign-key">foreign key</a></li>
    <li><a href="#unique-constraint">unique constraint</a></li>
    <li><a href="#indexing">indexing</a></li>
    <li><a href="#full-text-search">full text search</a></li>
  </ul>
</details>

## show databases
```
SHOW DATABASES;
```
<br />

## create database
 ```
CREATE DATABASE database_name;
```
<br />

## delete database
```
DROP DATABASE database_name;
```
<br />

## use database
* before working with a database, we have to run the following line
    ```sh
    USE database_name;
    ```
<br />

## show engines
* This statement is used to display a list of existing storage engines in the database.
    ```sh
    SHOW ENGINES;
    ```
<br />

## set table engines
* This statement is used to display a list of existing storage engines in the database.
    ```sh
    ALTER TABLE dbname.tablename ENGINE=InnoDB;
    ```
<br />

## show tables
```
SHOW TABLES;
```
<br />

## create table
```
CREATE TABLE users 
(
    user_id    INT(11)      AUTO_INCREMENT,
    name       VARCHAR(100) NOT NULL,
    email      VARCHAR(100) NOT NULL,
    ava        VARCHAR(100) NOT NULL DEFAULT 'avatar.jpg',
    wallet_id  INT,
    created_at TIMESTAMP    NOT NULL DEFAULT CURRENT_TIMESTAMP,

    PRIMARY KEY(user_id)
) ENGINE = InnoDB;
```
<br />

## delete table
 ```
DROP TABLE table_name;
```
<br />

## truncate table
*This statement is used to remove all records from a table and reset AUTO_INCREMENT from zero.*
```
TRUNCATE table_name;
```
<br />

## desc table
* This statement is used to display the schema of the selected table.
    ```sh
    DESC table_name;
    ```
    _or_
    <br />

    ```sh
    SHOW CREATE TABLE table_name;
    ```
<br />

## rename table
```
ALTER TABLE table_name rename to new_table_name;
```
<br />

## edit column
* drop column
    ```sh
    ALTER TABLE users DROP COLUMN avatar;
    ```

* add column
    ```sh
    ALTER TABLE users ADD COLUMN avatar VARCHAR(100) NOT NULL DEFAULT 'avatar.png' AFTER email;
    ```

* rename column
    ```sh
    ALTER TABLE users CHANGE avatar img_profile VARCHAR(100) NOT NULL DEFAULT 'profile.png';
    ```

* modify column
    ```sh
    ALTER TABLE users MODIFY img_profile VARCHAR(255) AFTER user_id;
    ```
<br />

## primary key
* when creating a table. 
    ```sh
    CREATE TABLE wishlist 
    (
        wishlist_id INT(11),
        product_id  INT(11),
        quantity    INT(11) NO NULL DEFAULT 1,
        created_at  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

        PRIMARY KEY(wishlist_id)
    ) ENGINE = InnoDB;
    ```
    
* if the table has been created
    ```sh
    ALTER TABLE wishlist ADD PRIMARY KEY (wishlist_id);
    ```

* drop a primary key
    ```sh
    ALTER TABLE wishlist DROP PRIMARY KEY;
    ```
    NOTE: _DROP PRIMARY KEY doesn't work if auto_increment is active in target column_
<br />

## foreign key
* when creating a table. 
    ```sh
    CREATE TABLE wishlist (
        wishlist_id INT(11)   NOT NULL AUTO_INCREMENT,
        product_id  INT(11)   NOT NULL,
        quantity    INT(11)   NOT NULL DEFAULT 1,
        created_at  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

        PRIMARY KEY (wishlist_id),
        CONSTRAINT  fk_w_p FOREIGN KEY (product_id) REFERENCES products (product_id) ON DELETE CASCADE ON UPDATE CASCADE
    ) ENGINE=InnoDB;
    ```
    NOTE:  
    - default behavior of foreign key is RESTRICT, when we just write the following syntax: 
        ```
        CONSTRAINT  fk_w_p FOREIGN KEY (product_id) REFERENCES products (product_id)
        ```
        <br />
    
* if the table has been created
    ```sh
    ALTER TABLE users ADD CONSTRAINT fk_u_w FOREIGN KEY (wallet_id) REFERENCES wallets(wallet_id);
    ```

* drop a foreign key
    ```sh
    ALTER TABLE wishlist DROP CONSTRAINT fk_w_p;
    ALTER TABLE wishlist DROP INDEX fk_w_p;
    ```

* behavior of foreign key
    The behavior I mean is what happens to the main table if the reference table is changed.<br />
    |behavior    |    on Delele    |    on Update    |
    |    :---:   |      :---:      |      :---:      |      
    |RESTRICT    |  NOT ALLOWED    |   NOT ALLOWED   | 
    |CASCADE     |  akan dihapus   |  akan diupdate  |
    |NO ACTION   |   data tetap    |   data tetap    |
    |SET NULL    | Dijadikan null  | dijadikan null  |

<br />

## unique constraint
* when creating a table. 
    ```sh
    CREATE TABLE users(
        user_id    INT PRIMARY KEY AUTO_INCREMENT,
        avatar     VARCHAR(100) NOT NULL DEFAULT 'ava.png',
        name       VARCHAR(100) NOT NULL,
        email      VARCHAR(100) NOT NULL,
        wallet_id  INT,
        created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

        UNIQUE KEY name_is_unique(name)
    ) ENGINE = InnoDB;
    ```
    
* if the table has been created
    ```sh
    ALTER TABLE users ADD CONSTRAINT name_is_unique UNIQUE(name);
    ```

* drop a unique constraint
    ```sh
    ALTER TABLE users DROP CONSTRAINT name_is_unique;
    ```
<br />

## check constraint
* when creating a table. 
    ```sh
    CREATE TABLE products(
        product_id  INT PRIMARY KEY AUTO_INCREMENT,
        name        VARCHAR(100) NOT NULL,
        price       INT NOT NULL DEFAULT 1000,
        quantity    INT NOT NULL DEFAULT 0,
        category_id INT NOT NULL,
        created_at  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

        CONSTRAINT price_check CHECK (price >= 1000)
    ) ENGINE = InnoDB;
    ```
    
* if the table has been created
    ```sh
    ALTER TABLE products ADD CONSTRAINT price_check CHECK (price >= 1000);
    ```

* drop a constraint check
    ```sh
    ALTER TABLE products DROP CONSTRAINT price_check;
    ```
<br />

## indexing
* when creating a table. 
    ```sh
    CREATE TABLE products(
        product_id  INT PRIMARY KEY AUTO_INCREMENT,
        name        VARCHAR(100) NOT NULL,
        price       INT NOT NULL DEFAULT 1000,
        quantity    INT NOT NULL DEFAULT 0,
        category_id INT NOT NULL,
        created_at  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

        INDEX price_index(price)
    ) ENGINE = InnoDB;
    ```
    NOTE:  
    - you can create many indexes as you want 
    - you can create multiple indexes by using the following syntax:
        ```
        INDEX price_quantity_index(price,quantity);
        ```
        <br />
* if the table has been created
    ```sh
    ALTER TABLE products ADD INDEX price_index(price);
    ```

* drop a index
    ```sh
    ALTER TABLE produk DROP INDEX price_index;
    ```
<br />

## full text search
* when creating a table. 
    ```sh
    CREATE TABLE products
    (
        poduct_id   INT(11)      NOT NULL,
        name        VARCHAR(100) NOT NULL,
        price       INT          NOT NULL DEFAULT 1000,
        quantity    VARCHAR(100) NOT NULL DEFAULT 0,
        keyword     VARCHAR(100),
        description VARCHAR(100),
        created_at TIMESTAMP    NOT NULL DEFAULT CURRENT_TIMESTAMP,
        
        FULLTEXT product_search (name,keyword,description)
    ) ENGINE = InnoDB;
    ```
    NOTE:  
    - fulltext does't work for price field because integer 
    - how to use fulltext search:
        ```
        SELECT * FROM products WHERE MATCH(name,keyword.description) AGAINST("ayam bakso" IN NATURAL LANGUAGE MODE);
        SELECT * FROM products WHERE MATCH(name,keyword.description) AGAINST("-ayam +bakso" IN BOOLEAN MODE);
        SELECT * FROM products WHERE MATCH(name,keyword.description) AGAINST("ayam" WITH QUERY EXPANSION);
        ```
        <br />
* if the table has been created
    ```sh
    ALTER TABLE produk ADD FULLTEXT product_search(name,keyword,description);
    ```

* drop a index
    ```sh
    ALTER TABLE products DROP INDEX product_search;
    ```
<br />
