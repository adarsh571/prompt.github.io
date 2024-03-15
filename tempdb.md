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
| **Purpose**             | **Tempdb**                                                                                      | **Databases**                                                |
|-------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| ****                    | Primarily used for temporary storage during query execution                                     | Serve as persistent storage for data                         |
| ****                    | Holds temporary objects like global or local temporary tables, table variables, and work tables | Store user-defined tables, views, indexes, and other objects |
| ****                    | Acts as a scratchpad for sorting, spooling, and other intermediate operations.                  |                                                              |
| **Lifetime**            | Recreated every time SQL Server restarts.                                                       | Data persists even after server restarts.                    |
| ****                    | No data persists across sessions.                                                               |                                                              |
| **Durability**          | Non-durable: No data is saved between sessions.                                                 | Durable: Data is persisted                                   |
| **Schema and Metadata** | Contains system objects related to temporary storage.                                           | User-defined schemas and objects.                            |
| ****                    | No user-defined schemas.                                                                        |


## Functions of tempdb:

1. TempDB is to act something like a page or swap file would at the operating system level. If an SQL Server operation is too large to be completed in memory or the initial memory grant for a query is too small, the operation can be moved to disk in **tempdb**.
2. TempDB is to store temporary tables. Anyone who has created a temporary table in T-SQL using a pound or hash prefix (#) or the double pound/hash prefix (##) has created an object in TempDB as this is where those are stored.

