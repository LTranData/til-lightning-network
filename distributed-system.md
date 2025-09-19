# Strong Consistency

Strong consistency is a guarantee that all users see the same data at the same time, ensuring that any changes made to the data are immediately visible to everyone. This means that once a write operation is confirmed, any subsequent read operation will reflect that change, regardless of where or when it is accessed

- Read consistency: Any read operation on the data will return the most recent write value or a value that satisfies specific consistency guarantees.
- Write consistency: When a write operation is performed, the data will be propagated to all relevant nodes in the system, ensuring that all replicas are updated with the latest value before the operation is considered successful.

https://www.geeksforgeeks.org/system-design/strong-consistency-in-system-design/

# Eventual Consistency

Eventual consistency is a consistency model used in distributed systems where, after some time with no updates, all data replicas will eventually converge to a consistent state. This model allows for replicas of data to be inconsistent for a short period, enabling high availability and partition tolerance

- Eventual consistency guarantees that if no new updates are made to the data, all replicas will eventually converge to the same value, even in the presence of network delays or temporary partitions.
- While it may lead to temporary inconsistencies, eventual consistency is suitable for scenarios where immediate consistency is not critical, such as in social media feeds or shopping carts, and provides a balance between consistency, availability, and partition tolerance in distributed systems.

https://www.geeksforgeeks.org/system-design/eventual-consistency-in-distributive-systems-learn-system-design/

# Distributed system

- Core Concepts
    - Scalability: The ability to increase computing power and capacity by adding more machines to the system. 
    - Fault Tolerance: Designing systems to continue operating correctly even when parts of the system (hardware or software) fail. 
    - Consistency: Ensuring that all nodes in a distributed system have a coherent and up-to-date view of the data. 
    - Coordination: Managing concurrent processes and ensuring proper agreement or ordering of events in a distributed environment. 
    - Communication: Understanding how processes and nodes interact, often through message passing, RPCs, and handling network latency. 
- Key Characteristics 
    - Resource Sharing: The ability to use hardware, software, or data across multiple machines in the system.
    - Concurrency: Multiple users or processes can perform tasks simultaneously on different machines.
    - Openness: How easily the system can be extended, improved, and integrated with other systems.
    - Transparency: Hiding the complexity of the distributed nature of the system from users and applications.
    - Heterogeneity: Distributed systems often involve different network hardware, operating systems, and programming languages.
- Essential Design Patterns and Algorithms
    - Consensus Algorithms: Protocols like Paxos and Raft for achieving agreement among distributed nodes. 
    - Replication Management: Techniques for managing multiple copies of data to improve availability and performance. 
    - Circuit Breaker Pattern: Prevents a series of failed calls from bringing down the entire system. 
    - Bulkhead Pattern: Isolates components so that a failure in one doesn't affect others.

# Centralized State Management Service

A centralized state management service can be configured as the service discovery to keep track of the state of every node in the system. This approach provides a strong consistency guarantee, but the primary drawbacks are the state management service becomes a single point of failure and runs into scalability problems for a large distributed system.

# Gossip protocol - Peer-To-Peer State Management Service

A gossip protocol is a decentralized communication method to spread information, like system state or failure status, across a network. Each node periodically shares information with a few randomly chosen peers, and those peers then propagate the information further to other nodes.

- Scalable
They are scalable because in general, it takes O(logN) rounds to reach all nodes, where N is number of nodes
- Fault-tolerance
They have the ability to operate in networks with irregular and unknown connectivity. They work really well in these situations because as we’re going to see a node shares the same information several times to different nodes, so if a node is not accesible the information is shared anyways through a different node. In other words there are many routes by which information can flow from its source to its destinations
- Robust
No node plays a specific role in the network, so a failed node will not prevent other nodes from continuing sending messages
- Convergent consistency
Gossip protocols achieve exponentially rapid spread of information and, therefore, converge exponentially quickly to a globally consistent state after a new event occurs, in the absence of additional events. Propagate any new information to all nodes that will be affected by the information within time logarithmic in the size of the system
- Extremely decentralized
Gossip offers an extremely decentralized form of information discovery, and its latencies are often acceptable if the information won’t actually be used immediately

