[![Java CI with Maven](https://github.com/coolsamson7/mappit/actions/workflows/maven.yml/badge.svg)](https://github.com/coolsamson7/mappit/actions/workflows/maven.yml)
# mappit

`Mappit` is a pure Kotlin object mapper framework used to avoid boilerplate and error prone manual mapping code vastly simplifying the implementation of typical use-cases ( DTO/Entity conversions, etc. )

The basic idea is to rely on a declarative approach to specify the mapping logic. In contrast to various other - mostly java based - implementations, it is completely non-invasive and does not require any changes to the affected classes ( e.g. by adding annotations ).

The other unique detail - at least i havn't found it at a glance - is, that it supports immutable classes, which write properties via val constructor arguments only.

Let's look at a simple example:

Assuming classes `ProductEntity`, `ProductTO`, `PartEntity`, `PartTO` and an immutable data class `Money`, the following DSL like call would specify the required operations

```kotlin
val mapper = mapper {
    mapping(ProductEntity::class, ProductTO::class) {
        map { "priceCurrency" to path("price", "currency") }
        map { "priceValue" to path("price", "value") }
        map { "parts" to "parts" deep true }
        map { matchingProperties() except properties("parts") }
     }

    mapping(PartEntity::class, PartTO::class) {
        map { matchingProperties() }
     }
}
```
and could be used by calling one of the various map methods, such as

```kotlin
val result = mapper.map<ProductTO>(productEntity)
```

# Features

Implemented features are

* fully type-safe. All operations are verified according to the involved types
* with the help of lambdas and infix operators a quite readable DSL like spec
* one-to-one and wild-card mappings
* integration of constant values as sources
* mapping of paths as source or target operations
* handling of immutable classes ( by collecting constructor arguments )
* support of deep mappings, including handling of cycles
* mapping of different collection types in each other ( list, array, ... )
* inheritance of mapping definitions
* automatic conversions of the different primitive types ( short, float, etc. )
* manual conversions inside a mapping or as a general rule in a mapper

# Performance

As this topic is often discussed, Mappit is pretty FAST!

All operations are translated - with the help of javassist - to plain Java-Code which enables performance near manual code...

Let's take a look at the details

Check the corresponding Wiki https://github.com/coolsamson7/mappit/wiki
