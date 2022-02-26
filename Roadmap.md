TODO every rationale should stop with a short example; the examples should be small conformance test projects
TODO jeder punkt, der auch in anderen Sprachen existiert, sollte eine Sprache referenzieren

This document is a short summary of what's already in the language.
It is meant to serve as a complete description rather than a sales document or a comparison with another programming language.
Also, it contains what is planned to get into the language.
Further, there is a short section of things that will never get into the language.
Lastly, all features have a short rationale.


# Existing Features


## Core concepts:

 - Static name and type binding
   * Any modern programming language does that ;)

 - Language-defined built-in types (tyr.lang.native)
   * Having as few special rules as possible allows library authors almost the flexibility of language authors
   * Having base constructs defined in Tyr code enforces the code generator to create good code for everybody
   * Having implementation of base concepts like casts and type checks in Tyr code makes their cost transparent
   * Native functions can be realized as single CPU instruction where applicable and yield the same code as predefined operators in most C-style languages

 - Language-defined operators/operator overloading (tyr.lang.operator)
   * Using commonly understood operator symbols results in a massive increase of conciseness and readability
   * Having this in the language allows library authors to introduce symbols that are understood in their domain
   * Type-specific symbols, precedence and associativity are a cure to C++'s abuse of unplausible operator symbols

 - Call operators/Functors (apply/update)
   * 
   
 - Properties can modify entity definitions (e.g. tyr.lang.native or tyr.lang.operator.precedence)
   * This is similar to attributes in C++ or annotations in Java
   * The user can define arbitrary properties 
   * Currently, the only way of doing something with user-defined properties requires the creation of a tool working on .tl-files. This is pretty easy, as .tl-files are regular OGSS files holding TIR type instances.

 - semicolon inference
 
 - identifier literals
 
 - typed integer, unsigned and float literals

 - CT/RT-bracing ([], ())
   * Having 
 
 - Function representational transparency (even constructors are usable functions)
 
 - Function placement realizations
   * copying the same code over and over is expensive, increases maintenance cost and results in more and, therefore, slower program startup
   * identical function folding can only partly undo this harm and will only reduce the execution overhead on startup and memory

 - Elaboration checks/implicit infinite predefines
 
 - test concept built into the language
 
 - nocompile tests
   * allow to ensure that something is not possible, e.g. not accessible from other packages

 - C API (extern realization)
 
 - Sourcecode and string literals are UTF-8-encoded and have \n line endings



## Modular programming (libraries, scopes):

 - Version guard (tl/TIR Package hash)
 
 - Compile against compiled libraries (Ext Refs, this/ext state)
   * for a user, this is like Java's JARs
   * in fact, this is a lot more and more efficient, because ExtRefs use OGSS IDs into dependent tl
   * Smaller tl-files
   * No classloader, RT-check required
   * ODR is checked by the compiler instead of C++'s user-imposed checking
   * Macros, templates and consistency
   * Templates are not re-instantiated if an instance exists in a dependency

 - Language defined package/build system (draupnir)
  * required to scale
  * fix issues with packaging in C++ and Java
   
 - Library scopes (e.g. tyr.lang)
  * ensure that library contributions do not conflict
  * make clear who contributes what in projects with a lot of dependencies
  
 - Implicit directory-based subscoping (e.g. tyr.lang.operator)
  
 - Scoped imports (with)
  * Allow sources and top level entities to use members without having to fully qualify names
  * Basically C++'s using/using namespace
  
 - Default visibility is library-private
  * public without exports in Java 9+
 
 - Public visibility exports entities to other libraries
  * dlexports in C++, exports in Java 9+



## Structured programming
   
 - Statements are expressions

 - blocks are expressions
  * massive help in concise high performance programming
  
 - if else
  * standard ternary operator
  
 - switch int
  * standard switch without falthrough
  * fallthrough is dangerous and not required because we have OOP
  
 - while/do while
 
 - verbose multi line string literals

 - function pointers
 
 - default parameters
 
 - optional by-name parameter passing

 - basic CT code evaluation



## Generic programming (macros, templates, generics):

- λ-polymorphic templates over types and CT-literal values (int, bool, string)

- Felden-template type theory (TIR-substitutions/-updates)
 
- Compiler-enforced ODR
 
- Mutually recursive template definitions
  * Example: [tyr.lang.container.ArrayBuffer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/arrayBuffer.tyr) and [tyr.lang.container.Iterator](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/iterator.tyr)
 
