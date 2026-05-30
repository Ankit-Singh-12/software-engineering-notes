# Inheritance

Inheritance is an OOPS concept that allows a child class to reuse the properties and methods of a parent class, improving code reusability and scalability.

Simple terms: “A child class reuses and extends a parent class.”

Example idea: Dog inherits from Animal, so Dog automatically has whatever Animal has (like eat() or sleep()), and can add its own stuff (bark()).

Inheritance allows one class to:

- reuse code from another class
- extend existing functionality
- avoid writing duplicate code

## Core idea in OOP/Python terms

In OOP, inheritance lets you build a hierarchy of classes where:

- An existing class is the **parent** (also called base or superclass).

- A new class is the **child** (also called derived or subclass).

- The child class:

    - inherits attributes and methods from the parent.

    - can add new attributes/methods.

    - can override (change) parent methods.

Conceptually:

- class Child(Parent): → Child is a Parent (IS-A relationship), with extra or specialized behavior.

## Basic Syntax of Inheritance

```python
class Parent:
    pass


class Child(Parent):
    pass
```

## Terminology

#### Parent / base / superclass

- The class whose properties and methods are inherited.

- Example: class Animal: ...

#### Child / derived / subclass

- The class that inherits from another class.

- Example: class Dog(Animal): ...

#### IS-A relationship

- Inheritance should model a logical “is-a” relationship:

    - Dog is an Animal

    - Car is a Vehicle

    - Employee is a Person

- If that sentence sounds wrong (“Square is a Rectangle?” sometimes debatable), inheritance might not be the right tool.

#### Basic Example of Inheritance

```python
class Animal:

    def eat(self):
        print("Animal is eating")


class Dog(Animal):

    def bark(self):
        print("Dog is barking")


d1 = Dog()

d1.eat()
d1.bark()
```

Output:
```python
Animal is eating
Dog is barking
```

Explanation

Dog inherits from Animal.

So Dog can access:

- eat() from parent class
- bark() from its own class

## Types of inheritance

- **Single inheritance**

    One child class inherits from one parent class.

    Example:

    ```python
    class Animal:

        def eat(self):
            print("Eating")


    class Dog(Animal):

        def bark(self):
            print("Barking")
    ```

    Structure

    ```text
    Animal → Dog
    ```

- **Multiple inheritance**

    One child class inherits from multiple parent classes.

    Example:

    ```python
    class Father:

        def skills(self):
            print("Driving")

    class Mother:

        def talents(self):
            print("Cooking")

    class Child(Father, Mother):
        pass

    c1 = Child()

    c1.skills()
    c1.talents()
    ```

    Structure

    
    Output:

    ```text
    Father
          \
           Child
          /
    Mother
    ```

- **Multilevel**

    A chain: Child inherits from Parent, which inherits from Grandparent.

    Example:

    ```python
    class GrandParent:

        def house(self):
            print("Grandparent House")

    class Parent(GrandParent):

        def car(self):
            print("Parent Car")

    class Child(Parent):

        def bike(self):
            print("Child Bike")
    ```

    Structure

    
    Output:

    ```text
    GrandParent → Parent → Child
    ```

- **Hierarchical Inheritance**

    Multiple child classes inherit from the same parent class.

    Example:

    ```python
    class Animal:

        def eat(self):
            print("Eating")

    class Dog(Animal):

        def bark(self):
            print("Bark")

    class Cat(Animal):

        def meow(self):
            print("Meow")
    ```

    Structure

    
    Output:

    ```text
        Animal
       /      \
     Dog      Cat
    ```

- **Hybrid Inheritance**

    Combination of the above types in one hierarchy

    Example

    Combination of:

    - multilevel
    - multiple
    - hierarchical inheritance

## Method overriding and super() and other inheritance functions

### Method overriding

- If parent and child both define a method with the same name, the child’s version overrides the parent’s for child objects.

- Used when the subclass wants to customize or specialize behavior.

    Conceptual example:

    ```python
    class Animal:
    def speak(self):
        print("Some sound")

    class Dog(Animal):
        def speak(self):
            print("Woof")   # overrides Animal.speak
    ```

    Calling Dog().speak() uses the child’s implementation.

### super() in Python

- super() is a built-in Python function used to access methods and constructors of the parent class from the child class.

- Very common inside __init__ to reuse parent initialization logic.

- Important for:

    - Code reuse

    - Multiple inheritance (follow MRO properly)

Conceptual example pattern:

```python
class Parent:
    def __init__(self, x):
        self.x = x

class Child(Parent):
    def __init__(self, x, y):
        super().__init__(x)  # initializes Parent part
        self.y = y           # Child-specific
```

> Note: If asked “Why use super() instead of Parent.__init__(self, x)?”:    
super() respects method resolution order (MRO) and plays better with multiple inheritance.

