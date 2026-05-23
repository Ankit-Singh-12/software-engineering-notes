# Encapsulation

Encapsulation is an OOPS concept in which data members and methods are combined into a single unit (class), and access to the data is restricted to improve security and control. 

Simple terms: “Keep data and the functions that work on that data together in one unit (a class), and control how the data is accessed or modified from outside.”

> **ProTip:** Encapsulation = wrapping + access control.

In OOP theory, encapsulation has two sides:

1. **Bundling:**

    Group related data (attributes) and behavior (methods) into a single unit → a class.

2. **Access control / data protection:**

    Restrict direct access to an object’s internal state and force others to use a controlled interface (methods, properties).

In Python:

- You still bundle attributes and methods inside classes.

- Access control is done by convention and name mangling, not by strict keywords like private/protected.

- You often use:

    - Public attributes/methods

    - “Protected” (convention) attributes/methods

    - “Private” (name-mangled) attributes/methods

    - Getters/setters or @property for controlled access.

## Access levels in Python

### 1. Public members

- **Syntax:** normal names (balance, deposit, get_balance)

- **Access:** can be accessed from anywhere (inside or outside the class).

- **Use when:** the attribute/method is part of the public interface of the class.

```python
class Account:
    def __init__(self, balance):
        self.balance = balance    # public
```

### 2. “Protected” members (single underscore)

- **Syntax:** _balance

- **Meaning:** “internal use only” by convention, but still accessible from outside.

- **Use when:** attribute should not be used directly by external code, but may be used in subclasses.

```python
class Account:
    def __init__(self, balance):
        self._balance = balance   # protected by convention
```

### 3. “Private” members (double underscore and name mangling)

- Syntax: __balance

- Python automatically transforms __balance into _ClassName__balance (this is name mangling).

- This:

    - Makes accidental access/override harder, especially in subclasses.

    - Is not true security, but prevents unintentional misuse.

> **Note:** **Name mangling** is the process in Python where private variables starting with double underscore (__) are internally renamed by Python to _ClassName__variableName to avoid accidental access or overriding.

```python
class Account:
    def __init__(self, balance):
        self.__balance = balance  # private (name-mangled)
```

Attempting obj.__balance fails; internally it’s _Account__balance.

## Getters, setters, and properties

Encapsulation is not just hiding; it’s controlled access. The classic pattern: keep data non-public, expose methods.

### Classic getters and setters

- **Getter** → read the value

- **Setter** → update the value with validation

```python
class Account:
    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance

    def set_balance(self, amount):
        if amount < 0:
            raise ValueError("Negative balance not allowed")
        self.__balance = amount
```

### Pythonic way: @property

Python prefers @property to keep usage clean while still enforcing rules.

```python
class Account:
    def __init__(self, balance):
        self.__balance = balance

    @property
    def balance(self):
        return self.__balance

    @balance.setter
    def balance(self, amount):
        if amount < 0:
            raise ValueError("Negative balance not allowed")
        self.__balance = amount
```
```python
acc = Account(100)
acc.balance       # calls getter
acc.balance = 50  # calls setter with validation
```

This is strong evidence of encapsulation: the internal representation is hidden, and only a controlled interface is exposed.

> **Note:** **@property** is a decorator in Python that allows a method to be accessed like an attribute. It is mainly used for encapsulation, validation, and controlled access to class data.

## Why encapsulation is used

- ### Data Protection / Integrity

    Encapsulation protects data from wrong or accidental changes.

    Example

    Imagine a bank account balance:
    ```python
    account.balance = -5000
    ```
    This should not happen.

    With encapsulation, we control updates and prevent invalid values.

    > Simple Interview Line : Encapsulation protects object data from invalid or accidental modification.

- ### Validation and Business Rules

    Before changing data, we can check whether the value is valid.

    Example

    ```python
    if salary < 0:
        print("Invalid salary")
    ```

    So encapsulation allows us to apply rules before updating data.

    > Simple Interview Line: Encapsulation allows validation before modifying data.

- ### Abstraction / Hiding Complexity

    Users only see simple methods.

    They do not need to know:

    - how data is stored
    - how calculations happen internally

    Example

    User simply does:

    ```python
    car.start()
    ```

    They don't care how the engine internally works.

    > Simple Interview Line: Encapsulation hides internal implementation and exposes only necessary functionality.

- ### Maintainability and modularity

    If internal code changes, outside code usually remains unaffected.

    Example

    Initially:
    ```python
    self.salary
    ```
    Later you change it to:
    ```python
    self.monthly_salary
    ```

    If users access it through methods/properties, their code still works.
    
    Explanation:
    Instead of exposing the real variable directly, we expose a controlled interface using @property.

    ```python
    class Employee:
    def __init__(self):
        self._monthly_salary = 50000

    @property
    def salary(self):
        return self._monthly_salary
    ```
    Even if `self.salary` is internally changed to `self._monthly_salary`, users can still access it using the salary property.

    > Simple Interview Line: Encapsulation makes code easier to maintain because internal changes do not affect external code.

- ### Flexibility

    We can improve or modify internal logic later without changing how users interact with the class.

    Example

    Today:
    ```python
    return self._salary
    ```

    Tomorrow:
    ```python
    return self._salary - tax
    ```

    User still does:
    ```python
    emp.salary
    ```

    No external code changes.

    > Simple Interview Line: Encapsulation provides flexibility to change implementation without breaking existing code.

## Other interview-adjacent terms around encapsulation

- **Data hiding:** Hiding internal data from outside direct access; achieved using non-public attributes and controlled access.

- **Abstraction vs encapsulation**

    - **Encapsulation:** how you bundle and hide data + methods in a class.

    - A**bstraction:** what you expose as a high-level interface, hiding unnecessary details.

- **Interface / API of a class:** The public methods and properties that outside code is supposed to use; result of encapsulation and abstraction working together.

- **Immutability:** Another way to protect state: don’t let it change at all once created; this is a design choice related to encapsulation.

### Summary

> “Encapsulation is an OOP concept where we bundle data and the methods that operate on that data into a single unit, usually a class, and restrict direct access to the internal state. In Python, we achieve this by using naming conventions like _ and __ for non-public attributes, and by exposing controlled access through methods or properties, which lets us validate data, protect object integrity, and change the internal implementation without breaking external code.”

