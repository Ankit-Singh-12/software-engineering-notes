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

- **Cohesion** → “How related things are INSIDE a class”
- **Coupling** → “How dependent classes are on EACH OTHER”

> **Summary:** Cohesion is the degree to which methods and data inside a class are related to each other and work together toward a single responsibility. High cohesion is preferred because it improves maintainability and readability.

## Coupling

> Coupling is the degree of dependency between two classes or modules.

**Coupling means how much one class or module depends on another class or module.**

- If changing one class forces changes in another class, the classes are said to be **tightly coupled**.

- If classes can work independently with minimal dependency, they are **loosely coupled**.

#### **Tight Coupling vs Loose Coupling**

| Tight Coupling | Loose Coupling |
|---|---|
| Classes strongly depend on each other | Classes depend less on each other |
| Hard to modify | Easy to modify |
| Hard to test | Easy to test |
| Less flexible | More flexible |
| Changes can break many classes | Fewer side effects |
