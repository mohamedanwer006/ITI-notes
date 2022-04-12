# SQL
Structured Query Language (SQL) is a programming language for creating structured queries.
- DDL (Data Definition Language)
- DML (Data Manipulation Language)
- DCL (Data Control Language)

Database schema:
- a schema is a set of tables and their relationships
- there is one owner of a schema who has access to manipulate the structure of the schema
- a schema does not represent a person

Data types :
- determines the type of the data 

Database Constraints:
- a constraint is a set of rules that are enforced on the data in a table

`Primary Key` Constraints:
- not null 
- unique

`Not Null` constraints:

- enforces that a column cannot be null

`Unique` constraints:
- enforces that a column value must be unique

Referential integrity: 'FK'
- a foreign key is a column in a table that references a column in another table


`Check` constraints:
- enforces that a column value must satisfy a condition (e.g. a value must be greater than 0)



## DDL 
Data Definition Language (DDL) is a programming language for creating tables and columns. but not manipulating data.
- Create 
- Edit
- Delete

|**Commands**||||
|---|---|---|---|
|Create|Alter|Drop|Truncate|
||||

```sql
CREATE TABLE <table_name> (
<columName> <dataType> <constraints> ,<columName> <dataType> <constraints>
)
-- Example:
CREATE TABLE Student (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    ssn TEXT UNIQUE,
    dob DATE,
    street TEXT,
    zone TEXT,
    phone TEXT,
    FOREIGN KEY(ssn) REFERENCES Employee(ssn)
)
``` 

|Data type| Constraints |
|:-------:|:-----------:|
| INTEGER | PRIMARY KEY |
| CHAR    | NOT NULL    |
| NUMBER  | UNIQUE      |
| DATE    | NOT NULL    |

Add a column to a table:

```sql
ALTER TABLE <table_name> ADD <column_name> <data_type> <constraints>
-- Example
ALTER TABLE Student ADD phone NUMBER

```

remove a column from a table:

```sql
ALTER TABLE <table_name> DROP <column_name>
-- Example
ALTER TABLE Student DROP phone

```
Remove a table from a schema:

```sql
DROP TABLE <table_name>
-- Example
DROP TABLE Student

```

remove data of a table but keep the table structure:
Truncate a table:

```sql
TRUNCATE TABLE <table_name>
-- Example
truncate table Student
```


## DCL DATA control language 

- `GRANT` create privileges to a user
- `REVOKE` remove privileges from a user

use a command to give `privilege`  access to data
- System Privileges
- Object Privileges

Create privilege:

```sql
GRANT <privilege> ON <object> TO <user>
-- Example
GRANT SELECT ON Student TO admin
-- 
GRANT ALL ON Student TO ahmed

GRANT ALL ON Student TO ahmed With GRANT OPTION 
-- allow all DML operations on Student table to ahmed 
-- with GRANT OPTION give the privilege to share privilege with other users

```
Revoke privilege:

```sql
REVOKE <privilege> ON <object> FROM <user>
-- Example
REVOKE SELECT ON Student FROM admin
--
REVOKE ALL ON Student FROM ahmed
```


## DML Data manipulation language 

|**Commands**||||
|---|---|---|---|
|INSERT|UPDATE|DELETE|SELECT|
||||


### insert  data into a table

```sql
INSERT INTO <table_name> (<column_name>,<column_name>,<column_name>) VALUES (<value>,<value>,<value>)
-- Example
INSERT INTO Student (id,name,ssn,dob,street,zone,phone) VALUES (1,'ahmed',123456789,'12/12/12','sakan','sakan','123456789')
-- other way
insert into Student values (1,'ahmed',123456789,'12/12/12','sakan','sakan','123456789')

-- or
insert into Student (id,name,phone) values (1,'ahmed','123456789')

```
### update data in a table

```sql
UPDATE <table_name> SET <column_name> = <value> WHERE <column_name> = <value>

-- Example
update employee set salary=10000 where id=5
-- if you not use where it will update all rows
update employee set salary=10000 ,dno=10 where id=5

```

### delete data from a table 
work on record level
```sql
DELETE FROM <table_name> WHERE <column_name> = <value>
-- Example
DELETE FROM Student WHERE id = 1

```

