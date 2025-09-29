# Day 29 – Decorator Pattern  

## 📄 Summary  
The **Decorator Pattern** lets you wrap an object with additional behavior, without modifying its structure.  

## 💡 Problem It Solves  
When objects require extra functionality, developers often reach for inheritance. The Decorator Pattern avoids subclass bloat by allowing runtime composition of behaviors.  

## 🌎 Real-world Mapping  
A coffee shop order system: you can start with a base coffee and then “decorate” it with milk, sugar, or flavors. Each decoration is applied independently, allowing many combinations without creating unique classes for each variation.  

## 🛠 My Mini Implementation  
```javascript
// Base component
class Coffee {
  cost() {
    return 5;
  }
  description() {
    return "Plain Coffee";
  }
}

// Decorator base
class CoffeeDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  cost() {
    return this.coffee.cost();
  }
  description() {
    return this.coffee.description();
  }
}

// Concrete decorators
class MilkDecorator extends CoffeeDecorator {
  cost() {
    return this.coffee.cost() + 2;
  }
  description() {
    return this.coffee.description() + ", Milk";
  }
}

class SugarDecorator extends CoffeeDecorator {
  cost() {
    return this.coffee.cost() + 1;
  }
  description() {
    return this.coffee.description() + ", Sugar";
  }
}

// Usage
let myCoffee = new Coffee();
myCoffee = new MilkDecorator(myCoffee);
myCoffee = new SugarDecorator(myCoffee);

console.log(myCoffee.description()); // "Plain Coffee, Milk, Sugar"
console.log("Cost: $" + myCoffee.cost()); // Cost: $8
```