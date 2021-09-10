
<p align="center">
    <h1 align="center">MANY MORE</h1>
    <p align="center">
        <a href="../README.md"><strong>« back to menu</strong></a>
    </p>
    <br />
    <br />
</p>

<details open="open">
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#transaction">transaction</a></li>
    <li><a href="#locking">locking</a></li>
    <li><a href="#user-management">user management</a></li>
    <li><a href="#backup-database">backup database</a></li>
    <li><a href="#restore-database">restore database</a></li>
  </ul>
</details>

## transaction
*   ```
    START TRANSACTION;
    # your sql syntax
    # your sql syntax
    # your sql syntax
    # your sql syntax
    COMMIT;
    # or
    ROLLBACK;
    ```  
*   ```
    START TRANSACTION;
    SELECT * FROM table_name FOR UPDATE;
    # your sql syntax
    # your sql syntax
    # your sql syntax
    # your sql syntax
    COMMIT;
    # or
    ROLLBACK;
    ```  
    <br/>

## locking 
*   locking table
    ```
    LOCK TABLE table_name READ;
    # your dml syntax
    # your dml syntax
    # your dml syntax
    # your dml syntax
    UNLOCK TABLES;
    ```  

    _or_
    <br/>

    ```
    LOCK TABLE table_name WRITE;
    # your dml syntax
    # your dml syntax
    # your dml syntax
    # your dml syntax
    UNLOCK TABLES;
    ```  

*   locking instance
    ```
    LOCK INSTANCE FOR BACKUP;
    # your ddl syntax
    # your ddl syntax
    # your ddl syntax
    # your ddl syntax
    UNLOCK INSTANCE;
    ```  
    <br/>

## user management
* create user
    ```
    CREATE USER 'abdul'@'%';
    CREATE USER 'john'@'localhost';
    ```
* set password
    ```
    SET PASSWORD FOR 'abdul'@'%' = PASSWORD('your_pass');
    SET PASSWORD FOR 'john'@'localhost' = PASSWORD('your_pass');
    ```
* delete user
    ```
    DROP USER 'abdul'@'%';
    DROP USER 'john'@'localhost';
    ```
* give access
    ```
    # all table
    GRANT SELECT,UPDATE ON database_name.* TO 'john'@'localhost';
    # specific table
    GRANT SELECT,UPDATE,INSERT,DELETE ON database_name.table_name TO 'john'@'localhost';
    ```
* show access
    ```
    SHOW GRANTS FOR 'john'@'localhost';
    ```
* drop access
    ```
    # all table
    REVOKE SELECT ON database_name.* FROM 'john'@'localhost';
    # specific table
    GRANT UPDATE,INSERT,DELETE ON database_name.table_name FROM 'john'@'localhost';
    ```

## backup database
* ```
  /opt/lampp/bin/mysqldump database_name --user root --password --result-file=/opt/lampp/htdocs/database_name.sql
  ```
## restore database
* ```
  # method 1
  /opt/lampp/bin/mysql --user root --password database_name < /opt/lampp/htdocs/database_name.sql
  
  # method 2
  USE database_name;
  SOURCE /opt/lampp/htdocs/database_name.sql;
  ```