### Delete vs Truncate
truncate : 
- delete all data from a table
- cannot rollback 
- it commit automatically
- de-allocate the physical storage
  
delete : 
- delete specific data from a table using where
- can rollback
- no commit automatically

```sql
-- remove all data from a table and keep structure of the table
truncate <table_name> 
```


### select data from a table

```sql
SELECT <column_name>,<columName> ,<[colum name]> FROM <table_name> WHERE <column_name> = <value>
-- Example
-- return specific columns from a table
select name,ssn,[phone number] from student
-- return all data from student table
select * from student

-- return specific record from a table
Select * from Student where id = 1

```
#### comparison and logical operators
```sql
-- retrieve data from a table where column value is greater than a value

select * from employee where salary > 1500

-- get fname  from employee where salary >= 1500 and salary <= 2000

-- and operator
select fname from employee where salary >= 1500 and salary <= 2000

-- between operator
select fname from employee where salary between 1500 and 2000

-- or operator
select * from employee where salary = 1500 or salary = 2000

-- in operator 
select * from employee where salary in (1500,2000)

```
multi row operators :
- in
- between
  
single row operators :
- =
- \> 
- <

## Like operator
used to match a pattern in a string

```sql
-- return all data from employee table where fname contains 'a'

select * from employee where fname like '%a%'

-- select ahmed or ahmad 
select * from employee where fname like 'ahm?d'

```
## Alias 
used to rename a column

> does not change anything on database

```sql
select fname as name from employee

select fname , salary*0.1 as Bonus from employee
 
```
## Concatenation operator
used to link two or more columns

```sql
select fname + ' ' + lname as [Full Name] from employee
where salary > 10000
```
## order by
used to sort data
>by default it sort by ascending order

```sql
select * from employee order by salary

select * from employee order by salary asc

select * from employee order by salary desc

select * from employee order by salary asc,fname desc
```

## Distinct
used to filter duplicate data from a table

```sql
select distinct supervisor from employee
```

## Inner Join
used to join two tables

it is primary key foreign key relationship


```sql
-- = joine
select * from employee , department 
where MGRSSN = SSN


select fname,lname from employee,department
where employee.dno = department.dno

select fname,lname from employee e ,department as d
where e.dno = d.dno

-- use inner join to get all data from employee and department
select * from employee e inner join department d on e.dno = d.dno

```

## Outer , Full , Left , Right Join
```sql
-- left outer join

-- full data retrieve from left table
select fname,dname from employee e left outer join department d on e.dno = d.dno
-- right outer join
select fname,dname from employee e right outer join department d on e.dno = d.dno
-- full data retrieve from right table

-- full outer join
select fname,dname from employee e full outer join department d on e.dno = d.dno
-- combine two tables
```
## Self join
used as recursive join on same table

```sql
-- get employee name and  supervisor name from employee table
select fname ,fname from employee as e,employee as s 
where e.superssn = s.ssn

```
## Sub-Queries
```sql
select * from employee where salary > (select salary from employee where fname ='ahmed' and lname='ali')

-- Display the employees data who earn higher
-- than all the employees in department 10
select * from employee where salary >all(
    select salary from employee where
    dno=10)
)
-- Display the employees data who earn higher
-- than any employee in department 10

select * from employee where salary >any(
    select salary from employee where
    dno=10)
)

```

## Built in functions max , min , sum,count functions
```sql
select max(salary) , min(salary) from employee

select max(salary) as maxSalary , min(salary)as minSalary from employee

-- count
select count(ssn) as employee ,count (salary) as salary from employee

--  count ignore null

-- sum
select sum(salary) as totalSalary from employee

```

## group by and having

```sql
select avg(salary) as averageSalary from employee group by dno 
having max(salary) > 1800

-- add condition to group by using having not where
```

<!--  -->

```sql
-- Display the department name and the maximum
-- salary for each department given that its
-- average is greater than 1200, sort the result by
-- department name

select dname,max(salary) from employee as e ,departments as d  where e.dno =d.dno 
group by dname 
having avg(salary) > 1200
order by dname


```

- `From` clause first step DBMS call
- `where` clause second step
- `group by` clause third step 
- aggregate function
- implement `having` clause condition
- implement `select` clause data retrieve should be in group by clause
- `order by` clause
