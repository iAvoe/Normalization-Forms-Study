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
|     302910    |     Co-op     Parking     Food    |     Y     N     Y    |     400.00     150.00     100.00    |
|     319825    |     Parking     Food              |     Y     Y          |     150.00     100.00               |
|     327447    |     Food                          |     Y                |     100.00                          |
|     349223    |     Parking     Food              |     Y     N          |     150.00     100.00               |

**1st Normal Form (1NF):**
 - does not eliminate redundancy, but rather eliminate repeating groups
 - ×duplicated mapping - √1 to 1 mapping (columns are still multi-valued)
 - lack of PK

|     STU_NO                          |     FEE                           |     PAID             |     AMOUNT                          |
|-------------------------------------|-----------------------------------|----------------------|-------------------------------------|
|     302910     302910     302910    |     Co-op     Parking     Food    |     Y     N     Y    |     400.00     150.00     100.00    |
|     319825     319825               |     Parking     Food              |     Y     Y          |     150.00     100.00               |
|     327447                          |     Food                          |     Y                |     100.00                          |
|     349223     349223               |     Parking     Food              |     Y     N          |     150.00     100.00               |

**Explain anomaly:**
 - An inconsistency of data (e.g., payment becomes w/out ID) resulting from update, insertion, or deletion == the 3 types of anomalies
 - Update & Deletion anomalies are very frequent if database is not normalized

**If the information on STU_NO 302910 is dropped, a deletion anomaly arises. What does this mean?**
 - Information lost - Amount for Co-op

**Types of 1NF anomalies:**
 - update
 - deletion
 - insertion

**Explain update anomaly:**
 - If we update address on 1 row under table below, update anomaly arises (2 different addresses for the same person)
 
| 002 | Tom | 11, VA Street, New York | B |
|-----|-----|-------------------------|---|
| 003 | Tom | 11, VA Street, New York | C |

**2nd Normal Form (2NF):**
 - remove attributes that are not dependent on whole PK (but composite keys)
 - 2 relations are introduced:
  - key to PK
  - key to Composite Keys (relate both with PK & other key)

|     STU_NO    |     FEE        |     PAID    |     AMOUNT    |
|---------------|----------------|-------------|---------------|
|     302910    |     Co-op      |     Y       |     400.00    |
|     302910    |     Parking    |     N       |     150.00    |
|     302910    |     Food       |     Y       |     100.00    |
|     319825    |     Parking    |     Y       |     150.00    |
|     319825    |     Food       |     Y       |     100.00    |
|     327447    |     Food       |     Y       |     100.00    |
|     349223    |     Parking    |     Y       |     150.00    |
|     349223    |     Food       |     N       |     100.00    |

**Types of 2NF anomalies:**
 - insertion (cannot insert a new item without corresponding attribute from it)
 - deletion (deleting item attribute will cause data loss of the distributor)
 - update (phone number gets duplicated by amount of purchases)
   - less vulnerble than 1NF, however

**3rd Normal Form (3NF):**
 - filtering: functionally dependent SOLELY on the PK, outer values are moved to other tables
 - ×transitive dependencies for non-PKs

**Explain transitive dependency (TD):**
 - If A-->B, B-->C are 2 functional dependencies (FD), then A--(TD)->C

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