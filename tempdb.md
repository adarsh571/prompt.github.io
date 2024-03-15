## Introduction : 'tempdb' Database

- Tempdb is a system database in SQL Server that serves as a global resource available to all users connected to an instance of SQL Server, Azure SQL Database, or Azure SQL Managed Instance.
- Think of it as a dumping ground for anything that doesn’t fit in memory. It’s like the utility drawer in your kitchen—it holds temporary stuff that various processes need.
- Tempdb is non-durable, meaning that it gets recreated every time SQL Server restarts. Nothing is saved from one session to another.

### What Does Tempdb Hold?
- Temporary User Objects: These are explicitly created by users and include:
- Global or local temporary tables and indexes.
- Temporary stored procedures.
- Table variables.
- Tables returned in table-valued functions.
- Cursors.

The temdb can also be used for internal operations like rebuilding indexes (when the SORT_IN_TEMPDB is ON), and queries using UNION, DBCC checks, GROUP BY, and ORDER BY. Hash join and Hash aggregate operations.

### Let’s explore the differences between Tempdb and other databases in SQL Server
|              | **Tempdb**                                                                                      | **Databases**                                                |
|-------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
|**Purpose**                    | Primarily used for temporary storage during query execution                                     | Serve as persistent storage for data                         |
|                    | Holds temporary objects like global or local temporary tables, table variables, and work tables | Store user-defined tables, views, indexes, and other objects |
|                     | Acts as a scratchpad for sorting, spooling, and other intermediate operations.                  |                                                              |
| **Lifetime**            | Recreated every time SQL Server restarts.                                                       | Data persists even after server restarts.                    |
|                     | No data persists across sessions.                                                               |                                                              |
| **Durability**          | Non-durable: No data is saved between sessions.                                                 | Durable: Data is persisted                                   |
| **Schema and Metadata** | Contains system objects related to temporary storage.                                           | User-defined schemas and objects.                            |
|                     | No user-defined schemas.                                                                        |


## Functions of tempdb:

1. TempDB is to act something like a page or swap file would at the operating system level. If an SQL Server operation is too large to be completed in memory or the initial memory grant for a query is too small, the operation can be moved to disk in **tempdb**.
2. TempDB is to store temporary tables. Anyone who has created a temporary table in T-SQL using a pound or hash prefix (#) or the double pound/hash prefix (##) has created an object in TempDB as this is where those are stored.

#### Example to create temp Table

SQL Server provides many ways to create temporary tables

1. Using SELECT INTO:
    - You can create a temporary table by selecting data from an existing table and storing it in a new temporary table.
    - The name of the temporary table starts with a hash symbol (#).
```
SELECT product_name, list_price
INTO #trek_products
FROM products
WHERE brand_id = 9;
```
2. Using CREATE TABLE:
    - You can explicitly create a temporary table using the CREATE TABLE statement.
    - The name of the temporary table also starts with a hash symbol (#).

```
CREATE TABLE #haro_products (
    product_name VARCHAR(MAX),
    list_price DECIMAL(10, 2)
);
```
3. TempDB can also be called explicitly in a few ways. Tables can be generated in TempDB by referencing the database in a create statement. The code looks exactly like a normal DDL operation, but when run in TempDB the table is, by definition, a temporary table.
```
CREATE TABLE TempDB.dbo.products (
    product_name VARCHAR(MAX),
    list_price DECIMAL(10, 2)
);
```
**Remember, temporary tables are like whiteboards—useful for jotting down notes during a meeting, but they get wiped clean afterward!**

1. TempDB is regenerated upon every start of the SQL Server instance. 
2. Any objects that may have been created in TempDB during a previous session will not persist upon a service restart.

### Global temporary tables

- To create a temporary table that is accessible across connections. In this case, you can use global temporary tables.
- Unlike a temporary table, the name of a global temporary table starts with a double hash symbol (##).
```
CREATE TABLE ##temp_products (
    product_name VARCHAR(MAX),
    list_price DEC(10,2)
);
```
You can access the ##temp_products table from any session.

### Dropping temporary tables

#### Automatic removal
    - SQL Server drops a temporary table automatically when you close the connection that created it.
    - SQL Server drops a global temporary table once the connection that created it is closed and the queries against this table from other connections are completes.

#### Manual Deletion
    - From the connection in which the temporary table created, you can manually remove the temporary table by using the DROP TABLE statement:
> DROP TABLE ##table_name;

