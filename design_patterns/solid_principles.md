# Solid Principles

SOLID is an acronym for 5 design principles introduced by Robert C. Martin (Uncle Bob) that make your code maintainable, scalable, and easy to extend.

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

This principle is closely related to the concept of **separation of concerns**, which suggests that you should divide your programs into components, each addressing a separate concern.

- Violating SRP makes code: 

    - Hard to read (too many concerns mixed)
    
    - Hard to test (you must set up many things for one test)
    
    - Hard to change safely (a change for logging may break business logic)

Code Example:

To illustrate the single-responsibility principle and how it can help you improve your object-oriented design, say that you have the following FileManager class:

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

In this example, your FileManager class has two different responsibilities. It manages files using the .read() and .write() methods. It also deals with ZIP archives by providing the .compress() and .decompress() methods.

This class violates the single-responsibility principle because there is more than one reason for changing its implementation (file I/O and ZIP handling). This implementation also makes code testing and code reuse harder.

To fix this issue and make your design more robust, you can split the class into two smaller, more focused classes, each with its own specific concern:

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

Now, you have two smaller classes, each having only a single responsibility:

- FileManager takes care of managing a file.
- ZipFileManager handles the compression and decompression of a file using the ZIP format.

These two classes are smaller, so they’re more manageable. They’re also easier to reason about, test, and debug.

The concept of responsibility in this context may be pretty subjective. Having a single responsibility doesn’t necessarily mean having a single method. Responsibility isn’t directly tied to the number of methods but to the core task that your class is responsible for, depending on your idea of what the class represents in your code.