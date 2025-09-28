# Day 28 – Mixin Pattern

## 📄 Summary  
The Mixin Pattern allows adding behavior or properties to objects/classes by “mixing in” methods, rather than relying on inheritance. It helps share functionality across otherwise unrelated classes.

## 💡 Problem It Solves  
When multiple classes need the same behavior but don’t share a natural inheritance relationship, mixins let you compose shared methods without forcing a hierarchical relationship.

## 🌎 Real-world Mapping  
- UI components: different widgets might share logging, animation, or state utilities via mixins.  
- Domain models: e.g. an audit mixin that adds `createdAt` / `updatedAt` behavior to many entities.  
- Utility libraries: e.g. injecting event emitter methods into classes via mixin.

## 🛠 My Mini Implementation  
```javascript
class Vehicle {
  constructor(type) {
    this.type = type;
  }
}

const honkMixin = {
  honk() {
    console.log(`${this.type} goes “Beep beep!”`);
  }
};

const moveMixin = {
  move(speed) {
    console.log(`${this.type} moves at ${speed} mph.`);
  }
};

// Apply mixins to Vehicle prototype
Object.assign(Vehicle.prototype, honkMixin, moveMixin);

const car = new Vehicle("Car");
car.honk();           // Car goes “Beep beep!”
car.move(60);         // Car moves at 60 mph.
```