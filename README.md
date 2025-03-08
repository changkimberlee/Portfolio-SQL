# Funnel and Churn SQL Queries Portfolio
This project illustrates how I used SQL queries to build a dashboard for Funnel and Churn monitoring in a project. I also describe my approach to [SQL Performance Optimization Best Practices](#sql-performance-optimization-best-practices).

At a rapidly scaling startup, I led user growth and engagement efforts for a key product in the one country. Recognizing the severe limitations of our existing data infrastructure, I took the initiative to build a robust analytical foundation. Leveraging SQL, I designed and implemented a sophisticated dashboard that enabled systematic funnel and churn analysis over time as we launched different campaigns. This solution provided crucial insights into user behavior and churn patterns over time, allowing us to track key performance indicators (KPIs), enabling rigorous A/B testing and data-backed decisions. I empowered the team to make data-driven decisions and effectively evaluate various growth strategies to find the most efficient and effective approach. This proactive approach helped optimize user acquisition and retention strategies in an originally data-scarce environment, directly contributing to the product's market penetration.
- [Here](../main/funnelchurn_sql.ipynb) is my SQL code that produced funnel, churn and conversion rate views (illustrated with illustrative fictitious data)

- [Here](../main/funnelchurn_visualization_python.ipynb) is my illustrative dashboard visualization, reproduced here with python

Churn can be defined in various ways. Here I look at the difference in MAU between consecutive months as an indication of **network problems**, the difference in total submissions as an indication of **product discovery** problems, and difference in total approved submissions as a **product design** problem.

- **Define the Funnel**: (Users in Country 1) Join app -> Complete onboarding -> Engage with any tasks on app --> Submit submission on project (task) ID 101 --> Submission approved

- **Define "Active"**: "Monthly active user" can be defined in different ways depending on the business goals. In this case, it's tied to any engagement with the app platform (active on app), and specific engagement behavior with the product (submissions).


## SQL Performance Optimization Best Practices
It is easy to write an SQL query, but it is harder to build an efficient SQL query infranstructure. An inefficient infranstructure affects the work of all the teams and products that rely on this infranstructure! Leading to less data-based decisions. Here's a breakdown of my best practices to make my SQL run as efficiently as possible:

**1. Smart Indexing (Indexing)**

* **Think of it as a card catalog:** We create lists (indexes) of important information (columns) used in `WHERE`, `JOIN`, and `ORDER BY` clauses.
    * This speeds up data retrieval by allowing the database to directly locate data.
* **Composite Indexes:** For queries with multiple `WHERE` conditions, create indexes that include all relevant columns.
    * This allows for more efficient searching, and migitating the computationally intensitive `GROUP BY` function.
* **Only index the important stuff:** Too many lists (indexes) slow down `INSERT`, `UPDATE`, and `DELETE` operations.
    * Index strategically, focusing on frequently queried columns.
* **Keep lists organized:** Regularly analyze and rebuild indexes to prevent fragmentation, which can degrade performance.

**2. Asking Smart Questions (Query Optimization)**

* **Be specific:** Only ask for the books (columns) you really need. Avoid `SELECT *`.
    * Retrieving unnecessary data increases processing time and network traffic.
* **Filter early:** If you only want red books, use `WHERE` clauses to filter data as early as possible.
    * This reduces the amount of data the database has to process.
* **Use the best method:** Sometimes, `EXISTS` is more efficient than `IN` for checking the existence of rows.
    * `UNION ALL` is faster than `UNION` when duplicate removal is not required.
* **Optimize Subqueries:** Rewrite subqueries as joins when possible, as joins are often more efficient.
* **Efficient Joins:** Choose the correct `JOIN` type (e.g., `INNER JOIN`, `LEFT JOIN`) and ensure join columns are indexed.
* **Use `LIMIT`/`TOP`:** Limit result sets when only a subset of rows is needed.
* **Avoid Functions in `WHERE`:** Using functions on indexed columns in `WHERE` clauses can prevent the database from using the index.

**3. Organizing the Library (Database Design)**

* **Keep things tidy:** Avoid having the same book (data) in multiple places (normalization).
    * This reduces redundancy and improves data integrity.
* **Use the right shelf size:** Store information in the smallest possible spaces (appropriate data types).
    * Smaller data types consume less storage and improve performance.
* **Divide big sections:** If one section (table) is huge, break it into smaller parts (partitioning).
    * This improves query performance and manageability.
* **Denormalization (when needed):** In some cases, denormalization can improve read performance.

**4. Keeping the Library Running Smoothly (Server Configuration)**

* **Give it enough space:** Make sure the library (database server) has enough room (memory) for everyone (data caching).
    * Sufficient memory allows the database to cache frequently accessed data.
* **Make it easy to move around:** Use fast ways to get books (data) in and out (optimize disk I/O).
    * Use fast storage devices (SSDs) and configure the database server to optimize disk I/O.
* **Tune it up:** Adjust the library (database server parameters) to work best for how we use it (workload optimization).
    * Adjust parameters like buffer pool size and query cache size.
* **Keep track of what's where:** Regularly update our book records (database statistics).
    * Accurate statistics help the query optimizer make informed decisions.

**5. Checking How Well It Works (Query Analysis)**

* **See how it searches:** Check the steps the library takes to find a book (query execution plan) using `EXPLAIN` or `ANALYZE`.
    * This helps identify performance bottlenecks.
* **Find slow searches:** Identify which questions (queries) take a long time (query profiling).
    * Use profiling tools to identify slow-running queries.
* **Try different things:** Make small changes and see if they help (iterative testing).
    * Test changes and monitor performance.

**In simple terms:** We want to organize our data and ask questions in a way that lets us find the answers quickly and efficiently, using technical methods to optimize the process.
