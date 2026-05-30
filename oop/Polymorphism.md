# Polymorphism

Polymorphism is the ability of one interface to be used for different underlying data types or the ability of the same method to perform different actions.

> Polymorphism = one interface, many implementations (or “same method name, different behavior depending on the object”).

Polymorphism means "many forms" — the ability of a single method, function, or object to behave differently based on the context or input. Think of it like a person who is a son at home, an employee at work, and a customer at a shop — same person, different behaviors in different contexts.

Example: dog.speak() and cat.speak() use the same method name speak(), but the actual output is different for each animal.

## Detailed definition in OOP / Python terms

In OOP, polymorphism lets us write code that works with different types of objects in a uniform way, and the correct behavior is chosen at runtime based on the actual object.

In Python, polymorphism mainly shows up as:

- Different classes providing methods with the same name (often via inheritance + overriding).

- Functions that accept any object that has a particular method, without caring about its exact class (duck typing).

> “Polymorphism lets the same method call behave differently depending on the object’s type.”

## Types of polymorphism in Python

### Runtime Polymorphism / Method Overriding (Dynamic Polymorphism)

Different classes implement the same method name, and we can call that method without caring about the exact class.

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):
    def speak(self):
        print("Woof")

class Cat(Animal):
    def speak(self):
        print("Meow")

def make_it_speak(animal: Animal):
    animal.speak()   # depends on actual object

make_it_speak(Dog())  # Woof
make_it_speak(Cat())  # Meow
```
When you pass a Dog or Cat into make_it_speak, Python calls the correct speak() automatically.

This is runtime polymorphism achieved via method overriding.

Key terms:

- Method overriding: Subclass provides its own implementation of a method with the same name and signature as in the parent.

- Dynamic dispatch / runtime binding: The decision about which method version to call is made at runtime based on the actual object.

### Compile-Time Polymorphism / Method Overloading	 (Static Polymorphism)

Python does not support method overloading in the same way as Java or C++.

In Python, if you define multiple methods with the same name, the last one replaces the previous one.

But we can simulate method overloading using:

- default arguments
- *args

```python
# Python simulation of overloading using default arguments
class Calculator:
    def add(self, a, b, c=0):
        return a + b + c

calc = Calculator()
print(calc.add(2, 3))      # 5
print(calc.add(2, 3, 4))   # 9  ← same method, different behavior
```

another example using *args:

```python
class Test:
    def add(self, *args):
        if len(args) == 2:
            return args[0] + args[1]
        elif len(args) == 3:
            return args[0] + args[1] + args[2]
        else:
            raise TypeError("Unsupported arity")
```

The method to call is decided at compile time. Achieved through Method Overloading and Constructor Overloading.

## Duck typing (informal/structural polymorphism)

> “If it walks like a duck and quacks like a duck, it’s a duck.”

Python doesn't check the type of an object, only whether it has the method being called. This is Python's native form of polymorphism.

If two objects support the same method(s), you can treat them polymorphically.

```python
class Dog:
    def speak(self):
        print("Woof")

class Human:
    def speak(self):
        print("Hello")

def make_it_speak(thing):
    thing.speak()    # no base class required

make_it_speak(Dog())
make_it_speak(Human())
```

In Python:



- We don’t care about the class type, we care about whether the object supports the methods/operations we need.

- Any object with a speak() method can be passed to a function that calls obj.speak(), regardless of its class.

This is polymorphism without explicit inheritance:

## Function / operator polymorphism

Same function/operator works on different types.

### Built-in function polymorphism

```python
len("abc")        # 3
len([1, 2, 3])    # 3
len({1: "a", 2: "b"})  # 2
```

### Operator overloading

When the same operator works differently for different objects, it is called operator overloading

```python
print(1 + 2)          # 3
print("a" + "b")      # "ab"
print([1, 2] + [3, 4])  # [1, 2, 3, 4]
```

+ means different things based on operand types.

You can define your own overloads with “dunder” methods:
```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2        # calls __add__
print(v3.x, v3.y)   # 4 6
```

## Advantages of Polymorphism

Flexibility — one interface works for many types

Extensibility — add new classes without changing existing code (Open/Closed Principle)

Clean Code — eliminates massive if-else or switch chains

Enables Design Patterns — Strategy, Factory, Observer all rely on polymorphism

Reusability - Shared interface reused by many implementations; client code doesn’t care which one is used.

## Abstract Classes & Interfaces (Enforced Polymorphism)

When you want to force child classes to implement a method, use Abstract Base Classes (ABC) in Python.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):     # Must be overridden
        pass

class Circle(Shape):
    def area(self): return 3.14 * 5 * 5

class Square(Shape):
    def area(self): return 4 * 4

# Shape()  ← ❌ Cannot instantiate abstract class
print(Circle().area())  # 78.5
print(Square().area())  # 16
```

