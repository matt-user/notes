
# Designing Data-Intensive Applications

## Preface

* Companies now handle huge volumes of data and traffic
* Clock speeds are barely increasing, parallelism is going to increase
* Downtime is more unacceptable
* Scaling can be a form of premature optimization

## Foundations of Data systems

* Fundamental ideas of data systems, distributed or not

### Chp 1: Reliable, Scalable, and Maintainable Applications

* Problems are amount of data, complexity of data, and speed of change
* Apps need to
  * Store data - database
  * Remember result of expensive op - cache
  * Allow users to search data - search indexes
  * Send message - stream processing
  * Periodically crunch data - batch processing
* Trade offs between dbs, caches, etc
* Boundaries between data systems are blurred
* Single tool can not handle all needs
* Api hides implementation from clients
* Reliability - system should work correctly even in face of adversity (hardware, software faults, and human error)
* App performs function that user expects
* Prevents unauthorized access and abuse
* Fault - one component deviating from spec
* Failure - when system stops providing service
* Fault tolerance should be tested
* Hardware faults 
* Add redundancy to components
* Software errors
* crash on bad input
* Using up shared resource
* Human errors
* Config errors by operators is leading cause of outages
* Make if fast to rollback config changes
* Monitoring shows early warning signals
* Scalability - as data volume, traffic, or complexity should be reasonable ways of dealing w growth
* Must define load in terms of system
* Ex: distribution of followers per user
* Latency is duration a requesti s waiting to be handled
* Response time is what the client sees
* Amazon: 100ms increase in response time reduces sales by 1%
* Only takes a small number of slow requests to block cpu
* When several backends are used it takes a single slow backend to slow down whole request
* Scaling up - more powerful machine
* Scaling out - distribute load across multiple machines
* Elastic - add more machines when load increases
* Maintainability - people working should be able to work productively
* Majority of cost of software is maintenance
* Operability - make it easy for ops team to keep system running
* Simplicity - easy for new engineers to understand system
* Abstraction removes complexity
* Evolvability - easy to make changes to system

## Chp 2: Data Models and Query Languages

* Data models change how we thinking about the problem we are solving
* Data models are layered on top of eachother
* Each layer hides complexity
* Sql - relational data model - tables are relations
* Hide impl details
* Generalize well
* No sql - need for greater scalability, more specialized query operations
* Orm - object relational matching - translate code to db model
* Removing duplication - normalization in dbs
* Network model - access records by following a path
* Difficult to make changes to data model
* Schema is explicit and all data must conform to it
* Schema on read is similar to dynamic type checking, vis versa with schema on write
* Schema changes can cause downtime and be slow
* Locality advantage if you need large parts of document at same time
* There are declarative and imperative query languages
* In declarative you specify conditions and transformations, but not how it is achieved
* Map reduce - process large amounts of data in bulk across many machines
* Doc model is good for 1-many or no relationship records
* Graph model for many-many
* Property graph - uid, outgoing edges, incoming edges, kv pairs
* Graph - table for edges, table for vertices
* Triple stores - similar to property graph - (subject, predicate, object) (Jim, likes, bananas)
* Object can be primitive or other subject (vertex)

## Chp 3: Storage and Retrieval

