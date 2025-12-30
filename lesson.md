## **Section 1: The Art of Asking (Selection, Filtering & Sorting)**


### **Learning Objectives**

Learners will be able to retrieve specific columns, apply mathematical operators, filter datasets, and organize their results using sorting.

### **Theory Recap – *The "Conversation" with Data***

**Tip:** "Imagine you're looking for a specific house. First, you choose what details to look at 'SELECT'. Then, you narrow it down to your favorite neighborhood 'WHERE'. But finally, you want to see the cheapest ones first. That is where 'ORDER BY' comes in. It doesn't change your data; it just organizes the 'view' so the most important information sits right at the top."

### **Workshop**

**Task 1: Basic Retrieval & Sorting**

Open DbGate and create a new connection to the DuckDB database file `db/unit-1-4.db`.

> The table we will be using is `main.resale_flat_prices_2017`. It contains HDB's resale flat prices based on registration date from Jan-2017 onwards.
>
> The description for each column (a.k.a data dictionary) is as follows:
>
> | Title               | Column Name         | Data Type                  | Unit of Measure | Description |
> | ------------------- | ------------------- | -------------------------- | --------------- | ----------- |
> | Month               | month               | Datetime (Month) "YYYY-MM" | -               | -           |
> | Town                | town                | Text (General)             | -               | -           |
> | Flat type           | flat_type           | Text (General)             | -               | -           |
> | Block               | block               | Text (General)             | -               | -           |
> | Street name         | street_name         | Text (General)             | -               | -           |
> | Storey range        | storey_range        | Text (General)             | -               | -           |
> | Floor area sqm      | floor_area_sqm      | Numeric (General)          | sqm             | -           |
> | Flat model          | flat_model          | Text (General)             | -               | -           |
> | Lease commence date | lease_commence_date | Datetime (Year) "YYYY"     | -               | -           |
> | Remaining lease     | remaining_lease     | Text (General)             | -               | -           |
> | Resale price        | resale_price        | Numeric (General)          | $               | -           |
>
> Compare them with the data types in DuckDB's table.

**Examples:**

```sql
-- See all columns 
SELECT * FROM resale_flat_prices_2017;
```

```sql
-- Select specific columns and sort by price (Ascending is default) 
SELECT town, flat_type, resale_price 
FROM resale_flat_prices_2017 
ORDER BY resale_price;
```

```sql
-- Sort by price from highest to lowest 
SELECT * FROM resale_flat_prices_2017 ORDER BY resale_price DESC;
```

```sql
-- Sort by town alphabetically, then by price (highest first) 
SELECT town, street_name, resale_price
FROM resale_flat_prices_2017
ORDER BY town ASC, resale_price DESC;
```

**Exercise:** Select any 3 columns from the table.


```
What's Your Query?
```

* **QUESTION** "If our table had 100 columns and a million rows, what would happen to our computer's memory if we always used SELECT *?"

**Task 2: Transformations & Filtering**

>#### Operators
>Mathematical Operators are used to perform mathematical operations on data. The following are some commonly used operators:
>
>| Operator | Description    |
>| -------- | -------------- |
>| `+`      | Addition       |
>| `-`      | Subtraction    |
>| `*`      | Multiplication |
>| `/`      | Division       |
>| `%`      | Modulo         |
>
>#### Filters
>
> Filters are used to filter data based on a condition. The `WHERE` clause is used to filter data in a `SELECT` statement. It commonly uses comparison operators to compare values.
>
>The following are some commonly used comparison operators and logical operators 
>
>| Operator | Description      |
>| -------- | ---------------- |
>| `=`      | Equal            |
>| `<>`     | Not equal        |
>| `>`      | Greater          |
>| `>=`     | Greater or equal |
>| `<`      | Less             |
>| `<=`     | Less or equal    |
>| `AND`    | Logical AND      |
>| `OR`     | Logical OR       |
>| `NOT`    | Logical NOT      |

Examples:

```sql
SELECT * FROM resale_flat_prices_2017 WHERE town = 'BUKIT MERAH';
```

We can introduce line breaks (new lines) to make the query more readable:

```sql
SELECT *
FROM resale_flat_prices_2017
WHERE town = 'BUKIT MERAH';
```

```SQL
-- Calculate price in thousands and rename for clarity  
SELECT street_name, resale_price / 1000 AS price_k  -- Use the `AS` keyword to rename the column
FROM resale_flat_prices_2017  
WHERE town = 'PUNGGOL'   
  AND resale_price > 500000  
ORDER BY resale_price DESC;
```

> There are also non-mathematical operators which we will explore later.
>
> #### Functions
>
> Functions are used to perform operations on data. The following are some example functions:
>
> | Function   | Description                                                              |
> | ---------- | ------------------------------------------------------------------------ |
> | `ABS()`    | Returns the absolute value of a number.                                  |
> | `ROUND()`  | Returns a numeric value rounded to a specified number of decimal places. |
> | `LOWER()`  | Returns a string in lowercase.                                           |
> | `UPPER()`  | Returns a string in uppercase.                                           |
> | `LENGTH()` | Returns the length (number of characters) of a string.                   |
> | `TRIM()`   | Removes leading and trailing spaces from a string.                       |
> | `CONCAT()` | Concatenates two or more strings.                                        |

