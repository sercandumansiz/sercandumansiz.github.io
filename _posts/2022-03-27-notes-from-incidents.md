---
layout: post
title:  "Notes from Incidents #1"
date:   2022-03-27 20:30:00 +0300
categories: incident-notes
---

### What is Oplog in MongoDB?
**oplog**: A capped collection that stores an ordered history of logical writes to a MongoDB database. The oplog is the basic mechanism enabling replication in MongoDB.

**capped-collection**: A fixed-sized collection that automatically overwrites its oldest entries when it reaches its maximum size. The MongoDB oplog that is used in replication is a capped collection.

*note 1*: MongoDB applies database operations on the **primary** and then records the operations on the primary's oplog.
The secondary members then copy and apply these operations in an asynchronous process. **All replica set members contain a copy of the oplog**, in the local.oplog.rs collection, which allows them to maintain the current state of the database.

#### What effects Oplog Size?

**Updates to Multiple Documents at Once**
The oplog must translate multi-updates into individual operations in order to maintain idempotency. This can use a great deal of oplog space without a corresponding increase in data size or disk use.

**Deletions Equal the Same Amount of Data as Inserts**
If you delete roughly the same amount of data as you insert, the database will not grow significantly in disk use, but the size of the operation log can be quite large. (for ex: applying Outbox Pattern with delete approach.)

**Significant Number of In-Place Updates**
If a significant portion of the workload is updates that do not increase the size of the documents, the database records a large number of operations but does not change the quantity of data on disk.

### What is Replication Lag?
Approximate number of seconds the secondary is behind the primary in write application. Only accurate if the lag is larger than 1-2 seconds, as the precision of this statistic is limited.

#### What to do before and after when you face with Replication Lag?

- First be sure that you have an alert condition for Replication Lag.
- Make sure that your Oplog size configured based on your needs.
- Don't forget Oplog has limited size.
- Increase bandwidth between the primary and secondary.
- Make sure that your applications follow MongoDB best practices to not have large Oplog size.
- [MongoDB Troubleshoot Replica Sets](https://www.mongodb.com/docs/manual/tutorial/troubleshoot-replica-sets/)

reference: https://www.mongodb.com/docs/