
<p align="center">
    <h1 align="center">DML</h1>
    <p align="center">
        the syntax below is used to create, update and delete data on table.<br />
        <a href="../README.md"><strong>« back to menu</strong></a>
    </p>
    <br />
    <br />
</p>

<details open="open">
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#insert-data">insert data</a></li>
    <li><a href="#update-data">update data</a></li>
    <li><a href="#delete-data">delete data</a></li>
  </ul>
</details>

## insert data
* Single row
    ```sh
    INSERT INTO products(product_id,name,price,quantity,category_id) 
    VALUES('P0001','Popcorn Creamello 150gram',12000,40,1);
    ```
* Multiple row
    ```sh
    INSERT INTO products(product_id,name,price,quantity,category_id) 
    VALUES('P0002','Tricks backed chips',10000,50,1),
          ('P0003','Rondoleti wafer',29000,10,1),
          ('P0004','Kripik pisang sale',10000,80,1);
    ```
<br />

## update data
    ```sh
    UPDATE products 
    SET name = 'Kripik pisang sale manis', price = 12500,quantity = 76 
    WHERE product_id = 'P0004';
    ```
    NOTE:  
    - if you not use where clause sql will change all data. so be careful
    - we can use arithmetic operations when updating data
        ```
        UPDATE products set price = price + 500 WHERE product_id = 'P0004';
        ```
<br />

## delete data
    ```sh
    DELETE FROM products
    WHERE products_id = 'P0001';
    ```
    NOTE:  
    - if you not use where clause sql will delete all data. so be careful
<br />