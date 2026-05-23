# Abstraction

Abstraction is an OOPS concept that hides complex internal implementation and exposes only essential functionality to the user.

Simple terms: “Hiding internal implementation details and showing only the important features to the user.”

> Abstraction = focusing on what something does, not how it does it.

Example: We use a list.append() in Python without caring how it manages memory internally. You only care that “append adds an element to the end.”

In OOP, abstraction is about:

1. Hiding implementation details

    Internal complexity (algorithms, data structures) is hidden inside a class or module.

2. Exposing a clean interface

    Only the essential methods and properties are visible to the outside world.

In Python, we achieve abstraction by:

- Designing classes with clear public methods that callers should use.

- Keeping helper methods/attributes non-public (using _name / __name).

- Using abstract base classes (ABCs) and interfaces-like designs to specify what must be done, not how.

## Key terms around abstraction 

- **Data abstraction**

    - We hide how data is stored and maintained.

    - We expose only operations needed to work with that data.

    Example: A BankAccount class hides the internal balance representation and exposes methods like deposit(amount) and withdraw(amount).

- **Process / functional abstraction**

    - We hide how a process is implemented and expose it as a simple operation.

    Example: A sort() function hides the sorting algorithm; we just call sort().

- **Abstract class**

    - An abstract class is a special type of class that cannot be instantiated and is mainly used to define a common structure or contract for child classes. It acts like a blueprint for other classes.

    - It often contains:

        - Common attributes and methods for all subclasses.

        - One or more abstract methods (declared but not implemented).
    
    - In Python, we typically use the abc module:

        - Inherit from ABC

        - Use @abstractmethod decorator

    Example:

    Suppose we create a general class:

    ```python
    class Vehicle
    ```

    Every vehicle should:

    - start()
    - stop()

    But:

    - a car starts differently
    - a bike starts differently
    - an airplane starts differently

    So instead of writing logic in the parent class, we only define the structure.

    This parent blueprint is called an abstract class.

    ```python
    from abc import ABC, abstractmethod
    class Vehicle(ABC):

        @abstractmethod
        def start(self):
            pass

        @abstractmethod
        def stop(self):
            pass

    class Car(Vehicle):

        def start(self):
            print("Car is starting")

        def stop(self):
            print("Car is stopping")

    class Bike(Vehicle):

        def start(self):
            print("Bike is starting")

        def stop(self):
            print("Bike is stopping")

    c1 = Car()
    b1 = Bike()
    
    c1.start()
    b1.stop()
    ```
