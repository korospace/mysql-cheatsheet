
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
*   This statement is used to change data with specific row at table
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
*   This statement is used to delete data with specific row at table.
    ```sh
    DELETE FROM products
    WHERE products_id = 'P0001';
    ```
    NOTE:  
    - if you not use where clause sql will delete all data. so be careful
<br />

## select data
* All column. 
    ```sh
    SELECT * FROM products;
    ```
    
* specific column
    ```sh
    SELECT product_id, name FROM products;
    ```

* alias
    ```sh
    SELECT p.product_id AS 'ID Produk', 
           p.name       AS 'Produk name' 
    FROM products AS p;
    ```

* order by
    ```sh
    SELECT * FROM products 
    ORDER BY price DESC;
    ```
    NOTE:  
    - ASC is used to sort from smallest to largest
    - DESC is used to sort from largest to smallest

* limit
    ```sh
    SELECT * FROM products LIMIT {offset},{limit data};
    ```
    ```sh
    SELECT * FROM products LIMIT 2,1;
    ```
    NOTE: _sql above syntax will display product data starting from row with index 2 and display only one data_

* distinct <br/>
    _This statement is used to remove redundant data at table_
    ```sh
    SELECT DISTINCT price FROM products;
    ```
    | price(without distinct) | price(with distinct)|
    | :---: | :---: |
    | 10000 | 10000 |
    | 10000 | 12000 |
    | 10000 | 29000 |
    | 10000 |
    | 10000 |
    | 10000 |
    | 12000 |
    | 29000 |

* div <br/>
    *The DIV function is used for integer division.*
    ```sh
    SELECT price          AS 'without div', 
           price div 1000 AS 'with div' 
    FROM products;
    ```
    | without div | with div |
    |:---:        |:---:     |
    |       10000 |       10 |
    |       10000 |       10 |
    |       10000 |       10 |
    |       10000 |       10 |
    |       10000 |       10 |
    |       10000 |       10 |
    |       12000 |       12 |
    |       29000 |       29 |

    NOTE: _https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html_

* string function <br/>
    *The STRING function is used to manipulate character string effectively.*
    ```sh
    SELECT name, LOWER(name),UPPER(name),LENGTH(name) FROM products;
    ```
    | name | LOWER(name) | UPPER(name) | LENGTH(name) |
    | :---: | :---: | :---: | :---: |
    | Popcorn Creamello 150gram | popcorn creamello 150gram | POPCORN CREAMELLO 150GRAM |           25 |
    | Tricks backed chips       | tricks backed chips       | TRICKS BACKED CHIPS       |           19 |
    | Rondoleti wafer           | rondoleti wafer           | RONDOLETI WAFER           |           15 |
    | Kripik pisang sale        | kripik pisang sale        | KRIPIK PISANG SALE        |           18 |
    | Mie ayam pangsit          | mie ayam pangsit          | MIE AYAM PANGSIT          |           16 |
    | Mie ayam bakso            | mie ayam bakso            | MIE AYAM BAKSO            |           14 |
    | Mie ayam pangsit+bakso    | mie ayam pangsit+bakso    | MIE AYAM PANGSIT+BAKSO    |           22 |
    | Seblak original           | seblak original           | SEBLAK ORIGINAL           |           15 |

    NOTE: _https://dev.mysql.com/doc/refman/8.0/en/string-functions.html_

* timestamp function <br/>
    *The TIMESTAMP function is used to manipulate temporal values.*
    ```sh
    SELECT created_at,TIME(created_at),DAY(created_at),MONTH(created_at),YEAR(created_at) FROM products;
    ```
    | created_at | TIME(created_at) | DAY(created_at) | MONTH(created_at) | YEAR(created_at) |
    | :---: | :---: | :---: | :---: | :---: |
    | 2021-07-13 05:43:22 | 05:43:22         |              13 |                 7 |             2021 |
    | 2021-07-13 05:43:26 | 05:43:26         |              13 |                 7 |             2021 |
    | 2021-07-13 05:43:26 | 05:43:26         |              13 |                 7 |             2021 |
    | 2021-07-13 05:43:26 | 05:43:26         |              13 |                 7 |             2021 |
    | 2021-07-13 06:19:02 | 06:19:02         |              13 |                 7 |             2021 |
    | 2021-07-13 06:19:02 | 06:19:02         |              13 |                 7 |             2021 |
    | 2021-07-13 06:19:02 | 06:19:02         |              13 |                 7 |             2021 |
    | 2021-07-13 06:19:02 | 06:19:02         |              13 |                 7 |             2021 |

    NOTE: _https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html_
<br />
