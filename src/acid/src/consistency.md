# Consistency
## consistency prevents corrupt data and unwanted data in the database
- there should be a rollback in case of a transaction that is not qualified to be ended and has an effect on the data model  
- in some cases, if the unqualified transaction is executed and has an effect on the database the data will be corrupt
- in conclusion, if in the middle of your transaction, the atomicity is violated for example some queries make an effect and execute and others do not, as a result, you have inconsistency data or in other words corrupted data
- atomicity and isolation is in charge of making sure that consistency is not violated in the data
- ## consistency and corrupted data:
we all have a set of rules for our database
On the other hand, the database also has some rules or constraints for itself
the consistency guarantees that these rules shouldn't violated by anyone
as I explained we have some types of rules and constraint
- data integrity constraint 
- - like rules of foreign key primary key ...
- business logic
- - like you defined that the like is always greater than 0 and it's positive

**Now if any of the above rules and constraints are violated in a transaction, well that transaction has to rollback**
It ensures that in any circumstances in crash errors and others, the data model remains reliable and the integrity of data is preserved.
- ## eventual consistency:
### when you have a distributed system of db and the syncing between each other db worker can result in inconsistency (eventual consistency because we assume that it's going to be synced)  
It can be the case that you have a replicate database worker and also a main one 
so they have to be synced to each other 
the case of inconsistency here is that you update something in one of the worker databases and read from another one 
in that case, you may encounter the eventual consistency situation or in other words temporary inconsistency

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


### [Back to ACID](https://github.com/imaarov/DBA.md/tree/main/src/acid)
