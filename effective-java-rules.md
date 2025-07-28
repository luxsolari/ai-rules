# The Complete "Effective Java" Ruleset for AI-Assisted Development

A comprehensive guide based on all 90 items in Joshua Bloch's "Effective Java, 3rd Edition."

## Chapter 2: Creating and Destroying Objects

*   **Item 1: Consider static factory methods instead of constructors.** They have names, are not required to create a new object each time, can return an object of any subtype, and can vary the returned object's class based on input.
*   **Item 2: Consider a builder when faced with many constructor parameters.** The Builder pattern is more readable and safer than telescoping constructors or JavaBeans for objects with numerous parameters.
*   **Item 3: Enforce the singleton property with a private constructor or an enum type.** A single-element enum is often the best way to implement a singleton, as it's concise and protects against serialization and reflection issues.
*   **Item 4: Enforce noninstantiability with a private constructor.** For utility classes that are just groupings of static methods and fields, a private constructor prevents instantiation.
*   **Item 5: Prefer dependency injection to hardwiring resources.** Classes should not create their own dependencies. Instead, pass resources (or factories) into the constructor to improve flexibility, reusability, and testability.
*   **Item 6: Avoid creating unnecessary objects.** Reuse objects whenever possible, especially immutable ones. Avoid unintentional object creation like autoboxing in loops.
*   **Item 7: Eliminate obsolete object references.** Null out references once they are no longer needed to prevent memory leaks, especially in classes that manage their own memory, like caches or stacks.
*   **Item 8: Avoid finalizers and cleaners.** They are unpredictable, slow, and dangerous. Use `try-with-resources` and implement `AutoCloseable` for resource cleanup instead.
*   **Item 9: Prefer `try-with-resources` to `try-finally`.** It's more concise, readable, and provides better diagnostics by not suppressing exceptions.

## Chapter 3: Methods Common to All Objects

*   **Item 10: Obey the general contract when overriding `equals`.** Ensure `equals` is reflexive, symmetric, transitive, consistent, and handles nulls correctly.
*   **Item 11: Always override `hashCode` when you override `equals`.** Equal objects must have equal hash codes to ensure they function correctly in hash-based collections like `HashMap` and `HashSet`.
*   **Item 12: Always override `toString`.** Providing a good `toString` implementation makes your class more pleasant to use and easier to debug.
*   **Item 13: Override `clone` judiciously.** The `Cloneable` interface is flawed. Prefer copy constructors or copy factories for duplicating objects.
*   **Item 14: Consider implementing `Comparable`.** Implement `Comparable` for value classes with a natural ordering to allow them to be easily sorted and used in comparison-based collections.

## Chapter 4: Classes and Interfaces

*   **Item 15: Minimize the accessibility of classes and members.** Make classes and members as private as possible to enforce encapsulation and create loosely coupled components.
*   **Item 16: In public classes, use accessor methods, not public fields.** Exposing fields directly gives up the ability to enforce invariants, take action on modification, and maintain thread safety.
*   **Item 17: Minimize mutability.** Strive to make classes immutable. They are simpler, thread-safe, and can be shared freely. If not possible, limit mutability as much as you can.
*   **Item 18: Favor composition over inheritance.** Inheritance violates encapsulation. Use composition and forwarding to reuse code from existing classes, which is more robust and flexible.
*   **Item 19: Design and document for inheritance or else prohibit it.** If a class is not designed for subclassing, prevent it by making it `final` or making its constructors non-public.
*   **Item 20: Prefer interfaces to abstract classes.** Interfaces are more flexible, allowing for mixins and non-hierarchical type frameworks. Use abstract classes only when there's a strong "is-a" relationship and you need to share implementation code.
*   **Item 21: Design interfaces for posterity.** It is very difficult to change a public interface once it has been released. Use default methods with caution, as they aren't a panacea for all interface evolution problems.
*   **Item 22: Use interfaces only to define types.** Do not use interfaces as a way to export constants (the "constant interface" anti-pattern). Use enums or non-instantiable utility classes instead.
*   **Item 23: Prefer class hierarchies to tagged classes.** Tagged classes (classes with a `type` field that determines behavior) are verbose, error-prone, and inefficient. Use subclasses to represent different flavors of an object.
*   **Item 24: Favor static member classes over nonstatic.** Non-static member classes (inner classes) have an implicit reference to their enclosing instance, which can cause memory leaks. Make member classes `static` if they don't need access to the enclosing instance.
*   **Item 25: Limit source files to a single top-level class.** Placing multiple top-level classes in one file can lead to confusing build dependencies and behavior.

## Chapter 5: Generics

