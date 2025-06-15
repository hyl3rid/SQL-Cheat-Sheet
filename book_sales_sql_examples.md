# Book‑Sales SQL Cheat‑Sheet
---
### Basic SELECT
```sql
SELECT (pick_a_column) FROM BookSalesData;
```
### Inspect entire table
```sql
SELECT * FROM BookSalesData;
```
```
| SaleID | SalesPerson   | SaleDate   | BookGenre            | BookTitle            | SalesAmount |
|-------:|---------------|------------|----------------------|----------------------|-------------|
| 1      | John Smith    | 2023‑01‑10 | Epic Poetry          | Paradise Lost        | 500.00      |
| 2      | Alice Webster | 2023‑01‑15 | Epic Poetry          | The Divine Comedy    | 300.50      |
| 3      | John Smith    | 2023‑02‑20 | Epic Poetry          | The Iliad            |  50.00      |
| 4      | Alice Webster | 2023‑02‑25 | Classical Literature | The Odyssey          | 450.75      |
| 5      | John Smith    | 2023‑03‑10 | Epic Poetry          | The Aeneid           | 100.00      |
| 6      | Bob William   | 2023‑03‑10 | Religious Allegory   | Pilgrim's Progress   | 200.00      |
| 7      | Alice Webster | 2023‑03‑15 | Classical Literature | Faust                | 800.75      |
| 8      | John Smith    | 2023‑04‑05 | Epic Poetry          | Paradise Lost        | 550.25      |
| 9      | Bob William   | 2023‑04‑05 | Religious Allegory   | Pilgrim's Progress   | 150.00      |
| 10     | David Mitchel | 2023‑07‑05 | Classical Literature | Faust                | 325.00      |
| 11     | Charlie Wilson| 2023‑07‑10 | Epic Poetry          | The Aeneid           |  45.00      |
| 12     | David Mitchel | 2023‑07‑15 | Religious Allegory   | Paradise Lost        | 125.00      |
| 13     | Charlie Wilson| 2023‑07‑20 | Epic Poetry          | The Odyssey          |  25.50      |
| 14     | Charlie Wilson| 2023‑08‑10 | Classical Literature | Faust                | 700.50      |
| 15     | Bob William   | 2023‑09‑05 | Classical Literature | The Odyssey          | 350.00      |
```

---

### AND / OR / NOT Filter Example
```sql
SELECT BookGenre,
       SalesPerson,
       SaleDate,
       BookTitle,
       SalesAmount
FROM BookSalesData
WHERE BookGenre = 'Classical Literature'
OR BookGenre = 'Religious Allegory';
```

---

### Sorting & Distinct
```sql
SELECT DISTINCT SalesPerson,
       SalesAmount
FROM BookSalesData
ORDER BY SalesAmount DESC; -- or ASC
```

---

### Limit Output
```sql
SELECT * FROM SalesData
LIMIT 5;
```

---

### Aggregation with GROUP BY
Aggregation functions: COUNT(), SUM(), AVG(), MIN(), and MAX()
```sql
SELECT SalesPerson,
       BookGenre,
       BookTitle,
       SUM(SalesAmount) AS TotalSold
FROM BookSalesData
GROUP BY SalesPerson, BookGenre, BookTitle;
```

---

### Table Definition (Books)
Data Types
INTEGER or INT: whole numbers.
DECIMAL or DEC: DECIMAL(p, s), p = number of digits and s = number of decimal digits.
CHAR: CHAR(size), size = number of characters
DATE: The DATE represents dates.
PRIMARY KEY is main column.
```sql
CREATE TABLE Books (
    bookID         INT,
    title          CHAR(100),
    genre          CHAR(50),
    price          DECIMAL(8,2),
    stockQuantity  INT,
    PRIMARY KEY (bookID)
);
```

---

### Get table definitions
Returns the schema details (column names, types, nullability) for the BookSalesData table.
```sql
DESCRIBE BookSalesData;
```

---

### Insert New Book Sale
```sql
INSERT INTO BookSalesData
    (SaleID, SalesPerson, SaleDate, BookGenre, BookTitle, SalesAmount)
VALUES
    (16, 'Mary Jones', '2023‑10‑15', 'Epic Poetry', 'Paradise Regained', 800.00);
```

---

