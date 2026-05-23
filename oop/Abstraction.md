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

- ### **Data abstraction**

    - We hide how data is stored and maintained.

    - We expose only operations needed to work with that data.

    Example: A BankAccount class hides the internal balance representation and exposes methods like deposit(amount) and withdraw(amount).

- ### **Process / functional abstraction**

    - We hide how a process is implemented and expose it as a simple operation.

    Example: A sort() function hides the sorting algorithm; we just call sort().

- ### **Abstract class**

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

    Output

    ```python
    Car is starting
    Bike is stopping
    ```

    Explanation:

    - Inherit ABC
        ```python
        class Vehicle(ABC):
        ```
        This makes Vehicle an abstract class.

        Without inheriting ABC, abstraction will not work properly.

    - Create Abstract Method

        ```python
        @abstractmethod
        def start(self):
            pass
        ```

        This method:

        - has no implementation
        - only defines a rule

        It tells child classes:

        “You must create this method.”
    
    ### What is @abstractmethod?

    It is called a: **decorator**.

    It converts a normal method into an abstract method. It is declared in an abstract class without implementation. It defines the contract (“this method must exist”), but not the body.

    Subclasses must override it.

    > **Important:** Child Class MUST Implement All Abstract Methods. If even one abstract method is missing, object creation fails.

    ### Why Do We Need Abstract Classes?

    Abstract classes are used when:

    - multiple classes should follow common rules
    - we want consistency in child classes
    - we want to force method implementation
    - we want a common blueprint

    ### Abstract Class vs Normal Class

    | Abstract Class | Normal Class |
    |---|---|
    | Cannot create objects | Can create objects |
    | Used as blueprint | Used directly |
    | May contain abstract methods | Contains fully implemented methods |
    | Used for inheritance | Used normally |

- ### **Interface**

    An interface is a blueprint that only defines what methods a class must have, but does not define how those methods work.

    It only contains:

    - method declarations
    - rules for child classes

    An interface says:

    > “Any class using me must implement these methods.”

    It focuses completely on:

    ```
    WHAT to do
    ```
    not
    ```
    HOW to do it
    ```

    How Does Python Create Interfaces?

    Python mainly uses:

    - abstract classes
    - abstract methods

    to achieve interface-like behavior.

    | Interface | Abstract Class |
    |---|---|
    | Full abstraction | Partial abstraction possible |
    | Mostly method declarations | Can contain normal methods |
    | Defines rules only | Defines rules + common logic |
    | No implementation focus | Can contain implementation |
    | Used for standardization | Used for base functionality |

## Abstraction vs Encapsulation

**Encapsulation**

- **Mechanism**: bundling data + methods and controlling access.

- **Think**: “hide the data behind methods/properties.”

**Abstraction**

- **Mechanism**: exposing only essential features, hiding implementation logic and unnecessary details.

- **Think**: “hide the complexity behind a simple interface.”

One nice way to put it:

> Encapsulation is more about how you hide data.

> Abstraction is more about how you hide complexity and define high-level behavior.

Another angle:

    Abstraction is a design-level concept (deciding what the object should expose), while encapsulation is more implementation-level (how those details are hidden/organized).

| Encapsulation | Abstraction |
|---|---|
| Hides data | Hides implementation |
| Focuses on security | Focuses on simplicity |
| Achieved using access modifiers | Achieved using abstract classes |

## How abstraction is implemented in Python

**Abstract Base Classes (ABCs)**

- Define a base class with abstract methods that subclasses must implement.

```python
from abc import ABC, abstractmethod
```

**Hiding implementation details in classes**

- Keep internal helper methods/attributes non-public (_helper, __internal).

- Expose only a minimal set of public methods as the “API” of your class.

**Using modules and functions as abstraction**

- Even simple functions can abstract complexity:

- def send_email(to, subject, body) abstracts SMTP details.

## Benefits of abstraction

- **Reduces complexity**
    
    Users interact with a simple interface instead of complex internals.

- **Improves maintainability**
        
    You can change the implementation without affecting callers as long as the interface remains the same.

- **Increases reusability**

    Abstract base classes/interfaces allow multiple implementations with the same contract.

- **Supports loose coupling**

    Code depends on high-level abstractions, not concrete implementations.


### Summary

> “Abstraction is an OOP concept where we expose only the essential features of an object and hide the underlying implementation details. In Python, we achieve abstraction by designing classes with clear public methods, hiding internal helpers, and using abstract base classes with abstract methods to define a common interface. This reduces complexity, makes code easier to maintain, and allows different implementations to share the same high-level behavior.”