*   **Item 26: Don’t use raw types.** Raw types (like `List` instead of `List<String>`) subvert the type system and should not be used in new code.
*   **Item 27: Eliminate unchecked warnings.** Pay attention to and eliminate every unchecked warning. If you can prove the code is safe, suppress the warning with an `@SuppressWarnings("unchecked")` annotation on the smallest possible scope.
*   **Item 28: Prefer lists to arrays.** Generics and arrays don't mix well. Lists are fully type-safe, while arrays are not.
*   **Item 29: Favor generic types.** When you design new types, make them generic so they can be used more safely and flexibly by clients.
*   **Item 30: Favor generic methods.** Generic methods, like generic types, are safer and easier to use than methods that require clients to cast.
*   **Item 31: Use bounded wildcards to increase API flexibility.** Use wildcards (`? extends T` and `? super T`) on input parameters that are producers or consumers to make your APIs more flexible (PECS: Producer-Extends, Consumer-Super).
*   **Item 32: Combine generics and varargs judiciously.** Varargs methods can be unsafe with generic types due to array creation. Use `@SafeVarargs` only on methods that are proven to be type-safe.
*   **Item 33: Consider typesafe heterogeneous containers.** For containers where you want to store different types, parameterize the key rather than the container, using `Class<T>` as a type token.

## Chapter 6: Enums and Annotations

*   **Item 34: Use enums instead of `int` constants.** Enums are more type-safe, readable, and powerful than integer constants.
*   **Item 35: Use instance fields instead of ordinals.** The `ordinal()` method is fragile and error-prone. Store data associated with an enum constant in an instance field.
*   **Item 36: Use `EnumSet` instead of bit fields.** `EnumSet` provides a type-safe, high-performance set implementation for enums.
*   **Item 37: Use `EnumMap` instead of ordinal indexing.** For mapping data to enums, `EnumMap` is more concise, safer, and generally as fast as array-based approaches.
*   **Item 38: Emulate extensible enums with interfaces.** While enums cannot be extended, you can create an interface that the enum implements, allowing clients to implement the same interface.
*   **Item 39: Prefer annotations to naming patterns.** Annotations provide a robust way to add metadata to program elements, superior to error-prone naming conventions.
*   **Item 40: Consistently use the `@Override` annotation.** Use `@Override` on every method that is intended to override a superclass declaration to prevent errors.
*   **Item 41: Use marker interfaces to define types.** A marker interface (like `Serializable`) defines a type, which gives you compile-time checking. Marker annotations do not. Use an interface if you want to define a type; use an annotation if you want to mark other program elements or use a framework that relies on annotations.

## Chapter 7: Lambdas and Streams

*   **Item 42: Prefer lambdas to anonymous classes.** Lambdas are more concise and less boilerplate-heavy than anonymous classes for implementing functional interfaces.
*   **Item 43: Prefer method references to lambdas.** When a lambda does nothing but call an existing method, use a method reference for even greater conciseness and clarity.
*   **Item 44: Favor the use of standard functional interfaces.** Use the standard functional interfaces in `java.util.function` whenever.
*   **Item 45: Use streams judiciously.** Streams are powerful but can be hard to read and debug if overused or used for complex logic. Refactor complex stream pipelines into helper methods.
*   **Item 46: Prefer side-effect-free functions in streams.** The functions you pass to stream operations should be pure functions—their result should depend only on their input, and they should not modify any external state.
*   **Item 47: Prefer `Collection` to `Stream` as a return type.** Returning a `Collection` is more flexible for your users, as it can be both iterated over and processed as a stream.
*   **Item 48: Use caution when making streams parallel.** Parallel streams can offer performance benefits, but only for certain data sources and operations. Always measure performance before and after to ensure it's a net win.

## Chapter 8: Methods

*   **Item 49: Check parameters for validity.** Document and enforce preconditions on your methods. Use Javadoc `@throws` to specify exceptions for invalid parameters.
*   **Item 50: Make defensive copies when needed.** If a class has mutable components that it gets from or returns to clients, it must defensively copy them to protect its invariants.
*   **Item 51: Design method signatures carefully.** Choose method names carefully, avoid long parameter lists, and favor interfaces over classes for parameter types.
*   **Item 52: Use overloading judiciously.** Avoid confusing overloadings, especially those with the same number of parameters, as the choice of which one to invoke can be surprising.
*   **Item 53: Use varargs judiciously.** Varargs can be convenient but can have performance costs and interact poorly with generics.
*   **Item 54: Return empty collections or arrays, not nulls.** Returning `null` forces clients to add boilerplate code to handle it. Returning an empty collection or array is cleaner.
*   **Item 55: Return `Optional`s judiciously.** `Optional` is a better way to represent a possibly-absent value than returning null. However, it should not be used for collections and can have a performance cost.
*   **Item 56: Write doc comments for all exposed API elements.** Thoroughly document every exported class, interface, constructor, method, and field using Javadoc.

## Chapter 9: General Programming

