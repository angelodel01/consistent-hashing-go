# consistent-hashing-go
An implementation of consistent hashing on a distributed system in Go
***note: this readme assumes that you understand consisten hashing as used in Amazon's DynamoDB implementation***


**Run Program**
To run this program simply type
```
go run con_hash.go
```
In the terminal and you'll see the logs printing out

## Understanding Logs

**Adding Nodes**

For every add node command you'll see the nodes adding themselves they'll print
a log with this format:
```
Adding node: {id:<node id> hash_1:<first hash> hash_2:<second hash>}
```
For node virtualization each node hashes itself twice into the hash ring so you
can see it's two hashes as well as it's id print in the log format. Example:
```
Adding node: {id:2 hash_1:7 hash_2:26}
```

**Put Operations**
For every put(key, value) command you'll see the nodes pringing the put operation
results with this format:
```
Performing put(key: <key>, value: <value>) hashes to: <hash from key>, got node id: <node id>
```
Note that for the "hashes to" part is the hash from the key and isn't necessarily
the hash of the node as consistent hashing will cause the hash to linearly probe
after hashing.
Example:
```
Performing put(key: 'Maria', value: 100) hashes to: 13, got node id: 1
```

**Get Operations**
For every get(key) command you'll see the nodes pringing the put operation
results with this format:
```
Performing get(key: <key>) hashes to: <hash of key>, got node id: <node id>
```
Note that for the "hashes to" part is the hash from the key and isn't necessarily
the hash of the node as consistent hashing will cause the hash to linearly probe
after hashing.
Example:
```
Performing get(key: 'Tim') hashes to: 5, got node id: 2
```

**Node Death**
Just like adding nodes, node death is fairly straight forward you'll see a node
receiving and responding to a kill command formatted like this:
```
Node : <node id> dies
```
Example:
```
Node : 0 dies
```
note: I use the kill command to intentionally kill a node and then perform a get
operation afterwards to simulate node death.

**Ring Prints***
You'll see 2 times that the hash ring itself will be printed, it will simply be an
array where non hashed locations are '-1' and hashed locations are the id of the
node that hashes there. Example:
```
Ring [-1 -1 -1 -1 -1 -1 -1 2 3 -1 -1 -1 0 -1 1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 2 -1 -1 -1 -1 -1 -1 0 4 -1 -1 -1 -1 4 -1 -1 -1 -1 -1 -1 1 -1 -1 -1]
```
note: Because each physical node hashes to multiple virtual nodes you may see a node
id multiple times within the ring. In a larger scale implementation of this there
would be many virtual nodes to one physical node.
