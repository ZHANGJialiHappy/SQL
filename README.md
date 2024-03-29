# SQL
## 8/2/2024
### PostgreSQL-specific code
```
extract(year from C.held)
```
```
to_char(S.recordmax(R.result), '0D99')
```
in postgres, we have join(inner join), full join, left join, right join
### go over
```
concat('a','b') : concatenate 2 charactors
```
```
cast(number as varchar(2)) : convert 2 digits number to charactors
```
```
where a is null      where a is not null : assert if a is null
```
## 2/5 
### QUERY（SELECT）
```
SELECT …,...,   
FROM…
```
```
SELECT * FROM
```
```
SELECT DISTCT … 
FROM …
```
```
SELECT TOP … 
FROM ....    /*(SQL server, Access)*/
```
```
SELECT … 
FROM… 
FETCH FIRST  .. ROWS ONLY /*(DB2)*/
```
```
SELECT … 
FROM… 
WHERE ROWNUM <= ...   /*(Oracle)*/
```
```
SELECT … 
FROM … 
LIMIT …      /*(MySQL, MariaDB, postgreSQL, SQLite)*/
```
```
SELECT … 
FROM … 
LIMIT 5 OFFSET 3 ; 
SELECT … 
FROM … 
LIMIT 3,5
```
    
/*  ….   */   使用注释

## 3/5 
### Order
ORDER BY Clause 
```
SELECT … FROM … 
ORDER BY …
```
```
SELECT a, b, c  
FROM … 
ORDER BY b,c
```
```
SELECT a, b, c 
FROM … 
ORDER BY 2 /*2nd column*/, 3 /*3rd column*/
```
```
SELECT a, b, c  
FROM … 
ORDER BY b DESC, c
```

### Filter
```
SELECT … FROM …
WHERE … = …
```

### WHERE Clause operator

|               |               |       |
| -------------:|--------------:| -----:|
| <>            |  !=           | <     |
| >             | <=            | >=    |
| IS NULL       | BETWEEN...AND | !> !< |

```
SELECT … FROM …
WHERE … IN (...,...);
```
```
SELECT … FROM …
WHERE …=... OR …=...
```
```
SELECT … FROM …
WHERE …=... XOR …=...
 /* (A AND Not B) or (Not A AND B) */
 ```
```
SELECT … FROM …
WHERE …=... AND …=...
```
```
SELECT .... FROM …
WHERE NOT … = …
```

## 4/5
### 通配符 (Wildcard)
搜索模式（search pattern）
谓语 （predicate）
```
SELECT … FROM …
WHERE … LIKE ‘_ ab’      /* Microsoft Access 用？*/
```
```
SELECT … FROM …
WHERE … LIKE ‘a%’   /* LIKE ‘%a%’   LIKE ‘a%b’ */    /* Microsoft Access  use *  not % */
```
```
SELECT … FROM
WHERE … LIKE ‘[ab]%’  /*Only use in Access and SQL Server*/
              /*[a-f]*/
```
```
SELECT … FROM
WHERE … LIKE ‘[^ab]%’ /* Access use ！to object */;
SELECT … FROM
WHERE NOT … LIKE ‘[ab]%’ 
```

### Concatenate
Concat(...,...)
```
SELECT … +... +...  FROM ...  /* SQL Server*/;
SELECT … ||... ||...  FROM ... /* Access*/;
SELECT Concat(...,...)  FROM ...  /* MariaDB, My SQL*/;
```
AS 别名 AS “Maria DB”    AS[sql Server] 

arithmetic operators :     + - * /

REPLACE(‘ABC’, ‘A’, ‘D’, )

### Text Function 文本处理函数
|                                                                 |
| ---------------------------------------------------------------:|
|RTRIM(), LTRIM(), TRIM(LEADING | TRAILING | BOTH 'x' FROM string)|
|NOW()                                                            |
|LENGTH(),     DATALENGTH(), LEN()                                |
|LOWER(),      LCASE(Access)                                      |
|SOUNDEX()                                                        |
|LEFT(),       RIGHT()                                            |
|UPPER(),      UCASE(Access)                                      |
|DATEPART(yy, SQL Server ), DATEPART(‘yyyy’, Access)              |
|SOUNDEX()                                                        |
|DATE_PART(‘year’, PostgreSQL), to_number(to_char(Oracle, 'YYYY'))|
|LIMIT 5 OFFSET 5                                                 |
|LEFT(name, 1),    RIGHT(name, 1)                                 |
|LOCATE("3", "W3Schools.com")                                     |

