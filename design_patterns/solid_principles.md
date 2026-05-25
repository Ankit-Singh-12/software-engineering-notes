# Solid Principles

SOLID is an acronym for 5 design principles introduced by Robert C. Martin (Uncle Bob) that make our code maintainable, scalable, and easy to extend.

What SOLID stands for:

- S — Single Responsibility Principle
- O — Open/Closed Principle
- L — Liskov Substitution Principle
- I — Interface Segregation Principle
- D — Dependency Inversion Principle


**The 5 Principles at a Glance**

| Letter | Principle             | One-liner                                    |
| ------ | --------------------- | -------------------------------------------- |
| S      | Single Responsibility | One class = One job                          |
| O      | Open/Closed           | Open to extend, closed to modify             |
| L      | Liskov Substitution   | Child class must not break parent's behavior |
| I      | Interface Segregation | Don't force useless methods on a class       |
| D      | Dependency Inversion  | Depend on abstractions, not concrete classes |

## 1. Single Responsibility Principle (SRP)

> **A class should have only one reason to change.**

This means that a class should have only one responsibility, as expressed through its methods. If a class takes care of more than one task, then you should separate those tasks into dedicated classes with descriptive names.

In detail:

- **“Responsibility”** = a reason for the code to change in future (like business logic, logging, persistence, UI).

- A class with many unrelated responsibilities becomes a “God object” or “Bloated class”.

This principle is closely related to the concept of **separation of concerns**, which suggests that we should divide our programs into components, each addressing a separate concern.

- Violating SRP makes code: 

    - Hard to read (too many concerns mixed)
    
    - Hard to test (we must set up many things for one test)
    
    - Hard to change safely (a change for logging may break business logic)

#### Python perspective:

- Split responsibilities into separate classes or functions:

    - One class for domain logic (e.g., Order)

    - Another for persistence (e.g., OrderRepository or OrderDAO)

    - Another for formatting or presentation (e.g., OrderPrinter)

- In Python, it’s also common to use small, focused functions and modules with clear responsibilities, not just classes.

Code Example:

To illustrate the single-responsibility principle and how it can help us improve our object-oriented design, say that we have the following FileManager class:

```python
from pathlib import Path
from zipfile import ZipFile

class FileManager:
    def __init__(self, filename):
        self.path = Path(filename)

    def read(self, encoding="utf-8"):
        return self.path.read_text(encoding)

    def write(self, data, encoding="utf-8"):
        self.path.write_text(data, encoding)

    def compress(self):
        with ZipFile(self.path.with_suffix(".zip"), mode="w") as archive:
            archive.write(self.path)

    def decompress(self):
        with ZipFile(self.path.with_suffix(".zip"), mode="r") as archive:
            archive.extractall()
```

In this example, our FileManager class has two different responsibilities. It manages files using the .read() and .write() methods. It also deals with ZIP archives by providing the .compress() and .decompress() methods.

This class violates the single-responsibility principle because there is more than one reason for changing its implementation (file I/O and ZIP handling). This implementation also makes code testing and code reuse harder.

To fix this issue and make our design more robust, we can split the class into two smaller, more focused classes, each with its own specific concern:

```python
from pathlib import Path
from zipfile import ZipFile

class FileManager:
    def __init__(self, filename):
        self.path = Path(filename)

    def read(self, encoding="utf-8"):
        return self.path.read_text(encoding)

    def write(self, data, encoding="utf-8"):
        self.path.write_text(data, encoding)

class ZipFileManager:
    def __init__(self, filename):
        self.path = Path(filename)

    def compress(self):
        with ZipFile(self.path.with_suffix(".zip"), mode="w") as archive:
            archive.write(self.path)

    def decompress(self):
        with ZipFile(self.path.with_suffix(".zip"), mode="r") as archive:
            archive.extractall()
```

Now, we have two smaller classes, each having only a single responsibility:

- FileManager takes care of managing a file.
- ZipFileManager handles the compression and decompression of a file using the ZIP format.

