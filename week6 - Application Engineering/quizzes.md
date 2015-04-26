##QUIZ: WRITE CONCERN

Provided you assume that the disk is persistent, what are the w and j settings required to guarantee that an insert or update has been written all the way to disk.

[( )]: w=0, j=0
[(X)]: w=1, j=1
[( )]: w=2, j=0
[( )]: w=1, j=0

What are the reasons why an application may receive an error 
back even if the write was successful. Check all that apply.

[X]: The network TCP connection between the application and the server 
		was reset after the server received a write but before a 
		response could be sent.
[X]: The MongoDB server terminates between receiving the write and responding to it.
[X]: The network fails between the time of the write and the time the client receives a response to the write.
[ ]: The write violates a primary key constraint on the collection.


##QUIZ: INTRODUCTION TO REPLICATION

What is the minimum original number of nodes needed to assure the election of a new Primary if a node goes down?

[( )]: 1
[( )]: 2
[(X)]: 3
[( )]: 5

##QUIZ: REPLICA SET ELECTIONS

Which types of nodes can participate in elections of a new primary?

[X]: Regular replica set members
[X]: Hidden Members
[X]: Arbiters
[ ]: Lawyers

##QUIZ: WRITE CONSISTENCY

During the time when failover is occurring, can writes successfully complete?

[( )]: Yes
[(X)]: No

##QUIZ: CREATING A REPLICA SET

Which command, when issued from the mongo shell, will allow you to read from a secondary?

[( )]: db.isMaster()
[( )]: db.adminCommand({'readPreference':'Secondary'})
[( )]: rs.setStatus("Primary")
[(X)]: rs.slaveOk()

##QUIZ: REPLICA SET INTERNALS

Which of the following statements are true about replication. Check all that apply.

[ ]: You can write to a primary or secondary node and the database will forward the write to the primary.
[X]: Replication supports mixed-mode storage engines. For examples, a mmapv1 primary and wiredTiger secondary.
[X]: A copy of the oplog is kept on both the primary and secondary servers.
[ ]: You can read from a primary or secondary, by default.
[X]: The oplog is implemented as a capped collection.



##QUIZ: FAILOVER AND ROLLBACK

What happens if a node comes back up as a secondary after a period of being offline and the oplog has looped on the primary?

[(X)]: The entire dataset will be copied from the primary
[( )]: A rollback will occur
[( )]: The new node stays offline (does not re-join the replica set)
[( )]: The new node begins to calculate Pi to a large number of decimal places


















