Design patterns are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a particular design problem in your code. Here's a brief overview of some of the most important design patterns categorized by their purpose:

### Creational Patterns
These patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.
- **Singleton**: Ensures a class has <mark style="background: #FFF3A3A6;">only one instance and provides a global point</mark> of access to it.
- **Factory Method**: Defines an interface for creating an object, but lets subclasses alter the type of objects that will be created.
- **Abstract Factory**: Lets you produce families of related objects without specifying their concrete classes.
- **Builder**: Allows constructing complex objects step by step. The pattern allows producing different types and representations of an object using the same construction code.
- **Prototype**: Lets you copy existing objects without making your code dependent on their classes.

### Structural Patterns
These patterns explain how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.
- **Adapter (or Wrapper)**: Allows objects with incompatible interfaces to collaborate.
- **Bridge**: Separates an object’s interface from its implementation, so the two can vary independently.
- **Composite**: Lets you compose objects into tree structures and then work with these structures as if they were individual objects.
- **Decorator**: Lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.
- **Facade**: Provides a simplified interface to a library, a framework, or any other complex set of classes.
- **Flyweight**: Reduces the cost of creating and manipulating a large number of similar objects.
- **Proxy**: Lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

### Behavioral Patterns
These patterns are all about class's objects communication. They help ensure that objects are well-structured and that the communication is streamlined.
- **Chain of Responsibility**: Lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.
- **Command**: Turns a request into a stand-alone object that contains all information about the request. This transformation lets you parameterize methods with different requests, delay or queue a request’s execution, and support undoable operations.
- **Interpreter**: Provides a way to evaluate language grammar or expression. This pattern is used in SQL parsing, symbol processing engine etc.
- **Iterator**: Lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).
- **Mediator**: Reduces chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.
- **Memento**: Lets you save and restore the previous state of an object without revealing the details of its implementation.
- **Observer**: Lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.
- **State**: Lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.
- **Strategy**: Lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.
- **Template Method**: Defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.
- **Visitor**: Lets you separate algorithms from the objects on which they operate.
