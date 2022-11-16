# Dependency Inversion Principle (DIP)
DIP is targeted towards loosely couping software modules so that high-level modules (which provide complex logic) are easily reusable and unaffected by changes in low-level modules(which provide utility features).
This principle states that:
1. High-level modules should not import anything from low-level modules. Both should depend on abstractions (for example, interfaces).
2. Abstractions should be independient of details. Details (concrete implementations) should depend on abstractions.\
In plain terms, this principle states that your classes should depend upon interfaces or abstract classes instead of concrete classes and functions. This makes your classes open to extension, following the open-closed principle.\
Let's look at an example. When building a store, you would want your store to make use of a payment gateway like stripe or any other preferred payment method. You might write your code tightly coupled to that API without thinking of the future.\
But then what if you discover another payment gateway that offers far better service, let's say PayPal? Then it becomes a struggle to switch from Stripe to Paypal, which should not be an issue in programming and software design.

```javascript
class Store {
    constructor(user) {
        this.stripe = new Stripe(user);
    }

    purchaseBook(quantity, price) {
        this.stripe.makePayment(price * quantity);
    }

    purchaseCourse(quantity, price) {
        this.stripe.makePayment(price * quantity);
    }
}

class Stripe {
    constructor(user) {
        this.user = user;
    }

    makePayment(amountInDollars) {
        console.log(`${this.user} made payment of ${amountInDollars`);
    }
}
```

Considering the example above, you'll notice that if you change the payment gateway, you won't just need to add the class - you'll also need to make changes to the ```Store```class. This does not just go against the Dependency Inversion Principle but also against the open-closed principle.\
To fix this, you must ensure that your classes depend upon interfaces or abstract classes instead of concrete classes and functions. For this example, this interface will contain all the behavior tou want tour API to have and doesn't depend on anything. It serves as an intermediary between the high-level and low-level modules.

```javascript
class Store {
    constructor(paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    purchaseBook(quantity, price) {
        this.paymentProcessor.pay(price * quantity);
    }

    purchaseCourse(quantity, price) {
        this.paymentProcessor.pay(price * quantity);
    }
}

class StripePaymentProcessor {
    constructor(user) {
        this.stripe = new Stripe(user);
    }

    pay(amountInDollars) {
        this.stripe.makePayment(amountInDollars);
    }
}

class Stripe {
    constructor(user) {
        this.user = user;
    }

    makePayment(amountInDollars) {
        console.log(`${this.user} made payment of ${amountInDollars}`);
    }
}

let store = new Store(new StripePaymentProcessor('John Doe'));
store.purchaseBook(2, 10);
store.purchaseCourse(1, 15);
```

In the code above, you will notice that the ```StripePaymentProcessor``` class is an interface between the ```Store```class and the ```Stripe``` class. In a situation where you need to make use of PayPal, all you have to do is create a ```PayPalProcessor``` which would work with the ```PayPal``` class, and everything will work without affecting the ```Store``` class.

```javascript
class Store {
    constructor(paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    purchaseBook(quantity, price) {
        this.paymentProcessor.pay(quantity * price);
    }

    purchaseCourse(quantity, price) {
        this.paymentProcessor.pay(quantity * price);
    }
}

class PayPalPaymentProcessor {
    constructor(user) {
        this.user = user;
        this.paypal = new PayPal();
    }

    pay(amountInDollars) {
        this.paypal.makePayment(this.user, amountInDollars);
    }
}

class PayPal {
    makePayment(user, amountInDollars) {
        console.log(`${user} made payment of ${amountInDollars}`);
    }
}

let store = new Store(new PayPalPaymentProcessor('John Doe'));
store.purchaseBook(2, 10);
store.purchaseCourse(1, 15);
```

You will also notice that this follows the Liskov Substitution Principle because you can replace it with other implementations of the same interface without breaking your application.