### Update an Existing Entry
```sql
UPDATE BookSalesData
SET SalesPerson = 'Mary Jones', SalesAmount = 35.00
WHERE SaleID = 11; -- Always include WHERE, if not all values will be updated
```

---

### Delete an Entry (Be Specific!)
```sql
DELETE FROM BookSalesData
WHERE SaleID = 4; -- Always include WHERE, if not table can be deleted
```

---

### Update Existing Table
Change column data type
```sql
-- ⚠️ Use with caution: changing data types can corrupt existing data
ALTER TABLE BookSalesData
MODIFY BookGenre INT;
```

Rename table
```sql
ALTER TABLE BookSalesData
RENAME TO Books;
```

Rename Column
```sql
ALTER TABLE BookSalesData
RENAME COLUMN BookGenre TO Genres;
```

Add new column
```sql
ALTER TABLE BookSalesData
ADD (Quantity INT NOT NULL, Price DEC(8,2) NOT NULL);
```

Delete column
```sql
-- ⚠️ This will permanently remove the column and its data
ALTER TABLE BookSalesData
DROP COLUMN BookGenre;
```

---

### Aliases
```sql
SELECT SaleID, SalesAmount AS SAmount, Quantity * Price AS Amount
FROM BookSalesData;
```

---

### Table Alias
```sql
SELECT t.SaleID
FROM BookSalesData as t;
```

---

### Compare values
```sql
SELECT SaleID AS ID,
Quantity * Price AS Amount, SalesAmount
FROM BookSalesData
WHERE SalesAmount <> Quantity * Price;
```

---

### Foreign keys
Specify foreign keys when creating a new table.
```sql
CREATE TABLE BookSalesData (
SaleID INT,
SalesPerson CHAR(32),
SaleDate DATE,
BookID INT,
...
PRIMARY KEY (SaleID),
FOREIGN KEY (BookID) REFERENCES Book(BookID)
);
```
Add a foreign key to existing table.
```sql
ALTER TABLE BookSalesData
ADD FOREIGN KEY (BookID) REFERENCES Book(BookID);
```

---

### Extract data from multiple tables
```sql
SELECT * 
FROM BookSalesData, Book
WHERE BookSalesData.SaleID = Book.SaleID;
```

---

### JOIN clause
```sql
SELECT * 
FROM BookSalesData
JOIN Book
ON BookSalesData.SaleID = Book.SaleID;
```

---

### Natural JOIN
```sql
SELECT * 
FROM BookSalesData AS sd
NATURAL JOIN Book AS b;
```

---

### Retrieving information from more than two tables
```sql
SELECT *
FROM Sale
JOIN Employee ON Sale.EmployeeID = Employee.EmployeeID
JOIN Book ON Sale.SaleID = Book.SaleID;
```

---

### OUTER JOIN
LEFT JOIN: includes all elements on the left side
RIGHT JOIN: includes all elements in the right side
FULL JOIN: includes matching elements and not matching elements
```sql
SELECT column1, column2, ...
FROM table1
LEFT|RIGHT JOIN table2
ON table1.common_column = table2.common_column;
```
```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;
```

---

### Subqueries
```sql
-- Step 2: Outerquery to filter Product based on Sale
SELECT Product.ProductID, Product.ProductName
FROM Product
WHERE ProductID NOT IN (
    -- Subquery: Fetch ProductID from Sale
    SELECT ProductID		
    FROM Sale
);
```

---

### Dates
Date data types:
- DATE
- DATETIME
- TIMESTAMP: tracks changes
- YEAR

Extract date details. Functions go after the SELECT statement:
YEAR(column)
MONTH(column)
WEEK(column)
DAY(column)
MINUTE(column)
SECOND(column)
NOW()
CURDATE()
CURTIME()
DATE()
EXTRACT()

Example:
```sql
SELECT student_id, full_name, enrollment_time, 
       EXTRACT(HOUR FROM enrollment_time) AS hour_enrolled, 
       EXTRACT(MINUTE FROM enrollment_time) AS minute_enrolled, 
       EXTRACT(SECOND FROM enrollment_time) AS second_enrolled, 
       EXTRACT(WEEK FROM enrollment_time) AS enrollment_week, 
       EXTRACT(YEAR FROM enrollment_time) AS enrollment_year
FROM students;
```
