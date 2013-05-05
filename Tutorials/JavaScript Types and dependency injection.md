Traditionally JavaScript development hasn't lent itself to be able to create larger applications and be structured
when doing it. There has been a few attempts at fixing this through letting people write in the language they are
already using on the serverside, be it Java, C# or others, and just have the JavaScript generated from the 
statically compiled code. The motivation behind having it generated from other languages seems to be that eternal
pursuit of writing once, run in many places. With this comes a whole set of new problems; tooling for debugging, 
the code in the browser not being the same as originally written. Similiarily with the introduction of CoffeeScript
and TypeScript tries to bridge this gap by creating a new language, one that sits in between but looks familiar
enough that Ruby and C# developers will feel productive. We think of this whole problem differently; sure enough,
JavaScript has its quirks, but we think that taking the hit of learning it makes a better solution. Instead of
changing the language and how it works, we believe in working with the language and rather introduce constructs
that will give the developer productivity through added functionality.

# Namespace
In most programming languages one has the concept of grouping code within named containers. Languages such as 
C# and C++ calls namespaces. There is no direct language construct in JavaScript for dealing with this, 
but it is fairly simple to achieve by using object literals. The containers being just object literals, hashes,
with the "type" being the key and the function or object literal it represents being the value.


# Types
JavaScript is a dynamic language and types are functions. Inheritance is done through prototypal inheritance.
Bifrost introduces a construct

# Inversion of Control
A technique often used in more statically typed languages is inverting the creation of instances and let something
manage the creation and the lifecycle of the objects and instead have the these instances injected into the
constructor of the type. With this one gets the flexibility of taking dependencies to contracts, or interfaces
representing a specific set of functionality it needs but not a concrete implementation. During unit testing 
one can easily give a fake implementation with a certain scenario represented, leaving you to focus on the unit
and not all other moving parts of the system. 

This concept has somewhat been around in JavaScript as well with concepts like AMD (Asynchronous Module Definition),
and quite a few concrete implementations of these.

What Bifrost does differently about this is that with all the knowledge we have, with what exists on the serverside
of files and applying some conventions we make it a lot simpler, and in many ways more explicit.