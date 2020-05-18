---
title: Design Pattern
date: 2020-05-17 22:30:06
categories:
  - design pattern
tags:
  - design pattern
type: "categories"
cover: "/img/designpattern/factory-method.png"
---

### Factory Method Pattern

```typescript
/**
 * The Creator class declares the facotor method that is supposed to
 * return an object of a Product class. The Creator's subclass usually
 * provide the implementation of this method
 *
 */

interface Product {
  operation(): string;
}

// TS allow us to define an abstract class using keyword abstract
abstract class Creator {
  /**
   * Note that the Creator may also provide some default implementation
   * of the factory method
   */
  public abstract factoryMethod(): Product;

  /**
   * Also note that, despite its name, the Creator's primary responsibility is not creating products.
   * Usually, it contains some core business logic that relies on Product object,
   * returned by the factory method. Subclasses can indirectly change that business logic
   * by overfiding the factory method and returning a different type of product from it.
   */
  public someOperation(): string {
    // Call the factory method to create a Produc object.
    const product = this.factoryMethod();
    return `Creator: The same creator's code has just worked with ${product.operation()}`;
  }
}

/**
 * Concrete Creators override the factory method in order to change
 * the resulting product's type.
 */
class ConcreteCreator1 extends Creator {
  /**
   * Note that the signature of the method still uses the abstract product
   * type, even though the concrete product is actually returned from the method.
   * This way the Creator can stay independent of conrete product classes.
   */
  public factoryMethod(): Product {
    return new ConcreteProduct1();
  }
}

class ConcreteCreator2 extends Creator {
  public factoryMethod(): Product {
    return new ConcreteProduct2();
  }
}

/**
 * Concrete Products provide various implementations of the Product interface.
 */
class ConcreteProduct1 implements Product {
  public operation(): string {
    return "Result of the ConreteProduct1";
  }
}

class ConcreteProduct2 implements Product {
  public operation(): string {
    return "Result of the ConcreteProduct2";
  }
}

/**
 * The client code works with an instance of a concrete creator, albeit through its base interface.
 * As long as the client keeps working with the creator via the base interface,
 * you can pass it any creator's subclass.
 */
function clientCode(creator: Creator) {
  console.log(
    "Client: I'm not aware of the creator's class, but it still works"
  );
  console.log(creator.someOperation());
}

const creator1 = new ConcreteCreator1();
const creator2 = new ConcreteCreator2();

clientCode(creator1);
clientCode(creator2);
```

### Abstract Factory

```typescript
/**
 * The Abstract Factory interface declares a set of methods that return
 * different abstract products. These products are called a family and are
 * related by a high-level theme or concept. Products of one family are usually
 * able to collaborate among themeselves. A family of products may have several variants,
 * but the products of one variant are incompatible with products of another.
 */
interface AbstractFactory {
  createProductA(): AbstractProductA;
  createProductB(): AbstractProductB;
}

/**
 * Concrete Factories product a family of products that belong to a single
 * variant. The factory guarantees that resulting products are compatible.
 * Note that signatures of the Concrete Factory's methods return an abstract
 * product, while inside the method a concrete product is instantiated.
 */
class ConcreteFactory1 implements AbstractFactory {
  public createProductA(): AbstractProductA {
    return new ConcreteProductA1();
  }

  public createProductB(): AbstractProductB {
    return new ConcreteProductB1();
  }
}

class ConcreteFactory2 implements AbstractFactory {
  public createProductA(): AbstractProductA {
    return new ConcreteProductA2();
  }

  public createProductB(): AbstractProductB {
    return new ConcreteProductB2();
  }
}

/**
 * Each distinct product of a product family should have a base interface.
 * All variants of the product must implement this interface.
 */
interface AbstractProductA {
  usefulFunctionA(): string;
}

/**
 * These Concrete Products are created by corresponding Concrete Factories.
 */
class ConcreteProductA1 implements AbstractProductA {
  public usefulFunctionA(): string {
    return "The result of the product A1.";
  }
}

class ConcreteProductA2 implements AbstractProductA {
  public usefulFunctionA(): string {
    return "The result of the product A2.";
  }
}

/**
 * Here's the base interface of another product. All products can interact
 * with each other, but proper interaction is possible only between products
 * of the same concrete variant.
 */
interface AbstractProductB {
  /**
   * Product B is able to do its own thing...
   */
  usefulFunctionA(): string;

  /**
   * ...but it also can collaborate with the ProductA
   *
   * The Abstract Factory makes sure that all products it creates are of
   * the same variant and thus, compatible.
   */
  anotherUsefulFunctionB(collaboration: AbstractProductA): string;
}

/**
 * These Concrete Products are created by corresponding Concrete Factories.
 */
class ConcreteProductB1 implements AbstractProductB {
  public usefulFunctionA(): string {
    return "The result of the product B1.";
  }

  public anotherUsefulFunctionB(collaboration: AbstractProductA): string {
    const result = collaboration.usefulFunctionA();
    return `The result of the B1 collaborating with (${result})`;
  }
}

class ConcreteProductB2 implements AbstractProductB {
  public usefulFunctionA(): string {
    return "The result of the product B2";
  }

  public anotherUsefulFunctionB(collaboration: AbstractProductA): string {
    const result = collaboration.usefulFunctionA();
    return `The result of the B2 collaborating with the (${result})`;
  }
}

/**
 * The client code works with factories and products only through abstract
 * types: AbstractFactory and AbstractProduct. This lets you pass any
 * factory or product subclass to the client code without breaking it.
 */
