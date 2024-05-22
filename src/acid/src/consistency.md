# Consistency
## consistency prevent the corrupt data and unwanted data in database
- there should be rollback in case of transaction that its not qualified to be ended and have effect the data model  
- in some case if the unqualified transaction actually executed and make the effect on the database the data will be corrupt
- in coculution if in middle of your transaction the atomicity violated for example some query make effect and execute and others not, in result you have unconsistence data or in other word corrupted data
- atomicity is in charge for make sure that consistency is not violated in data
- ## consistency and corrupted data:
we all have set of rules for our database
in other hand the database also have some rules or constraint for itself
the consistency guarantee that these rule shouldn't violated by anyone
as i explain we have some types in rules and constraint
- data integrity constraint 
- - like rules of foreign key primary key ...
- business logic
- - like you defined that the like is always greater that 0 and its positive

**Now if any of above rule and constraint are violated in a transaction, well that transaction have to rollback**
its ensure that in any circumstances in crash errors and others, the data model remain reliable and integrity of data is preserved.
- ## eventual consistency:
### its when you have distributed system of db and the syncing between each other db worker can result a unconsitency (eventual consistency because we assume that its going to be sync)  
its can be the case that you have replicates database worker and also a main one 
so they have to be sync to each other 
the case of inconsistency here its you update something in one of the worker database and read from other one 
in that case you may encounter the eventual consistency situation or in other word temporary unconsitency

## Consistency Visualization

### Scenario

1. **Initial State**:
    - Account A: $100
    - Account B: $200

2. **Transaction**:
    - Transfer $50 from Account A to Account B.

### Steps:
1. **Debit Account A by $50**
2. **Credit Account B by $50**

| Step              | Account A Balance | Account B Balance | Notes                         |
|-------------------|-------------------|-------------------|-------------------------------|
| Initial State     | $100              | $200              | Initial valid state           |
| Debit Account A   | $50               | $200              | Intermediate state (valid)    |
| Credit Account B  | $50               | $250              | Final state (valid)           |

### State Diagram

Initial State Intermediate State Final State Account A: $100 -> Account A: $50 -> Account A: $50 Account B: $200 -> Account B: $200 -> Account B: $250


- All states adhere to the rule that an account balance cannot be negative.

---

### Eventual Consistency Visualization

### Scenario

1. **Initial State**:
    - Node 1: Account A: $100
    - Node 2: Account A: $100

2. **Update**:
    - Transfer $50 from Account A on Node 1.

### Steps:
1. **Node 1**:
    - Debit Account A by $50 (Account A: $50)

2. **Propagation Delay**:
    - Node 1 and Node 2 have inconsistent states temporarily.

3. **Eventual Consistency**:
    - Account A updates are propagated to Node 2.

| Step                  | Node 1 Account A | Node 2 Account A | Notes                                    |
|-----------------------|------------------|------------------|------------------------------------------|
| Initial State         | $100             | $100             | Consistent state across nodes            |
| Debit on Node 1       | $50              | $100             | Inconsistent state due to update         |
| Propagation           | $50              | $100             | State being propagated (inconsistency)   |
| Final State           | $50              | $50              | Consistent state across nodes eventually |

### State Diagram

Initial State Update on Node 1 Propagation Final State 

| Node                | Step 1: Initial State    | Step 2: Update on Node 1  | Step 3: Propagation        | Step 4: Final State        |
|---------------------|--------------------------|---------------------------|----------------------------|----------------------------|
| **Node 1**          | Account A: $100          | Account A: $50            | Account A: $50             | Account A: $50             |
| **Node 2**          | Account A: $100          | Account A: $100           | Account A: $100            | Account A: $50             |

- Temporary inconsistency is resolved, achieving eventual consistency.
