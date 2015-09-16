# 6.814 Lecture 2: Data Models

## Summary of lecture

### 3 Data Models
1. Heirarchical (IMS + DL/1)
2. Network (CODASYL)
3. Relational (Codd + SQL)

### Things to keep in mind during lecture
1. Redundancy free data model
2. Data independence (physical and logical)
3. High-level languages

## Example Data Model

- animals (name, species, age, feed time)
- cages (number, size, building) -- lives in
- keepers (name, address) -- cared for by

## IMS

- Every record type has exactly one parent unless it is the root record type
- Each level in the tree is made up of segments

### Operations
1. getUnique(GU) - return first record in DB
2. getNext(GN) - return next record in heirarchy 
3. getNextParent(GNP)
4. Insert, delete

## CODASYL

- Aimed to solve data redundancy by removing heirarchy in IMS
- Solved using network model
- Collection of segments in model

### Operations
1. findInSegment
2. findNextInSegment

## Relational Model

- Simple representation (sets, i.e. tables)
- "simple" programming model
- no physical storage proposal

### Basic Rules
- Tuple: n-ary objects with fields (at least one is unique) and fields must be primitive
- Table: an unordered set of tuples
- Database: a collection of tables

### Relational Algebra
Operations on sets instead of records. Operations return new sets that can be then operated on.

- Selected: F(pred, R) -> R'
- Project: F(fields, R) -> R' (list of fields included) 
- Cross Product: F(R1, R2) -> R' (number of records in R1 * R2)
- Join: F(R1, R2, pred) -> R1 (select of pred on Cross(R1, R2)

### Identities

- SELECT(CROSS(A, B)) = CROSS(SELECT(A), SELECT(B))
- SELECT1(SELECT2(T)) = SELECT2(SELECT1(T))
- JOIN(A, B) = JOIN(B, A)
- JOIN(JOIN(A, B), C) = JOIN(A, JOIN(B, C))
