---
title: Chapter-1:Database_Systems
date: 2024-10-13 23:50:44
tags: Database, Database-Design-And-Implementation
---

This blog lists my learning and discussion from the Software Internals Book Club, which I am a part of. The book club is run by Phil Eaton. More on this [here](https://eatonphil.com/2024-database-design-and-implementation.html).

For this book I would be building my own SimpleDB Implementation in Golang more on that [here](https://github.com/NIKU-SINGH/simpleDB)

**Chapter 1** talks about the basic properties of databases and the setup of a Simple DB.

### Fundamental Properties of a Database

1. **Persistence**: Databases must be persistent; otherwise, records would disappear when the computer is turned off.
2. **Sharing**: Databases can be shared by multiple concurrent users.
3. **Accuracy**: Databases must remain consistent and accurate.
4. **Usability**: Databases should handle various queries, indexing, searching, and other functions efficiently.

### Record Storage

- The simplest way to make a database persistent is by storing records as text files, with one file per record type.
- **Problems with this approach**:
  - Large text files take too long to update and read.
  - Searching in large text files is inefficient as it requires sequential scans.
  - Concurrent writes by multiple threads can lead to data corruption.
  - Interoperability between different applications and machines becomes complex.
  - A machine crash during an update could lead to inconsistent records.
  - Replicating the database for high availability across multiple machines becomes difficult.

### Multi-user Access

- When databases are shared, multiple concurrent users may try to read or update a record simultaneously, leading to data inconsistencies. Concurrency control through locks ensures that records are locked until the first user completes the task.

### Dealing with Catastrophe

- Databases should handle crashes effectively. If a crash occurs during an update, the database must revert to a consistent state, ensuring that updates are atomic in nature.

### Memory Management

The author gives an excellent analogy of buying a cookie to explain memory management:

- **Kitchen (RAM)**: Quick access (5 seconds).
- **Nearby Grocery Store (Flash Drive)**: Slower access (5000 seconds).
- **Mail Order (Disk)**: Slowest access (500,000 seconds).

This analogy highlights the speed differences between memory types, with RAM being the fastest and disk being the slowest.

### Usability

- A database must support query languages and utility functions to extract data effectively. This ensures users don't have to implement their own methods for accessing data.

### Simple DB Setup

Our club member created a handy [gist for setting up and running a simple database from the CLI](https://gist.github.com/ankithooda/b0d624aec9b3ed2882713d59feba4b11) without an IDE.

### Exercises

1. **Suppose an organization needs to manage a small number of shared records (e.g., 100 records).**

   1. Would it make sense to use a commercial database system for managing these records?
      - No, using a simple solution like Google Sheets, Notion, or Excel would make more sense for small datasets.
   2. What database features would not be required?
      - Features like indexing, query optimization, caching, row-level locking, and concurrency would be unnecessary.
   3. Could a spreadsheet be used to store these records? What are the potential problems?
      - Yes, but problems include lack of advanced features like complex queries and data security, as well as limited data integrity controls.

2. **Suppose you want to store a large amount of personal data. What database features wouldn’t you need?**

   - You wouldn’t need sharding, real-time analytics, or row-level security if no one else accesses the data. Some features like query optimization might still not be necessary.

3. **How large does your data need to be before storing it in a database becomes necessary?**

   - For personal data, if the dataset grows to over 100,000 records, using a database system would become essential for efficiency.
   - Other factors include the need for concurrency control, data security, and backup mechanisms.

4. **Comparing a version control system (like Git) to a database system:**
   1. Does a version control system have a concept of a record?
      - Yes, it can treat commits as records.
   2. How does check-in/checkout correspond to database concurrency control?
      - It functions similarly to record locks, ensuring no conflicts between changes.
   3. How does a user commit changes and undo uncommitted changes?
      - Users commit changes with the `commit` command and can undo with `reset` or `revert`.
   4. How well does saving updates as difference files satisfy the needs of a database?
      - This strategy mirrors database logging mechanisms that record incremental changes, ensuring that the current state can be recreated.

### Resources Shared from the Community

1. ![The actual speed of reading from different storage devices](/images/Database_Design_And_Implementation/Chapter_1/latencies.png)

2. [Latency Cheat Sheet](https://cheat.sh/latencies)  
   ![speed_latencies](/images/Database_Design_And_Implementation/Chapter_1/reads.png)

3. [Interactive Latency Research](https://colin-scott.github.io/personal_website/research/interactive_latency.html)