### final / Preventing Inheritance

If you don't want a class to be inherited, declare it as final in Java. In Python, there's no native final, but conventions or __init_subclass__ can restrict it.

Conceptual example pattern:

```python
class FinalClass:
    def __init_subclass__(cls):
        raise TypeError("This class cannot be inherited")

class Child(FinalClass):
    pass

##This will Throw error:
TypeError: This class cannot be inherited
```

### isinstance() Function

Checks whether an object belongs to a class.

```python
isinstance(s1, Student)
```

### issubclass() Function

Checks whether a class inherits another class.

```python
issubclass(Student, Person)
```

## What is NOT Inherited
child classes do not inherit:

- Constructors

- Destructors

- Overloaded operators

- Private members (they exist but are not directly accessible)

##  Method Resolution Order (MRO)

MRO (Method Resolution Order) is the exact order in which Python searches classes to find an attribute or method when you access it on an object.

You can see this order for any class C using:

```python
C.__mro__      # tuple
C.mro()        # list
```
**Why MRO matters (especially in multiple inheritance)**

MRO is mostly interesting when:

- You have multiple inheritance

- The same method name appears in more than one parent class

Example:

```python
class A:
    def foo(self): print("A")

class B:
    def foo(self): print("B")

class C(A, B):
    pass

c = C()
c.foo()        # Which one? A or B?
```

Here MRO decides whether A.foo or B.foo is called. In Python 3, the order for C is:

```python
C -> A -> B -> object
```

So A.foo is used.

Python computes this MRO using the C3 linearization algorithm, which ensures:

- Children come before parents

- The order of base classes (class C(A, B)) is respected

- The order is consistent even with “diamond” inheritance patterns

Quick mental model

- Think of MRO as a flattened, ordered list of classes that respects:

    - Inheritance relationships

    - Left-to-right order in the class definition

- Whenever there’s a conflict (same method in multiple parents), the class that appears earlier in the MRO list wins.

## Advantages of Inheritance

- Code Reusability — Write once in parent, use in all children

- Extensibility — Add new behavior in child without touching parent

- Polymorphism — Enables treating different child objects through a common parent reference

- Abstraction — Hides implementation details behind a common interface

- Cleaner, modular design — Parent handles generic behavior; children specialize.

## Limitations

- Tight coupling between parent and child — changes in parent can break child classes

- Multiple inheritance leads to the Diamond Problem (ambiguity of which parent's method to call)

- Deep inheritance chains are harder to debug and maintain

- Slower to process as it must pass through multiple classes

## Diamond Problem

The Diamond Problem happens in multiple inheritance when a class inherits from two parents that themselves inherit from the same base class, forming a diamond-shaped hierarchy.

```text
    A
   / \
  B   C
   \ /
    D
```

- B and C both inherit from A.

- D inherits from both B and C.

If A defines a method foo, and both B and C override foo, then when D calls foo, the question is:

> “Which foo should be used: from B or from C? And how many times should A’s foo run?”

This ambiguity is the Diamond Problem.

### How Python solves it: 
### MRO + C3 linearization

Python solves the diamond problem using:

1. Method Resolution Order (MRO)

    - For any class, Python computes a linear order (a list) of classes to search for attributes/methods. 

2. C3 linearization algorithm

    - This algorithm builds that linear order such that:

        - A class appears before its parents.

        - The left-to-right order in the class definition is respected.

        - Each class appears only once in the MRO.

For the diamond:

```python
class A:
    def foo(self):
        print("A")

class B(A):
    def foo(self):
        print("B")

class C(A):
    def foo(self):
        print("C")

class D(B, C):
    pass
```

The MRO of D is:

```python
(D, B, C, A, object)
```

So when you call:

```python
D().foo()
```

Python looks in this order:

1. D – no foo

2. B – finds foo here → calls B.foo

3. It won’t go to C or A for this call, because the first match wins.

This deterministic, linear order removes ambiguity.

### Cooperative multiple inheritance with super()

To avoid duplicate calls to A in deeper diamond patterns, Python encourages cooperative multiple inheritance using super().

```python
class A:
    def foo(self):
        print("A")
        # no super() here usually, as it's often the top

class B(A):
    def foo(self):
        print("B")
        super().foo()

class C(A):
    def foo(self):
        print("C")
        super().foo()

class D(B, C):
    def foo(self):
        print("D")
        super().foo()
```

Given the MRO D -> B -> C -> A -> object:

- D().foo() calls:

    - D.foo → print("D"), then super().foo() goes to B.foo

    - B.foo → print("B"), then super().foo() goes to C.foo

    - C.foo → print("C"), then super().foo() goes to A.foo

    - A.foo → print("A")

Each class’s foo runs exactly once, and the order is well defined.

This is how Python solves the diamond problem:

- Use MRO (C3 linearization) to define a single search order.

- Use super() consistently so calls follow that MRO and don’t repeat base classes.

## Core Terminology Summary:

| Term               | Also Called              | Meaning                                                     |
| ------------------ | ------------------------ | ----------------------------------------------------------- |
| Parent Class       | Base Class / Superclass  | The class whose properties are inherited geeksforgeeks      |
| Child Class        | Derived Class / Subclass | The class that inherits from the parent geeksforgeeks       |
| extends            | —                        | Keyword used to inherit a class (Java/JS) scribd            |
| super()            | —                        | Used to call the parent class constructor or methods scribd |
| IS-A relationship  | —                        | Describes inheritance: a Dog IS-A Animal press.rebus        |
| HAS-A relationship | Aggregation              | Describes composition: a Car HAS-A Engine scribd            |

## Inheritance Vs Composition

- **Inheritance:** “is-a” relationship → one class is a specialized type of another.

- **Composition:** “has-a” relationship → one class is made up of other objects as parts.

### Inheritance

One class (child / subclass) extends another (parent / base class) and automatically gets its data and methods.

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):
    def speak(self):
        print("Woof")
