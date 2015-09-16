# 6.814 Lecture 3: Relational Algebra and Normalization

## Example Schema
Animals (name, age, species, cage, keptby)
Keepr (id, name)


## Relational Algebra
- Projection: select a subset of columns
- Selection: select a subset of rows

### SQL
- SELECT list -> Projection
- FROM list -> all teables referenced
- WHERE -> SELECT and JOIN

## Relational vs Codasyl

### Cons of Codasyl
- Complex coding interface
- Lack of physical and logical data independence

### Pros of relational
- Simplified programming language

### Cons of relational
- Seemingly not efficient

## Data Independence

### Physical Data Independence
Programs still work even if the way data is stored is changed. Allows for physical tuning of the data structures to happen in parallel to programs running

### Logical Data Independence
Programs still work even if the schema (logical representation) changes. For instance, adding a column doesn't break code that isn't aware that new column exists.  

#### Operations that can be done
1. Add columns (easy as long as you aren't using `SELECT *`)
2. Create tables
3. Use a SQL View to create a logical representation of a table using existing tables. The View table is not created, simply creates a SQL interface for the View Table.

## Study Break
Given animals table:
animals: (name STRING, cageno INT, keptby INT, age INT, feedtime TIME)

Write a view that will allow the folowing schema changes (while maintaining backwards copatibility)
- Key of table is animalId instead of name
- animals can be in multiple cages
- Age -> Birthday

*NOTE: I was suppose to do this separately for each question, i unfortunately did them all together*

```
animals -> animals2
animals2 -> add column "id" default is random id (incrementing), set to primary key
animals2 -> rename column "age" to "birthday", make birthday a date
aniamls2 -> drop column "cageno"

CREATE table cageAssignment -> cols (animal_id, cage_id)
CREATE table cage -> cols (id)

CREATE View animals as 
(
SELECT name, (SELECT cage_id FROM cageAssignment LIMIT 1) AS cageno, keptby, (current_date - birthday).years AS age, feedtime FROM animals2
)
```

## Schema Normalization
How we transform a collection of tables into a representation that has good properties

#### Example Schema

Hobby Schema: (SSN (key), Name, Address, Hobby, Cost)

#### Goal of Normalization
- reduce redundancy
    - hard to represent hobbies with no people
    - updates can cause data to be out of sync
    - redundancy also wastes space in our database

*Normalization*: Write down the anomolies and then try to remove them

N to N relationship between people (SSN, Name, Address) and hobbies (name, cost). This means that one person can have mulitple hobbies, and multiple people can have the same hobby.

#### Normalized Schema

```
Tables
-------
person: ssn, name, address
hobby: name, cost
person_hobby: person_ssn, hobby_name
```

## BCNF (Boyce Codd Normal Form) Algorithm for doing Schema Normalization

```
While some relation R is not in BCNF:
    Find an FD (functional dependency) F=X->Y that violates BCNF on R
    split R into:
        R1 = (X U Y)
        R2 = R - Y
```

How do we know if something is in BCNF?

```
A set of relations is in BCNF if:

For every functional dependency X->Y,
in a set of functional depencies F over a relation R,
X is a superkey key of R,

(where superkey means that X contains a key of R)
```

#### Example Schema
Hobby Schema: (SSN (key), Name, Address, Hobby, Cost)

#### Functional Dependencies
In plain words, it's a relationship described by a key that *defines* or points to other attributes.

- SSN -> name, address
- Hobby -> cost
- SSN, Hobby -> name, cost, address


#### Why Does This Matter? Here's a confusing example

Accounts, clients, office

- functional dependencies
    - client, office -> account
    - account -> office

Account | Client | Office
--------|--------|------
a | joe | 1
b | mary | 1
a | john | 1
c | joe | 2

There's no great way to turn this into separate tables without having to split table information and have redundancy.
