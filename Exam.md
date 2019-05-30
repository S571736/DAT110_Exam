# Distributed System Tasks

## Task 1 - Processes and Communication
**Differentiate between the following:**

#### **a. Persistent and transient communication**
 * Electronic mail system is a typical example of persistent communication.
 * With persistent communication, a message that has been submitted for transmission is stored by communication 
 middleware as long as it takes to deliver it to the receiver. In this case the middleware will store the message at 
 one or several of the storage facilities. It is not a necessity for the sending application to continue execution 
 after submitting the message. Likewise, the receiving application does not need to be executing when the message is 
 submitting.
 * In Transient communication, a message is stored by the communication system while the sending and receiving 
 application are executing. More precisely if the middleware cannot deliver a message due to a interruption during 
 transmission or because the recipient is not currently active, the message will simply be discarded
 
 Communication can as well as beeing persistent and transient, be synchronous and asynchronous.
  
 
#### **b. Synchronous and asynchronous communication**
 * **Asynchronous** communications characteristic features is that a sender continues immediately after it has submitted
 its message for transmission. This means that the message is temporarily stored immediately by the middleware 
 upon submission. 
 * With **Synchronous** communication the sender is blocked until its request is known to be accepted. Essentially there
 are three points where synchronization takes place.
    1. The sender may be blocked until the middleware notifies that it will take over transmission of the request.
    2. the sender may synch until its request has been delivered to the intended recipient. 
    3. synchronization may take the place by letting the sender wait until its request has been fully processed, that is, 
    up to the time that the recipient returns a response.
 
#### **c. Concurrent and Iterative servers**
 * with **Iterative** servers, the server itself handles the request and, if necessary returns a response to the 
 requesting client.
 * **Concurrent** servers does not handle the request itself, but passes it to a separate thread or another process, 
 the immediately starts waiting for the next incoming request. Multithreaded servers are example of concurrent servers.
 Alternate implementation of a concurrent server is to fork a new process for each new incoming request. This approach
 is followed in many Unix systems. The thread or process that handles the request is responsible for returning a 
 response. to the requesting client.
 
#### **d. Stateful and stateless servers**
*  **Stateless** server does not keep information on the state of its clients, and can change its own state without
having to inform any client. Web servers are stateless. It only responds to incoming HTTP requests, which can be either 
for uploading a file to the server or for fetching a file. **Note** that in many stateless designs, the server does 
actually maintain information on tis clients, but crucial is the fact that if this information is lost, it will not lead 
to a disruption of the service offered by the server.
* In contrast, **stateful** servers generally maintains persistent information on its clients. This means that the 
information needs to be explicitly deleted by the server. Typical example is a file server that allows a client to keep 
a local copy of a file, even performing update operations. Such a server would maintain a table containing 
*(client, file)* entries. Such a table allows the server to keep track of which client currently has the update 
permissions on which client currently has the update permissions on which file, and thus possibly also the most recent 
version of that file. This approach can improve the performance of read and write operations as perceived by the client.

#### **e. Multithreaded clients and multithreaded servers** 
* A **Multithreaded client** can be for example be when loading in a web browser where it starts fetching the HTML page 
and subsequently displays it. To hide communication latencies as much as possible, some browser start displaying data
while it's coming in. An example of this is you can see and read the text of a page, but the pictures are not loaded in
yet. So as soon as the main HTML file has been fetched, separate threads can be activated to take care of fetching 
separate files and parts of the page.
* **Multithreaded servers**: The main use of multithreading in distributed systems is found at the server side. Practice 
shows that multithreading simplifies the server code considerable, but also makes it much more easier to develop servers
that exploit parallelism to attain high performance, even on uniprocessor systems.

## Task 2
The figure below shows a chord ring that implements a distributed hash table of m=5bits to provide fault tolerance and 
availability of service. Four servers 4, 13, 21, and 26 (bold and red) are replicated to provide exactly the same 
services to clients. Each server replica maintains five finger table entries.
![chord](res/chord.png)
##### 1.compute the finger table for each replica
**4**
* 1 -> 13
* 2 -> 13
* 3 -> 13
* 4 -> 13
* 5 -> 21

**13**
* 1 -> 21
* 2 -> 21
* 3 -> 21
* 4 -> 26
* 5 -> 4

**21**
* 1 -> 26
* 2 -> 26
* 3 -> 26
* 4 -> 4
* 5 -> 4

**26**
* 1 -> 4
* 2 -> 4
* 3 -> 4
* 4 -> 4
* 5 -> 13
##### 2. what is the address size for this ring
2^5 = 32, size is 5 bits
##### 3. Resolve key = 10 from server replica 26(show your steps)
go from 26 -> 4 because 4 =< 10 < 13. And then 4 -> 13 since 13 is responsible for 10
##### 4. Resolve key = 5 from server replica 4(show your steps)
 directly to 13 since 4 < 5 < 13. this is because 13 is responsible for node 5
##### 5. Which server replicas are responsible for the keys = 0, 4, 15, 16, 24, 26, 30(show the formula used)
4: 0, 4 and 30. 21: 15, 16. 26: 24 and 26

## Task 3

An overlay network is formed by 5 processes to be used to multicast messages as shown in the above figure. Process C is 
at the root of the multicast tree. The delays between the routers for the physical network are given. Use this figure 
to answer the following questions