* Fundamentally db stores and retrieves
* Dbs use Log - append only data file
* Indexing - keep additional metadata to help you locate what you want
* Overhead on writes
* Some dbs allow you to add/remove indexes
* Hash index - keep in memory hash map where key is mapped to byte offset in file (location of value)
* Good where small amount of values are updated frequently
* Compaction - throw away dup keys in log
* Merging and compaction can be done in bg thread
* To delete key must append special deletion record
* Crash recovery sped up by snapshots
* Ways to detect and ignore partially written records
* Hash table must fit in memory
* Range queries are not efficient
* Sorted string table - merging segments is efficient
* Do not need to keep an index of all keys in memory
* This is bc keys are related to eachother and sorted, unlike hash map
* Can group and compress related records
* Memtable writes not in disk are lost in crash
* Can avoid this by having separate, unsorted log on every write
* Bloom filter - memory efficient data structure for approximating contents of a set
* Size tiered compaction - newer and smaller SStables are successively merged into older and larger
* Leveled compaction - key range is split into smaller sstables and older data is moved into separate levels
* B-tree is most widely used indexing structure
* Have sorted kv pairs
* Break db down in fixed size blocks or pages
* read/write 1 page at a time - corresponds to underlying hardware
* Each page is identified by an address or location
* 1 page is designated as root
* Each child is responsible for continuous range of keys
* Branching factor is number of refs to child pages
* Algo ensures depth like avl tree
* Writes overwrite page w new data - does not change location of page
* Include wal - write ahead log for reliability - append only file which is written to before pages writes
* Instead of wal some use - copy on write - modified page is written to diff location - and parent point to new location
* Can save space by abbreviating keys
* can  lay out tree such that leaf pages are sequential on disk
* Leaf pages may have ptrs to siblings
* Lsm are faster for writes, b trees for reads
* Write amplification - multiple writes over dbs lifetime
* Lsm trees can be compressed better
* Clustered index - store indexed row directly in index
* Covering index - stores some of tables columns in index
* Additional storage - write overhead
* Concatenated index - 1 key multiple columns - index specify column order
* In memory dbs are being developed
* Performance advantages on writes
* Data warehouse - run analytics on separate  db
* Extract-transform-load - process of getting data into warehouse
* Fact table - each row contains event that occurred at particular time
* Can have multiple sort orders
* Materialized aggregate - cache counts or sums that queries use most often

## Chp 4: Encoding and Evolution

* Schema on read - don’t enforce schema
* Rolling upgrade - staged rollout - new version to a few nodes at a time
* No service downtime
* Backward compatibility - newer code can read data written by older code
* Forward comp - older code can read data written by newer code
* When you want to write data to file you have to encode it as some sequence of bytes
* Need to translate between representations
* Encoding is often tied to language but this can be problematic
* Field tags - numbers which are aliases for fields - more compact
* Can add new field tags to change schema
* Avro
* Nothing to identify field or data types
* Go through fields in order of schema and use schema to get data type of field
* Writer’s schema - encodes data
* Reader’s schema decodes
* They don’t have to be the same - only compatible
* Writer schema included at beginning of file w millions of records
* Or include version and fetch schema
* A server itself can be a client to another service
* Consumers may publish messages from brokers themselves


## Part 2: Distributed Data

* Use multiple machines for scalability, fault tolerance, and latency
* Vertical scaling is simple, but more expensive and less fault tolerant
* Replication - keep copy of same data on different nodes

## Chp 5: Replication

* Replication - keep same copy of data on multiple machines
* Difficulty lies in changes to replicated data
* Single leader
* Multi leader
* Leaderless
* Each replica needs to write data
* Leader-based replication - one replica is designated leader, leader writes data then sends to followers, 
* Client can read from anywhere
* Not restricted to dbs
* Synchronous - wait for follower write
* Async - do not wait for follower write
* Usually only 1 sync follower
* When leader fails a follower is promoted
* Wal can make replication couple to storage engine
* Can use different formats for replication and storage - logical log
* Trigger - custom app code to execute when data changes
* Split brain - 2 nodes think they are leaders
* WAL write ahead log - append only sequence of bytes containing all writes to db
* Follower processes log
* Replication topology - communication paths along which writes are propagated from one node to another
* read scaling architecture
* disconnected operation - app continue working when network interrupt
* read after write consistency - users should always see data they submitted themselves

## Chp 6: Partitioning

* partition - logical division of data
* avoid hot spots
* key range - partition keys which are sorted
* hash partition - hash key and use modulo to find partition
* document partitioned indexes - secondary index are stored in same partition as primary key and value
* term partitioned - secondary indexes are partitioned separately
* 


