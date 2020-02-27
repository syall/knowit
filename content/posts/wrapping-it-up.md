---
title: "Wrapping It Up"
date: 2020-02-27T14:04:29-05:00
draft: true
---

Although this is my first post, it seems like it is time to wrap it up already.

`badum tss`

The topic I will cover is the relationship between functions, objects, data, code, etc., generic abstractions that coders work with everyday!

Let's start with a simple canonical example in Java:

```java
// Person.java
class Person {

    public String firstName;
    public String lastName;

    public Person(String f, String l) {
        this.firstName = f;
        this.lastName = l;
    }

    public void greet(Person b) {
        System.out.println(
            this.firstName + " greets " + b.firstName
        );
    }

}
```

Seems harmless until we are tasked to use this class.

Where do we create a Person object?

By itself, a Java class with no main method independently has no use at all.

Because of this fact, what exactly is this class encoding?

Based on the 4 Object-Oriented principles, we might say that we are encapsulating data and behavior.

In essence, we are simply encoding data!

However, what if I wanted to create a function that did not belong to a class?

Because of Java's choice of abstraction, there is not really a notion of a traditional function.

Now let's take a look at another example of a similar class in JavaScript:

```javascript
// Person.js
class Person {

    constructor(f, l) {
        this.firstName = f;
        this.lastName = l;
    }

    greet(b) {
        console.log(
            this.firstName + " greets " + b.firstName
        );
    }

}
```

Seems similar enough.

There are no fields declared in the class block, but "fields" can still be assigned by object properties.

However, the underlying implemention is completely different.

The syntax above is syntactic sugar for the following syntax:

```javascript
// Person.js
function Person(f, l) {
    this.firstName = f;
    this.lastName = l;
}
Person.prototype.greet = function(b) {
    console.log(
        this.firstName + " greets " + b.firstName
    );
}
```

How did our class turn into a function?

In JavaScript, every Object has a prototype, and so does the function Person.

To create a Person Object, you write

```javascript
new Person(a, b);
```

So now there is a relationship between functions and objects in JavaScript whereas in Java there are only objects.

These types of technical decisions lead to different ways of thinking.

These ways of thinking are typically called Programming Paradigms.

These paradigms are decided at the language's semantic design and are designed to solve and model certain problems.

However, once you start using these different programming languages, you realize these ways of thinking in practice never mutually exclusive.

For example, Haskell in considered a Pure Functional Programming Language.

However, when considering a tuple in Haskell, it essentially is an object without behavior implemented.

Even more, when one wants to apply a function to a tuple, whether it is between multiple tuples or other data, the pure functions implement behavior for the tuple!

Even if the language is functional, what happens is that data is passed around to functions that implement behavior, sounding extremely similar to Object Oriented Programming!

In comparison with C, a similar abstraction is produced.

In C, structs store data but do not have any methods attached, typically transformed through functions that take in a struct type and other optional data to mutate the members in the struct.

The most clear distinction of this similar abstraction is in Rust, most easily seen by example.

```rust
// Person.rs
struct Person {
    first_name: String,
    last_name: String
}
impl Person {
     pub fn new(f: String, l: String) -> Person {
        Person {
            first_name: f,
            last_name: l
        }
     }
    pub fn greet(self: &Self, b: Person) {
        println!("{} greets {}", self.first_name, b.first_name);
    }
}
```

The `struct` holds all of the data for the object while the `impl` defines all the behavior for the object.

Not only this, but the behavior for the `struct` includes the function `new` that returns a struct!

As programmers, there are many different types of ways at looking at problems which people religiously argue about.

However, at the lowest level, the problems are all related to data encapsulation and manipulation.

Different implementations and features of the language can help for developer efficiency and application safety but the fundamental ideas are still the same.

The language design enforces certain paradigms and creates idiomatic ways to solve classes of problems, but in most languages, the code is translatable.

In fact, the programs that translates these languages into other languages are called compilers where the source code truly is treated as data.

To conclude, there are three points that capture the essence of programming paradigms.

* Different Paradigms can solve the same problem but differ in Implementation
* Programming Languages are not so disjoint as most are Translatable
* All Code is Data and all Data is Code
