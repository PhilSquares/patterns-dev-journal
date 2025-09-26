# Day 26 – Strategy Pattern

## 📄 Summary
The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It lets the algorithm vary independently from the clients that use it.

## 💡 Problem It Solves
Avoids large conditionals when different behaviors are needed. Instead, behaviors are isolated in their own strategy objects and can be swapped dynamically.

## 🌎 Real-world Mapping
A payment system: some customers pay with credit card, others with PayPal, others with cryptocurrency. The checkout process (context) is the same, but the payment strategy changes.

## 🛠 My Mini Implementation
```javascript
// Strategy implementations
class CreditCardPayment {
  pay(amount) {
    console.log(`Paid $${amount} with Credit Card.`);
  }
}

class PayPalPayment {
  pay(amount) {
    console.log(`Paid $${amount} with PayPal.`);
  }
}

class CryptoPayment {
  pay(amount) {
    console.log(`Paid $${amount} with Cryptocurrency.`);
  }
}

// Context
class PaymentContext {
  constructor(strategy) {
    this.strategy = strategy;
  }

  setStrategy(strategy) {
    this.strategy = strategy;
  }

  checkout(amount) {
    this.strategy.pay(amount);
  }
}

// Usage
const payment = new PaymentContext(new CreditCardPayment());
payment.checkout(50); // Paid $50 with Credit Card.

payment.setStrategy(new PayPalPayment());
payment.checkout(75); // Paid $75 with PayPal.

payment.setStrategy(new CryptoPayment());
payment.checkout(100); // Paid $100 with Cryptocurrency.
```