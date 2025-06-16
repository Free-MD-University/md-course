# elastic search:

# definition :

search engine that organize data into document.
each document can contains multiple data, each data with his own type like :
-number
-text
-vector

# index :

collection of document that share ome characteristique . each document inside the index need that fields have the same datatype. but this is not mandatory that they have the same fields.

## Shards :

a shard in elastic is the possiblity to divide the data for each index. each document will be divided by the number of shards in the same way. for exemple fields 1 and 2 of each doc will be in shard 1 and the rest in shards 2.

## replica:

allow to duplicate an index. in case of an index fall, ECK will try to give you
the replica. the number of replica inscrease the speed search too.

# Document:

a document is a json like data store in an index. each document will have a unique id created by ECK at the insert time .
