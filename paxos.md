# Paxos Consensus Algorithm

Paxos, in its basic form, is an algorithm that can be used by a group of processes that wish to achieve consensus on **a single result**, despite failures
in individual processes. 

## Assumptions

1. Each process has access to persistent storage where it can record its current state. Processes may die but Paxos requires 
   that a process should be able to recover its state from persistent storage.
   
2. Processes communicate via messaging. Messages may be lost due to network issues.

## Goals of the algorithm

1. The goal of Paxos is to achieve consensus on **a single value**. 

   > Once a value is chosen, all further attempts to get consensus will result in the same value. Hence Paxos in its basic form 
     cannot be used to select more than one value. 

   > The selected value in Paxos may not be the value that a `proposer` originally wanted to get consensus on. This property 
     ensures that Paxos can handle scenarios where a `proposer` has failed, and another `proposer` must therefore learn about 
     the value that was last agreed on.

## Basic ideas

1. Paxos allows any number of `proposers`. 

   > It is recommended that one `proposer` acts as a leader, although this is not required. Leader election is not specified by Paxos. 
     However the algorithm ensures that any `proposer` can function correctly. In the absense of a leader it is
     possible for two or more `proposers` to thrash around each failing to obtain consensus.

2. Each `proposer` in Paxos must be able to generate a unique monotonically increasing proposal number `n`. No two proposers should
   generate the same proposal number ever. 
    
   > A suggested approach is to make this number a pair, process id and a counter.
   
3. Paxos orders events by the proposal number, hence a number `1` by `proposer` `A` precedes the number `2` by `proposer` `B`. 

   > The proposal number acts a logical timestamp and is used to place a total ordering on all proposals. See Lamport's paper on this 
     topic.
   
4. The Paxos algorithm requires knowing what constitutes a majority or quorum for obtaining a consensus.

5. The Paxos algorithm is for achieving consensus on **a single value**. Once consensus is achieved the value is frozen and no further
   selection of a value is possible. To select multiple values an extended form of Paxos called Multi-Paxos must be used.

## Algorithm

### Phase 1 - select a proposer

In the first phase, a proposer generates its next proposal number `n`, and sends a `prepare` message to all processes. 
The processes receiving the `prepare` message are called `acceptors`. The `prepare` message is a request that:
   
1. The acceptor promise to not accept proposals less than proposal number `n`. 
  
   > This prevents acceptors from accepting proposals from older proposers.
      
2. The acceptor return the highest proposal number less than `n` that the `acceptor` ever accepted and the accepted `value`.
      
   > This request enables the `proposer` to learn of a previous consensus `value` - and allows Paxos to handle failures of `proposers`.
   
   > Each process is required to remember, despite crashes, the highest proposal number it ever accepted, along with its 
     associated value.
   
   > Each process is also required to remember the max proposal number `n` of a `prepare` message it ever promised to.
   
On receiving a `prepare` request each process must do following;
   
1. If the proposal number `n` in the `prepare` request is less than a proposal number already promised then ignore it. 
      
   > This enables processes to reject `prepare` messages from older proposers. It might be beneficial to reject rather than
     ignore such a request so that the `proposer` gets to know it is out of date, but this is not required by Paxos.
      
2. Otherwise, reply with a promise to never accept any proposal less than `n`, and include the last accepted proposal
   number and its value, if this process has ever accepted a value. The process must record the proposal number `n`
   in durable storage before responding.
      
   > Note that an `acceptor` is free to respond to a new `proposer` with higher `n`.
      
### Phase 2 - select a value

1. The `proposer` must obtain promises from a majority of processes, else has to abandon the proposal. 
   
   > The `proposer` may only hear back from `acceptors` if its proposal number `n` was greater than any proposal 
     already submitted. It may use a timeout to avoid waiting indefinitely.
   
2. If any promise response contains previously accepted proposal number/value then the `proposer` must use the 
   value of the highest such proposal number, instead of the value it originally wanted to get agreement on. 
      
   > This is how Paxos ensures that a new `proposer` learns of the last consensus value. Because the promises
     are from a majority, it follows that at least one response will contain the last known consensus value and the
     highest proposal number seen prior to `n`.
      
3. If none of the promises contained a proposal number / value then the proposer should propose the value it wanted
   to. 

4. Proposer now broadcasts its value and proposal number `n` to all processes in an `accept` message.
   
5. Processes that receive `accept` message must accept the proposal and the value iif they have not promised to 
   ignore proposal number `n`. If accepting, they must store the accepted value and proposal number in persistent 
   storage before replying.
      
6. Once the proposer receives an ack to its `accept` message from a majority of processes it can consider the value 
   chosen.
      

