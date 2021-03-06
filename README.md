# How do I run it?

```
./gradlew run
```

# How do I test it?

```
./gradlew check
```

# How do I build a docker image and push to bintray?

```
./gradlew pushDocker
```

This will:

* build the project
* run all tests
* build the docker image
* push it to the remote repository (manually disabled right now though)


# Why kotlin?

Although both Java and Javascript are very safe choices, we have decided to adopt Kotlin, due to:

* Easier handling of non-blocking code compared to Java using coroutines
* Increased developer happiness
* Interoperability with Java and the JVM

## Coroutines

Coroutines allows you to write non-blocking code in an imperative style. In other words, you can replace this:

```java

class Foobar {
    public CompletableFuture<Long> visitors(String id) {
        return findDocumentById(id).thenCompose(document -> 
                getVisitorsForUrl(document.getUrl())
        );
    }
}
```

with this:

````kotlin

class Foobar {
    suspend fun visitors(id: String) {
        val doc = findDocumentById(id)
        return getVisitorsForUrl(doc.url)
    }
}
````

## Developer happiness

```kotlin
// Null safe navigation
if(findUserByName("Annie")?.age == 32) {
    ...
}


// Immutable data classes
data class User(val name: String, val age: Int)

val user = User("Annie", 32)
val olderUser = user.copy(age = user.age + 1)
```

and much more.

## What alternatives were considered?

* Java
* Javascript

## Links

* https://kotlinlang.org/docs/reference/index.html
* https://kotlinlang.org/docs/tutorials/koans.html


# Why ktor.io?

In fact, any simple http server would do. In the end ktor.io was chosen due to its native support for kotlin coroutines.

## What alternatives were considered?

* [vert.x](http://vertx.io/docs/vertx-web/java/)
* [undertow](http://undertow.io/)
* [spring-boot](https://projects.spring.io/spring-boot/) - Too "heavy" for our liking

# Why spek?

Spek allows for nested tests in a style very familiar to rspec or jasmine users.

## What alternatives were considered?

* [http://junit.org/junit4/](Junit4) - `@BeforeClass` is awkward with Kotlin
* [http://junit.org/junit5/](Junit5) - Too awkward to create nested classes
* [http://spockframework.org/spock/docs/1.1/index.html](Spock) - The dynamic nature of the language isn't worth it

## Links

* http://spekframework.org/
