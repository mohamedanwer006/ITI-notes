# Normalization
The process of decomposing unsatisfactory "bad" relations by breaking up their attributes into smaller relations.

### Normalization of data
a process that takes a table through a series of tests (Normal Forms).
- Certify the goodness of a design and thus to 
  - minimize redundancy
  - insert anomalies
  - update anomalies
  - delete anomalies
  - frequent Null Values
- Use Normalization to another method for create a database design.


## functional dependencies
a constraint between two attributes (columns) or two sets of columns
> Value of A uniquely determines the value of B

## Functional Dependencies (FDs)Types 
1. Fully dependent
> non key attributes depend on the key 
1. partially dependent
> non key attribute depends on part of the key
3. transitive dependent 
> non key attribute depends on  non key attribute depends on the key



## Normal Form
Conditions using keys and FDs of a relation to certify whether the relation schema is in a normal form.
1. 1NF first normal form
2.  2NF second normal form
3.   3NF third normal form

## First Normal Form
- Multi valued Attribute
> place it in a new tys PK as a FK
- Repeating group
> place it in a new tys PK as a FK
- Composite attribute
> subparts each in a columns when necessary


## Second Normal Form
- First normal form
- Partial Dependency (composite primary key)
>non-keys carrying the key they depend on and place them in a new table


## Third Normal Form
- Second normal form
- Transitive Dependency
> non-key attributes carrying the non-key attribute they depend on and place them in a new table