### Numeric Functions
ABS()       
COS()     
SIN()       
TAN()       
EXP()       
PI()      
SQRT() 
ROUND(123, -2)  
Floor()
Ceiling()
CAST(... AS int)

MOD()    /* it is in Oracle*/  a%number /* sql server/

### Aggregate Function
AVG(DISTINCT ...)     
COUNT()     
MAX()      
MIN()     
SUM()       

## 7/5
```
SELECT … FROM ...
WHERE
GROUP BY 
HAVING
ORDER BY
```

## 8/5
### SUBQUERY
```
SELECT cust_id
FROM Orders
WHERE order_num IN(SELECT order_num
                    FROM OrderItems
                    WHERE prod_id = 'RGAN01'
                    );
```
```
SELECT cust_name, cust_state, (
                               SELECT COUNT(*)
                               FROM Orders
                               WHERE Orders.cust_id = Customers.cust_id
                               ) AS orders
FROM Customers
ORDER BY cust_name;
```
```
SELECT name, CONCAT
                  (CAST
                       (ROUND(100*population/(SELECT population FROM world 
                                              WHERE name='Germany')
                          ,0 )
                   AS int)
              , '%')
FROM world
WHERE continent='Europe';
```

ANY, ALL
SELECT … FROM … WHERE …> ANY(Subquery) 
SELECT … FROM … WHERE …> ALL(Subquery) 

### Operation order:    FROM 》WHERE》GROUP BY》HAVING》SELECT》ORDER BY

### Join
WHERE Table.column = table2.column2

### Self Join
```
SELECT Table1.id1, table 2.name
FROM table AS table1, table AS table2
WHERE table1.id1=table2.id2 AND ...=....;
```

/*Correlated subquery*/
```
WHERE population>ALL(SELECT 3*population 
                      FROM world y
                      WHERE x.continent=y.continent AND x.name!=y.name
                     )
 ```

### Natural Join

### Outer Join
```
/*Inner Join*/
SELECT Table1.id1, table 2.name
FROM table1 INNER JOIN table2
ON  table1.id1=table2.id2;
```

LEFT JOIN,
RIGHT JOIN

### Create several Join
```
SELECT actor.name
FROM movie, actor, casting
WHERE movie.id=casting.movieid 
             AND actor.id=casting.actorid 
             AND movie.title= 'Alien';
             
SELECT actor.name
FROM actor
JOIN casting
ON casting.actorid = actor.id
JOIN movie
ON movie.id = casting.movieid
WHERE movie.title = 'Alien';
```

## 10/5
### Union/Compound Query
```
SELECT … FROM …
WHERE ….=....
UNION
SELECT … FROM…
WHERE ….=...
```

