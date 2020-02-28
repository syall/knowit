---
title: "Idioms"
date: 2020-02-27T14:04:29-05:00
draft: true
---

Just like human languages, programming languages also have idioms.

Just as people who use the same language have characteristic ways of speaking thoughts, programming language users have characteristic ways of coding ideas.

**Programming Idioms come with the class of a problem paired with language implementation.**

For example, Go is a language that strongly enforces idiomatic solutions to practically every problem, even formatting and commenting.

On the other hand, Perl and Raku are languages that embrace the philosophy "there's more than one way to do it" (TIMTOWDI), so it is idiomatic to do it whichever way you see fit.

In this article, we will dive into idiomatic ways different programming languages might implement the popular programming paradigm: Object-Oriented Programming (OOP).

## Java

Let's start with a simple example in Java, the canonical pure OOP giant:

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

If you've seen Java code before, the code is fairly straightforward.

But how do we **actually** use this class?

Independently, this Java class has no functionality at all.

Does that mean that this class is encoding nothing?

If you have learned the four OOP Principles from school or brushed up on them for technical interviews, you might say that the class simply describes what a Person object encapsulates.

To generalize what this traditional class syntax is, a class encodes an object template that encapsulates the associated definition of data and behavior.

## JavaScript

Let's take a look at an analogous class in JavaScript, a multiparadigm language:

```javascript
// Person.js
class Person {

    constructor(f, l) {
        this.firstName = f;
        this.lastName = l;
    }

    greet(b) {
        console.log(this.firstName + ' greets ' + b.firstName);
    }

}
```

Seems similar enough, but what are the differences?

There are no fields declared in the class block, but fields can still be assigned via `this` in the constructor. There is also a lack of Java bloat but overall, the syntax seems analogous.

However, to your surprise (maybe), the underlying implemention is completely different!

The `class` syntax was only introduced in the ECMAScript 2015 (ES6) standard and is syntactic sugar for the following:

```javascript
// Person.js
function Person(f, l) {
    this.firstName = f;
    this.lastName = l;
}

Person.prototype.greet = function(b) {
    console.log(this.firstName + ' greets ' + b.firstName);
}
```

The code looks completely different, so how are these equivalent?

The Person function is attaching properties to the Person prototype, like in the ES6 constructor method.

When the Person function is invoked with `new`, it will use the Person function's prototype object as the prototype of a new object!

We could make the desugared version look more traditional, but even then the code still has not changed in its expressive power:

```javascript
// Person.js
function Person(f, l) {

    this.firstName = f;
    this.lastName = l;

    this.greet = function(b) {
        console.log(this.firstName + ' greets ' + b.firstName);
    }

}
```

So there is a relationship between functions and objects in JavaScript whereas Java only has objects with methods.

Sidenote: JavaScript is a unique case in that the standard is continuously evolving, such that even major syntactic sugar can be added (and is able to deceive programmers).

## Python

Next we will look at another popular programming language, Python:

```python
# Person.py
class Person:

    def __init__(self, f: str, l: str):
        self.firstName = f
        self.lastName = l

    def greet(self, b):
        print(self.firstName + " greets " + b.firstName)

```

Here we see a language that did not create syntactic sugar for OOP but is still more flexible than Java.

The class block with both the constructor and `greet` method follow Java's traditional structure, but the dynamic assignment of attributes to an object is similar to JavaScript.

The inherent design of the OOP system in Python infers organization of code structure yet includes easy modification of objects at runtime.

## Haskell

For another example, let's consider Haskell, a pure functional programming language:

```haskell
-- person.hs
data Person = Person { firstName :: String, lastName :: String }

greet :: Person -> Person -> IO ()
greet (Person a _) (Person b _) = doputStrLn (a ++ " greets " ++ b)
```

In this case, Haskell has absolutely no built-in OOP support so the code is an adhoc simulation.

Haskell allows you to define data types, which are essentially data containers attached to a type, hence data types.

The new data type is created in the strongly typed system so functions, such as `greet`, are compatible with, but not owned by, the data type.

Although it is not a perfect analogy to the Java and JavaScript examples (the behavior is not encapsulated with the data), the thought process behind this snippet of code essentially has the same functionality.

## C

In C, a pure imperative language, a similar abstraction is produced:

```c
// Person.c
typedef struct _Person {
    char* firstName;
    char* lastName;
} Person;

Person* newPerson(char* f, char* l) {
    Person* person = malloc(sizeof(Person));
    person->firstName = f;
    person->lastName = l;
    return person;
}

void greet(Person* a, Person* b) {
    printf("%s greets %s", a->firstName, b->firstName);
}
```

The C programming language also does not have any OOP system built-in, so the code we have is once again an adhoc simulation.

The struct define a memory format that holds data, like Haskell's data type.

The struct also does not have any behavior related to the data, only constrained by function parameter types, like Haskell function types.

In essense, the shortcuts that we used in Haskell to simulate OOP were the same that we used in C, although the languages use two completely different programming paradigms.

## Rust

As a final example, Rust, a multiparadigm language, has a compromise from what we've seen so far:

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

Here are the major characteristics of the Rust example:

* There is a clear separation of data with `struct` and behavior with `impl`
* Encapsulation of both the data and behavior is clear by name
* Strong types are created from the struct definition

So it seems that Rust has similarities in syntax to the adhoc simulation that we saw in Haskell and C, yet still have true encapsulation that exists in Java, Python, and JavaScript.

## Putting it All Together

Although all the snippets are simulating the same type of programming paradigm, the language implementations create a difference in their level of flexibility.

Ideas can be expressed, but the implementation dictates how the code is written and to what extent those ideas can be purely executed (lack of the other OOP principles besides encapsulation in adhoc simulation).

I tried to display what OOP looks like in all of these different languages, but the roadblocks always came up when the language design did not support the expressiveness required.

Although there are probably clever ways to make the Haskell (functions in a data type) and C (function pointers in a struct) simulations more OOP than they are now, those languages were not designed for that.

**Language design is intentional and enforces certain paradigms that lead to idiomatic solutions for classes of problems.**
