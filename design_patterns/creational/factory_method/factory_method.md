# Factory Method

Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

At the core of the Factory Method pattern is the concept of delegation. Instead of the client code directly creating objects, it delegates the responsibility to a Factory Method.

This method resides in an abstract Creator class or interface, defining an interface for creating objects. Concrete subclasses of the Creator implement this Factory Method, allowing them to create specific instances of objects. This delegation promotes loose coupling between the client code and the objects it uses, enhancing flexibility and maintainability.

### Problem

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the Truck class.

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

Great news, right? But how about the code? At present, most of your code is coupled to the Truck class. Adding Ships into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app’s behavior depending on the class of transportation objects.

### Solution