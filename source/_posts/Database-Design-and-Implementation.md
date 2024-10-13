---
title: Database-Design-and-Implementation
date: 2024-10-13 22:43:07
tags: Database, Database-Design-And-Implementation
---

This Blog lists my learning and discussion from Software Internals Book club that I am a part of. This Book club is run by Phil Eton more on this [here](https://eatonphil.com/2024-database-design-and-implementation.html). 

**Chapter 1** talks about the basics properties of Database and setting up of Simple DB.

### Fundamentals properties of a database

1. Databases must be persistent. Otherwise, the records would disappear as soon as the computer is turned off.
2. Databases can be shared. be shared by multiple concurrent users.
3. Databases must be kept accurate. (Consistency)
4. Databases must be usable. Ability to handle various queries, perform various functions,indexing, searching, etc

### Record Storage

- The most simple way to make DB persistent is to store records in the form of text files, one file per record type.
- **Problem with this approach**
    - large text ﬁles take too long to update and Read
    - Searching of records in large text files is very inefficient as we have to do sequential scans.
    - What if two threads try to write to the same file at the same time?
    - What if we now want to create a new application that uses the same database? What if that application is running on a different machine?
    - What if the machine crashes while our program is updating a record?
    - What if we want to replicate the database on multiple machines for high availability?

### Multi user Access

- When a database is shared chances are there there can be multiple concurrent users trying to read or update a record. It may result in wrong data if both of them try to change the same record simultaneously. Therefore concurrency should be controlled by adding locks to the record until the first user completes its task.

### Dealing with Catastrophe

- A Database should be able to handle crashes, what if during an update it crashes we may result it half records updated and half remained unchanged, so a database must revert back its changes in case of a crash that is it must be atomic in nature.

### Memory Management

The Author gives an analogy of buying a cookie from a store to explain this concept which in my sense is the best way

- Suppose you crave a chocolate chip cookie. There are three ways to get one:
    1. From Kitchen
    2. From nearby Grocery store
    3. via mail order
- In this analogy, your kitchen corresponds to RAM, the neighbourhood store corresponds to a ﬂash drive, and the the mail
order company corresponds to a disk.
- Suppose that it takes 5 seconds to get the cookie from your kitchen. Getting the cookie from the analogous store would require
5000 seconds, which is over an hour. This means going to the store, waiting in a very long line, buying the cookie, and returning. And getting the cookie from the analogous mail order company would require 500,000 seconds, which is over 5 days. That means ordering the cookie online and having it shipped using standard delivery. From this point of view, ﬂash and disk memory look terribly slow.

### Usability

- A Database must support a query language and utility functions in order to extract out the data more effectively and meaningfully, so that users don’t have to write their own implementation to access the data.

### Simple DB Setup

Our club member created a handy gist for setting up and running a simple database from the CLI [SimpleDB setup instructions from terminal - NO IDE](https://gist.github.com/ankithooda/b0d624aec9b3ed2882713d59feba4b11)

### Exercises

1. **Suppose that an organisation needs to manage a relatively small number of shared records (say, 100 or so).**
    1. Would it make sense to use a commercial database system to manage these records?
        1. I believe if the dataset is not large managing it in some sort of spreadsheet or google sheets or Notion DB would make sense as they are easily shareable. No doubt databases like PostgreSQL and MySQL can easily operate on these amount of data but they are made for large datasets and comes with lots of built in features which might be an overkill for a simple data.
    2. What features of a database system would not be required?
        1. **indexing**, **query optimization**, and **caching**  **row-level locking**, **optimistic concurrency**
    3. Would it be reasonable to use a spreadsheet to store these records? What are the potential problems?
        1. Yes for small set of data it can be used.
        2. Problems
            1. **Advanced Features**: Features like complex queries, triggers, and data security (e.g., role-based access control) are minimal compared to traditional database systems.
            2. **Data Integrity**: Unlike databases, these tools are less strict in maintaining data integrity (e.g., enforcing data types or relations).
2. **Suppose you want to store a large amount of personal data in a database. What features of a database system wouldn’t you need?**
    1. Shadring,Realtime Analytics, Row level security (as its personal so no one is going to access it except me), indexing and query optimization,data partitioning and more features but these are the features I know as of now.
3. **Suppose you want to store a large amount of personal data in a database. What features of a database system wouldn’t you need?**
    1. How large would the data have to get before you would break down and store it in a database system?
        1. Suppose I am a big shopaholic and shop everyday from various sites in that case the data is going to be very large and would need a place where I can store and query it effectively.
        2.  A rule of thumb could be around **100,000 records** or more, where the performance of simpler solutions degrades significantly
        3.   **Concurrency**: If multiple users need to access or modify the data at the same time, a database with built-in concurrency control mechanisms (like **transactions** and **locking**) becomes necessary.
        4.  If the data is sensitive (e.g., personal information), we’ll likely need a database sooner, regardless of data size, due to the enhanced **security**, **backup**, and **encryption** features database systems offer.
    2. What changes to how you use the data would make it worthwhile to use a database system?
        1. Multi user access
        2. frequent updates
        3. search and filtering
        4. backup and recovery
        5. security
        6. complex queries
4. If you know how to use a version control system (such as Git or Subversion), compare its features to those of a database system.
    1. Does a version control system have a concept of a record?
    2. How does check-in/checkout correspond to database concurrency control?
    3. How does a user perform a commit? How does a user undo uncommitted changes?
    4. Many version control systems save updates in difference ﬁles, which are small ﬁles that describe how to transform the previous version of the ﬁle into the new one. If a user needs to see the current version of the ﬁle, the system starts with the original ﬁle and applies all of the difference ﬁles to it. How well does this implementation strategy satisfy the needs of a database system?

# Resources shared from Community

1. The actual speed of reading from different storage devices : https://06536373581211777256.googlegroups.com/attach/2ccae46a120df/data_read_speed.png?part=0.1&view=1&vt=ANaJVrH1gVECTdjJE5uC1Id79Amda2R5GL76enKBM-RenpTrJ_60qUkx1gF_-D1OY0l3YWfnL2fCLWIUAwEa4fqCgD2O3u2pWwQqe89GqtP573EW_kXS6is
2. https://cheat.sh/latencies 
3. https://colin-scott.github.io/personal_website/research/interactive_latency.html