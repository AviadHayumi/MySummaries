# System Design

## Load balancers 
there are 2 types :
- hardware 
- softaware (sofwatre is more cheaper)

There are some types for load balancers algoritems : 
- HAProxy - manage the helath & remove/add machines to the pool 
- Roud robin - first then second then third etc ...
- Round robin Weighted server (if server is stronger hardware is more wheighted server then probably gonna get more requests)
- Least connections - send to the server handle least connections 
- Least Response TIme - send to the fastest server
- Source IP Hash -client ip will be hashed so same client will be directed to the same server 
- URL Hash - based on the url the request is coming 

--------------------------	

## Cache 

### what is caching ? 

like short term memory , which has limited space , but this is fast than longer momery (db).

contains at most , recently accessed items (recently request data likely to be requested again)
	
### where it can be added ?
- web browser
- web application 
- etc..

### expmale of cache tech :
- memCache
- reddis


### Types
- Application server chache 
placing chache on request (if we use load balancer we can  lose data because cache is on the node)
		
- Distribute Cache
each node conatin part of cache data by consisent hashing algoritem 
each request that looks for data will know by consistent hashing to which node to go
we can increase the cache by adding more nodes (Horizonal scale)
			
- Gloabl Cache
    ask for x , x doesnt exists ? get x from db/disk/etc.. and save to cache , asks for x again ? get x from cache

- CDN (Content Distribution Network) explore
	
### Cache Invalidation (update data to cache)
- Write through cache
When you have to insert new data you insert it to db and ALSO to the chache 
This is very realiable but costs more in insertion.
- Write around chache
Write only to db and not to chache 
This is not realiable.
- Write back chache 
write first to chache , after some interval the new data will be taken from chache and will be written to db.
risk of data lost when node is down.

### Cache Eviction Policy (contain important data)
- FIFO - data updated fitst to chache - remove it.
- LIFO - last data updated to chache - remove it.
- LRU - Least Recently Used (Doubly linked list , hash map).
- Least Freqently Used 
- Random Replcement

--------------------------	
	
### Sharding/Data partitioning
### What is sharing ?
divide your data to smaller chunk.
break big database to smaller parts.

### Types of scaling
Horizntol scale
add more servers to scale

Verical Scale (not very efficent)
increase cpu,ram to scale
		
**sharding is based on horizontal scale**

		
### Sharding Methods 
- Horizontal partitioning 
according to ranges we storing data , for example we take personID , all that starts with 1 goes to node 1 2 to 2 and so on to 9
put whole Document/Recored/Row in diffrent node.
- Vertical partitioning
divide table based feature like country
we can not balance in this way.
if we have more users in france than usa this is a problem.
- Directory based Parititoning 
we query dictionray server that direct us to required node
		
### Sharding Criteria
- Hash based 
using hashing on entity value
- List Partitioning
based value , i,e : regions
- Round Roibn Partitioning (care that db will be balance)
				first 1 then 2 then 3 then 4 etc...
- Composite Partitioning :  ????
		
### Sharding challanges
- ACID Compliance
- Joins Infefficent

--------------------------	

## Indexes
Improve query on db , cost on writes.
when we put index to a column , we store the column and pointer for the whole document	

### Type of indexing
- Ordered : ascending , decending
- Hash : more scalable

--------------------------	

## Proxy Server

### What is proxy server ?
software/hardware between client and server 
we can filter the request
blocking porn sites 
we can log requests from proxy
adding headers
coordinates requests from multiple servers , optimize traffic requests

Proxy can serve response from the cachce 
some computers from school asks for website , proxy take to website once and when another computer asks it serve it his data instead of fetch this again 
provide annonitimity

--------------------------	
		
## Message Queue
asyc service to service communication calls used in serveless and microservices architectures
Producer insert data and Consumer pull it
data is kept on queue until consumer take it
fault tolerance , when broker fail the data is still avilable 
example of mq : activemq,kafka,qpid5,rocketmq,jboss,joram,rabbitmq,sun open mq , tarantool
	
--------------------------	

## SQL vs NO.SQL

### SQL
stores data in raw and columns , each record has fixed schema.
sql query
scale by increasing ram,cpu,storage

### NO SQL
each record can be diffrent from another
scale by adding servers
unstructed sql query
designed to be scaled on multiple data servers
acid compliance for performance??? explore 
coloumns can be added on fly ,each row doesnt have to contain each columns

### Types
- Key value : Reddis, Voldemart, Dynamo
- Document db : mongoDB, CouchDB
Wide column db : Coulumn db , analyze dataset (cassandera ,hbase)
- Graph db : data is saved in graph stracture (neo4j , InfinteGraph)
	
### ADVANTAGE NOSQL
- schema change a lot
- data stored across diffrent regions

### DISADVANTAGE
- data not consistent
- sql using transcation

--------------------------	

## Monolitiech vs MicroServices
	
### Monolitich
- stick to single framework
- fault tolerance , small something can break down the system
- add new features required sometimes change whole system
- hard to scale  (Vertical scale)
		
### MicroServices
Monolitich is divided to small micro-independant-services
each serivce has own lnb,chache,rest api , etc...
micro-services talks by http calls

benfits:
- single capabillites
- independant as a product
- decoupling
- cd
- auotonomy , scalabiliy
- horizontal scale

--------------------------	

## Hashing - use case
- Hashing is used while indexing db table query on big o(1)
- ctyptograpic - take value that important , password , card number and make it to another value while transfring in the internet
- shaeding keys.
- finding duplicates - if two objects has same hash code 

--------------------------	

## Consistent Hashing
> explore

------------------------

## kafka
distrubited sreaming platfrom
kafka is fast uses io efficently by comperssing records
stream data to data lakes ,applications and real time analytict

-------------------

## LRU (Least Recently Items)
most common eviction policy.
Chace eviction policy which means what doing when chache is full
LRU chache is implemented as linked list and hashmap
doubly linked list is implemented to store list of pages with most 
recently used page is in the first on the list (insertion linked list is o1) 
hashmap here is to help us in searching in o(1) instead o(n)

when thing is accessed we either take the thing from his place on list and move 
it to the start ot if not present create and put it on start.
if list is full remove last element on the list.

-------------------------------

## Apache Hadoop
inspired by google map reduce and google file system.
collection of open source projects that faciliate using a network of many computers to solve problem
involving massive amount of data and computation.  (in map reduce way)
all modules in haddop should automaticly handle failure.

open sources in hadoop : zookeeper, storm ,solr ,spark , hadoop map reduce , hive ,yarn hdfs , pig ,spark , hbase , cassandera , kafka

### hadoop file system 
spilt to files give each node file , nodes process the data in parallel.
java based.
replicating data in diffrent nodes.
NameNode - all the config , how many data node etc...
data nodes 	can talk to each other to rebalnce data , move copies , keep replication high 
most use : store big data , data lakes , facebook acesses what users is doing for analyze in the future.
cannot handle real time analyze.
hdfs is not dyanic storage , you are not delete changing files.
write once read anywhere.

### hbase  
distrubited no sql open source db based on java 
hbase is on top of hadoop
comporssed the data in memeory for fast read/write
can store any data , file image etc...
mainly used for application read and write not for analytict

### yarn
managing computing resoucres in cluster.

### map reduce 
programing model for large scale.

### zookeeper 
manage all the softaware in hadoop
coordinate between distrubited applications
sharing config to all nodes


--------------------------	

## solr
------------
## cassandera
--------------
## url shortner

	

		
		
			