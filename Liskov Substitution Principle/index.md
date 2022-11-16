# Liskov Substitution Principle (LSP)
LSP defines that in an inheritance, objects of a superclass (or parent class) should be substitutable with objects of it's subclasses (or child class) without breaking the application or causing any error.\
In very plain terms, you want the objects of your subclasses to behave the same way as the objects your superclass.\
A very common example is the rectangle, square scenario. It's clear that all squares are rectangles because they are quadrilaterals with all four angles being right angles. But not every rectangle is a square. To be a square, it's sides must have the same length.\
Bearing this in mind, suppose you have a rectangle class to calculate the area of a rectangle and perform other operations like set color:

```javascript
class Rectangle {
    setWidth(width) {
        this.width = width;
    }

    setHeight(height) {
        this.height = height;
    }

    setColor(color) {
        this.color = color;
    }

    getArea() {
        return this.width * this.height;
    }
}
```

Knowking fully well that all squares are rectangles, you can inherit the properties of the rectangle. Since the width and height has to be the same, then you can adjust it:

```javascript
class Square extends Rectangle {
    setWidth(width) {
        this.width = width;
        this.height = width;
    }

    setHeight(height) {
        this.width = height;
        this.height = height;
    }
}
```

Taking a look at the example, it should work properly:

```javascript
let rectangle = new Rectangle();
rectangle.setWidth(10);
rectangle.setHeight(5);
console.log(rectangle.getAre()); // 50
```

In the above, you will notice that a rectangle is created, and the width and height are set. Then you can calculate the correct area.\
But according to the LSP, you want the objects of your subclasses to behave the same way as the objects of your superclass. Meaning if you replace the ```Rectangle``` with ```Square```. everything should still work well:

```javascript
let square = new Square();
square.setWidth(10);
square.setHeight(5);
```

You should get 100, because the ```setWidth(10)``` is supposed to set both the width and height to 10. But because of the ```setHeight(5)```, this will return 25.

```javascript
let square = new Square();
square.setWidth(10);
square.setHeight(5);
console.log(square.getAre()); // 25
```

This breaks the LSP. To fix this, there should be a general class for all shapes that will hold all generic methods that you want the objects of your subclasses to have access to. Then for individual methods, you create an individual class for rectangle and square.

```javascript
class Shape {
    setColor(color) {
        this.color = color;
    }

    getColor() {
        return this.color;
    }
}

class Rectangle extends Shape {
    setWidth(width) {
        this.width = width;
    }

    setHeight(height) {
        this.height = height;
    }

    getArea() {
        return this.width * this.height;
    }
}

class Square extends Shape {
    setSide(side) {
        this.side = side;
    }

    getArea() {
        return this.side * this.side;
    }
}
```

This way, you can set the color and get the color using either the super or subclasses:

```javascript
// superclass
let shape = new Shape();
shape.setColor('red');
console.log(shape.getColor()); // red

// subclass
let rectangle = new Rectangle();
rectangle.setColor('red');
console.log(rectangle.getColor()); // red

// subclass
let square = new Square();
square.setColor('red');
console.log(square.getColor()); // red
```