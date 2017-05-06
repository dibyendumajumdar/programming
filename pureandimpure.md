# Purity versus Pragmatism
Over the years I have often come across Solution Architects and Systems Analysts who want every design to be pure,
based on whatever principles they adhere to. Often you find that such purists have never actually written a program or
built software by their own hands. They tend to have a theoretical view of how software should be developed and 
rely on nice design diagrams.

There are many pitfalls in persuing a very purist approach. 

## Design Paradigms Change
Over time what is considered good design changes. To take only one example, at one time everyone thought that Object Oriented
design was the answer, but that view has now changed.

## When a Pistol Would Do
The attention given to the design of a piece of software should be in proportion to its criticality and importance rather than
applying a generic yard stick. You would not fire a rocket when a pistol shot is all that is needed. Similarly when designing a
software system, there will be certain components and component interactions that require a great deal of attention to ensure
that the system is built properly. The effort can range from
writing up system specifications to building and testing prototypes. For really critical systems, you may want to specify 
the system in something like [TLA](http://lamport.azurewebsites.net/tla/tla.html) so that the specification can be checked 
for correctness.

## Key Measure Is Cost Of Change
It is not immediately apparent whether a system design is good or bad, no matter how well it adheres to principles of the 
designers. Software systems need to change over time, and only with time it becomes apparent whether the design is effective.
An effective design is one that allows a system to evolve over time. Components need to be replaced, enhanced, or discarded.
If the design allows this to happen relatively painlessly over time then it is a useful design.

Conversely if a system is throw-away and will not change over time, then less investment in design is appropriate.

## Modularity and Loose Coupling
The main approach to creating a system that is change friendly is to compose it out of loosely coupled components. This is not a 
new concept. The criteria to apply when deciding how to decompose a system into components was analysed by [David Parnas back in 1972](https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf).

There is sometimes a misunderstanding about what loose coupling means. For some purists, loose coupling means that components
must be physically separate processes, communicating via messaging only. This was the approach taken by the J2EE architecture which has
now fallen out of favour. 

Loose coupling can be achieved in a monolithic system too, where all the code
executes inside one monolithic process, by adopting a design where components have defined interfaces and there is a defined hierarchy
of components. See [talks by John Lakos](https://www.youtube.com/watch?v=QjFpKJ8Xx78) on how to design software in this way.

## Pragmatic Design
Design decisions need to be pragmatic. An approach that works if you have adopted an agile development process is to start with simplest 
designs and gradually add complexity only when needed. But this can be quite expensive over time as your code may need to be 
refactored several times. Having a robust automated regression test suite is very important for this approach, as you would want
to ensure that everytime you refactor the system still continues to meet the customer requirements.

Due to the cost of refactoring it is sometimes necessary to invest time up-front to particular aspects of the system. This pays 
off if the design helps to avoid major revisions later on. But it is important to proceed with caution as you don't want to design
for scenarios that will never occur. 

## Design As Balancing Act
The central point here is that software design is a balancing act. You have to take a pragmatic view and spend your effort in the
most productive way. A purist approach that is inflexible and believes there is only one right way is too brittle. It leads to
systems that are brittle and difficult to change. This may seem counter intuitive, but it is a consequence of investing too much
into one rigid set of rules that then makes it much harder to make fundamental changes to the system.