##### 1. What is the delay in the overlay network when a message is multicast from C to D?
C - Rc - Rb - B - Rb - Ra - A - Re - Rd - D = 1 - 25 - 1 - 1 - 25 - 1 - 1 - 80 - 30 - 1 = 166
##### 2. What is the delay in the overlay network when a message is multicast from C to E?
C - Rc - Rb - B - Rb - Ra - A - Re - Rd - D - Rd - Re = 1 - 25 - 1 - 1 - 25 - 1 - 1 - 80 - 30 - 1 - 1 - 30 - 1 = 198
##### 3. Compute the stretch/relative delay penalty (RDP) when a message is multicast from C to E provided that the best path (least-cost) will be used by the underlying physical routers to route message from C to E
Overlay cost was 198, underlay route is C - Rc - Rd - Re - E = 1 - 10 - 30 - 1 = 42. RDP = 198/42 = 4,7
##### 4. if the tree is modified such that path A -> D is removed and a new path A -> E is created resulting in a new path for the tree as: C -> B -> A -> E -> D. Compute the relative delay penalty to route message from C to E.
C - Rc - Rb - B - Rb - Ra - A - Ra - Re - E = 1 - 25 - 1 - 1 - 25 - 1 - 1 - 80 - 1 = 136. RPD = 136/42 = 3,2
##### 5. Based on your calculations in (3) and (4), which tree would be more efficient to route a message from C to E? Explain your answer.
most efficient would be the new tree as the RDP is much smaller using the overlay. C - E is the only route effected, but
is very effected.

## Task 4
##### 1. Describe the benefits and challenges of replication in a distributed system
It improves performance with having bigger size, it can handle bigger load. And can also cover bigger geographical 
areas. It increases the availability by having copy of data all over the globe instead of just one place. Its increased
scalability helps avoiding bottlenecks on the main server by balancing load between main and replicated servers. 
Scalability problems often treated as a performance problem. Replication and caching improve performance. It's possible 
improve performance by placing copies of data close to the processes using them, this reduces access time. This is a 
trade-off due to the copies requiring more bandwidth to keep up to date.
The reliability of the system is bigger. If a replica crashes, we can switch to another. Servers with non-faulty data 
outvotes those with faulty data. **Price**: Having multiple copies lead to consistency problem. When a copy is modified,
copy becomes different from the rest. To maintain consistency, updates have to be carried out on all copies. this is 
quite costly in terms of performance. There are also read-write conflict and write-write conflicts where operations 
happen concurrently and they can't see what the other part is doing.
##### 2. Differentiate between Remote-write protocols and Local-write protocols
#### **Remote-Write** 
* All write operations are forwarded to the primary replica
* Primary performs the update on its local copy and forwards the update to backup servers
* Backup server performs the update and send acknowledgement to the primary
* Primary sends ack to initial process
* Read operations are carried out locally at each replica

**Remote-Write provides**
* A simple way to implement sequential consistency
* Guarantees that client see the most recent write operations.

 **However, communication overhead is high in Remote-Write protocols**
 * Client blocks until all the replicas are updated
 
 **Remote-Write Protocols are applied to distributed systems that require fault-tolerance**
 * Replicas are mostly placed on the same LAN reduce latency
 * Non-blocking approach is possible - not fault tolerant and read of most recent write is not guaranteed.
 
 #### **Local-Write**
 * All write operations require the primary copy to first be migrated to local replica server
 * Read operations are carried out locally at each replica.
 * Can provide sequential consistency
 
 **Main issue: where is the data item right now?**
 * More time can be used by processes to locate the primary data item first
 
 **Advantage**
 * Multiple successive write operations can be carried out locally while reading simultaneously
 * Possible only if non-blocking protocol is used - updates are propagated to replicas after finishing with local 
 updates.
 
## Task 5
#### 1. Differentiate between lamport clock and vector clock
**Lamport clock**
* Clock synchronization needs to be absolute
* Processes can use the order of occurrence of events rather than absolute time occurrence
* A process increments its counter before each event in that process.
* When a process sends a message, it includes its counter value with the message.
* On receiving a message, the receiver process sets its counter to be greater than the maximum of its own value and the 
received value before it considers the message received

**Vector clock**
* Initially all clocks are set to zero
* Each time a process experiences an internal event, it increments its own logical clock in the vector by one
* Each time a process prepares to send a message, it sends its entire vector along with the message being sent.
* Each time a process receives a message, it increments its own logical clock in the vector by one and updates each 
element in its vector by taking the maximum of the value in its own vector clock and the value in the vector in the 
received message 

**Small example**
Two events a, z

Lamport clock: C(a) < C(z) --> Conclusion: none

Vector clocks: V(a) < V(z) --> Conclusion: a -> ... -> z

Vector clock timestamps tell us about causal event relationships
#### 2. Discuss how distributed algorithm works in mutual exclusion algorithm
When a process wants to enter a critical section it increments its clock and multicast a message to all other processes.
It then waits for reply/permission from every other process. If there are no other process using the mutex section, it 
will reply OK and enters the critical section and gains mutex lock. If another process is using the mutex area, it will 
not reply until it's done. If there are two processes that wants to use the area at the same time they compare their 
logical clocks and the one with the lowest value will get access first.
#### 3. Discuss how centralized algorithm works in mutual exclusion algorithm

#### 4. Discuss how token-ring algorithm works in mutual exclusion algorithm

#### 5. Explain briefly how bully election algorithm works

## Task 6
#### 1. Explain how process resilience can be achieved in distributed system

#### 2. Differentiate between flat process group and hierarchical process group and discuss their benefits and drawbacks

#### 3. Explain feedback implosion and how to use nonhierarchical feedback control to mitigate it

#### 4. Discuss briefly the five classes of failures that can occur in RPC systems

## Task 7
Discuss the cryptography schemes that can be used to provide confidentiality, integrity, authentication, and non-repudiation.

## Task 8
Explain the following cloud service models
#### 1. Infrastructure as a service

#### 2. Platform as a service

#### 3. Software as a service