These two classes are smaller, so they’re more manageable. They’re also easier to reason about, test, and debug.

The concept of responsibility in this context may be pretty subjective. Having a single responsibility doesn’t necessarily mean having a single method. Responsibility isn’t directly tied to the number of methods but to the core task that our class is responsible for, depending on our idea of what the class represents in our code.

Good interview angle:
> We can say: “When I see a large class doing validation, business logic, DB calls, and logging, I split it into multiple smaller classes or helpers so each has a single responsibility. That improves testability and maintainability.”

## 2. Open/Closed Principle (OCP)

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

We should be able to add new behavior without touching existing, tested code. Use inheritance or abstract classes to extend.

In detail:

- “Open for extension” = we can add new features/behaviors.

- “Closed for modification” = we don’t keep touching the old, stable code every time requirements change.

This reduces regression bugs when adding new features.

#### Python perspective:

- Use polymorphism and abstractions. For example:

    - A base class or protocol like PaymentMethod with a pay() method.

    - Add new payment modes (e.g., CreditCardPayment, UPIPayment) by creating new subclasses instead of editing big if/elif chains.

- We can also use composition and strategy pattern: pass in a “strategy” object (e.g., a DiscountStrategy) instead of branching on types.

Code Example:

To understand what the open-closed principle is all about, consider the following Shape class:

```python
from math import pi

class Shape:
    def __init__(self, shape_type, **kwargs):
        self.shape_type = shape_type
        if self.shape_type == "rectangle":
            self.width = kwargs["width"]
            self.height = kwargs["height"]
        elif self.shape_type == "circle":
            self.radius = kwargs["radius"]
        else:
            raise TypeError("Unsupported shape type")

    def calculate_area(self):
        if self.shape_type == "rectangle":
            return self.width * self.height
        elif self.shape_type == "circle":
            return pi * self.radius**2
        else:
            raise TypeError("Unsupported shape type")
```

The initializer of Shape takes a shape_type argument that can be either "rectangle" or "circle". It also takes a specific set of keyword arguments using the **kwargs syntax. If we set the shape type to "rectangle", then we should also pass the width and height keyword arguments so that we can construct a proper rectangle.

In contrast, if we set the shape type to "circle", then we must also pass a radius argument to construct a circle. Finally, if we pass a different shape, like a "triangle", for example, then we’ll get an error because the class doesn’t support this shape.

Additionally, the class has a flaw: it doesn’t constrain the shape-type checking to a single place, the initialization. Instead, it requires checking all across the logic.

Shape also has a .calculate_area() method that computes the area of the current shape according to its .shape_type attribute:

```python
>>> from shapes_ocp import Shape

>>> rectangle = Shape("rectangle", width=10, height=5)
>>> rectangle.calculate_area()
50
>>> circle = Shape("circle", radius=5)
>>> circle.calculate_area()
78.53981633974483
```

The class works. We can create circles and rectangles, compute their area, and so on. However, imagine that we need to add a new shape, maybe a square. How would we do that? Well, the option here is to add another elif clause to .__init__() and to .calculate_area() so that we can address the requirements of a square shape.

Having to make these changes to create new shapes means that our class is open to modification, which violates the Open-Closed Principle.

> Note: OCP doesn’t mean our classes should never be modified. It means that we should minimize the need to modify existing classes when adding new functionality to our code.

```python
from abc import ABC, abstractmethod
from math import pi

class Shape(ABC):
    def __init__(self, shape_type):
        self.shape_type = shape_type

    @abstractmethod
    def calculate_area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        super().__init__("circle")
        self.radius = radius

    def calculate_area(self):
        return pi * self.radius**2

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("rectangle")
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

class Square(Shape):
    def __init__(self, side):
        super().__init__("square")
        self.side = side

    def calculate_area(self):
        return self.side**2
```

In this code, we completely refactored the Shape class, turning it into an abstract base class (ABC). This class provides the required interface (API) for any shape that we’d like to define. That interface consists of a .shape_type attribute and a .calculate_area() method that we must override in all the subclasses.

