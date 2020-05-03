Effective Java 3rd

The Supplier interface is a functional interface that has a single method get(), which returns a type T object.

Chapter 5 Generics
Item 26: Don’t use raw types

This is because raw types don’t have any type information and they exist only for being backward compatible with pre-generics code.

A generic class or interface is a class or interface with one or more type parameters. Collectively, they are called generic types.

Each generic type defines a set of parameterised types.  For example, List<E> is a generic type, and List<String> is a parameterised type of List<E>.

The raw type is the generic type without any accompanying type parameters. The raw type of List<E> is List.

Subtyping rules - List<String> is not a subtype of List<Object>. List<String> is a subtype of List.

Use unbounded wildcard types when you don’t know or care what the actual type parameter is.

Item 27: Fix all unchecked warnings

``` java

allprojects {
    gradle.projectsEvaluated {
        tasks.withType(JavaCompile) {
            options.compilerArgs << "-Xlint:unchecked" << "-Xlint:deprecation"
        }
    }
}

```

Item 28: Prefer lists to arrays

Arrays are covariant. It means that String[] is a subtype of Object[] because String is a subtype of Object.
Generics are invariants. It means that List<String> is not a subtype of List<Object>.

Arrays are reified. Array elements are checked for types at runtime. Generics are implemented by erasure. The checks are only at compile time.

Item 29: Favour generic types

Heap pollution: The runtime type of an object doesn’t match its compile-time type.
Item 30: Favour generic methods

Chapter 7. Lambdas and Streams
Item 42: Prefer lambdas to anonymous classes