/* display repeated result of 2 SELECT*/
```
SELECT … FROM …
WHERE ….=....
UNION ALL
SELECT … FROM…
WHERE ….=...
```
### CASE WHEN
```
SELECT mdate, team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS score1, 
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS score2
FROM game LEFT JOIN goal 
  ON matchid = id
GROUP BY mdate, team1, team2
```
## 11/5
### Insert row
```
INSERT INTO Customers(cust_id,
                cust_name,
                cust_address)
            
VALUES('001',
       'May',
       'Vej')
```
### Insert SELECT
```

INSERT INTO Customers(cust_id,
                      cust_contact,
                      cust_email,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
  
 SELECT cust_id,
       cust_contact,
       cust_email,
       cust_name,
       cust_address,
       cust_city,
       cust_state,
       cust_zip,
       cust_country
FROM CustNew;
```
### Copy a table to another
```
SELECT *, column
INTO CustCopy
FROM Customers;
```
### Update
Update "Customers" table. A customer's Email is changed to 'kim@thetoystore.com' , and his cust_id is '1000000005'.
```
UPDATE Customers
SET cust_email = 'kim@thetoystore.com' 
WHERE cust_id = '1000000005';
```
### Delete a row
Delete a customer whose ID is '1000000006'
```
DELETE FROM Customers
WHERE cust_id = '1000000006';
```
### Create Table
```
CREATE TABLE Products
(
    prod_id    CHAR(10)   NOT NULL,
    order_num  INTEGER    NOT NULL,
    cust_add   CHAR(50)   ,
 )
 ```
 ### Add a column
 ```
 ALTER TABLE Vendors
ADD vend_phone CHAR(20);

ALTER TABLE Vendors
DROP COLUMN vend_phone;
```
### Delete a table
```
DROP TABLE CustCopy;
```
### Create a VIEW table
```
CREATE VIEW ProductCustomers AS
SELECT cust_name, cust_contact, prod_id FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num;

SELECT cust_name, cust_contact FROM ProductCustomers
WHERE prod_id = 'RGAN01';
```
### Window function (T-SQL,e.g. catch top 10 in every continent)
```
SELECT * 
FROM (SELECT continent,population, 
                                 RANK() OVER(
                                             PARTITION BY continent ORDER BY population DESC
                                             ) ranking
      FROM world) T
      WHERE ranking<=10
 ```
 ### Rolling Calculations (e.g. YTD totals)
 ```
 SELECT
       SUM(MonthlySales) OVER (ORDER BY SalesMonth ROWS UNBOUNDED PRECEDING) AS YTDSales
      ,MonthlySales AS 'M_S'
      , SalesMonth
 FROM
 (SELECT SUM(s.SalesAmount) AS 'MonthlySales',
        MONTH(s.OrderDate) AS 'SalesMonthly
 FROM FactInternetSales AS s
 WHERE YEAR(s.OrderDate)=2017
 GROUP BY MONTH(s.OrderDate)
 ) AS s
 GROUP BY SalesMonthly,M_S
 ORDER BY SalesMonth ASC
;
 
 ```
 ### ISTNULL
 ```
 SELECT ISNULL(NULL, 'W3Schools.com')
 ```
 ### DATE (T-SQL)
 EOMONTH();
 DATEDIFF(interval, date1, date2);
 GETDATE()
 DATE_FORMAT(last_update, '%D', '%M', %'Y')
 
 ### Common table expressions 
 ```
 WITH A (a1, b2, c3)
 AS (
     SELECT a AS a1, b AS b2, SUM(c) AS c3
     FROM table
     GROUP BY a, b)
 SELECT * , CASE WHEN c3>200 THEN 1 ELSE 0 END AS W
 FROM A
 ```
 ### Ranks
|                                 |                                    |
| -------------------------------:|-----------------------------------:|
|RANK()                           |PERCENT_RANK()                      |
|DENSE_RNAK()                     |                                    |
|ROW_NUMBER()                     |                                    |

 ### GROUP_CONCAT()
 SELECT GROUP_CONCAT(phone ORDER BY phone ASC SEPARATOR ';')
 
 ### Variables
 ```
 /* MySQL */
 SET @varialble = 'PENELOPE';
 ```
 ```
 /* SQL Server */
 DECLARE @variable varchar(30) = 'PENELOPE';
 ```
 ```
 /* Oracle */
 DECLARE variable NUMBER;
 BEGIN
     SELECT xx FROM Table WHERE Field_name>variable;
 END;
 ```
 ### Use case to filter
 ```
SELECT *
FROM table
WHERE 
    CASE WHEN a > 5 THEN 'Keep'
         WHEN a <= 5 THEN 'Exclude' END = 'Keep';
```
 
# Useful information
https://www.w3schools.com/sql/sql_ref_sqlserver.asp
# Check version first!
|                                 |                                    |
| -------------------------------:|-----------------------------------:|
|SELECT @@version                 |Microsoft.                          |
|SELECT * FROM v$version          |Oracle                              |
|SELECT version()                 |MySQL, PostgreSQL                   |


















