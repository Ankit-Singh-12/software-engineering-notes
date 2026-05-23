# Object Oriented Programming

OOPS is a programming style in which we create classes and objects to represent real-world things, making code cleaner, reusable, and easier to manage.

For example, an object could represent a person with properties like a name, age, and address and behaviors such as walking, talking, breathing, and running. Or it could represent an email with properties like a recipient list, subject, and body and behaviors like adding attachments and sending.

Put another way, object-oriented programming is an approach for modeling concrete, real-world things, like cars, as well as relations between things, like companies and employees or students and teachers. OOP models real-world entities as software objects that have some data associated with them and can perform certain operations.

## Advantages of OOP

- Provides a clear structure to programs
- Makes code easier to maintain, reuse, and debug
- Helps keep your code DRY (Don't Repeat Yourself)
- Allows you to build reusable applications with less code

## Classes and Objects

### Class

A class is a blueprint or template used to create objects. A class defines what an object should look like.

It defines:
- variables (attributes/properties)
- functions (methods/behaviors)

A class does not hold actual values for different users or items until objects are created from it.

#### Example

```python
class Employee:

    # Class Attribute
    company = "Pear"

    def __init__(self, name, salary):

        # Instance Attributes
        self.name = name
        self.salary = salary

    def display_details(self):
        print("Company:", Employee.company)
        print("Name:", self.name)
        print("Salary:", self.salary)
```

> **Note:** Class Attribute: This variable belongs to the class itself. It is shared among all objects. Every employee object will have the same company name unless changed.

> **Note:** Instance Attributes: These belong to each object separately. Different employees can have different names and salaries.

### Objects

An object is a real instance of a class. Objects store actual values inside instance attributes and can access class attributes and methods.

#### Example

```python
# Creating Objects
emp1 = Employee("Ankit", 50000)
emp2 = Employee("Rahul", 70000)

emp1.display_details()

print()

emp2.display_details()
```

#### Output 
```python
Company: Pear
Name: Ankit
Salary: 50000

Company: Pear
Name: Rahul
Salary: 70000
```

Here:

- emp1 and emp2 are objects
- Both are created from the Employee class

## Four Pillars of OOP

Object-Oriented Programming (OOPS) is mainly based on four important concepts called the four pillars of OOPS:

1. Encapsulation: Encapsulation is the process of binding data and methods together into a single unit (class) and restricting direct access to some data.

2. Abstraction: Abstraction means hiding internal implementation details and showing only essential features to the user.

3. Inheritance: Inheritance is the process where one class acquires properties and methods of another class.

4. Polymorphism: Polymorphism means one method or function can behave differently in different situations.