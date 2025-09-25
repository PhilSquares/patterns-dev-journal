# Day 25 – Prototype Pattern

## 📄 Summary  
Prototype Pattern in JavaScript leverages the language’s prototypal inheritance to share methods and behavior across instances, enabling memory-efficient and flexible object designs.

## 💡 Problem It Solves  
If every instance holds its own copy of all methods, memory and duplication become issues. The Prototype Pattern ensures methods are shared via the prototype, and lets you patch or extend behavior later.

## 🌎 Real-world Mapping  
- A `Person` class where every person should have a `greet()` method — you define it once on the prototype, not per person.  
- Extending a `Widget` class after deployment: adding a `refresh()` method to the widget prototype causes all existing widgets to gain it.  
- Using `Object.create(protoObj)` to make lightweight instances with a shared prototype.

## 🛠 My Mini Implementation  
```javascript
// Using ES6 class
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
    console.log(`${this.name} says: Woof!`);
  }
}

// Instances
const dog1 = new Dog("Daisy");
const dog2 = new Dog("Max");

dog1.bark(); // Daisy says: Woof!
dog2.bark(); // Max says: Woof!

// Extend prototype after creation
Dog.prototype.play = function() {
  console.log(`${this.name} is playing`);
};

dog1.play(); // Daisy is playing
dog2.play(); // Max is playing

// Manual prototype chaining example
const animal = {
  eat() {
    console.log(`${this.name} is eating`);
  }
};

const cat = Object.create(animal);
cat.name = "Whiskers";
cat.meow = function() {
  console.log(`${this.name} meows`);
};

cat.eat();  // Whiskers is eating
cat.meow(); // Whiskers meows
```

⚠️ Tip: Avoid mutating shared prototype state (e.g. shared arrays) unless intentional.