Task 1
For the current month, calculate total quantity sold per product.
Rank products by quantity sold (descending).
Insert top 5 into TopSellersLog.
Use a transaction and error handling.
Use dynamic SQL for flexibility (e.g., month selection).

You are working for a retail company. You need to create a stored procedure that generates a monthly sales report, including:
Total quantity sold per product
Total revenue per product
Rank products by revenue (within the month)
Store the results in a MonthlyProductReport table
Demonstrates aggregation (SUM, GROUP BY)
Shows use of joins (Products + Sales)
Uses ranking functions (RANK or DENSE_RANK)
Uses parameterized stored procedures


Task 2
 You must implement:
1. Normalized Schema (with dummy data)
Tables involved:
Customers (CustomerID, Name, RegisteredAt)
Orders (OrderID, CustomerID, OrderDate)
OrderItems (OrderItemID, OrderID, ProductID, Quantity, UnitPrice)
Products (ProductID, ProductName, Price, Category)
2. A View (vw_CustomerPurchaseSummary)
A summary per customer showing:
Total Orders
Total Amount Spent
Average Order Value
First and Last Purchase Date
Lifetime Value Tier (low/medium/high) — based on total spent
You can calculate this using CASE or create a scalar-valued function.
3. A Table-Valued Function:
Given a @CustomerID, return all products that customer has purchased more than once.
4. A Stored Procedure (proc_UpdateHighValueCustomers)
Inserts high-value customers (based on monthly spend > $1000) into a HighValueCustomerLog table.
Include date filter, error handling, and performance considerations (e.g., avoid re-inserting already logged data).
5. Query Optimization Tips:
Use indexed views or computed columns where appropriate.
Test EXISTS vs IN, JOIN vs APPLY, and optimize execution plan.
🧠 Skills it Demonstrates:
Complex JOIN and GROUP BY logic
Analytical thinking via derived metrics
Window functions (e.g., ROW_NUMBER, RANK, MIN/MAX OVER)
Real-world use of views, scalar + table-valued functions
Conditional logic with CASE
Transaction-safe stored procedures
Optional: Add index hints, OPTION (RECOMPILE), or dynamic filtering for advanced tuning


Task 3
Tasks:
Write a query that returns:
Month
Category name
Total Sales
Total Items Sold
Top-selling Product per Category (by revenue)
Average Order Value

Optimize your query using:
Proper joins and subqueries/CTEs
Indexes (suggest which columns should be indexed)

Use window functions instead of correlated subqueries when possible.
RANK() OVER (PARTITION BY <group_column> ORDER BY <sort_column> DESC)
group by აჯგუფებს ერთნაირ ჩანაწერებს, ფართიშენ ბაი ითვლის ან პოულობს მონაცემს ჯგუფში


Measure performance:
Use EXPLAIN PLAN or SET STATISTICS TIME ON / DBMS_XPLAN.DISPLAY to measure cost.
Identify bottlenecks and improve the query (avoid full table scans, reduce row reads, etc.).
Implement partitioning if the orders table has 100M+ rows.

✅ Task 2: Handle Historical Tracking with SCD Type 2
Scenario:
Your company needs to track historical changes in employee department assignments and salary bands.
Tables:
employees(emp_id, name, join_date)
employee_history(emp_id, department_id, salary_band, start_date, end_date, is_current)

Tasks:
Write a stored procedure to insert new records into employee_history:
If an employee's department or salary changes, expire the old record and insert a new one.
Maintain is_current flag and date range.
Ensure atomicity using transactions.
Tune the procedure:
Add indexes on (emp_id, is_current) and (emp_id, start_date, end_date)
Avoid using NOT EXISTS when possible. Prefer MERGE if supported.
Use MERGE/UPSERT logic to optimize changes.

✅ Task 3: Monitor and Resolve Query Slowness
Scenario:
You are assigned a task to investigate a slow-running customer churn report.
Current Query:
sql
Copy
Edit
SELECT c.customer_id, c.name, COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_date BETWEEN '2023-01-01' AND '2023-12-31'
GROUP BY c.customer_id, c.name
HAVING COUNT(o.order_id) < 3;
Tasks:
Find performance issues:
Is the filter applied correctly?
Is the join causing unnecessary work?
Are there missing indexes?
Rewrite the query:
Move filters to subqueries if needed
Use derived tables or CTEs
Add or suggest indexes:
On orders(order_date, customer_id)
On customers(customer_id)
Compare performance:
Before and after optimization
Collect query plan cost and duration
Write a job to cache the result into a materialized view or temp table if the query is used frequently.

🚀 Bonus Actions to Include:
Use table partitioning for large tables (by year/month in orders).
Use statistics update (UPDATE STATISTICS, ANALYZE TABLE) when tuning.
Use hints where appropriate (/*+ INDEX(...) */, OPTION(FORCE ORDER)).
Create covering indexes where it helps reduce key lookups.
Evaluate using indexed views if aggregation is common.


