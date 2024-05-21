# Isolation
### In the meantime that your transaction is running other transactions may accure and run concurrently,well are you ready for having a stateless data in your transaction or not?
- usually you don't want read phenomena in your transaction.
- if your state in your transaction can or will manipulate you may think that you will get unwanted results.
- **its all about concurrent transaction that can ruined your transaction result**

### read phenomena in detail:

- #### dirty read or read phenomena
this can happen while you reading a row or anything from your database in your transaction (this is first transaction) that another transaction (the second one) created that, **the other transaction that create the row or whatever its still in middle of its own transaction and its not committed yet !!!**
so the data in first transaction its completly reliable on the other transaction that cause the creation
this mean the data can deleted (rollback situation happen in second transaction)
- ### non-repeatable reads
 when you read the same value in database multiple time **but in the middle of the this two reading first value get change** .    
 - its can be the case that you read the property and then you read that again indirectly
 - its rare but you can have duplicate or same alike query in your transaction
- ### phantom reads
when you read some rows in db in a transaction like you getting a range in db and you make it again later in that transaction but like others examples the second one is different from first one, as you can think another concurent transaction made that change and that change is adding some row in that range.
- you may think that the past one and current isolation level are the same kinda; well that what i have that problem but they are **not** ofcourse.
- ### Lost updates
when you write something in the middle of your transaction (and you didnt commit that like all the other levels) and after that you read it again
i think you should know better by now that the data is change its lost what you wrote
after all levels well you know what cause that; ofcourse another concurrent transaction change or delete what you just write in transaction and now its gone

- ## Isolation levels (+ sql commnad)
-  ### Read uncommitted: 
- - - #### allow to see other uncommitted change from your transaction
- - #### completly without isolation
- - #### its the lowest level
- ### Read committed:
- - #### allow you to see only committed changes 
- - #### one of most the popular level
- - #### higher level compare to previous 
- ### Repeatable read:
- - #### solve non-repeatable read phenomena
- - #### the rows you read not gonna change
- - #### higher level compare to previous 
- ### Snapshot:
- - #### make snapshot of whole database in start of transaction so every query use the data that database have in the time of starting the transaction
- - #### higher than previous but not highest yet
- ### Serializable
- - ### no concurrency at all
- - ### highest level

## Dirty Read Visualization

### Scenario

### Initial State
- **Account A**: $100
- **Account B**: $50

### Transactions Overview

#### Transaction 1 (T1)
1. **Start Transaction**
2. **Debit $30 from Account A**
3. **Credit $30 to Account B**
4. **Do not commit yet**

#### Transaction 2 (T2)
1. **Start Transaction**
2. **Read Account A**
3. **Read Account B**
4. **Commit**

### Step-by-Step Illustration

| Step | Transaction | Operation                           | Account A | Account B | Notes                       |
|------|--------------|--------------------------------------|-----------|-----------|-----------------------------|
| 1    | T1           | Start Transaction                   | $100      | $50       |                             |
| 2    | T1           | Debit $30 from Account A            | $70       | $50       | Uncommitted change          |
| 3    | T1           | Credit $30 to Account B             | $70       | $80       | Uncommitted change          |
| 4    | T2           | Start Transaction                   | $70       | $80       |                             |
| 5    | T2           | Read Account A                      | $70       | $80       | Reads uncommitted data from T1 |
| 6    | T2           | Read Account B                      | $70       | $80       | Reads uncommitted data from T1 |
| 7    | T2           | Commit                              | $70       | $80       |                             |
| 8    | T1           | Rollback                            | $100      | $50       | Reverts changes             |

### Explanation

- **Step 2-3 (T1)**: Transaction 1 modifies Account A and Account B but doesn't commit the changes.
- **Step 5-6 (T2)**: Transaction 2 reads the modified values from Account A and Account B, which are uncommitted changes made by Transaction 1.
- **Step 8 (T1)**: If Transaction 1 rolls back, the changes are undone, and the state of the accounts reverts to the initial state.
- **Note:**
-**Step 8(T1) can be happen before step 7 and you may repeat the query directly or indirectly and then you notice that the value is change**





## Non-Repeatable Read Visualization

### Scenario

### Initial State
- **Account A**: $100

### Transactions Overview

#### Transaction 1 (T1)
1. **Start Transaction**
2. **Read Account A**
3. **Read Account A again later**

#### Transaction 2 (T2)
1. **Start Transaction**
2. **Debit $30 from Account A**
3. **Commit**

### Step-by-Step Illustration