> Note: The example above and some examples in the next sections use Python’s ABCs to provide what’s called **interface inheritance**. In this type of inheritance, subclasses inherit interfaces rather than functionality. In contrast, when classes inherit functionality, then we’re presented with **implementation inheritance**.

This update makes the class closed for modification. Now, we can add new shapes to our class design without the need to modify Shape. In every case, we’ll have to implement the required interface, which also makes our classes polymorphic.

Good interview angle:
> We can say: “Whenever I see a big if/else on type, I consider replacing it with abstraction and polymorphism so that adding a new type means adding a new class, not touching existing ones.”

## 3. Liskov Substitution Principle (LSP)

> A subclass must be replaceable by its parent class without breaking the program.

If B is a child of A, we should be able to use B anywhere we expect A without breaking anything or changing behavior expectations.In practice,  this principle is about making our subclasses behave like their base classes without breaking anyone’s expectations when they call the same methods.

In detail:

- A subclass must not:

    - Weaken the guarantees of the base class

    - Change the expected behavior in surprising ways

    - Throw new kinds of errors in normal usage scenarios the base class would handle

- Violating LSP makes inheritance dangerous; code using the base type can fail when a subclass instance is passed.

#### Python perspective:

- When we implement subclasses, ensure:

    - Method signatures match (same parameters/return types, broadly).

    - Preconditions are not stronger (don’t require extra conditions to work).

    - Postconditions are not weaker (don’t do less than promised).

- If a subclass needs to disable or change most behaviors of the parent, inheritance might be wrong; prefer composition.

Code Example:

Rectangle and Square inheritance. If Square inherits Rectangle but changes how width/height setters work, code expecting Rectangle behavior may break.

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height
```

In Rectangle, we’ve provided the .calculate_area() method, which operates with the .width and .height instance attributes.

Because a square is a special case of a rectangle with equal sides, we think of deriving a Square class from Rectangle in order to reuse the code. Then, we override the setter method for the .width and .height attributes so that when one side changes, the other side also changes:

```python
class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)

    def __setattr__(self, key, value):
        super().__setattr__(key, value)
        if key in ("width", "height"):
            self.__dict__["width"] = value
            self.__dict__["height"] = value
```

In this snippet of code, we’ve defined Square as a subclass of Rectangle. As a user might expect, the class constructor takes only the side of the square as an argument. Internally, the .__init__() method initializes the parent’s attributes, .width and .height, with the side argument.

We’ve also defined a special method, .__setattr__(), to hook into Python’s attribute-setting mechanism and intercept the assignment of a new value to either the .width or .height attribute. Specifically, when we set one of those attributes, the other attribute is also set to the same value:

```python
>>> from shapes_lsp import Square

>>> square = Square(5)
>>> vars(square)
{'width': 5, 'height': 5}

>>> square.width = 7
>>> vars(square)
{'width': 7, 'height': 7}

>>> square.height = 9
>>> vars(square)
{'width': 9, 'height': 9}
```

Now, we’ve ensured that the Square object always remains a valid square, making our life easier for the small price of a bit of memory. Unfortunately, this violates the Liskov substitution principle because we can’t replace instances of Rectangle with their Square counterparts.

When someone expects a rectangle object in their code, they might assume that it’ll behave like one by exposing two independent .width and .height attributes. Meanwhile, our Square class breaks that assumption by changing the behavior promised by the object’s interface. That could have surprising and unwanted consequences, which would likely be hard to debug.

While a square is a specific type of rectangle in mathematics, the classes that represent those shapes shouldn’t inherit from each other via a parent-child relationship if we want them to abide by the LSP. One way to solve this problem is to create a base class for both Rectangle and Square to extend:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def calculate_area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def calculate_area(self):
        return self.side ** 2
```

Shape becomes the type that we can substitute through polymorphism with either Rectangle or Square, which are now siblings rather than a parent and a child. Notice that both concrete shape types have distinct sets of attributes and different initializer methods, and they could potentially implement other behaviors. The only thing that they have in common is the ability to calculate their area.