## Polymorphism vs inheritance vs overloading 

### Inheritance

- Mechanism to create a new class from an existing one (IS-A relationship).

- Often used as a base to enable polymorphism but not the same thing.

- Inheritance is about reusing code

### Polymorphism

- Behavior concept: same method call → different actual behavior based on object type.

- Frequently implemented using inheritance + method overriding, or duck typing.

- polymorphism is about changing behavior

### Method overriding (runtime polymorphism)

- Same method signature in parent and child.

- Child’s version is used when called on child objects.

### Method overloading (compile-time polymorphism)

- Same method name with different parameter lists in the same class.

- Python doesn’t do this in the strict Java/C++ sense; you simulate it via default args, *args, **kwargs.

Operator overloading (__add__, __eq__, etc.) is also a form of polymorphism: the same operator symbol works differently for different types.

We need inheritance to achieve runtime polymorphism via overriding, but compile-time polymorphism (overloading) doesn't need inheritance at all

## super() in Overriding — Important Edge Case

When overriding, if we want to extend (not replace) the parent's behavior, we call super():

```python
class Animal:
    def speak(self):
        return "Animal speaks"

class Dog(Animal):
    def speak(self):
        parent_speech = super().speak()   # calls Animal.speak()
        return parent_speech + " + Dog barks!"

print(Dog().speak())
# Animal speaks + Dog barks!
```

## Key Terms Around Polymorphism

| Term               | What it means                                                         | Type         |
| ------------------ | --------------------------------------------------------------------- | ------------ |
| Method Overloading | Same method name, different parameters in the same class              | Compile-time |
| Method Overriding  | Child class redefines a method already in parent                      | Runtime      |
| Dynamic Dispatch   | JVM/interpreter decides which overridden method to call at runtime    | Runtime      |
| Duck Typing        | Python's way of polymorphism — "if it walks like a duck, it's a duck" | Runtime      |
| Abstract Method    | Method declared in parent with no body, must be overridden in child   | Runtime      |
| Interface / ABC    | Contract that forces child classes to implement certain methods       | Runtime      |
| Upcasting          | Treating a child object as a parent type                              | Runtime      |

## Method Overloading vs Method Overriding

|                     | Method Overloading    | Method Overriding    |
| ------------------- | --------------------- | -------------------- |
| Where               | Same class            | Parent-Child classes |
| Signature           | Different parameters  | Same parameters      |
| Type                | Compile-time (Static) | Runtime (Dynamic)    |
| Inheritance needed? | ❌ No                  | ✅ Yes                |
| super() used?       | ❌ No                  | ✅ Often yes          |
| Python support      | Simulated only        | ✅ Native             |

## Interview Traps to Watch Out For
⚠️ Python doesn't support true method overloading natively — defining the same method name twice just overwrites the first one. Use default args or *args to simulate it.

⚠️ Constructor overloading in Python is handled with __init__ using default parameters, not multiple constructors like Java

⚠️ Private methods cannot be overridden — they are hidden, not inherited

⚠️ Static methods cannot be overridden — they belong to the class, not the instance

### Summary

> “Polymorphism is an OOP concept where the same interface or method call can lead to different behavior depending on the object. In Python, we typically see polymorphism when subclasses override methods of a base class and we call those methods through a common reference, or when we use duck typing and write functions that work with any object that provides a particular method. This makes code more flexible, extensible, and easier to maintain because client code doesn’t need to know the exact class of the object.”