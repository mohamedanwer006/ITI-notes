# Entity Relationship Diagram
it is a way to make conceptual design.
representing the relationships between entities.

- Entity  `Student`
- Attributes:
`name` 
`age` 
- Relationships:
`has`,`owns`

### In building a data model a number of questions must be addressed:
1. What entities need to be described in the model?
2. What characteristics or attributes of those entities
  need to be recorded?
3. Can an attribute or a set of attributes be identified
  that will uniquely identify one specific occurrence of an entity?
4. What associations or relationships exist between entities?



## how to make conceptual design.
Entity `employee` 

attributes:
1. single attribute:  single line
`salary`,`id`,`name`,'date of birth'

2. multiple attributes: double line
`phone`
3. composite attributes:
`address`=>  `street`,`city`,`state`,`Zone`

4. Derived attributes:
`age` calculate from date of birth

----
Define unique identifier
`id` under line

Strong entity => single line
- has unique identifier
Weak entity => double line
- doesn't  has  unique identifier
- depends on other entity


## Relation degree 
connections between entities
### 1- degree of relationship
`Diamond shape ` 
- binary `has` ,`work`,`owns`
- recursive relationship  `sup`
- ternary relationship `Skilled use` between three entities

### 2- Cardinality
specify the number of entities that can be connected to an entity
- many to many => N to M
- one to many => 1 to M
- one to one => 1 to 1
### 3- Participation
minimum number of relationship instances that each entity can participate with
- `must` double line  between entity
- `may` single line between entity

### identifying relationships between owner entity and weak entity => double line
### Attributes on relationship 

----
----

