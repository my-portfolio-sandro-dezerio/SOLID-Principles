# Open Closed Principle (OCP)
OCP states that software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.\
This definition can be confusing, but an example and further clarification will help you understand.\
THere are two primary attributes in the OCP:
- It is <b>open</b> for extension - This means you can extend what the module can do.
- It is <b>closed</b> for modification - This means you cannot change the source code, even though you can extend the behavior of a module or entity.\
COP menas that a class, module, function, and other entities can extend their behavior without modifying their source code. In other words, an entity should be extendable without modifying the entity itself. How?\
For example, suppose you have an array of ```ìceCreamFlavours```, which contains a list of possible flavors. In the ```makeIceCream``` class, a ```make()```method will check if a particular flavor exists and logs a message.
```javascript
const iceCreamFlavors = ['chocolate', 'vanilla'];

class makeIceCream {
    constructor(flavor) {
        this.flavor = flavor;
    }

    make() {
        if (iceCreamFlavors.indexOf(this.flavor) > -1) {
            console.log('Great success. You now have ice cream.');
        } else {
            console.log('Epic fail. No ice cream for you.');
        }
    }
}
```
The code above fails the OCP principle. Why? Well, because the code above is not open to an extension, meaning for you to add new flavors, you would need to directly edit the ```ìceCreamFlavors``` array. This means that the code is no longer closed to modification.\
To fix this, you would need an extra class or entity to handle addition, so you no longer need to modify the code directly to make any extension.

```javascript
const iceCreamFlavors = ['chocolate', 'vanilla'];

class makeIceCream {
    constructor(flavor) {
        this.flavor = flavor;
    }
    make() {
        if (iceCreamFlavors.indexOf(this.flavor) > -1) {
            console.log('Great success. You now have ice cream.');
        } else {
            console.log('Epic fail. No ice cream for you.');
        }
    }
}

class addIceCream {
    constructor(flavor) {
        this.flavor = flavor;
    }
    add() {
        iceCreamFlavors.push(this.flavor);
    }
}
```

Here, we've added a new class - ```addIceCream``` - to handle addition to the ``iceCreamFlavors``` array using the ```add()``` method. This means your code is closed to modification but open to an extension because you can add new flavors without directly affecting the array.

```javascript
let addStrawberryFlavor = new addIceCream('strawberry');
addStrawberryFlavor.add();
makeStrawberryIceCream.make();
```

Also, notice that SRP is in place because you created a new class.