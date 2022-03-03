This document is a short summary of what's already in the language.
It is meant to serve as a complete description rather than a sales document or a comparison with another programming language.
Also, it contains what is planned to get into the language.
Further, there is a short section of things that will never get into the language.
Lastly, all features have a short rationale and an example.


# Existing Features


## Core concepts:

- C-style Syntax
  * Everybody should be able to read Tyr code in order to learn from examples.
  * Note: the closest relative in terms of syntax is Scala
  * Also in: most modern programming and scripting languages
  * Example: [Fibonacci example](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/fibonacciFields/main.tyr)


- Static name and type binding
  * This is mandatory for some other concepts and most type-related features
  * If done well, this will always increase productivity in medium and large scale projects
  * It is the foundation of good IDEs and code analysis tools
  * Also in: almost all programming and some scripting languages
  * Example: [Cross pair swap](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/qswap/mar.tyr)


- Language-defined built-in types (tyr.lang.native)
  * Having as few special rules as possible allows library authors almost the flexibility of language authors
  * Having base constructs defined in Tyr code forces the compiler to create good code for everybody
  * Having implementation of base concepts like casts and type checks in Tyr code makes their cost transparent
  * Native functions can be realized as single CPU instruction where applicable and yield the same code as predefined operators in most C-style languages
  * Also in: some dynamic languages; Swift
  * Example: [bool](https://github.com/tyr-lang/stdlib/blob/master/lang/src/bool.tyr) and [Ref](https://github.com/tyr-lang/stdlib/blob/master/lang/src/pointer.tyr#L35)


- Language-defined operators/operator overloading (tyr.lang.operator)
  * Using commonly understood operator symbols results in a massive increase of conciseness and readability
  * Having this in the language allows library authors to introduce symbols that are understood in their domain
  * Type-specific symbols, precedence and associativity are a cure to C++'s abuse of unplausible operator symbols
  * Also in: Scala, Swift; many others
  * Example: [Integer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/integer.tyr)


- Call operators/Functors (apply/update)
  * Complex function-like types can be used like functions
  * Containers are partial functions and should be used like that
  * Also in: Scala; many others in related variants
  * Example: [Array usage](https://github.com/tyr-lang/test.conformance/blob/master/0.5.0/accept/arrayTest/mar.tyr), [Array implementation](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/array.tyr)


- Properties can modify entity definitions (e.g. tyr.lang.native or tyr.lang.operator.precedence)
  * The user can define arbitrary properties 
  * Currently, the only way of doing something with user-defined properties requires the creation of a tool working on .tl-files. This is pretty easy, as .tl-files are regular OGSS files holding TIR type instances.
  * Also in: C++ (attributes), Java (annotations); many others in related variants
  * Example: [Ref](https://github.com/tyr-lang/stdlib/blob/master/lang/src/pointer.tyr#L35)


- Semicolon inference and whitespace insensitivity
  * Improves productivity and readability if code is formatted anyway
  * Flexibility for project-specific coding guidelines
  * Flexibility for formatting of complex expressions like object initialization
  * Also in: Common in modern languages
  * Example: [Ref](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/iterable.tyr)


- Identifier literals
  * Allow code generators and migration tools to escape keywords
  * Straight-forward way of using constructor implementations as functions to re-initialize objects
  * Also in: Scala
  * Example: [Explicit constructor function call](https://github.com/tyr-lang/test.conformance/blob/d7524f0b08976f831ecbe980c48f82dcdccc202c/0.6.0/accept/superNewClassDirect/mar.tyr#L11)

 
- Typed integer, unsigned and float literals
  * In practice, representation of a numeric value matters as it affects precision or overflows
  * Having typed integer literals is more concise and readable than having a cast and braces on every integer literal when doing bit operations
  * This is required for strict forward-flow typing without implicit coercions
  * Also in: All C-style languages in a less excessive form
  * Example: [Literals](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/literals/mar.tyr)


- CT/RT-bracing ([], ())
  * Compile time and runtime function and type applications have different requirements and effects and should, hence, be distinguished
  * Using brackets instead of comparison operators improves readability and resolves ambiguities
  * Also in: Scala
  * Example: [Template type constructor call](https://github.com/tyr-lang/test.conformance/blob/master/0.5.0/accept/minTemplateNew/mar.tyr)

 
- Function representational transparency
  * All functions can be used like static functions of their implicit type. Even constructors and destructors
  * A dispatching function is still a function and should be usable like one
  * This saves memory in text segments, reduces copy and past programming and allows reuse of existing accessible implementations
  * Also in: Scripting languages
  * Example: [Explicit constructor function call](https://github.com/tyr-lang/test.conformance/blob/d7524f0b08976f831ecbe980c48f82dcdccc202c/0.6.0/accept/superNewClassDirect/mar.tyr#L11)

 
- Function placement realizations (def f := g)
  * Copying the same code over and over is expensive, increases maintenance cost and results in more and, therefore, slower program startup
  * Identical function folding can only partly undo this harm and will only reduce the execution overhead on startup and memory
  * Furthermore, this allows elegant and concise resolution of inheritance conflicts
  * Also in: (none known)
  * Example: [ArrayBuffer append](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/container/arrayBuffer.tyr#L60) and [Override conflict resolution](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/overrideKeep/mar.tyr)


- Per entity elaboration checks/implicit infinite predefines
  * Elaboration allows a compiler to run multiple phases at the same time and keep track of the progress per entity
  * Elaboration checks are required for expressiveness, inference and recursive templates
  * Many languages do this either top-down requiring predefines or by reduction of expressiveness
  * Note: together with compiled modules, this contributes about half of the implementation's complexity and code in semantic analysis
  * Also in: Ada (per package instead of per entity)
  * Example: [Iterator.toBuffer](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/container/iterator.tyr#L120)


- Test concept built into the language
  * Encourage test-driven development
  * Built-in tests can do things that test frameworks cannot, e.g. bypass access checks, noCompile tests
  * Tests are close to the implementation whilst testcode is automatically removed in production
  * Also in: (none known)
  * Example: [bool](https://github.com/tyr-lang/stdlib/blob/master/lang/src/bool.tyr)


- noCompile tests
  * Allow to ensure that something is not possible, e.g. not accessible from other packages
  * Can ensure that certain operations cannot be performed on unmodifiable views
  * Can ensure that certain implicit conversions are not possible
  * Also in: (none known)
  * Example: [Unmodifiable](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/arithmetic/mar.tyr), [inaccessible](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/privateExt/mar.tyr), [inapplicable](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/castClass/mar.tyr)


- C API / extern realization
  * C is the lingua franca of programming
  * Allow access to C data and functions under Tyr conventions
  * Also in: Ada (pragma import); almost all languages in various forms
  * Example: [Overloaded OpenGL API](https://github.com/feldentm/thyGL/blob/891b248b75f0f98c13a2395e34e5587efbf086a4/src/gl.tyr#L62), [OpenGL integration](https://github.com/feldentm/thyGL/blob/891b248b75f0f98c13a2395e34e5587efbf086a4/package.draupnir#L7)


- Sourcecode and string literals are UTF-8-encoded and have \n line endings
  * This is required for deterministic builds if bytes are read from string literals
  * Massive simplification of code generation
  * This is exactly what you need in cloud programming
  * Also in: common in modern languages
  * Example: [Strings](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/strings/mar.tyr), [UTF-8](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/stringEncoding/mar.tyr)



## Modular programming (libraries, scopes):

- Version guard (tl/TIR Package hash)
  * Compiled Tyr libraries contain hashes preventing accidental combination of incompatible dependency versions
  * Prerequisite for compiler-enforced ODR preventing all ODR-related bugs
  * This is a must have for large scale projects
  * Also in: (none known); this is related to having only specific versions in package systems like maven
  * Example: (none; abuse results in an error message)

 
- Compile against compiled libraries (Ext Refs, this/ext state)
  * Smaller .tl-files, no reprocessing of code or declarations
  * Faster compilation as loading .tl-files is a lot faster than parsing equivalent headers or sources
  * For a user, this is like Java's JARs. However, this is a lot more and more efficient, because ExtRefs use OGSS IDs into dependent tl
  * No classloader or RT-check required
  * ODR is checked by the compiler instead of C++'s user-imposed checking; if it compiles, it is also consistent
  * Templates are not re-instantiated if an instance exists in a dependency
  * Also in: Java is loosely related; some big inhale compilers/code analysers
  * Example: [project](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/test3/package.draupnir) and [code](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/test3/main.tyr)


- Language defined package/build system (draupnir)
  * Massive productivity boost compared to Make or shell scripts
  * Required for a module concept in a language definition
  * Required to scale
  * Fix issues with packaging in C++ and Java
  * Note: There is a difference between there is one or more build systems for a programming language and the build system is defined by the language authors
  * Also in: GNAT/Ada, cargo/Rust; many others
  * Example: [project](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/test3/package.draupnir)


- Library scopes
  * Ensure that library contributions do not conflict
  * Make clear who contributes what in projects with a lot of dependencies
  * Imports reduce the coding overhead to a minimum
  * Also in: Non-legacy Java 9 with modules and a module name is package name convention
  * Example: [project](https://github.com/feldentm/thySDL/blob/master/package.draupnir) and [code](https://github.com/feldentm/thySDL/blob/master/src/sdl.tyr)


- Implicit directory-based subscoping
  * Force code repositories into a clean structure
  * Helps large-scale software development
  * Also in: Java with IDE support
  * Example: [tyr.lang.container](https://github.com/tyr-lang/stdlib/tree/master/lang/src/container)


- Scoped imports (with)
  * Allow sources and top level entities to use members without having to fully qualify names
  * Also in: C++'s using/using namespace
  * Example: [file](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/importSimple/mar.tyr), [type](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/importsInner/mar.tyr)


- Default visibility is library-private
  * If no visibility is defined, the entity can be used within the same library
  * This allows grouping library-internal code into useful packages without being forced to publish internal API or introduce a friend concept
  * Also in: public without exports in Java 9+
  * Example: [Tyr runtime type IDs](https://github.com/tyr-lang/stdlib/blob/master/lang/src/class.tyr)


- Public visibility exports entities to other libraries
  * A clear public API of a library results in low global maintenance cost and improved scalability of development
  * Also in: dlexports in C++, exports in Java 9+; many others
  * Example: [runtime type checks and casts](https://github.com/tyr-lang/stdlib/blob/master/lang/src/class.tyr)



## Structured programming
   
- Statements are expressions
  * Most statements have a last operation's result that can serve as result of the statement
  * Tyr, in fact, has types to cover all potential results, even for return statements
  * Redruces need to predefine variables or to create variables just to move single values to the next statement
  * Also in: many others
  * Example: [Complex operations in single expression tests](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/stringBytes/mar.tyr)


- Blocks are expressions
  * Massive help in concise high performance programming
  * Often an alternative to single call site utility functions
  * Also in: gnu C++; many others
  * Example: [Free after evaluation](https://github.com/tyr-lang/test.conformance/blob/master/0.5.0/accept/objectCreationEffectless/mar.tyr); [syntactic blocks are simple expressions](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/statements/main.tyr)


- if else
  * Standard ternary operator or switch bool
  * Also in: almost all others
  * Example: [bool.&&](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/bool.tyr#L17)


- switch int
  * Standard switch without falthrough
  * Fallthrough is dangerous and not required because we have OOP
  * Also in: most others
  * Example: [Switch with templates and return](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/switchReturn/main.tyr)


- while/do while
  * Standard loops; while is void, do while yields the last iteration's result
  * Also in: most others
  * Example: [Simple loops](https://github.com/tyr-lang/test.conformance/blob/d7524f0b08976f831ecbe980c48f82dcdccc202c/0.6.0/accept/semicolonInference/mar.tyr#L46)


- Verbose multi line string literals
  * Easy way of entering text to be shown to the user
  * Also in: most modern languages; sometimes with escaping or interpolation
  * Example: [Strings](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/strings/mar.tyr)


- Function pointers
  * standard concept
  * Also in: all system programming languages
  * Example: [local pointer](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/applicableObjectMemberVar/mar.tyr)


- Default parameters
  * Improve usability of APIs with rarely used options
  * Note: This makes overload resolution a lot more complex. It is, therefore, uncommon in languages with overload resolution
  * Also in: Ada, C++; many others
  * Example: [default parameters](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/byNameOrder3/mar.tyr)


- Optional by-name parameter passing
  * Increase robustness and readability of calls taking several parameters
  * In compiled languages like Tyr, it has better runtime performance than builder pattern or passing a struct
  * Note: This makes overload resolution and the function type theory more complex
  * Also in: Ada, R; most scripting languages
  * Example: [by-name parameters](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/byNameOrder/mar.tyr)


- Basic CT code evaluation
  * Evaluating code in the compiler results in faster runtime and increases expressiveness
  * Note: In combination with type inference, this can lead to errors in seemingly unchanged parts of the program
  * Note: This requires a lot of code in the compiler
  * Also in: All programming languages with optimizing compilers
  * Example: [Literal from unary minus](https://github.com/tyr-lang/test.conformance/blob/d7524f0b08976f831ecbe980c48f82dcdccc202c/0.4.0/accept/switch/mar.tyr#L47)



## Generic programming (macros, templates, generics):

- λ-polymorphic templates over types and CT-literal values (int, bool, string)
  * Proven efficient abstraction mechanism
  * Also in: Ada, C++
  * Example: [Integer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/integer.tyr)


- Felden-template type theory (TIR-substitutions/-updates)
  * Realization of λ-polymorphicm over substitutions in a folded Herbrand-Interpretation of the underlying Tyr code allows a straight-forward and type-safe realization of mutully recursive template definitions
  * The mechanism can be used to have template definitions and template instances in a fully compiled form in the IR
  * The machanism can be reused to realize generalized binders
  * The resulting instance is fully typed and can be reused saving the need for subsequent identical template instanciations
  * Enables structured linking instead of string-based linking
  * Note: this implies that template parameters are typed and bound on definition (in contrast to on instantiation as in C++)
  * Also in: none, but could be used to implement Ada generics
  * Example: [ArrayBuffer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/arrayBuffer.tyr) <-> [Iterator](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/iterator.tyr)


- Compiler-enforced ODR
  * In a dependency diamond, two equivalent instances of a template can be visible. Linking them will result in only one surviving as there can be only one definition (ODR). If the two instances resulted out of different versions of a template, this will lead to hard to diagnose bugs. Hence, we let the compiler ensure that this cannot happen.
  * This is a requirement for ExtRefs in TIR, i.e. refering to entities in dependencies by ID instead of by Linkname
  * Required in large-scale projects
  * Also in: (none known)
  * Example: (none)


- Mutually recursive template definitions
  * Template types can use each other and provide functions to convert between one another
  * This is a prerequisite for a modern container library
  * Also in: none known; common in generic languages with global type inference
  * Example: [tyr.lang.container.ArrayBuffer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/arrayBuffer.tyr) and [tyr.lang.container.Iterator](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/iterator.tyr)


- Transparent type alias definitions
  * Allow common template instances to be provided under a shorter name
  * Also in: C (typedef)
  * Example: [tyr.lang.int](https://github.com/tyr-lang/stdlib/blob/master/lang/src/integer.tyr#L112)


- Must inline (tyr.lang.inline)
  * A function that will always be inlined to the caller is closely related to typed macros
  * This can improve performance for functions known to cost less than the call overhead
  * In contrast to macros, Tyr inline functions can have static overloading allowing type-based optimized algorithms to be used
  * Note: This is not related to inline optimization (may inline) which is provided by the backend
  * Note: In Tyr, any function with a tyr.lang.Block or tyr.lang.BlockParameter parameter is implicitly must inline. This is because a Block has no explicit runtime representation.
  * Also in: many programming languages
  * Example: [Object.getClass](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/object.tyr#L20)


- Block parameters (tyr.lang.Block)
  * Block parameters are the high-performance counter part of lambda expressions
  * Block parameters, in constrast to macros, can be understood by IDEs and humans
  * Block parameters are inserted by an inline call that will insert a copy into the called function at each call to eval()
  * Also in: none known; to some extend Smalltalk-style languages but without static typing and naming
  * Example: [bool.&&](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/bool.tyr#L15)


- BlockParameter parameters (tyr.lang.BlockParameter)
  * Block parameter parameters allow functions whose calls are realized by inlining to access the caller's local variables
  * This creates a communication channel with an inline function caller without breaking the function abstraction. The channel is bidirectional as the inline function can read and update the variable and the caller can bind the variable in passed Block instances
  * This is loosely related to closures and lambda parameters
  * Example: [Iterator.foreach](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/container/iterator.tyr#L40), [ArrayBuffer.appendAll](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/container/arrayBuffer.tyr#L79)


- Generalized binder (f(..) .. do {..})
  * Generalized binder are a generalization of the unary binding done by generalized for loops. The generalization allows usage of arbitrary functions, arbitrary number of bound variables and bound blocks.
  * It enables library authors to write high-quality container APIs without resorting to language magic
  * Note: A surprising hell lot of code is required to implement this correctly in the compiler
  * Also in: loosely related to generalized for loops in Scala or generalized quantifiers in some theorem provers like IVIL
  * Example: [fold](https://github.com/tyr-lang/test.conformance/blob/master/0.4.0/accept/generalizedBinder/main.tyr)

 
- Generalized binder variable type inference ("".forall c do {true})
  * If the binder is not overloaded, the compiler can infer bound variable types helping conciseness and readability
  * Also in: none; closely related to lambda expression parameter inference
  * Example: [Un-/Buffered forall](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/arrayToBuffer/mar.tyr)


- Explicit implicit conversions (tyr.lang.Proxy, def implicit)
  * The language has no implicit conversions to keep the language definition clean and understandable
  * As implicit conversions are required for concise code, there are two mechanisms that allow library authors to add implicit conversions. A function implicit accessible via closed lookup with a single parameter is used to perform implicit conversions. If a type has a Proxy property, failed closed lookups will continue in the Proxy's argument type. Usually, one or more implicits are paired with a proxy property.
  * Also in: many others; usually in a form where implicit implies Proxy
  * Example: [References](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/pointer.tyr#L35), [delegates](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/groovyDelegate/mar.tyr)


- Type templates ([..])
  * Common way to realize λ-polymorphism
  * Also in: C++; many others
  * Example: [Integer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/integer.tyr)



## Object-oriented programming:

- Full lattice type theory
  * Straight-forward implementation of a lot of type operations
  * Join and meet are functions
  * Increased expressiveness
  * Note: The top type (tyr.lang.any) cannot have static or dynamic runtime instances as it has no representation
  * Also in: Scala; no systems programming language known
  * Example: [FlatArray](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/flatArray.tyr)


- Single inheritance between flat types of the same structure (type)
  * In Tyr, direct subtypes of any define the in-memory representation of a type instance
  * Types sharing the same representation may still be extended with further or specialized operations
  * Also in: Ada; many others
  * Example: [pointer, Ref, observablePointer](https://github.com/tyr-lang/stdlib/blob/master/lang/src/pointer.tyr)


- Single inheritance between classes (class)
  * Common efficient form of runtime <=-polymorphism
  * Also in: Java; many others
  * Example: [Array](https://github.com/tyr-lang/stdlib/blob/master/lang/src/container/array.tyr)


- O(1) class member access, type check and cast
  * Encourage correct abstraction
  * Deep class hierarchies impose no runtime penalty
  * Note: The current algorithms require link time constant creation and prevent runtime code loading
  * Also in: none known; member access is usually constant whereas checks and casts are eC/O(n) in the height of the inheritance chain
  * Example: [instanceof](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/class.tyr#L92)


- Multiple inheritance of structural promises (interface <: Object)
  * Sometimes, single inheritance just doesn't reflect the problem to be solved
  * This is rarely affecting hot code. Hence, the TCO-optimal realization is to allow as much abstraction as possible and pay a little extra price on member access and type checks if the target object's representation is unknown
  * Interfaces can have both, fields and functions. And, functions can have implementations.
  * Also in: Scala; many others in slightly different forms
  * Example: [Example](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/overrideMulti/mar.tyr)


- Interfaces can inherit classes
  * Allowing interfaces to implement classes can be used to reduce the applicability of interfaces to an adequate subset of the class hierarchy
  * Using the known super class member representation results in much more efficient generated code
  * Also in: Scala except for the code generation
  * Example: [Example](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/interfaceSuperClass/mar.tyr)


- Types, classes and interfaces can have function and field members
  * Allow full flexibility for any kind of member access
  * Also in: C++ multiple inheritance and structs, although representation is different
  * Example: [Example](https://github.com/tyr-lang/test.conformance/blob/master/0.5.0/accept/interfaceFieldOverride/mar.tyr)


- Classes and interfaces can have class members residing in their i/vtable
  * This is actually a very powerful and type-safe alternative to reflection
  * Access cost is identical to icalls/vcalls of object functions
  * Also in: Most scripting languages with meta tables but without static typing
  * Example: [Shared human-readable names](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/classFieldDynamicVar/mar.tyr)
 
- Multiple inheritance of unrepresented data structures (interface <: any)
  * Extension of code reuse for common properties to flat types
  * Intended to be used to define the required structure of template arguments
  * Also in: Ada generics requirements; some others in various forms
  * Example: [Bottom types](https://github.com/tyr-lang/stdlib/blob/master/lang/src/nothing.tyr)


- Single level return type overload resolution and type inference
  * Overload resolution is required for expressive and readable APIs
  * Type inference increases productivity and readability of code
  * Single level inference prevents situations where complex equations affect the legality of code
  * Single level inference is closest to what users do when they read code
  * Single level inference is closely related to what type checkers do in languages without inference
  * Also in: Some others with slightly different rules and restrictions, e.g. Java(var), C++(auto)
  * Example: (none required)


- Explicit this
  * Allow creation of APIs on Types that can be converted implicitly to the defining type such as pointers, proxies or boxes
  * Can be useed to prevent mutating operations on flat and constant objects
  * Also in: Ada
  * Example: [Integer.++](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/integer.tyr#L11)


- VTable pointer set before constructor call on object allocation (new)
  * Allow members to be used on semi-constructed objects to increase code reuse
  * Also in: Java
  * Example: (none required)
 
- Types have destructors to release their ressources (delete)
  * Most types have memory as their only ressource
  * Some types hold other ressources, like file handles, locks, server communication, state transitions. Not releaseing them can have desastrous consequnces. See Java's try-with-ressource and finalizers desaster.
  * Also in: C++
  * Example: [Iterator.toBuffer](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/container/iterator.tyr#L120)


- Super constructor / destructor calls (T.new/T.delete)
  * Syntax to call super constructors
  * In contrast to most languages, the call can be anywhere in the body. This leads to cleaner code and is equivalent to allowing function calls before evaluation of the super constructor call
  * Also in: Any OOP language
  * Example: (none required)


- Shortcut field initializer (new(f := x))
  * Shortcut field initializer syntax leads to very short constructor implementations
  * Also in: Scala
  * Example: [qswap](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/qswap/mar.tyr)


- Explicit and implicit overrides
  * Like in most OOP languages, overrides can happen explicitly or implicitly
  * To make overrides safer, targets can be provided explicitly
  * Also in: none known allowing explicit targets; C++ is kind of related but on a per-type granularity instead of per-member
  * Example: [Override many](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/overrideMulti/mar.tyr)


- Explicit methodslots
  * Interface conflict resolution requires that explicit method slots can be assigned if identical inherited signatures belong to functions with different semantics. Although this is a rare case, it may result in very expensive fixes requiring proxy objects and mappings
  * Also in: (none known)
  * Example: [Explicit override](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/overrideKeep/mar.tyr)


- Scoped private/protected visibility (private[] protected[])
  * Private determines in which packages an entity can be directly accessed
  * In contrast to other languages, protected determines who can override access a member
  * Also in: Extension of Scala's accessibility
  * Example: [Access in library, override in package](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/protectedPrivateA/a/mar.tyr)


- Inherited visibility (override)
  * Override is also a visibility and implicitly allows anybody who can access an overridden entity access to the target entitiy
  * Prevent useless "cannot reduce visibility" errors by enforcing the correct behaviour
  * Note: This can lead to visibilities that cannot be expressed otherwise for multi inheritance
  * Example: (none required)



## Type-oriented programming:

- All ground types are defined in Tyr code
  * Saves code in the compiler and make language semantics transparent in the standard library
  * Also in: Common in modern programming languages
  * Example: (none required)
 
- Types are values / Type usages are expressions
  * The original research question behind Tyr was to check if a programming language can exist in which types are treated like any other value
  * It turns out that this is easily possible if anticipated from the very beginning. Still, there are limits to the usefulness of this
  * Although currently not implemented, it can be extended to RT instances of Type. This is currently not done because Types have no runtime properties. Hence, there would be overhead without benefit. CT usage of Type instances has certainly various applications.
  * Also in: none known; usually, types have syntax different from expressions and statements
  * Example: [Type.!!](https://github.com/tyr-lang/stdlib/blob/c15296504c2eedf2e7344ca617d90eb0637c9878/lang/src/type.tyr#L43), [cast](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/castType/mar.tyr), [type from local CFG](https://github.com/tyr-lang/test.conformance/blob/master/0.6.0/accept/topVar/mar.tyr)


- All types are instances of tyr.lang.Type/Class/Interface
  * The obvious way to type type instances
  * Runtime-represented types are instances of subclasses to reflect their representation
  * Also in: Java (Class)
  * Example: (none required)



# Planned Features
 
- [continuous] extended CT evaluation
  * likely an unlimited endevour

- [0.7] function templates
  * planned from the beginning

- [0.7] Forall-polymorphic generic template parameters at least for Object subtypes
  * the last research question exected to be answered in the forseeable future

- [0.7] Variant template arguments at least for Object subtypes
  * required by some special types

- [likely 0.7] Java-style varargs or CT HLists argument types; any varargs
  * required to build some APIs
  * any varargs should reflect C varargs

- [likely 0.7] enum
  * required for good C APIs

- [likely soon] tagged union
  * required for good C APIs

- [likely soon] inline enum, inline type val (no RT representation)
  * nice to have code quality feature

- [0.7] switch type / switch enum
  * must have for case handling
  * like Visitors, it can reduce linear switching costs to constant time, but requires no Visitor base type to have been declared upfront

- [0.7] warning free / category-specific tests
  * increase precision of tests by reducing expected messages to certain categories
  * most tests should not even create a warning; some should create a specific warning

- [0.8] provable tests (CT evaluation + optimized to true)
  * a lot of high performance abstractions should be optimized out by the compiler at least for base cases
  * required to assert that certain properties depend only on compile time constants

- [0.8] leak free tests
  * most test should not cause memory leaks

- [1.0] optional checking of unchecked conversions in tests
  * unchecked conversions could be checked if the code generator would emit explicit checks
  * these checks are expensive and, hence, not useful in production
  * these checks can detect programming errors


- [1.0] optional checking of unsound variance in tests, maybe also in production
  * unsound variance could be checked if the code generator would emit explicit checks for parameter/field access
  * these checks are expensive and, hence, usually not useful in production
  * these checks can detect programming errors


- [0.8] Exceptions, try-catch, try-finally
  * Exceptions are a proven error propagation mechanism
 
- [0.8] Fatal exceptions
  * Some errors cannot be resolved and must result in orderly program termination
 
- [1.0] Type unions and intersections
  * Required for precise typing of e.g. if else or switch results
 
- [1.0] initializer elaboration order checks (check that RT values are not used before initialization)
  * Value initialization should be checked akin to entity initialization
  * Note: this is more complex, because the global CFG has to be analyzed correctly
  * Note: this can also be implemented as external checker
 
- [1.0] global initializer / shutdown hooks
  * Useful applications in IO and other ressource management e.g. shutdown of global connection pools, termination of child proceses on crash

- [1.0] Project parameters, e.g. architecture
  * Required for platform and architecture specific code

- [1.0] Template type parameter inference
  * nice to have productivity feature
  * note: ambiguities to herbrand types must be investigated; may require new syntax for herbrand type access

- [1.x] Self-hosting compiler
  * must have display of productive usability

- [1.x] Static memory and ressource management via MAY ownership
  * If the compiler could know that memory descopes, it should also free it
  * note: Rust is a MUST ownership system which is something completely different



# Non-Features

- Semantics-based parsing
  * Parsing is a separate phase which is required to build good IDEs


- Type safety in the sense of formal correctness of cast free code
  * We have a C API
  * Code is not free of casts
  * It will just encourage the usage of unchecked casts
  * Code should reflect the architecture and thoughts of programmers
  * Nonetheless, we will try our best not to introduce any unsound conversion
    that cannot be checked by changing the compiler backend.


- Top-level data entities
  * Harmful for tool support and IDEs
  * Harmful for high-quality error messages and quickfix proposals
  * type var/vals are a good alternative


- Nested classes
  * Harmful for tool support and IDEs
  * Harmful for high-quality error messages and quickfix proposals


- Lambda expressions / Closures / Anonymous functions
  * Harmful for high-quality and high-performance code
  * Harmful for tool support and IDEs
  * Generalized binders and function objects are good alternatives


- GC / implicit RC for heap objects
  * Having a GC in a programming language is the most fundamental decision and it cannot be changed later on. When Tyr started in 2017, it was meant as a research project for the type-oriented programming paradigm. Hence, the name Tyr (TYpe-oriented Research language). In such a setup, having a GC is not an option.
  * In practice, GC overhead is at least a factor of two
  * In practice, GC does not prevent memory leaks


- Runtime Tyr code loading
  * Cannot be combined with O(1) OOP algorithms and a lot of OOP-related optimizations.
    This is because the exact type hierarchy is no longer known.
    Hence, one cannot, for instance, remove entities from itables or vtables that have only one realization in a given program.
  * is not really required in the cloud age
  * runtime code loading can cause very complex and expensive bugs, especially with ODR violations in C++ and Java
  * Still, even today one can use dlopen and load C or C++ code at runtime


- Just-In-Time compilation
  * If no runtime code loading exists, JIT is a bad idea in almost all cases


- Easy-to-use cheap reflection
  * Allows code styles that harm scalability and performance
  * Class members are a good alternative
