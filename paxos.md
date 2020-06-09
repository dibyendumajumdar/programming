# Paxos Consensus Algorithm

Paxos is an algorithm that can be used by a group of processes that wish to achieve consensus on some result, despite failures
in individual processes.

## Assumptions

1. Each process has access to persistent storage where it can record its current state. The process may die but Paxos requires 
   that the process should be able to recover itself from persistent storage.
   
2. Communications between processes are reliable. 

## Goals of the algorithm

1. The goal of Paxos is to achieve consensus on some value; this value may not be the value that a proposer originally wanted to
   get consensus on. This property ensures that Paxos can handle scenarios where the proposer has failed, and another proposer
   must therefore learn about the value that was last agreed on.

## Basic ideas

1. Paxos can have any number of proposers, but it is recommended that one proposer acts as a leader. Leader election is not 
   specified by Paxos.

2. Each proposer in Paxos must be able to generate a unique monotonically increasing proposal number. No two proposers should
   generate the proposal number ever. 
   
3. Paxos orders events by the proposal number, hence a number 1 by proposer A precedes the number 2 by proposer B. This idea relates 
   back to the notion of logical clocks.
   
4. The Paxos algorithm requires knowing what constitutes a majority.

5. The Paxos algorithm is for achieving consensus on one value. The algorithm has to be restarted to agree another value.

## Algorithm

### Phase 1 - select a proposer

   In the first phase, a proposer generates its next proposal number, and sends a `prepare` message to all processes with this
   number. The processes receiving the `prepare` message are called `acceptors`. The `prepare` message is a request for two things:
   
   a) Promise to not accept proposals less than my proposal number `n`. This prevents acceptors from accepting proposals from older
      proposers.
      
   b) Inform me of the highest proposal number less than `n` that you ever accepted and the accepted value. This request enables 
      the proposer to learn of a previous consensus value - and allows Paxos to handle failures of proposers.
   
   Each process is required to remember the highest proposal number it ever accepted along with its associated value.
   Each process is also required to remember the max proposal numberit ever saw.
   
   On receiving the prepare request each process must do following;
   
   a) If the proposal number `n` in the `prepare` request is less than a proposal number already seen then ignore/reject the request. 
      This enables processes to reject `prepare` messages from older proposers.
      
   b) Reply with a promise to never accept any proposal less than `n`. And include the last accepted proposal
      number and its value (if this process ever accepted a value). It follows that this must be a proposal less than
      `n`.
      
### Phase 2 - select a value

   a) The proposer needs to get promises from a majority of processes, else has to abandon the proposal.
   
   b) If any promise response contains previously accepted proposal number / value then the proposer must use the 
      value of the highest such proposal number, in place of the value it originally wanted to get agreement on. 
      This is basically how Paxos ensures that a new proposer learns of the last consensus value. Because the promises
      are from a majority, it follows that at least one response will have any last known consensus value with the
      highest proposal number. In effect this also allows the proposer to synchronise its own proposal number to the
      be greater than this value the known highest value.
      
   c) If none of the promises contained a proposal number  / value then the proposer should propose the value it wanted
      to. 

   d) Proposer now broadcasts the chosen value and proposal number to all processes in an `accept` message.
   
   e) Processes that receive `accept` message must accept the proposal and the value iif they have not promised to 
      ignore proposal numbers such as this one. They must store the accepted value and proposal number in persistent 
      storage before replying.
      
### Phase 3 - inform the value
 
   a) Once the proposer receives an ack to its `accept` message from a majority of processes it can consider the value 
      chosen. It then sends a `commit` message to all the processes and the algorithm is finished.
      
      
      
