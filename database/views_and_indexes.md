# Views
it is a database object

- A view is a logical table based on a table or another view.
- A view contains no data of its own but is like a window through which data from tables can be viewed or changed.
- The tables on which a view is based are called base tables.
- The view is stored as a SELECT statement in the data dictionary.


## Creating a view
```sql
CREATE VIEW view_name 
AS 
SELECT Fname ,Lname ,Pname , Hours
From Employee ,Project,Works_on
WHERE SSN=ESSN AND PNO=PNUMBER
```
### To retrieve data from a view
as normal table
```sql
SELECT * FROM view_name


-- With check option
CREATE VIEW Suppliers
AS
SELECT
FROM suppliers
WHERE status > 15
WITH CHECK OPTION;

-- validate condition every time
-- you add data 

```

## modifying a view

```sql
CREATE OR REPLACE VIEW view_name
-- Example
CREATE OR REPLACE VIEW vw_work_hrs
AS
SELECT Fname, Lname, Pname,Hours
FROM Employee, Project, Works_on
WHERE SSN=ESSN AND PNO-PNUMBER AND Dno=5:
```

## Advantage of views
1. Restrict data access
2. Make complex queries easier
3. Provide data independence 
4. Present different views of the same data 


## Types of Views
|Feature |Simple Views| Complex Views|
|---|---|---|
|Number of tables| One| One or more|
|Contain functions| No |Yes|
|Contain groups of data| No |Yes|
|DML operations through a view|Yes| Not always

----

# Indexes
Use indexes to solve the following problem
1. Not sorted
2. Scattered

why indexes?
- They are used to speed up the retrieval of records in response to certain search conditions
- May be defined on multiple columns.
- Can be created by the user or by the DBMS.
- Are used and maintained by the DBMS.

> indexes cause overhead on the database. DML operations(insert,update,delete)

### Index Creation Guidelines 

Create an index when:
- Retrieving data heavily from table.
- Columns are used in search conditions and joins.
- Column contain large number of nulls.

## Creating an Index
```sql
CREATE INDEX index_name ON table_name (column_name)
Create index emp_inx on employee(salary)
```
## Remove an Index
```sql
DROP INDEX index_name
```
