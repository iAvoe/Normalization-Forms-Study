**Normalization:**
 - data organization and simplification
 - could make data mode complex - more tables
 - could make query more complex - more joins

**Dependency:**
 - Restaruant --> Dish, Restaruant --> Delivery Area, Dish -× Delivery Area (4NF unsatisfied)
   - 2 Functional Dependencies (FD), 1 Transitive Dependencies (TD) as Rest.-->DArea
 - Salesman --> Product, Salesman --> Product brand, Brand --> Product (5NF unsatisfied)
   - 3FD, 2TD

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
 - update (phone number col. gets duplicated by amount of purchases)
   - less vulnerble than 1NF, however

**Explain Super Key & Candidate Key**
 - A set of one or more attributes (columns), which can uniquely identify a row in a table (forming FD)
 - Candidate key is selected from the set of super keys
   - CK should not have redundant attribute - minimal super key
 - Example: all 3 column headings are super keys
   - SKs: {Emp_SSN}, {Emp_Number}, {Emp_SSN, Emp_Number}, {Emp_SSN, Emp_Name}, {Emp_SSN, Emp_Number, Emp_Name}, {Emp_Number, Emp_Name}
   - Minimalized SKs becomes cadidates - {Emp_SSN}, {Emp_Number}

| Emp_SSN   | Emp_Number | Emp_Name  |
|-----------|------------|-----------|
| 123456789 | 226        | Steve     |
| 999999321 | 227        | Ajeet     |
| 888997212 | 228        | Chaitanya |
| 777778888 | 229        | Robert    |

**3rd Normal Form (3NF):**
 - filtering: functionally dependent SOLELY on the PK, miltu-dependances are moved to other tables
 - ×transitive dependency (TD) for non-PKs - split table if unsatisfied

**Explain transitive dependency (TD):**
 - If A-->B, B-->C are 2 functional dependencies (FD), then A--(TD)->C

**Example of 0~3NF + ERD:**

![Purchase order 1](Purchase1.png)
![Purchase order 2](Purchase2.png)

 - Keys: Order date, P.O. #, Vendor ID, Vendor name, Address, City, Prov, Postal, Purchase line, Qty, Item ID, Item name, Unit price, Line total, Total

 - **0NF:** multiple-to-1 mapping:
![0NF](0NF-purch.png)

 - **1NF:** 1-to-1 mapping, calculated value (Line total, total) removed:
![1NF](1NF-purch.png)

 - **2NF:** Find Functional Dependencies (FDs)
   - *Keys dependent on PO_No:*
      - Order_date
      - Vendor_ID
      - Name
      - Address, City, Prov, Postal
   - *Keys dependent on {PO_No, Line}*
      - Qty
      - Item ID
      - Description
      - Price
   - Create 2NF table by splitting 1NF based on these values:

 - **3NF:** Removal of multi-dependace
   - `[dbo.PO_headers]` = {PO_No} {Order_date} {Vendor_ID}
   - `[dbo.Vendors]`       = {Vendor_ID} {Name} {Address} {City} {Prov} {Postal}
   - `[dbo.Items]`           = {Item_ID} {Description} {Price}
   - `[dbo.PO_lines]`      = {PO_No} {Line} {Qty} {Item_ID}
 
 - **Creating ERD out of 3NF**
   - `[dbo.PO_headers]` = PK{PO_No} {Order_date} FK{Vendor_ID}
   - `[dbo.Vendors]`       = PK{Vendor_ID} {Name} {Address} {City} {Prov} {Postal}
   - `[dbo.Items]`           = PK{Item_ID} {Description} {Price}
   - `[dbo.PO_lines]`      = PKFK{PO_No} {Line} {Qty} FK2{Item_ID}


   - `[dbo.PO_headers]` one to many towards `[dbo.PO_lines]`
   - `[dbo.Vendors]`       one to many towards `[dbo.PO_headers]`
   - `[dbo.Items]`           one to many towards `[dbo.PO_lines]`
   - `[dbo.PO_lines]`     

**Online Transaction Processing (OLTP)**
 - Emphasizes speed and simplicity of access over data integrity
   - Explicitly de-normalized for this purpose
   - Also known as Big Data, Data Warehouse
 - Business Intelligence (BI) tools work with OLAP data to provide end users with the ability to analyze and report 
   - Data in an OLAP database is often conceptualized around the concept of a cube
   - BI tools allow slicing, dicing, drilling down
    - Data analysis by "click chart to zoom" topology

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

 -----

**ACID rules**:
 - atomicity, consistency, isolation, durability
 - The properties of database transactions to guarantee data validity despite errors, power failures, mishaps

**Transaction**:
 - a sequence of database operations that satisfies the ACID properties
   - which can be perceived as a single logical operation on data