```

Here:

- Dog is an Animal.

- Dog inherits Animal’s interface and can override behavior.

Key characteristics

- Tight coupling: child is strongly tied to parent’s design.

- Good when: subclass really is a more specific version of the base type and can substitute it without surprises (Liskov Substitution).

### Composition

A class contains other objects and delegates work to them instead of inheriting from them.

```python
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self, engine: Engine):
        self.engine = engine

    def start(self):
        self.engine.start()
```

Here:

- Car has an Engine.

- Car reuses Engine’s behavior by holding it, not inheriting from it.

Key characteristics

- Loose coupling: Car depends on an interface/behavior, not on a fixed parent class.

- You can easily swap implementations (e.g. ElectricEngine, DieselEngine) at runtime.

- Often preferred in design: “Favor composition over inheritance”.

### When to use which

**Use inheritance when:**

- The relationship is a clear “is-a”.

- The base class defines a meaningful abstraction that naturally generalizes multiple subclasses.

- You’re extending frameworks (e.g., subclassing Django’s Model, Flask views, etc.).

**Use composition when:**

- You want flexibility and easier refactoring.

- You want to combine behaviors (logging, caching, retry, validation, etc.).

- The relationship is more like “has-a”, “uses-a”, or “can-do”.

### Inheritance vs Composition

| Aspect               | Inheritance                                                          | Composition                                                              |
| -------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Basic relationship   | “Is-a” (Dog is an Animal) digitalocean                               | “Has-a” (Car has an Engine) digitalocean                                 |
| Code reuse mechanism | Reuse by extending a base class                                      | Reuse by containing and delegating to other objects                      |
| Coupling             | Tighter: child depends on parent’s design and changes digitalocean   | Looser: class depends on collaborators via interfaces/behavior wikipedia |
| Flexibility          | Less flexible; hierarchy is fixed at design time                     | More flexible; components can be swapped at runtime wikipedia            |
| Hierarchy depth      | Encourages deep class hierarchies if overused                        | Encourages flatter, more modular designs veerpalbrar.github              |
| Substitutability     | Supports polymorphism via “subclass of” relationship                 | Supports polymorphism via composed object interfaces                     |
| Typical use case     | True specialization: Square extends Shape                            | Behavior assembly: Logger, Cache, Notifier inside Service stackoverflow  |
| Risk / pitfall       | Fragile hierarchies, diamond problem, misuse of “is-a” stackoverflow | Many small objects; more wiring/boilerplate sometimes                    |
| Design mantra        | Use when subclass genuinely is a base type                           | “Favor composition over inheritance” in most designs wikipedia           |

### IS-A vs HAS-A 

|          | IS-A (Inheritance)         | HAS-A (Aggregation/Composition)          |
| -------- | -------------------------- | ---------------------------------------- |
| Meaning  | Child IS a type of Parent  | Class contains another class as a member |
| Example  | Dog IS-A Animal            | Car HAS-A Engine                         |
| Keyword  | extends / inherits         | Object as attribute                      |
| Use when | True specialization exists | One object uses another scribd           |

### Summary

> “Inheritance is an OOP concept where a child class reuses and extends the properties and behavior of a parent class. In Python, we define inheritance using class Child(Parent):, which lets the child automatically get the parent’s attributes and methods, and optionally override them and add new features. This supports code reusability, models real-world IS-A relationships, and enables polymorphism. Python supports single, multiple, multilevel, hierarchical and hybrid inheritance, and we commonly use super() to call parent methods, following the method resolution order.”