**Exercise:** "I want to find a home for my parents. They need something larger than 100sqm, but my budget is strictly under $600,000. How would we write that rule?"

```
What's Your Query?
```

### **Q\&A**

<details>
  <summary>Common Hurdle: "Do I need to capitalize SELECT?" </summary>
  
   Answer: SQL is case-insensitive for keywords, but upper-case is industry standard for readability.
</details>


### **Reflection**

* **Business Use Case:** How would a real estate app like PropertyGuru use these filters when a user moves a slider on their screen?

---

**Section 2: Finding the Big Picture (Aggregates & Grouping)**

### **Learning Objectives**

Learners will be able to summarize data using aggregate functions and use the HAVING clause to filter those summaries.

### **Theory Recap – *The "Bucket" Analogy***

**Instructor Script:** "We know WHERE filters individual rows before they are grouped. But what if you want to filter the groups themselves? Imagine you grouped all flats by town and calculated their average prices. Now, you only want to see towns where that average is over $600,000. You can't use WHERE because the average didn't exist until you grouped them. We use HAVING for this. Think of WHERE as the 'Pre-filter' and HAVING as the 'Post-grouping filter."

> ### Aggregate functions
>
>Aggregate functions are used to perform calculations on a set of values and return a single value. The following are some commonly used aggregate functions:
>
>| Function  | Description                            |
>| --------- | -------------------------------------- |
>| `COUNT()` | Returns the number of rows in a table. |
>| `SUM()`   | Returns the sum of values in a column. |
>| `AVG()`   | Returns the average value of a column. |
>| `MIN()`   | Returns the minimum value in a column. |
>| `MAX()`   | Returns the maximum value in a column. |
>| `FIRST()` | Returns the first value in a column.   |
>| `LAST()`  | Returns the last value in a column.    |
>
> ### Group by
>
>The `GROUP BY` clause is used to group rows that have the same values into summary rows. It is often used with aggregate functions to perform calculations on each group. The `GROUP BY` clause comes after the `WHERE` clause and before the `ORDER BY` clause.

### **Workshop**

**Task 3: Basic Aggregates**

```SQL
-- How many transactions happened?  
SELECT COUNT(*) FROM resale_flat_prices_2017;
```

```sql
-- What is the most expensive flat ever sold in this dataset?  
SELECT MAX(resale_price) FROM resale_flat_prices_2017;
```

```sql
-- Average price per town 
SELECT town, AVG(resale_price) AS avg_price
FROM resale_flat_prices_2017
GROUP BY town;
```

**Task 4: Grouping & Having**

```SQL
-- Filter to show only towns with high average prices 
SELECT town, AVG(resale_price) AS avg_price  
FROM resale_flat_prices_2017  
GROUP BY town  
HAVING avg_price > 600000 -- Only show expensive towns  
ORDER BY avg_price DESC;
```

* **Socratic Prompt:** "Look at this query. If I move the price filter to a WHERE clause, would I be filtering the towns, or the individual sales? Let's try both and see how the numbers change."

### **Q\&A**

<details>
  <summary>Common Hurdle: "What happens if I forget the GROUP BY but use an AVG()? </summary>
  
   Answer: Most SQL engines will throw an error because they don't know how to display individual towns next to a single average
</details>


### **Reflection**

* **Business Use Case:** If you are a government planner, how does GROUP BY town help you decide where to build the next MRT station or school?

---

**Section 3: Advanced Logic & Data Cleaning**

### **Learning Objectives**

Learners will be able to categorize data using CASE, convert data types with CAST, and extract specific components from dates.

### **Theory Recap – *The "Translator"***

 "Data isn't always ready for analysis. A 'month' might be a text string like '2017-01' instead of a date object. `CAST` (or the :: shorthand) acts as our translator to fix types. Meanwhile, `CASE` allows us to create new labels—like tagging a flat as 'Large' or 'Small' based on its type—making our data much easier to read for a non-technical boss."

### **Workshop**

**Task 5: Categorizing with CASE**

```SQL
SELECT town, resale_price,  
CASE   
    WHEN resale_price > 1000000 THEN 'Million Dollar Club'  
    WHEN resale_price > 500000 THEN 'Mid-Range'  
    ELSE 'Entry-Level'   
END AS price_category  
FROM resale_flat_prices_2017;
```

```sql
-- Categorize flat sizes 
SELECT town, flat_type,
CASE 
    WHEN flat_type IN ('1 ROOM', '2 ROOM', '3 ROOM') THEN 'Small'
    WHEN flat_type = '4 ROOM' THEN 'Medium'
    ELSE 'Large' 
END AS flat_size
FROM resale_flat_prices_2017;
```

**Task 6: Dates and Casting**

```SQL
-- Convert text to a real date and extract the year  
SELECT   
    month,   
    CONCAT(month, '-01')::DATE AS transaction_date,  
    date_part('year', (month || '-01')::DATE) AS sale_year  
FROM resale_flat_prices_2017;
```

* **Question** "If the 'month' column is text '2017-01', can we add 1 month to it directly? Why do we need to CAST it to a DATE type first?"

### **Q\&A**

<details>
  <summary> Common Hurdle:** "What does the :: mean?" </summary>
  
   Answer: It’s a shorthand for the CAST function in DuckDB and Postgres.
</details>


### **Reflection**

* **Business Use Case:** Why is it important to standardize date formats when merging data from two different countries?
