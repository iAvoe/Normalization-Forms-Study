***Normalization:**
 - data organization
 - could make data mode complex - more tables
 - could make query more complex - more joins

**Dependency:**
 - Restaruant --> Dish, Restaruant --> Delivery Area, Dish -× Delivery Area (4NF unsatisfied)
 - Salesman --> Product, Salesman --> Product brand, Brand --> Product (5NF unsatisfied)

**0th Normal Form (0NF):**
 - un-normalized model - multiple values in 1 column to match multiple rows
 - calculated values in the same table w/ supporting data

**1st Normal Form (1NF):**
 - ×duplicated attributes - split multi-valued columns
 - ×calculated values in the same table w/ supporting data
 - lack of PK

**2nd Normal Form (2NF):**
 - remove attributes that are not dependent on whole PK
 - ×lack of PK - a proper PK is elected (rule: functionally dependent on any proper subset)

**3rd Normal Form (3NF):**
 - filtering: functionally dependent SOLELY on the PK, outer values are moved to other tables

**Boyce Codd Normal Form (BCNF, 3.5NF):**
 - ×non-trivial dependency (PK-Key is not the only possible relationship of a key) 

**4th Normal Form (4NF):**
 - ×multivalued dependency
 - e.g., 3 columns are duplicating themselves to satisfy varible from other other 2 columns - split table into 2, based on most duplicated column)

**5th Normal Form (5NF):**
 - Data before 4NF that has complete dependency that cannot be formatted
 - Split into N tables (Table[a|b|c]: T[a|b], T[b|c], T[a|c])

**Domain Key Normal Form (DKNF):**
 - Domain constraint is on scope of permissible values
 - Key constraint is on scope of row
 - To avoid having general constraints in database
 - instead of apply constraint on table, write the condition onto a swparate table and choose not to put the condition wording into column title.
 - e.g., [Wealthy Person, Type cst{billinaire, millionare}, Networth] to [Wealthy Person, Networth][Type, min, max]