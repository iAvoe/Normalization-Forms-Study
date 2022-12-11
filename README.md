**Normalization:**
 - data organization and simplification
 - could make data mode complex - more tables
 - could make query more complex - more joins

**Dependency:**
 - Restaruant --> Dish, Restaruant --> Delivery Area, Dish -× Delivery Area (4NF unsatisfied)
 - Salesman --> Product, Salesman --> Product brand, Brand --> Product (5NF unsatisfied)

**0th Normal Form (0NF):**
 - un-normalized model - multiple-to-1 mapping
 - calculated values in the same table w/ supporting data

 |     STU_NO    |     FEE                           |     PAID             |     AMOUNT                          |
|---------------|-----------------------------------|----------------------|-------------------------------------|
|     302910    |     Co-op     Parking     Site    |     Y     N     Y    |     400.00     150.00     100.00    |
|     319825    |     Parking     Site              |     Y     Y          |     150.00     100.00               |
|     327447    |     Site                          |     Y                |     100.00                          |
|     349223    |     Parking     Site              |     Y     N          |     150.00     100.00               |

**1st Normal Form (1NF):**
 - does not eliminate redundancy, but rather eliminate repeating groups
 - ×duplicated mapping - √1 to 1 mapping (columns are still multi-valued)
 - lack of PK

 |     STU_NO                          |     FEE                           |     PAID             |     AMOUNT                          |
|-------------------------------------|-----------------------------------|----------------------|-------------------------------------|
|     302910     302910     302910    |     Co-op     Parking     Site    |     Y     N     Y    |     400.00     150.00     100.00    |
|     319825     319825               |     Parking     Site              |     Y     Y          |     150.00     100.00               |
|     327447                          |     Site                          |     Y                |     100.00                          |
|     349223     349223               |     Parking     Site              |     Y     N          |     150.00     100.00               |

**Explain anomaly:** an inconsistency of data (e.g., payment becomes w/out ID) resulting from update, insertion, or deletion == the 3 types of anomalies

**If the information on STU_NO 302910 is dropped, a deletion anomaly arises. What does this mean?**
Information is lost - Amount for Co-op

**Types of anomalies:** update, deletion, & insertion anomalies

**2nd Normal Form (2NF):**
 - remove attributes that are not dependent on whole PK
 - ×lack of PK - PK is elected (rule: functionally dependent on any proper subset)
 


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