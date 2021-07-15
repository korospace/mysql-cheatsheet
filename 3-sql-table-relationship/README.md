
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
    <li><a href="#insert-data">one to one</a></li>
    <li><a href="#update-data">one to many</a></li>
  </ul>
</details>

## one to one
***example case:***
* first, create a users table
    ```
    create table users(
        user_id    VARCHAR(100) PRIMARY KEY,
        name       VARCHAR(100),
    ) Engine = InnoDB;
    ```
* second, create a wallet table
    ```
    CREATE TABLE wallets(
        wallet_id INT PRIMARY KEY AUTO_INCREMENT,
        user_id   VARCHAR(100),
        balance   INT
    ) Engine = InnoDB;
    ```
* third, add foreign key to wallets table references to users table
    ```
    ALTER TABLE wallets 
    ADD CONSTRAINT fk_wallets_users 
    FOREIGN KEY (user_id) 
    REFERENCES users(user_id);
    ```
* fourth, make the user_id column in the wallets table as unique value
    ```
    ALTER TABLE wallets 
    ADD CONSTRAINT userid_unique 
    UNIQUE (user_id);
    ```
    NOTE: _This step is very important because it only allows one row in the wallets column to be connected to one row in the users column_
* insert data
    ```
    INSERT INTO users(user_id,name) VALUES('U001','Johan'),('U002','Zaki'),('U003','Maryam'),('U004','Jordan');
    INSERT INTO wallets(user_id,balance) VALUES('U001',1000000),('U003',2300000);
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