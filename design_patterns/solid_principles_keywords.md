## Cohesion

> Cohesion is the measure of how well the methods and data of a class belong together.

Cohesion means how strongly related the things inside a class or module are.
A class is said to have high cohesion when all its methods and variables work together for one specific purpose.

#### **High Cohesion vs Low Cohesion**

| High Cohesion               | Low Cohesion                        |
| --------------------------- | ----------------------------------- |
| One focused responsibility  | Multiple unrelated responsibilities |
| Easy to maintain            | Hard to maintain                    |
| Easy to understand          | Confusing                           |
| Reusable                    | Difficult to reuse                  |
| Changes affect fewer things | Changes break many things           |

**Example of Low Cohesion**

```python
class Employee:
    def calculate_salary(self):
        pass

    def save_to_database(self):
        pass

    def send_email(self):
        pass

    def generate_pdf_report(self):
        pass
```

This class is doing too many unrelated jobs:

- salary calculation
- database handling
- email sending
- PDF generation

This is low cohesion.

**Example of High Cohesion**

```python
class SalaryCalculator:
    def calculate_salary(self):
        pass

    def calculate_bonus(self):
        pass

    def calculate_tax(self):
        pass
```

All methods are related to salary calculation.

This is high cohesion.

#### **Relation Between Cohesion and SRP**

Cohesion is strongly connected to the Single Responsibility Principle (SRP).

- High cohesion → usually follows SRP
- Low cohesion → usually violates SRP

High cohesion is GOOD

Because:
- code becomes cleaner
- debugging becomes easier
- testing becomes easier
- maintenance becomes easier

#### **Difference Between Cohesion and Coupling**

| Cohesion                    | Coupling                     |
| --------------------------- | ---------------------------- |
| Relationship within a class | Relationship between classes |
| Focuses on one class/module | Focuses on dependencies      |
| High cohesion is good       | Low coupling is good         |
| Measures focus/responsibility      | Measures dependency         |


- **Cohesion** → “How related things are INSIDE a class”
- **Coupling** → “How dependent classes are on EACH OTHER”

> **Summary:** Cohesion is the degree to which methods and data inside a class are related to each other and work together toward a single responsibility. High cohesion is preferred because it improves maintainability and readability.

## Coupling

> Coupling is the degree of dependency between two classes or modules.

**Coupling means how much one class or module depends on another class or module.**

- If changing one class forces changes in another class, the classes are said to be **tightly coupled**.

- If classes can work independently with minimal dependency, they are **loosely coupled**.

### **Tight Coupling vs Loose Coupling**

**Tight Coupling**

If a charger works with only one specific phone model, then:
- changing the phone means changing the charger too

This is **tight coupling**.

---

**Loose Coupling**

If the charger uses USB-C and works with many devices:
- phones
- tablets
- earbuds

Then devices are more independent.

This is **loose coupling**.

| Tight Coupling | Loose Coupling |
|---|---|
| Classes strongly depend on each other | Classes depend less on each other |
| Hard to modify | Easy to modify |
| Hard to test | Easy to test |
| Less flexible | More flexible |
| Changes can break many classes | Fewer side effects |

#### Example of Tight Coupling

```python
class MySQLDatabase:
    def connect(self):
        print("Connected to MySQL")


class UserService:
    def __init__(self):
        self.db = MySQLDatabase()

    def get_user(self):
        self.db.connect()
```

Problem: UserService directly creates MySQLDatabase.

If later you want:

- PostgreSQL
- MongoDB
- Mock database for testing

you must modify UserService.

This is tight coupling.

#### Example of Loose Coupling

```python
class Database:
    def connect(self):
        pass


class MySQLDatabase(Database):
    def connect(self):
        print("Connected to MySQL")


class UserService:
    def __init__(self, db):
        self.db = db

    def get_user(self):
        self.db.connect()


db = MySQLDatabase()
service = UserService(db)
```

Now UserService does not care which database is used.

This is loose coupling.

Why Loose Coupling is Better

Loose coupling helps:

- scalability
- maintainability
- testing
- reusability
- flexibility

How to Reduce Coupling:

1. Use Abstraction

    Depend on interfaces or abstract classes instead of concrete classes.

2. Use Dependency Injection

    Pass dependencies from outside instead of creating them inside the class.

3. Use Composition

    Use objects inside classes instead of deep inheritance chains.

4. Follow SOLID Principles

    Especially:

    - DIP (Dependency Inversion Principle)
    - ISP (Interface Segregation Principle)

Best Design Rule

> High Cohesion + Low Coupling = Good Software Design

This is one of the most important software design concepts.

More: 

separation of concerns, 
Behavioral subtyping, is-a relationship, polymorphism correctness, Fat interface, role interfaces, duck typing, ABC. Dependency Injection (DI), Inversion of Control (IoC), loose coupling.

Cohesion — How focused a class is on a single responsibility (high cohesion = good)

Coupling — How dependent classes are on each other (low coupling = good)

Abstraction — Hiding implementation details via abstract classes/interfaces

Polymorphism — Subclass objects behaving as parent type (key for OCP & LSP)

Duck Typing — Python's way of saying "if it walks like a duck, it is a duck" — supports ISP naturally

Dependency Injection (DI) — Passing dependencies from outside instead of creating inside (DIP)

Inversion of Control (IoC) — The control of creating objects is inverted to an external system

ABC (Abstract Base Class) — Python's abc.ABC module used to define interfaces

Design Patterns — SOLID is the foundation; patterns like Factory, Strategy, Observer build on top of SOLID

“Fat interface” vs “lean interface”

“Client‑specific interface”

“ABC (Abstract Base Class)”

“Protocol / duck typing”

“Substitutability”

“Behavioral contract”

“Design by contract”

“Inheritance vs composition”

“Strategy pattern”

“Plug‑in architecture”

“Extensibility”

