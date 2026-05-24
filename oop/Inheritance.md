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

## Method overriding and super()

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






