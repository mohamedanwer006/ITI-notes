# Database  Fundamental

||Database ||
|:---:|:---:|:---:|
|Concept|Main components|DataBase users|


## Concept

Before Database => File Based system .e.g (Excel Sheet ,Word)

limitation  of fileBasedSystem
- separations & isolation of data
- Duplication of data 
- Program Data Dependence
- incompatible file formats

Database concept 
- Collection of related data 
- Database Management System DBMS

**Data itself** with  **DBMS software** called Database system


## Database main components 
- Application Programs
- DBMS software
   - Software to process query
   - Software to access stored data
     - StoredDB (Metadata)
     - Stored Database (actual data)

----
## Advantages of **DBMS**
- Controlling redundancy
- Restricting unauthorized access
- Sharing data
- Enforcing integrity constrains
- Inconsistency can be avoided
- Backup and Recovery

## Disadvantages of **DBMS**
- Need expertise to use
- DBMS is expensive `not only software but also hardware`
- May be incompatible with other DBMS 
----

## Database users

Database cycle 

| step1 | step2 | step3 | step4 |
|:---:|:---:|:---:|:---:|
|Analyze the data | Design database  | Implementation | Application Development |
|System analysis | Database Designer | DBA Database Administrator | Application Programer|

## DBMS Architecture
- External schema
  - what the user sees
- Conceptual schema
  - logical model of the database
- Physical schema
  - hardware model of the database

## Data models
- logical model/ conceptual model "users data , entities , attributes , relationships"
  
- Physical model "how data saved on hard disk"

##  Mapping
it is a process of request between levels

`Request` **=>** `External schema` **=>** `Conceptual schema` **=>** `Physical schema` **=>** `hard disk`

retrieve data from database  `Physical schema` **=>** `Conceptual schema` **=>** `External schema` **=>** `response`


## DBMS other functions
stored 
- text,number,image,audio,video
- date,time 
- geographic,geometric,
- Data mining

## Database Environment

### Centralized Database  `Database single point of failure`
1. mainframe environment  application and database on same server

2. client server environment  (two tier) 

3. Three tier architecture  
4. 
### Distributed Database  `Database distributed across multiple servers`
1. Replication database
   - Partial replication
   - Full replication
2. Fragmentation database
 - divided database in multiple servers