With this implementation in place, we can use the Shape type interchangeably with its Square and Rectangle subtypes when we only care about their common behavior:

```python
>>> from shapes_lsp import Rectangle, Square

>>> def get_total_area(shapes):
...     return sum(shape.calculate_area() for shape in shapes)
...

>>> get_total_area([Rectangle(10, 5), Square(5)])
75
```

Here, we pass a pair consisting of a rectangle and a square into a function that calculates their total area. Because the function only cares about the .calculate_area() method, it doesn’t matter that the shapes are different. This is the essence of the Liskov substitution principle.

Good interview angle:

> We can say: “LSP basically says: if I replace a base class object with a subclass object, the program should still behave correctly. If a child breaks the expectations of the parent’s interface, that’s a design smell, and I might switch from inheritance to composition.”

## 4. Interface Segregation Principle (ISP)

> Don’t force a class to implement methods it doesn’t need. Many small, focused interfaces are better than one huge one.

If a class doesn’t use particular methods or attributes, then those methods and attributes should be segregated into more specific classes.

In detail:

- Large, “fat” interfaces force clients to depend on methods they don’t use.

- This increases coupling and makes changes risky: changing one method in a big interface impacts many clients.

- Small, role‑specific interfaces keep dependencies minimal and focused.

#### Python perspective:

- Python does not have interface keyword, but we can:

    - Use abstract base classes (ABCs) from abc module.

    - Use protocols (from typing / typing_extensions).

- Instead of one BigRepository ABC with too many methods, define multiple ABCs, e.g.:

    - ReadableRepository

    - WritableRepository

    - SearchableRepository

- Classes implement only what they actually need.

Code Example:

Consider the following example of a class hierarchy to model printing machines:

```python
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print(self, document):
        pass

    @abstractmethod
    def fax(self, document):
        pass

    @abstractmethod
    def scan(self, document):
        pass

class OldPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in black and white...")

    def fax(self, document):
        raise NotImplementedError("Fax functionality not supported")

    def scan(self, document):
        raise NotImplementedError("Scan functionality not supported")

class ModernPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in color...")

    def fax(self, document):
        print(f"Faxing {document}...")

    def scan(self, document):
        print(f"Scanning {document}...")
```

In this example, the base class, Printer, provides the interface that its subclasses must implement. OldPrinter inherits from Printer and must implement the same interface. However, OldPrinter doesn’t use the .fax() and .scan() methods because this type of printer doesn’t support these functionalities.

This implementation violates the ISP because it forces OldPrinter to expose an interface that the class doesn’t implement or need. To fix this issue, we should separate the interfaces into smaller and more specific classes. Then, we can create concrete classes by inheriting from multiple interface classes as needed:

```python
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print(self, document):
        pass

class Fax(ABC):
    @abstractmethod
    def fax(self, document):
        pass

class Scanner(ABC):
    @abstractmethod
    def scan(self, document):
        pass

class OldPrinter(Printer):
    def print(self, document):
        print(f"Printing {document} in black and white...")

class NewPrinter(Printer, Fax, Scanner):
    def print(self, document):
        print(f"Printing {document} in color...")

    def fax(self, document):
        print(f"Faxing {document}...")

    def scan(self, document):
        print(f"Scanning {document}...")
```

Now Printer, Fax, and Scanner are base classes that provide specific interfaces with a single responsibility each. To create OldPrinter, we only inherit the Printer interface. This way, the class won’t have unused methods. To create the ModernPrinter class, we need to inherit from all the interfaces. In short, we segregated the Printer interface.

> Note: In Python, we’ll rarely define many abstract base classes as we did in the example above. We may instead rely on duck typing or mixin classes to make our code more Pythonic and flexible.

This class design allows us to create different machines with different sets of functionalities, making our design more flexible and extensible.

Good interview angle:

> We can say: “In Python I often use multiple small ABCs or protocols instead of a single large abstract class so that classes depend only on methods they need. That’s interface segregation.”

## 5. Dependency Inversion Principle (DIP)

