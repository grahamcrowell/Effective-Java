# Effective Java

<!-- ## Chapter 2: Creation and Destruction of Objects -->

### Item 1: Consider static factory methods instead of constructors

---

### Outline

- examples
- advantages
- disadvantages
- conventions
- conclusion
- related items

---

# Example

#### Classic Constructor

```java
public Boolean(boolean var1) {
   this.value = var1;
}
```

#### Static Factory Method

```java
public static Boolean valueOf(boolean var0) {
   return var0 ? TRUE : FALSE;
}
```

#### Google's Guava example:

```java
public static <E> HashSet<E> newHashSet() {
  return new HashSet<E>();
}
```

---

### Advantage 1: has a name

This constructor actually returns a probable prime

```java
BigInteger(int, int, Random)
```

Better expressed as a static factory method:

```java
BigInteger.probablePrime(int, int, Random)
```

<!-- _A class can only have a single constructor for a given signature._ -->

---

### Advantage 2: each invocation need not return a new instance

- allows immutable classes (Item 17)
  - use preconstructed instances
  - cache instances as they are created
  - avoid creation of duplicate objects
  - `java.lang.Boolean#valueOf(boolean)` never creates a new instance
- similar to Gang of Four "Flyweight" structual design pattern:

> use sharing to support a large number of fine grained objects effeciently

---

### Advantage 2: Instance controlled classes

A class that relies static factory methods to control which instances exist when is called:

> Instance Controlled

- able to guarentee singleton (Item 3)
- non-instaniable (Item 4)
- extend immutability (Item 17):
  - guarentee: `a.equals(b)` iff `a==b`
  - basis for Flyweight GOF pattern
  - Enum types provide this guarentee (Item 34)

---

### Advantage 3: can return an object of any subtype of their return type

- allows `interface` only APIs (Item 20) 
<!-- no public classes -->
- used by `java.util.Collections` Framework API
  - 45 classes merged into 1
- reduces _conceptual weight_
- forces client to consume interfaces (Item 64)

<!-- > - Java 8 added public static methods
> - Java 9 added private static methods -->

---

### Advantage 4: class of returned object can depend on input parameters

`EnumSet` class (Item 36) only has static factory methods

- currently one of 2 subclasses are depending number of enum elements
  - 1 could be removed without breaking the API
  - 3rd could be added without breaking the API

---

### Advantage 5: class of returned object need not exist when class containing method is returned

> Service Provider Framework

_system in which **providers** implement a **service**, and the system provides implementations to **clients**_

Example: JDBC

---

### Service Provider Framework

> decouples clients from implementations

1. **Service Interface** represents the implementation
  - `Connection` in JDBC
1. **Provider Registration API** used by providers to register their implementation
  - `DriverManager.registerDriver` in JDBC
1. **Service Access API** used by clients to get instances of the service
  - `Driver` in JDBC

---

### Disadvantage 1: cannot be subclassed

lessing in disguise:
- encourages composition instead of inheritence (Item 18)
- prerequisite for immutability (Item 17)
---

### Disadvantage 2: hard to find?

- Javadoc tool does not recongnize/highlight static factory methods as constructors

---

### Conventions

see book

- `from`
- `of`
- `valueOf`
- `instance` or `getInstance`
- `create` or `newInstance`
- `get`_Type_
- `get`_Type_
- `new`_Type_
- _type_

---

### Conclusion

Often static factories are preferable [vs constructors], so avoid the reflex to provide public constructors.

---

### Related

- Creating and Destroying Objects
  - Item 3 builder to handle many parameters
  - Item 4 how to enforce singleton
- Classes and Interfaces
  - Item 17 Minimize mutibility
  - Item 18 Favour Composition over Inheritance
  - Item 20 Prefer Interfaces over Abstract classes
- Enums and Annotations  
  - Item 34 Use enum not int constants
  - Item 36 Use EnumSet instead of bit fields
- General Programming
  - Item 64 Refer to object by with Interfaces