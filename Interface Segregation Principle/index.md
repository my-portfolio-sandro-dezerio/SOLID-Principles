# Interface Segregation Principle (ISP)
# Interface Segregation Principles (ISP)
ISP states that "a client should never be forced to implement an interface that it doesn't use, or clients shouldn't be forced to depend on methods they do not use". What does this mean?\
Just as the term segregation means - this is all about keeping things separated, meaning separating the interfaces.\
A typical interface will contain methods and properties. When you implement this interface into any class, then the class needs to define all it's methods. For example, suppose you have an interface that defines methods to draw specific shapes.

```javascript
interface ShapeInterface {
    calculateArea();
    calculateVolume();
}
```

When any class implements this interface, all the methods must be defined even if you won't use them or if they don't apply to that class.

```javascript
class Square implements ShapeInterface {
    calculateArea() {
        // ...
    }

    calculateVolume() {
        // ...
    }
}

class Cuboid implements ShapeInterface {
    calculateArea() {
        // ...
    }

    calculateVolume() {
        // ...
    }
}

class Rectangle implements ShapeInterface {
    calculateArea() {
        // ...
    }

    calculateVolume() {
        // ...
    }
}
```

Looking at the example above, you will notice that you cannot calculate the volume of a square or rectangle. Because the class implements the interface,  you need to define all methods, even the ones you won't use or need.\
To fix this, you would need to segregate the interface.

```javascript
interface ShapeInterface {
    calculateArea();
    calculateVolume();
}

interface ThreeDimensionalShapeInterface {
    calculateArea();
    calculateVolume();
}
```

You can now implement the specific interface that works with each class.

```javascript
class Square implements ShapeInterface {
    calculateArea() {
        // ...
    }
}

class Cuboid implements ThreeDimensionalShapeInterface {
    calculateArea() {
        // ...
    }

    calculateVolume() {
        // ...
    }
}

class Rectangle implements ShapeInterface {
    calculateArea() {
        // ...
    }
}
```