Certainly! Let's explore each design pattern with a brief explanation and a simple JavaScript example for each:

### Creational Design Patterns

#### Singleton Pattern
Ensures that a class has only one instance and provides a global point of access to it.
```javascript
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    return Singleton.instance;
  }
}

const instance1 = new Singleton();
const instance2 = new Singleton();

console.log(instance1 === instance2); // Output: true
```

#### Factory Method Pattern
Defines an interface for creating objects, but allows subclasses to alter the type of objects that will be created.
```javascript
class Product {
  constructor(name) {
    this.name = name;
  }
}

class ProductFactory {
  createProduct(name) {
    return new Product(name);
  }
}

const factory = new ProductFactory();
const product = factory.createProduct('Widget');

console.log(product.name); // Output: Widget
```

#### Abstract Factory Pattern
Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
```javascript
class AbstractFactory {
  createProductA() {}
  createProductB() {}
}

class ConcreteFactory1 extends AbstractFactory {
  createProductA() {
    return new ProductA1();
  }

  createProductB() {
    return new ProductB1();
  }
}

class ConcreteFactory2 extends AbstractFactory {
  createProductA() {
    return new ProductA2();
  }

  createProductB() {
    return new ProductB2();
  }
}

const factory1 = new ConcreteFactory1();
const productA1 = factory1.createProductA();
const productB1 = factory1.createProductB();

console.log(productA1.name); // Output: ProductA1
console.log(productB1.name); // Output: ProductB1
```

#### Builder Pattern
Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
```javascript
class Product {
  constructor(builder) {
    this.name = builder.name;
    this.price = builder.price;
  }
}

class ProductBuilder {
  constructor(name) {
    this.name = name;
  }

  withPrice(price) {
    this.price = price;
    return this;
  }

  build() {
    return new Product(this);
  }
}

const product = new ProductBuilder('Widget')
  .withPrice(10)
  .build();

console.log(product.price); // Output: 10
```

#### Prototype Pattern
Creates new objects based on an existing object through cloning.
```javascript
class Prototype {
  clone() {
    return Object.create(Object.getPrototypeOf(this));
  }
}

const prototype = new Prototype();
const cloned = prototype.clone();

console.log(cloned instanceof Prototype); // Output: true
```

### Structural Design Patterns

#### Adapter Pattern
Allows objects with incompatible interfaces to collaborate.
```javascript
class OldCalculator {
  constructor() {
    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          return term1 + term2;
        case 'subtract':
          return term1 - term2;
        default:
          return NaN;
      }
    };
  }
}

class NewCalculator {
  add(term1, term2) {
    return term1 + term2;
  }

  subtract(term1, term2) {
    return term1 - term2;
  }
}

class CalculatorAdapter {
  constructor() {
    this.calculator = new NewCalculator();
  }

  operations(term1, term2, operation) {
    switch (operation) {
      case 'add':
        return this.calculator.add(term1, term2);
      case 'subtract':
        return this.calculator.subtract(term1, term2);
      default:
        return NaN;
    }
  }
}

const adapter = new CalculatorAdapter();
console.log(adapter.operations(5, 3, 'add')); // Output: 8
```

#### Decorator Pattern
Allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class.
```javascript
class Pizza {
  getDescription() {
    return 'Plain pizza';
  }

  cost() {
    return 10;
  }
}

class CheeseDecorator extends Pizza {
  constructor(pizza) {
    super();
    this.pizza = pizza;
  }

  getDescription() {
    return `${this.pizza.getDescription()} with cheese`;
  }

  cost() {
    return this.pizza.cost() + 2;
  }
}

const pizza = new Pizza();
const cheesePizza = new CheeseDecorator(pizza);

console.log(cheesePizza.getDescription()); // Output: Plain pizza with cheese
console.log(cheesePizza.cost()); // Output: 12
```

#### Facade Pattern
Provides a simplified interface to a complex subsystem.
```javascript
class Subsystem1 {
  operation1() {
    return 'Subsystem1: operation1';
  }
}

class Subsystem2 {
  operation2() {
    return 'Subsystem2: operation2';
  }
}

class Facade {
  constructor() {
    this.subsystem1 = new Subsystem1();
    this.subsystem2 = new Subsystem2();
  }

  operation() {
    let result = 'Facade initializes subsystems:\n';
    result += this.subsystem1.operation1();
    result += ' | ';
    result += this.subsystem2.operation2();
    return result;
  }
}

const facade = new Facade();
console.log(facade.operation());
// Output:
// Facade initializes subsystems:
// Subsystem1: operation1 | Subsystem2: operation2
```

#### Proxy Pattern
Provides a surrogate or placeholder for another object to control access to it.
```javascript
class RealSubject {
  operation() {
    return 'RealSubject: operation';
  }
}

class Proxy {
  constructor(realSubject) {
    this.realSubject = realSubject;
  }

  operation() {
    return `Proxy: ${this.realSubject.operation()}`;
  }
}

const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);

console.log(proxy.operation()); // Output: Proxy: RealSubject: operation
```

### Behavioral Design Patterns

#### Chain of Responsibility Pattern
Passes a request along a chain of handlers, each processing a part of the request.
```javascript
class Handler {
  constructor(successor) {
    this.successor = successor;
  }

  handleRequest(request) {
    if (this.successor) {
      this.successor.handleRequest(request);
    }
  }
}

class ConcreteHandler1 extends Handler {
  handleRequest(request) {
    if (request === 'one') {
      console.log('ConcreteHandler1 handled the request');
    } else {
      super.handleRequest(request);
    }
  }
}

class ConcreteHandler2 extends Handler {
  handleRequest(request) {
    if (request === 'two') {
      console.log('ConcreteHandler2 handled the request');
    } else {
      super.handleRequest(request);
    }
  }
}

const handler1 = new ConcreteHandler1();
const handler2 = new ConcreteHandler2();

handler1.successor = handler2;

handler1.handleRequest('one'); // Output: ConcreteHandler1 handled the request
handler1.handleRequest('two'); // Output: ConcreteHandler2 handled the request
handler1.handleRequest('three'); // Output: (No output)
``

`

#### Iterator Pattern
Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
```javascript
class Iterator {
  constructor(items) {
    this.index = 0;
    this.items = items;
  }

  hasNext() {
    return this.index < this.items.length;
  }

  next() {
    return this.hasNext() ? this.items[this.index++] : null;
  }
}

const items = [1, 2, 3, 4, 5];
const iterator = new Iterator(items);

while (iterator.hasNext()) {
  console.log(iterator.next());
}
// Output:
// 1
// 2
// 3
// 4
// 5
```

#### Observer Pattern
Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
```javascript
class Subject {
  constructor() {
    this.observers = [];
  }

  addObserver(observer) {
    this.observers.push(observer);
  }

  notifyAll() {
    this.observers.forEach(observer => observer.update());
  }
}

class Observer {
  constructor(name) {
    this.name = name;
  }

  update() {
    console.log(`${this.name} has been updated.`);
  }
}

const subject = new Subject();
const observer1 = new Observer('Observer 1');
const observer2 = new Observer('Observer 2');

subject.addObserver(observer1);
subject.addObserver(observer2);

subject.notifyAll();
// Output:
// Observer 1 has been updated.
// Observer 2 has been updated.
```

These are just brief examples to illustrate each design pattern. Depending on your project's requirements, you might need to adapt these examples or combine multiple patterns to solve specific problems.
