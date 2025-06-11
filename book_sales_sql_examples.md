# Book‑Sales SQL Cheat‑Sheet
---

```sql
-- Basic SELECT
SELECT (pick_a_column) FROM   BookSalesData;

-- Inspect entire table
SELECT * FROM   BookSalesData;
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
| 12     | David Mitchel | 2023‑07‑15 | Religious Allegory   | Paradise Regained    | 125.00      |
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
FROM   BookSalesData
WHERE  BookGenre = 'Classical Literature'
   OR  BookGenre = 'Religious Allegory';
```

### Sorting & Distinct
```sql
SELECT DISTINCT SalesPerson,
       SalesAmount
FROM   BookSalesData
ORDER BY SalesAmount DESC;  -- or ASC
```

### Aggregation with GROUP BY
```sql
SELECT SalesPerson,
       BookGenre,
       BookTitle,
       SUM(SalesAmount) AS TotalSold
FROM   BookSalesData
GROUP  BY SalesPerson, BookGenre, BookTitle;
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

### Insert New Book Sale
```sql
INSERT INTO BookSalesData
    (SaleID, SalesPerson, SaleDate, BookGenre, BookTitle, SalesAmount)
VALUES
    (16, 'Mary Jones', '2023‑10‑15', 'Epic Poetry', 'Paradise Regained', 800.00);
```

### Update an Existing Entry
```sql
UPDATE BookSalesData
SET    SalesPerson = 'Mary Jones', SalesAmount = 35.00
WHERE  SaleID = 11;   -- Always include WHERE
```

### Delete an Entry (Be Specific!)
```sql
DELETE FROM BookSalesData
WHERE  SaleID = 4;    -- Never drop the whole table by accident
```

---

> **DESCRIBE BookSalesData;**
>
> Returns the schema details (column names, types, nullability) for the BookSalesData table.

---



