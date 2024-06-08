
# CAP Theorem Explained

## Introduction
The CAP theorem, also known as Brewer's theorem, is a fundamental principle that applies to distributed databases.The theorem states that in the presence of a network partition, a distributed database system can only guarantee two out of the following three properties:

1. [**Consistency** ( C )](src/0-consistency.md)
2. **Availability** ( A )
3. **Partition Tolerance** ( P )

## Consistency ( C )
Consistency means that all nodes in a distributed system have the same data at the same time...

## Availability ( A )
Availability ensures that every request (read or write) receives a response, regardless of whether it was successful or failed...

## Partition Tolerance ( P )
Partition tolerance means that the system continues to operate despite network partitions or communication breakdowns between nodes...

## The CAP Theorem Explained
The CAP theorem posits that in a distributed data store, it is impossible to simultaneously provide all three of these guarantees. Instead, a system can provide at most two of the three guarantees at any given time.

### Scenarios
1. **CP (Consistency and Partition Tolerance)**:
   - The system maintains consistency and tolerates partitions, but might not be available during a partition.
   - Example: Traditional relational databases (like some configurations of SQL databases).

2. **AP (Availability and Partition Tolerance)**:
   - The system remains available and tolerates partitions but may not be consistent.
   - Example: NoSQL databases like Cassandra, DynamoDB.

3. **CA (Consistency and Availability)**:
   - The system maintains consistency and availability as long as there is no partition. If a partition occurs, either consistency or availability will be sacrificed.
   - Example: Some traditional databases can achieve this in a single-node setup without network partitions.

## Implications
Understanding the CAP theorem helps in designing and selecting distributed databases based on the specific requirements of an application:

- **For financial applications** where consistency is critical, a CP system might be preferred.
- **For social media applications** where availability is crucial, an AP system might be the best choice.
- **For applications where partitioning is rare**, a CA system might be suitable, though it's less common in distributed systems due to the inevitability of network issues.

## Conclusion
Distributed system can imply the 2 of the CAP theorem, which and why are based of the needed of the business.  