*   **Item 57: Minimize the scope of local variables.** Declare variables at the point of first use to improve readability and reduce errors.
*   **Item 58: Prefer for-each loops to traditional for loops.** The for-each loop is more readable, less error-prone, and works on any `Iterable`.
*   **Item 59: Know and use the libraries.** Don't reinvent the wheel. Familiarize yourself with and use the rich features of the Java standard library.
*   **Item 60: Avoid `float` and `double` if exact answers are required.** For calculations requiring exact results, like monetary calculations, use `BigDecimal`, `int`, or `long`.
*   **Item 61: Prefer primitive types to boxed primitives.** Boxed primitives can lead to subtle bugs (`==` comparison) and performance issues due to object creation and unboxing `NullPointerException`s.
*   **Item 62: Avoid strings where other types are more appropriate.** Don't use strings to represent numeric data, enums, or complex aggregate types.
*   **Item 63: Beware the performance of string concatenation.** Using `+` in a loop to concatenate strings has quadratic performance. Use `StringBuilder` instead.
*   **Item 64: Refer to objects by their interfaces.** Using an interface as the type for variables, parameters, and return values makes your code more flexible.
*   **Item 65: Prefer interfaces to reflection.** Reflection is powerful but loses compile-time type checking, is verbose, and is slow. Use it only when absolutely necessary.
*   **Item 66: Use native methods judiciously.** Native methods are rarely needed for performance and introduce platform-dependency and safety risks.
*   **Item 67: Optimize judiciously.** Don't optimize prematurely. Write a good, clean program first, then use a profiler to find and fix performance bottlenecks.
*   **Item 68: Adhere to generally accepted naming conventions.** Follow the standard Java naming conventions for packages, classes, methods, and fields to improve readability.

## Chapter 10: Exceptions

*   **Item 69: Use exceptions only for exceptional conditions.** Do not use exceptions for ordinary control flow.
*   **Item 70: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors.** Checked exceptions force the caller to handle the error, while runtime exceptions indicate a bug.
*   **Item 71: Avoid unnecessary use of checked exceptions.** Overuse of checked exceptions can make an API painful to use. Consider returning an `Optional` or an empty collection instead.
*   **Item 72: Favor the use of standard exceptions.** Reuse standard exceptions like `IllegalArgumentException` and `IllegalStateException` to make your API easier to learn and use.
*   **Item 73: Throw exceptions appropriate to the abstraction.** Catch lower-level exceptions and re-throw them as exceptions that are meaningful in the context of the higher-level abstraction (exception translation).
*   **Item 74: Document all exceptions thrown by each method.** Use the `@throws` Javadoc tag to document every checked and unchecked exception a method can throw.
*   **Item 75: Include failure-capture information in detail messages.** An exception's detail message should include the values of all parameters and fields that contributed to the failure.
*   **Item 76: Strive for failure atomicity.** A failed method invocation should leave the object in the state it was in before the invocation.
*   **Item 77: Don’t ignore exceptions.** An empty `catch` block is a recipe for disaster. At a minimum, log the exception.

## Chapter 11: Concurrency

*   **Item 78: Synchronize access to shared mutable data.** Synchronization is necessary to ensure that one thread doesn't see an object in an inconsistent state while another is modifying it, and to ensure visibility of changes across threads.
*   **Item 79: Avoid excessive synchronization.** Hold locks for the shortest time possible. Do not call alien methods (methods designed for extension or provided by clients) from within a synchronized block.
*   **Item 80: Prefer executors, tasks, and streams to threads.** The `java.util.concurrent` framework provides high-level utilities for managing concurrency that are safer and more powerful than working directly with `Thread`s.
*   **Item 81: Prefer concurrency utilities to `wait` and `notify`.** Use higher-level concurrency utilities like `CountDownLatch`, `Semaphore`, `CyclicBarrier`, and `BlockingQueue` instead of the error-prone `wait` and `notify` methods.
*   **Item 82: Document thread safety.** Clearly document the thread safety guarantees of your classes using annotations like `@Immutable`, `@ThreadSafe`, or `@NotThreadSafe`.
*   **Item 83: Use lazy initialization judiciously.** If you need lazy initialization for performance on a static field, use the lazy initialization holder class idiom. For instance fields, use the double-check idiom.
*   **Item 84: Don’t depend on the thread scheduler.** Any program that relies on the thread scheduler for correctness or performance is likely to be non-portable. Do not rely on `Thread.yield` or thread priorities.

## Chapter 12: Serialization

*   **Item 85: Prefer alternatives to Java serialization.** Serialization is fragile and dangerous. Prefer structured data representations like JSON or Protocol Buffers.
*   **Item 86: Implement `Serializable` with great caution.** Implementing `Serializable` is a huge commitment. It makes the private implementation details of your class part of its public API.
*   **Item 87: Consider using a custom serialized form.** Don't accept the default serialized form unless it's a reasonable description of the object's logical state.
*   **Item 88: Write `readObject` methods defensively.** When deserializing an object, assume the byte stream is malicious and make defensive copies of all mutable components.
*   **Item 90: Consider serialization proxies instead of serialized instances.** The serialization proxy pattern is a robust way to handle the challenges of serialization for complex objects.
*   **Item 89: For instance control, prefer enum types to `readResolve`.** The `readResolve` feature can be used to substitute another instance for the one created during deserialization, but enum types provide this guarantee effortlessly.
*   **Item 90: Consider serialization proxies instead of serialized instances.** The serialization proxy pattern is a robust way to handle the challenges of serialization for complex objects. 