| Step | Transaction | Operation                           | Account A | Notes                         |
|------|--------------|--------------------------------------|-----------|-------------------------------|
| 1    | T1           | Start Transaction                   | $100      |                               |
| 2    | T1           | Read Account A                      | $100      | Initial read                  |
| 3    | T2           | Start Transaction                   | $100      |                               |
| 4    | T2           | Debit $30 from Account A            | $70       |                               |
| 5    | T2           | Commit                              | $70       | Change committed              |
| 6    | T1           | Read Account A again                | $70       | Sees modified data            |
| 7    | T1           | Commit                              | $70       |                               |

### Explanation

- **Step 2 (T1)**: Transaction 1 reads the initial value of Account A.
- **Step 4-5 (T2)**: Transaction 2 modifies Account A and commits the change.
- **Step 6 (T1)**: Transaction 1 reads Account A again and sees the updated value due to the committed changes by Transaction 2.

### Non-Repeatable Read Impact

- **Inconsistent Results**
- **Data Integrity Issues**





## Phantom Read Visualization

### Scenario

### Initial State
- **Account Table**:
  - Row 1: Account ID 1, Balance $100
  - Row 2: Account ID 2, Balance $50

### Transactions Overview

#### Transaction 1 (T1)
1. **Start Transaction**
2. **Read all accounts with Balance > $50**
3. **Read all accounts with Balance > $50 again later**

#### Transaction 2 (T2)
1. **Start Transaction**
2. **Insert new account with Balance $75**
3. **Commit**

### Step-by-Step Illustration

| Step | Transaction | Operation                                      | Account Table                           | Result Set (T1)         | Notes                         |
|------|--------------|-----------------------------------------------|-----------------------------------------|-------------------------|-------------------------------|
| 1    | T1           | Start Transaction                             | ID 1: $100, ID 2: $50                   |                         |                               |
| 2    | T1           | Read accounts with Balance > $50              | ID 1: $100, ID 2: $50                   | ID 1                    | Initial read                  |
| 3    | T2           | Start Transaction                             | ID 1: $100, ID 2: $50                   | ID 1                    |                               |
| 4    | T2           | Insert new account with Balance $75           | ID 1: $100, ID 2: $50, **ID 3: $75**    | ID 1                    | New row added                |
| 5    | T2           | Commit                                        | ID 1: $100, ID 2: $50, ID 3: $75        | ID 1                    | Change committed              |
| 6    | T1           | Read accounts with Balance > $50 again        | ID 1: $100, ID 2: $50, ID 3: $75        | **ID 1, ID 3**          | Sees new row                  |
| 7    | T1           | Commit                                        | ID 1: $100, ID 2: $50, ID 3: $75        | ID 1, ID 3              |                               |

### Explanation

- **Step 2 (T1)**: Transaction 1 reads accounts with a balance greater than $50, initially getting one result (Account ID 1).
- **Step 4-5 (T2)**: Transaction 2 inserts a new account with a balance of $75 and commits the change.
- **Step 6 (T1)**: Transaction 1 reads the accounts again with the same query and now sees two results (Account ID 1 and Account ID 3), which includes the newly inserted row by Transaction 2.

### Phantom Read Impact

- **Inconsistent Query Results**
- **Potential Logic Errors**




# Lost Update Visualization

## Scenario

### Initial State
- **Account A**: $100

### Transactions Overview

#### Transaction 1 (T1)
1. **Start Transaction**
2. **Read Account A**
3. **Update Account A (add $50)**
4. **Commit**

#### Transaction 2 (T2)
1. **Start Transaction**
2. **Read Account A**
3. **Update Account A (subtract $30)**
4. **Commit**

### Step-by-Step Illustration

| Step | Transaction | Operation                           | Account A | Notes                          |
|------|--------------|--------------------------------------|-----------|---------------------------------|
| 1    | T1           | Start Transaction                   | $100      |                                 |
| 2    | T1           | Read Account A                      | $100      | Initial read                   |
| 3    | T2           | Start Transaction                   | $100      |                                 |
| 4    | T2           | Read Account A                      | $100      | Initial read                   |
| 5    | T1           | Update Account A (add $50)          | $150      |                                 |
| 6    | T1           | Commit                              | $150      | Change committed               |
| 7    | T2           | Update Account A (subtract $30)     | $70       | Based on initial read          |
| 8    | T2           | Commit                              | $70       | Overwrites T1's update         |

### Explanation

- **Step 2 (T1)**: Transaction 1 reads the initial value of Account A ($100).
- **Step 4 (T2)**: Transaction 2 also reads the initial value of Account A ($100).
- **Step 5-6 (T1)**: Transaction 1 updates Account A by adding $50 and commits the change, resulting in Account A having $150.
- **Step 7-8 (T2)**: Transaction 2 updates Account A by subtracting $30 based on the initial read value of $100 and commits the change, resulting in Account A having $70, which overwrites the update made by T1.

### Lost Update Impact

- **Overwritten Changes**
- **Data Integrity Issues**
