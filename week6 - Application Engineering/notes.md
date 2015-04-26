##WRITE CONCERN

server has cpu, memory and disk
memory has cache of pages that write to disk
OR
memory has journal (log of fb processes)
when journal gets written to disk = persistent data

driver by default waits for response.  but journal may wait to write to disk
w=1
wait for this server
j = false

vulnerable if the journal doesn\t write to disk

w and j are write concern

default ( w = 1; j = false); //<- fast with window of vulnerability.  what if server crashes?

w = 0; //unaknowledged write.  w doesn't wait

w=0 and j=true is slow




##NETWORK ERRORS

let w=1 && j=1;

what if you do not get a response?  it might have worked but you merely may not have received the response (TCP reset e.g.)

generally, for an insert, it is possible to guard against erronousely received responses/errors.

insert will just re-write when you do it against

update is problematic because if you do not know starting value for a specfic field, you might not know whether it has been updated


##REPLICATION

Availability = 
Fault Tolerance =

if nodes go down or fire, how do ensure that we the data

replica sets = mongo nodes that act together, mirroring each other.  one primary and other secondaries with asychronous REPLICATION
app can only write to primary.
if primary goes down, secondarys elect a new primary.  App then connects to new primary

when the original primary comes back up, it becomes secondary.  minimum number is 3.  without three, there is no way to elect new primary


##REPLICA SET ELECTIONS

types of replica set nodes:  regular, arbiter, delayed/regular, hidden

arbiter is only there for voting.  (to ensure strict majority but does not have data on it)
delayed/regular is usually for disaster recovery, can vote but can not become primary, can backup at set intervals
hidden cannot become primary, maybe used for analytics p=0, can vote


##WRITE CONSISTENCY

1 primary at any time.  
writes and reads go to primary by default.  
but you can also set reads to use secondaries

##CREATING REPLICA SET

    mkdir -p /data/rs1 /data/rs2 /data/rs3

    mongod --replSet -rs1 --logpath "1.log" --dbpath /data/rs/1 --port 27017 --fork
    mongod --replSet -rs1 --logpath "2.log" --dbpath /data/rs/2 --port 27018 --fork
    mongod --replSet -rs1 --logpath "3.log" --dbpath /data/rs/3 --port 27019 --fork

not configued yet

You cannot create a host from a node that cannot become primary


##REPLICA SET INTERNALS

The oplog (operations log) is a special capped collection that keeps a rolling record of all operations that modify the data stored in your databases. 
MongoDB applies database operations on the primary and then records the operations on the primary’s oplog. 
The secondary members then copy and apply these operations in an asynchronous process. All replica set members contain a copy of the oplog, in the local.oplog.rs collection, which allows them to maintain the current state of the database.

oplog must be big enough to wait for replication sets.

##FAILOVER and ROLLBACK

w=1 and j=1, it is possible that you loose the rights to the primary if the primary goes down


##CONNECTING TO A REPLICA SET FROM THE NODE.JS DRIVER

driver does alot of this work.  give driver connection string with host names and ports (can be comma separated list)






