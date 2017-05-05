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
The attention given to the design of a piece of software should be in proportion to its crticality and importance rather than
applying a generic yard stick. You would not fire a rocket when a pistol shot is all that is needed. Similarly when designing
software system, there will be certain components and component interactions that require a great deal of attention to ensure
that the system is built properly. A suitable amount of effort needs to be devoted to such components. The effort can range from
writing up system specifications, and building and testing prototypes. For really critical systems, you may want to specify 
the system in something like [TLA](http://lamport.azurewebsites.net/tla/tla.html) so that the specification can be checked 
for correctness.

## Key Measure Is Cost Of Change
It is not immediately apparent whether a system design is good or bad, no matter how well it adheres to principles of the 
designers. Software systems need to change over time, and only with time it becomes apparent whether the design is effective.
An effective design is one that allows a system to evolve over time. Components need to be replaced, enhanced, or discarded.
If the design allows this to happen relatively painlessly over time then it is a useful design.

One of the approaches to achieve this kind of flexibility is to create a system made up of loosely coupled components.
But there is a misunderstanding of what loosely coupled means. For some purists, loosely coupled means that components
must be physically separate, communicating via messaging only. This was the approach taken by the J2EE architecture which has
now fallen out of favour. 

The question of how loose coupling is to be achieved is quite a tricky one. It can be achieved in a system where the code
executes within the same process by adopting a design where components have defined interfaces and there is a defined hierarchy
of components. See [talks by John Lakos](https://www.youtube.com/watch?v=QjFpKJ8Xx78) on how to design software in this way.
The trick is deciding which components should be remote and which local. 
