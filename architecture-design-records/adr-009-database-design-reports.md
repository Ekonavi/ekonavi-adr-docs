# ADR 009: Database Design for Reports

## Changelog
* 2023-07-12: Initial draft

## Status
DRAFT Not Implemented

## Abstract
The database design for Reports should accommodate both fixed and dynamic attributes of reporting data and allow for efficient query and data manipulation capabilities.

## Context

The reports feature needs to manage data of varying structure and complexity. The challenge lies in finding an effective method to store and query this semi-structured data, in addition to the regular, structured data.

### SQL vs NoSQL
Traditional SQL databases are great for storing structured data and provide strong ACID (Atomicity, Consistency, Isolation, Durability) guarantees. However, they tend to be less flexible when it comes to handling semi-structured or unstructured data.

NoSQL databases, on the other hand, are designed to handle semi-structured and unstructured data well. They offer flexibility in data modeling but often at the expense of ACID guarantees.

#### Pros and Cons of SQL and NoSQL
- SQL
    - Pros: ACID guarantees, mature technology, wide range of tools and support.
    - Cons: Less flexibility for semi-structured data, can be more complex to scale horizontally.

- NoSQL
    - Pros: Great flexibility for handling semi-structured data, built for horizontal scalability.
    - Cons: Weak or no ACID guarantees, less mature with fewer tools and less standardization.

Given the nature of the data, we are considering a hybrid approach using SQL for structured data and JSONB (a format in PostgreSQL) for semi-structured data within the same PostgreSQL database.

### Options Considered

#### 1. Pure Relational Structure
Create multiple tables to manage different types of data in reports. This ensures type safety, but complicates the data structure and queries, especially when data is dynamic and can be grouped in various ways.

#### 2. Hybrid Structure with JSONB
Store dynamic, semi-structured data in JSONB fields in a PostgreSQL database. This combines the advantages of SQL and NoSQL, providing both ACID guarantees and flexibility for semi-structured data. However, JSONB columns don't have native type safety and need careful handling to ensure data consistency.

## Decision
The hybrid structure with JSONB fields is chosen for managing report data. It strikes a balance between the flexibility of NoSQL for semi-structured data and the guarantees of SQL for structured data.

The "format" in the "reports_metadata" table will be updated to an array of objects, each representing a key-value pair. This will enforce a consistent order for the values, resolving potential ordering issues.

## Consequences

### Backwards Compatibility
The change will require updates in the way the application interacts with the database.

### Positive
- Improved flexibility to handle semi-structured report data.
- Retained benefits of SQL for structured data.
- Reduced complexity in data modeling and queries compared to a pure relational structure.

### Negative
- Loss of native type safety for data in JSONB fields. This needs to be handled at the application level.
- Potential need for additional query optimization or indices for efficient searching within JSONB data.

### Neutral
- The structure still fits within the overall PostgreSQL database, maintaining the benefits of a SQL database.

## Further Discussions
- Implementation details of indexing JSONB columns for efficient search.
- Strategies for ensuring data consistency in JSONB fields.
- Considerations for querying hierarchical JSONB data with GraphQL.

## References
- [PostgreSQL Documentation: JSON Types](https://www.postgresql.org/docs/current/datatype-json.html)
- [Hasura Documentation: Using Postgres JSONB with Hasura](https://hasura.io/blog/using-postgres-jsonb-with-hasura-603b6fe79748/)
- [Martin Fowler: NoSQL Databases](https://martinfowler.com/books/nosql.html)
- [GraphQL Documentation: Querying Hierarchical Data](https://graphql.org/learn/queries/#hierarchical-data)