function clientCode(factory: AbstractFactory) {
  const productA = factory.createProductA();
  const productB = factory.createProductB();

  console.log(productB.usefulFunctionA());
  console.log(productB.anotherUsefulFunctionB(productA));
}

/**
 * The client code can work with any concrete factory class.
 */
console.log("Client: Testing client code with the first factory type...");
clientCode(new ConcreteFactory1());

console.log("");

console.log(
  "Client: Testing the same client code with the second factory type..."
);
clientCode(new ConcreteFactory2());
```

### Strategy

```typescript
/**
 * The Context defines the interface of interest to clients.
 */
class Context {
  /**
   * @type {Strategy} The Context maintains a reference to one of the Strategy
   * objects. The Context does not know the concrete class of a strategy.
   * It should work with all strategy via the Strategy interface.
   */
  private strategy: Strategy;

  /**
   * Usually, the Context accepts a strategy through the constructor,
   * but also provides a setter to change it at runtime.
   */
  constructor(strategy: Strategy) {
    this.strategy = strategy;
  }

  /**
   * Usually, the Context allows replacing a Strategy object at runtime.
   */
  public setStrategy(strategy: Strategy) {
    this.strategy = strategy;
  }

  /**
   * The Context delegates some work to the Strategy object instead of
   * implementing multiple versions of the algorithm on its own.
   */
  public doSomeBusinessLogin(): void {
    console.log(
      "Context: Sorting data using the strategy (not sure how it do it)"
    );
    const result = this.strategy.doAlgorithm(["a", "b", "c", "d", "e"]);
    console.log(result.join(","));
  }
}

/**
 * The Strategy interface declares operation common to all supported
 * versions of some algorithm.
 *
 * The Context uses this interface to call the algorithm defined by Concrete Strategies.
 */
interface Strategy {
  doAlgorithm(data: string[]): string[];
}

/**
 * Concrete Strategies implement the algorithm while following the base
 * Strategy interface. The interface makes them interchangeable in the Context.
 */
class ConcreteStrategyA implements Strategy {
  public doAlgorithm(data: string[]): string[] {
    return data.sort();
  }
}

class ConcreteStrategyB implements Strategy {
  public doAlgorithm(data: string[]): string[] {
    return data.reverse();
  }
}

/**
 * The client code picks a concrete strategy and passes it to the context.
 * The client should be aware of the difference between strategies in order
 * to make the right choice.
 */
const context = new Context(new ConcreteStrategyA());
console.log("Client: Strategy is set to normal sorting");
context.doSomeBusinessLogin();

console.log("");

console.log("Client: Strategy is set to reverse sorting");
context.setStrategy(new ConcreteStrategyB());
context.doSomeBusinessLogin();
```

### Template Method

```typescript
/**
 * The Abstract Class defines a template method that contains a skeleton
 * of some algorithm, composed of calls to abstract primitive operations.
 *
 * Concrete subclasses should implement these operations, but leave the template
 * method itself intact.
 */
abstract class AbstractClass {
  /**
   * The template method defines the skeleton of an algorithm.
   */
  public templateMethod(): void {
    this.baseOperation1();
    this.requiredOperations1();
    this.baseOperation2();
    this.hook1();
    this.requiredOperations2();
    this.baseOperation3();
    this.hook2();
  }

  /**
   * These operations already have implementations.
   */
  protected baseOperation1(): void {
    console.log("AbstractClass says: I am doing the bulk of the work");
  }

  protected baseOperation2(): void {
    console.log(
      "AbstractClass says: But I let subclass override some operations"
    );
  }

  protected baseOperation3(): void {
    console.log(
      "AbstractClass says: But I am doing the bulk of the work anyway."
    );
  }

  /**
   * These operations have to be implemented in subclasses.
   */
  protected abstract requiredOperations1(): void;

  protected abstract requiredOperations2(): void;

  /**
   * These are hooks. Subclasses may override them, but it's not mandatory
   * since the hooks already have default (but empty) implementation. Hooks
   * provide additional extension points in some crucial places of the algorithm.
   */
  protected hook1(): void {}

  protected hook2(): void {}
}

/**
 * Concrete classes have to implement all abstract operations of the base class.
 * They can also override some operations with a default implementation.
 */
class ConcreteClass1 extends AbstractClass {
  protected requiredOperations1(): void {
    console.log("ConcreteClass1 says: Implemented Operation1");
  }

  protected requiredOperations2(): void {
    console.log("Concrete says: Implemented Operation2");
  }
}

/**
 * Usually, concrete classes override only a fraction of base class's opertions
 */
class ConcreteClass2 extends AbstractClass {
  protected requiredOperations1() {
    console.log("ConcreteClass2 says: Implemented Operation1");
  }

  protected requiredOperations2() {
    console.log("ConcreteClass2 says: Implemented Operation2");
  }

  protected hook1(): void {
    console.log("ConcreteClass2 says: Override Hook1");
  }
}

/**
 * The client code calls the template method to execute the algorithm.
 * Client code does not have to know the concrete class of an object it works with,
 * as long as it works with objects through the interface of their base class.
 */
function clientCode(abstractClass: AbstractClass) {
  abstractClass.templateMethod(); // Execute template method
}

console.log("Same client code can work with different subclasses");
clientCode(new ConcreteClass1());

console.log("");

console.log("Same client code can work with different subclasses:");
clientCode(new ConcreteClass2());
```
