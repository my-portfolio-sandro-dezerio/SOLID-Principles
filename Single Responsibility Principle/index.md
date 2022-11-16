# Single Responsibility Principle (SRP)
SRP states that a class should only have one reason to change. This means that a class should have only one job and do one thing.\
Let's take a look at a proper example. You'll always be tempted to put similar classes together - but unfortunately, this goes against this principle.\
The ```ValidatePerson```object below has three methods: two validation methods, (```ValidateName()``` and ```ValidateAge()```), and a ```Display()``` method.

```javascript
class ValidatePerson {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    ValidateName(name) {
        if(name.length > 3) {
            return true;
        } else {
            return false;
        }
    }

    ValidateAge(age) {
        if(age > 18) {
            return true;
        } else {
            return false;
        }
    }

    Display() {
        if(this.ValidateName(this.name) && this.ValidateAge(this.age)) {
            console.log(`Name: ${this.name} and Age: ${this.age}`);
        } else {
            console.log('Invalid');
        }
    }
}
```

The ```Display()``` method goes agains the SRP because the goal is that a class should have only one job and do one thing. The ```ValidatePerson``` class does two jobs - it validates the person's name and age and then displays some information.\
The way to avoid this problem is to separate code that supports different actions and jobs so that each class only performs one job and has one reason to change.\
This means that the ```ValidatePerson``` class will only be responsible for validating a user, as seen below:

```javascript
class ValidatePerson {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    ValidateName(name) {
        if (name.length > 3) {
            return true;
        } else {
            return false;
        }
    }

    ValidateAge(age) {
        if (age > 18) {
            return true;
        } else {
            return false;
        }
    }
}
```

While the new class ```DisplayPerson```will now be responsible for displaying a person, as you can see in the code block below:

```javascript
class DisplayPerson {
    constructor(name, age) {
        this.name = name;
        this.age = age;
        this.validate = new ValidatePerson(this.name, this.age);
    }

    Display() {
        if (
            this.validate.ValidateName(this.name) &&
            this.validate.ValidateAge(this.age)
        ) {
            console.log(`Name: ${this.name} and Age: ${this.age}`);
        } else {
            console.log('Invalid');
        }
    }
}
```
With this, you will have fulfilled the SRP, meaning our classes now have just one reason to change. If you want to change the ```DisplayPerson``` class, it won't affect the ```ValidatePerson``` class.