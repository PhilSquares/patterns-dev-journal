# Day 27 – Proxy Pattern

## 📄 Summary  
The Proxy Pattern provides a wrapper around a target object to intercept and control operations like property reads or writes, method calls, or access. It lets you inject additional logic (validation, logging, caching) in a transparent way.

## 💡 Problem It Solves  
When you need to augment, guard, or monitor interactions with an object (but don’t want to modify the object itself), Proxy acts as an intermediary that sits between the client and target object.

## 🌎 Real-world Mapping  
- Wrapping a configuration object to validate or sanitize assignments.  
- Logging all access to sensitive or critical data models.  
- Lazy initialization: e.g. wrapping a heavy resource so it’s only loaded when first accessed.  
- Virtual proxy: representing remote or expensive resources locally via a proxy.

## 🛠 My Mini Implementation  
```javascript
// target object
const person = {
  name: "John Doe",
  age: 42,
  nationality: "American",
};

// proxy wrapper
const personProxy = new Proxy(person, {
  get(target, prop) {
    if (!(prop in target)) {
      console.warn(`Property "${prop}" does not exist on target.`);
      return undefined;
    }
    console.log(`GET: ${prop} → ${target[prop]}`);
    return target[prop];
  },

  set(target, prop, value) {
    if (prop === "age" && typeof value !== "number") {
      console.error(`Invalid type for age: must be a number. Received ${typeof value}`);
      return false;  // stop the assignment
    }
    console.log(`SET: ${prop} from ${target[prop]} → ${value}`);
    target[prop] = value;
    return true;  // signal success
  }
});

// usage
console.log(personProxy.name);    // logs and returns “John Doe”  
personProxy.age = 43;             // logs “SET: age from 42 → 43”  
personProxy.age = "forty";        // logs error, rejects assignment  
personProxy.location = "USA";  
```