**Atomicity**:
 - either succeeds completely or fails completely, no middle ground/state
 - entire transactions fails and left database unchanged if any statement isn't satisfied
 - deny partial database updates, every update occurs in a whole

**Consistency**:
 - a transaction can only bring the database from 1 consistent state to another, preserving invariants
 - any data written to the database must be valid according to all defined rules (constraints, cascades, triggers)
 - free of illegal transactions, 
 - referential integrity (PK to FK relationship)

**Isolation**:
 - main goal of concurrency control
 - concurrent execution of transactions leave database in same state that would have been obtained if the transactions were executed sequentially
 - e.g., effects of incomplete transactions are invisible to other transactions

**Durability**:
 - remain commited transactions even in case of system failure
 - usually means to record in non-volatile memory

**Atomicity failure**:
 - money withdrawn from buyer did not correspondingly added to seller
 - E.g., did not group purchase-sell action with CREATE PROCEDURE AS BEGIN END statement

**Consistency failure**:
 - A+B=100, however validation check shows A+B=90
 - must revert transaction

**Isolation failure**:
 - 2 transations are modifying the same data, but the 2nd transaction happened to process in middle of transaction 1
 - If transaction 1 fails, then we must rollback transaction 2 to revert this mishap; known as a write-write conflict.

**Durability failure**:
 - The user was noticed that transaction succeeded, but actully no
 - E.g., machine crashed before data was registered, causing false positives

**ACID Implementation - Transacion Management**
 - **Begin transaction & Rollback**:
BEGIN TRANSACTION;
  PRINT '--- BEGIN TRANSACTION ---'
  UPDATE physicians SET specialty = 'Hematology' WHERE physician_id = 2;
  UPDATE patients SET allergies = 'Almonds' WHERE patient_id = 1251;
  PRINT '--- DURING TRANSACTION ---'
  SELECT * FROM physicians WHERE physician_id = 2;
  SELECT * FROM patients WHERE patient_id = 1251;
  PRINT '--- ROLLBACK ---'
ROLLBACK TRANSACTION;

 - **Commit**:
BEGIN TRANSACTION T1;
  PRINT '--- BEGIN TRANSACTION ---'
  UPDATE physicians SET specialty = 'Hematology' WHERE physician_id = 2;
  UPDATE patients SET allergies = 'Almonds' WHERE patient_id = 1251;
  PRINT '--- DURING TRANSACTION ---'
  SELECT * FROM physicians WHERE physician_id = 2;
  SELECT * FROM patients WHERE patient_id = 1251;
  PRINT '--- COMMIT ---'
COMMIT TRANSACTION;


**Concurrency Control / avoid integrity, consistency problems**:
 - simultaneous update from multiple users
 - late rollback execution causes uncommitted data being handled by other user
 - inconsistent retrievals - data is being executed and updated by different instances

**Serializability**:
 - the capability to turn concurrent operation into revertable serial

**Locks manager**:
 - in multiple levels
   - db
   - table
   - page
   - row
   - field
 - forces "1 person at a time"

**2 phase locking (2PL)**:
 - Expanding phase: locks are acquired, no locks are released
 - Shrinking phase: locks are released, no locks are acquired
 - 2 transactions cannot have conflicting locks
 - No unlock operation can procede a lock operation in the same transaction
 - No data are affected until all locks have been obtained

 - Shared lock (S): 1 user to read a record, others could read, no one could modify

 - Update lock (U): a temporary state before exclusive lock
An update lock can be placed only if no other update or exclusive lock exists
Which it can be placed on objects that already have shared locks

 - Exclusive lock (X): 1 user to modify a record, others could not read or modify


**Which SQL statement results in a shared lock? SELECT**: on rows; 

**Why bother locking a record being read?**
 - Prevents concurrent change breaks the calculations (e.g., movie thumbnail & description misatched, file size shows negative value)

**Why is an exclusive lock necessary?**
 - Prevents concurrent write that updates the same row

**Why prevent users from reading the record? INSERT & UPDATE on rows?**
 - The writing is unfinished, the data is not ready to be modified before the writing is done (lock get then released)

**Deadlock**: situation where 2 transactions blocks each other, both cannot proceed. Prevents false-updates

**Deadlock Control**:
 - Prevention (best for high probility): abort & reschedule transaction
 - Detection (best for low probility): roll back and reschedule transaction A, where B proceeds
 - Avoidance (best for low responsivenes systems)

**Recovery**:
 - unexpected failing occurs occasionally
 - hardware, power related
 - recovery restores database from a inconsisteny state to a previous consistent state
 - works best with atomic transactions

 - data goes to SQL logs first, and then written to database
 - the log has either logical operation performed, or before-after images of modified data

**Recovery operation**: restore to latest backup, then applies the transaction log