- Transparent type alias definitions (e.g. tyr.lang.int)
  * Also in: C (typedef)
  * Example: [tyr.lang.int](https://github.com/tyr-lang/stdlib/blob/master/lang/src/integer.tyr#L112)

- Must inline (tyr.lang.inline)

- Block parameters (tyr.lang.Block)
  * Block parameters are the high-performance counter part of lambda expressions
  * Block parameters, in constrast to macros, can be understood by IDEs and humans

- Blockparameter parameters (tyr.lang.BlockParameter)
  * Block parameter parameters allow functions whose calls are realized by inlining to access the caller's local variables

- Generalized binder (f(..) .. do {..})
  * This is the 2020 version of for loops
  * It enables library authors to write high-quality container APIs without resorting to language magic
   
- Generalized binder variable type inference ("".forall c do {true})
 
- Explicit implicit conversions (tyr.lang.Proxy, def implicit)

- Type templates ([..])



## Object-oriented programming:

 - full lattice type theory
 
 - single inheritance between flat types of the same structure (type)
 
 - single inheritance between classes (class)
 
 - O(1) class member access, type check and cast
 
 - multiple inheritance of structural promises (interface <: Object)
 
 - interfaces can inherit classes
 
 - Types, classes and interfaces can have function and field members
 
 - classes and interfaces can have class members residing in their i/vtable
   * this is actually a very powerful and type-safe alternative to reflection
   * access cost is identical to icalls/vcalls
 
 - multiple inheritance of unrepresented data structures (interface <: any)

 - single level return type overload resolution and type inference

 - explicit this
 
 - vtable pointer set before constructor call on object allocation (new)
 
 - types have destructors to release their ressources (delete)
   * Most types have memory as their only ressource
   * Some types hold other ressources, like file handles, locks, server communication, state transitions. Not releaseing them can have desastrous consequnces. See Java's try-with-ressource and finalizers desaster.
 
 - Super constructor / destructor calls (T.new/T.delete)
 
 - local scope member access (geht das wirklich? habe ich da einen test)
 type Y {
   def f(this: T)
   test ""{ var x = new T; x.f() }
 }
 
 - explicit and implicit overrides
 
 - explicit methodslots
 * interface conflict resolution
 
 - scoped public/protected visibility (private[])
 
 - scoped protected visibility (protected



## Type-oriented programming:

 - All ground types are defined in Tyr code
  * any
 
 - Types are values / Type usages are expressions
  * Type.!!
  * Blocks yielding types
 
 - All types are instances of tyr.lang.Type



# Planned Features
 
 - [continuous] extended CT evaluation

 - [0.7] function templates
 
 - [0.7] Forall-polymorphic generic template parameters at least for Object subtypes
 
 - [0.7] Variant template arguments at least for Object subtypes
 
 - [likely 0.7] enum
 
 - [likely soon] tagged union
 
 - [likely soon] inline enum, inline type val (no RT representation)

 - [0.7] switch type / switch enum
 
 - [0.7] warning free / category-specific tests
 
 - [0.8] provable tests (CT evaluation + optimized to true)
 
 - [0.8] leak free tests

 - [1.0] optional checking of unchecked conversions in tests

 - [1.0] optional checking of unsound variance in tests, maybe also in production

 - [0.8] Exceptions, try-catch, try-finally
 
 - [0.8] Fatal exceptions
   * Every Java programmer likes catch blocks eating InterruptedExceptions. Not.

 - [likely 0.7] Java-style varargs or CT HLists argument types
 
 - [1.0] Type unions and intersections
 
 - [1.0] initializer elaboration order checks (check that RT values are not used before initialization)
 
 - [1.0] global initializer / shutdown hooks

 - [1.0] Project parameters, e.g. architecture

 - [1.x] Self-hosting compiler
 
 - [1.x] Static memory and ressource management via MAY ownership
   * If the compiler could know that memory descopes, it should also free it
   * note: Rust is a MUST ownership system which is something completely different



# Non-Features

 - Type safety in the sense of formal correctness of cast free code
   * We have a C API
   * Code is not free of casts
   * It will just encourage the usage of unchecked casts
   * Code should reflect the architecture and thoughts of programmers
   * Nonetheless, we will try our best not to introduce any unsound conversion
     that cannot be checked by changing the compiler backend.

 - Top-level data entities
 
 - Nested classes
 
 - Lambda expressions / Closures / Anonymous functions

 - GC / implicit RC for heap objects
   * Having a GC in a programming language is the most fundamental decision and it cannot be changed later on. When Tyr started in 2017, it was meant as a research project for the type-oriented programming paradigm. Hence, the name Tyr (TYpe oriented Research language). In such a setup, having a GC is not an option.
   * In practice, GC overhead is at least a factor of two. 
 
 - runtime Tyr code loading
   * Cannot be combined with O(1) OOP algorithms and a lot of OOP-related optimizations.
     This is because the exact type hierarchy is no longer known.
     Hence, one cannot, for instance, remove entities from itables or vtables that have only one realization in a given program.
   * is not really required in the cloud age
   * runtime code loading can cause very complex and expensive bugs, especially with ODR violations in C++ and Java
   * Still, even today one can use dlopen and load C or C++ code at runtime

 - JIT
   * if no runtime code loading exists, JIT is a bad idea in almost all cases
 
 - easy to use cheap reflection
