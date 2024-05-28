# Durability

## Durability in acid mean that when a transaction ended successfuly the data be persisted, whatever the changes are they have to be persisted.

- well first you as well as me thinks that if the transaction is successfuly reach the end and its over, well then the data or change that made in the transaction 100% are persisted to database or better i say the disk, right?

- in the emeregencies cases like crash or failure or even power outage or external cases like that, its the time that the d in acid shine

- in the cases i told you or similar cases the our changes that made in transaction **MUST** be persisted and remain in disk 


### Explanation of Durability

-   **Commitment**: Once a transaction is committed, the database must ensure that all changes are saved permanently.
-   **Recovery**: In the event of a crash, the database should be able to recover to the last committed state.
-   **Persistence**: The changes made by committed transactions must be written to non-volatile storage like disk or SSD.
