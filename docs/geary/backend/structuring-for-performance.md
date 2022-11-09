Many ECSes advertise their speed over object-oriented approaches. How an ECS structures its data and the benefits or downsides that come with it is a complex topic, with many structures to explore. However, a common approach in designing an ECS is to keep related data together, specifically to help avoid cache misses.

I want to give an outline and link to resources to help anyone interested in this specific performance optimization.

## CPU Cache

Modern

!!! note ""
    [CppCon 2016: Timur Doumler "Want fast C++? Know your hardware!"](https://www.youtube.com/watch?v=BP6NxVxDQIs), a talk that goes over many concepts for writing cache friendly code.

There are some big advantages to packing data together like this over inheritance. Consider a system that modifies only the location of entities. With an OOP approach, you may have to load far bigger entity objects to eventually get to the data you need (their location.) Working with a packed list of locations, you already have all the data you need and can quickly load the next chunk into cache when iterating over it.
