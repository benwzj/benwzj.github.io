---
layout: post
title: Kotlin Annotation
date: 2024-07-04
category: Android
tags: Android Kotlin 
---

## What is Annotations

Annotations are means of attaching metadata to code. 

Annotations are a form of metadata that provide data about the program but are not part of the program itself. Annotations have **no direct effect on the operation** of the code they annotate. Instead, they are used by the compiler and various tools during the build process, and can also be accessed at runtime through reflection. 
Kotlin annotations can be applied to classes, functions, properties, property accessors, parameters, and constructors.

Kotlin comes with a set of built-in annotations.
You can define your own custom annotations.

There are three main mechanisms that handle annotations: 
- annotation processing, 
- reflection, and 
- lint. 

### Purposes

Annotations can be used for a wide range of purposes, from marking code as deprecated, to influencing how data is serialized, or even modifying the behavior of frameworks and libraries. 

- influencing the compiler’s behavior
- guiding the use of frameworks or libraries
- providing metadata for runtime reflection
- enforcing coding standards

### Built-in Annotations

Kotlin, like Java, comes with a set of built-in annotations. One commonly used annotation is `@Deprecated`, which marks a program element as deprecated. For example:
```kotlin
@Deprecated("This function will be removed in future releases.")
fun oldFunction() { }
```

### Declaring an custom Annotation
To declare an annotation, put the annotation modifier in front of a class:
```kotlin
annotation class Fancy
```

### Using Annotations

Once declared, this annotation can be used to annotate various program elements. 
For example:
```kotlin
@Fancy class MyClass { }
@Fancy fun myFunction() { }
```

### Features
- Support Annotation Parameters
- Java Interoperability
- Enhancing Code Readability and Maintainability: It better then comment in some circumstance, because it can, for example influencing the compiler’s behavior.
- Enforcing Coding Conventions and Safety, either at compile-time or runtime.
- Integration with Frameworks and Libraries: Many frameworks and libraries leverage annotations to allow developers to configure behavior or integrate custom logic seamlessly. By creating custom annotations, developers can extend these frameworks in powerful and flexible ways.
- Simplifying Configuration.

## Example

### Specify Annotation Targets
By default, a Kotlin annotation can be used on any declaration. However, you might want to restrict your annotation to certain types of declarations (e.g., functions, classes, properties). You can do this using the `@Target` meta-annotation.
```kotlin
@Target(AnnotationTarget.FUNCTION, AnnotationTarget.CLASS)
annotation class Loggable
```
In this example, `@Loggable` can only be applied to functions and classes.

### Add Parameters to Your Annotation
Annotations can have parameters to allow for more flexible and detailed configuration. Parameters are declared in the primary constructor of the annotation class.
```kotlin
annotation class Loggable(val level: String = "INFO")
```
Here, `@Loggable` includes an optional level parameter that specifies the logging level, with a default value of “INFO”.
```kotlin
@Loggable(level = "DEBUG")
class UserService
```

### Retention Policy
The retention policy of an annotation determines at which point the annotation is discarded during the compilation and execution of your program. Both Kotlin and Java support three types of retention policies:

Source: The annotation is only available in the source code and is discarded by the compiler.
Binary: The annotation is retained in the compiled class files but not available at runtime.
Runtime: The annotation is available at runtime through reflection.
In Kotlin, you specify the retention policy using the @Retention annotation:
```kotlin
@Retention(AnnotationRetention.SOURCE)
annotation class DebugLog
```
This @DebugLog annotation is only available in the source code, making it useful for annotations that are intended to be processed by tools that analyze the source code.

### use reflection

Reflection: read annotations at runtime.
To check if a function is annotated with @Loggable and print a message accordingly, you might use reflection like this:
```kotlin
fun checkLoggable(function: KFunction<*>) {
  function.findAnnotation<Loggable>()?.let {
    println("Function ${function.name} is loggable with level ${it.level}.") 
  }
}
    
// Usage
checkLoggable(::updateUser)
```

## Annotation Processing

Annotation processors are compiler plugins that generate code based on annotations at compile time. You know a third-party library includes an annotation processor when it requires using `annotationProcessor`, `kapt` or `ksp` instead of implementation as its build.gradle dependency configuration. Popular libraries that rely on annotation processing include Dagger (`@Provides`, `@Inject`), Moshi (`@Json`), and Room (`@Entity`, `@Dao`).

An annotation processor must be registered to the compiler for it to run during compilation. The most common way to register one is via Google’s AutoService library — just annotate your processor with `@AutoService(Processor.class)`.

## References

- [annotation official doc](https://kotlinlang.org/docs/annotations.html)