
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
    <li><a href="#select-data">select data</a></li>
    <li><a href="#where-clause">where clause</a></li>
    <li><a href="#controll-flow">controll flow</a></li>
    <li><a href="#agregat">agregat</a></li>
    <li><a href="#group-by">group by</a></li>
    <li><a href="#having-clause">having clause</a></li>
    <li><a href="#sub-queries">sub queries</a></li>
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
    <br />

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
    
    <br />

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
    
    <br />

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

## agregat
* count
    ```sh
    SELECT COUNT(product_id) AS 'total product' FROM products;
    ```
    | total product |
    | :---: |
    |            10 |
    
* sum
    ```sh
    SELECT SUM(quantity) AS 'summary of stock' FROM products;
    ```
    | summary of stock |
    | :---: |
    |              271 |
    
* avg
    ```sh
    SELECT AVG(price) AS 'average price' FROM products;
    ```
    | total stock |
    | :---: |
    |         271 |
    
* max value
    ```sh
    SELECT MAX(price) AS 'highest price' FROM products;
    ```
    | highest price |
    | :---: |
    |         29000 |
    
* min value
    ```sh
    SELECT MIN(price) AS 'lowest price' FROM products;
    ```
    | lowest price |
    | :---: |
    |        29000 |

<br />

## where clause
* = , !=
    ```sh
    SELECT * FROM products WHERE quantity != 10;
    ```
* < , > , <= , >=
    ```sh
    SELECT * FROM products WHERE price > 10000;
    ```
* AND , OR
    ```sh
    SELECT * FROM products WHERE price < 20000 AND quantity > 10;
    ```
* ( ) operator
    ```sh
    SELECT * FROM products WHERE (price > 10000 AND quantity > 10) OR price < 29000;
    ```
* IS NULL operator
    ```sh
    SELECT * FROM products WHERE column_name IS NULL;
    ```
* BETWEEN , NOT BETWEEN
    ```sh
    SELECT * FROM products WHERE quantity NOT BETWEEN 10 AND 40;
    ```
* IN , NOT IN
    ```sh
    SELECT * FROM products WHERE price NOT IN(10000,12000);
    ```
<br />

## controll flow
* IFNULL 
    ```sh
    SELECT category_id AS 'without IFNULL', 
           IFNULL(category_id,'not found') AS 'with IFNULL'
    FROM products;
    ```
    | without IFNULL | with IFNULL |
    | :---: | :---: |
    |              1 | 1           |
    |              1 | 1           |
    |              1 | 1           |
    |              1 | 1           |
    |              2 | 2           |
    |              2 | 2           |
    |              2 | 2           |
    |              3 | 3           |
    |           NULL | not found   |
    |           NULL | not found   |
    
* IF 
    ```sh
    SELECT quantity AS 'stock',
           IF( quantity <= 10,'low stock', 
                IF( quantity <= 40 ,'ready stock', 
                    IF( quantity > 40, 'over stock','over over stock'))) 
                        AS 'stock level' 
    FROM products;
    ```
    | stock | stock level |
    | :---: | :---: |
    |    40 | ready stock |
    |    50 | over stock  |
    |    10 | low stock   |
    |    80 | over stock  |
    |    10 | low stock   |
    |    10 | low stock   |
    |    10 | low stock   |
    |    10 | low stock   |
    |    23 | ready stock |
    |    28 | ready stock |
    
* case
    ```sh
    SELECT quantity AS 'stock', 
        CASE quantity 
            WHEN 10 THEN 'low stock' 
            WHEN 23 THEN 'ready stock' 
            WHEN 28 THEN 'ready stock' 
            ELSE 'over stock' 
        END AS 'stock level' 
    FROM products;
    ```
    | stock | stock level |
    | :---: | :---: |
    |    40 | over stock  |
    |    50 | over stock  |
    |    10 | low stock   |
    |    80 | over stock  |
    |    10 | low stock   |
    |    10 | low stock   |
    |    10 | low stock   |
    |    10 | low stock   |
    |    23 | ready stock |
    |    28 | ready stock |

<br />

## group by
*   ```sh
    SELECT price AS 'price group' , 
           COUNT(product_id) AS 'total product' 
    FROM products 
    GROUP BY price;
    ```
    | price group | total product |
    | :---: | :---: |
    |       10000 |             6 |
    |       11000 |             1 |
    |       12000 |             1 |
    |       22000 |             1 |
    |       29000 |             1 |
<br />

## having clause
*   ```sh
    SELECT price AS 'price group' , 
           COUNT(product_id) AS total_product 
    FROM products 
    GROUP BY price
    HAVING total_product > 1;
    ```
    | price group | total product |
    | :---: | :---: |
    |       10000 |             6 |
<br />

## sub queries
    ```sh
    SELECT name,price,category_id 
    FROM products 
    WHERE price = (
        SELECT MAX(price) 
        FROM products
    );
    ```
<br />
    | name                | price | category_id |
    | :---: | :---: | :---: |
    | Ayam bakar Bu Mirna | 30000 |        NULL |

    ```sh
    SELECT name,price,category_id 
    FROM products 
    WHERE price = (
        SELECT MAX(price) 
        FROM products 
        JOIN categories 
        ON (products.category_id = categories.category_id)
    );
    ```
<br />
    | name            | price | category_id |
    | :---: | :---: | :---: |
    | Rondoleti wafer | 29000 |           1 |

    ```sh
    SELECT MAX(price) 
    FROM (
        SELECT price 
        FROM products 
        JOIN categories 
        ON (products.category_id = categories.category_id)
    ) AS product_categories;
    ```
<br />
    | MAX(price) |
    | :---: |
    |      29000 |
<br />