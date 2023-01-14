This is a collection of Limits that any implementation may assume.
This list is not about data-types; it is about usage of language concepts.
Similar lists exist for other well-designed languages.

If any of these limits is breached, the semantics of the program is undefined.
A compiler crash is a valid behaviour under these circumstances.


## Language-defined limits

 - The maximum height of the latice of declared types is 2^30.
 - The maximum number of classes is 2^32.
 - The maximum number of members of a types is 2^30.
 - The maximum offset of a members inherited from a class interface is 2^32.


## Compiler-specific limits

A compiler may choose to expose parameters to configure compiler-specific limits where possible.

 - The number of types checked in implicit conversions (suggested 100)
 - The number of entities currently being elaborated (suggested 200)
 - The nesting depth of template applications, e.g. ```T[T[T[T[...]]]]``` (suggested 100)


## Fundamental limits

In essence, we assume that the compiler can be run on a 64bit architecture.

 - For any IR entity, no more than 2^62 instances can exist


# Rationale

The runtime type representation uses 32bit integers because most of the values are in the lower hundreds anyway.
The only counter example is Objects embedding flat arrays which is not common and should not slow down other use cases.

The current list of compiler-specific limits is used to ensure finite processing of broken code.
Most code would also compile if the limits would be set to ten.
