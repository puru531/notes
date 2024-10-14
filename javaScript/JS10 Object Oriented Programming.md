# What is Object Oriented Programming?

Object Oriented Programming (OOP) is a programming paradigm that relies on the concept of classes and objects. It is used to structure a software program into simple, reusable pieces of code blueprints (usually called classes), which are used to create individual instances of objects. There are many concepts associated with OOP, such as encapsulation, inheritance, and polymorphism.

- Object Oriented Programming (OOP) is a programming paradigm based on the concept of "objects".
- We use objects to model real-world or abstract features.
- Objects can contain data (attributes or properties) and code (methods). By using objects, we pack data and corresponding behavior into a single block.
- In OOP, objects are self-contained pieces/blocks of code.
- Objects are building blocks of applications, and they interact with one another.
- Interactions happen through a public interface (API): methods that the code outside the object can access and use to communicate with the object.

## Classes and Instances (Traditional OOP)

**Class**: A blueprint for objects. Classes can contain properties and methods to describe the behavior of an object. A class is a template for objects.

```javascript
User {
    username;
    password;
    email;
    login() {
        // code to login
    }
    logout() {
        // code to logout
    }
}
```

**Instance**: Objects created from a class are called instances. An instance is a specific object created from a particular class. There can be multiple instances of a class.

```javascript
{
    username: 'john_doe',
    password: 'password123',
    email: 'abc@example.com',
    login() {
        // code to login
    }
    logout() {
        // code to logout
    }
}
```

## Four fundamental principles of OOP

### 1. Abstraction

Abstraction is the concept of hiding the complex implementation details and showing only the necessary features of an object. It helps to reduce programming complexity and effort. For example, when you use a TV remote, you don't need to know how the remote works internally. You just need to know how to use the remote to control the TV.

### 2. Encapsulation

Encapsulation is the bundling of data (attributes) and methods (functions) that operate on the data into a single unit or class. It restricts direct access to some of an object's components, which means that a class has a public interface for interacting with the object, while the internal workings and data are hidden from the outside world.

Keeping properties and methods private inside the class, so they are not accessible from outside the class. Some methods can be exposed as public interfaces (API) to interact with the object.

### 3. Inheritance

Inheritance is a mechanism in which one class acquires the properties and behavior of another class. It represents the "is-a" relationship between classes. For example, a Car class can inherit properties and methods from a Vehicle class. This helps to reuse code and establish a hierarchical relationship between classes.

Inheritance makes all the properties and methods of the parent class available to the child class, forming a hierarchical relationship between classes. This allows us to reuse common logic and to model real-world relationships.

### 4. Polymorphism

Polymorphism is the ability of an object to take on multiple forms. It allows objects of different classes to be treated as objects of a common superclass. A class can override method it inherits from a parent class.

# OOP in JavaScript

In JavaScript, each object can serve as a prototype for another object. JavaScript is a prototype-based language, which means that objects can inherit properties and methods directly from other objects.

Prototypal inheritance: The prototype contains methods that are accessible to all objects linked to that prototype. When a method is called on an object, JavaScript looks for the method in the object itself. If it doesn't find the method, it looks for the method in the object's prototype. This process continues up the prototype chain until the method is found or the end of the chain is reached.

## 3 Ways of implementing prototypal inheritance in JavaScript

1. **Constructor Functions**:

   - Technique to create objects from a function.
   - This is how built-in objects like `Array`, `Date`, `and` `Object` are actually implemented.

2. **ES6 Classes**:
   - Modern alternative to constructor functions.
   - "Synthetic sugar" behind the scenes, ES6 classes work exactly like constructor functions.
3. **Object.create()**:
   - The simplest way to create objects with a specified prototype.
   - Not widely used in practice.

## Constructor Functions

```javascript
"use strict";

const Person = function (firstName, birthYear) {
  console.log(this);
};

new Person("Jonas", 1991); // Person {}     (empty object)
```

When we call a function with the `new` operator, the following happens:

1. A new empty object is created.
2. The function is called, and `this` keyword is set to the new empty object.
3. The new object is linked to a prototype.
4. The object is automatically returned from the constructor function.

```javascript
const Person = function (firstName, birthYear) {
  // Instance properties
  this.firstName = firstName;
  this.birthYear = birthYear;
};

const jonas = new Person("Jonas", 1991);
console.log(jonas); // Person {firstName: "Jonas", birthYear: 1991}

/*
When we call constructor function with the new operator, therefore a new empty object is created, then the function is called, and this keyword is set to the new empty object. Then in function, we set properties on this object. Finally, the object is automatically returned from the constructor function.
*/

//Now we can create multiple instances of Person

const matilda = new Person("Matilda", 2017);
const jack = new Person("Jack", 1975);
console.log(matilda, jack); // Person {firstName: "Matilda", birthYear: 2017} Person {firstName: "Jack", birthYear: 1975}

// JS does not have classes, but we can simulate classes using constructor functions, and hence the objects created from constructor functions are called instances of the class.
console.log(jonas instanceof Person); // true
```

### Methods in Constructor Functions

```javascript
const Person = function (firstName, birthYear) {
  // Instance properties
  this.firstName = firstName;
  this.birthYear = birthYear;

  // Method to calculate age -- This is not a good practice as it creates a new copy of the method for each instance
  this.calcAge = function () {
    console.log(2023 - this.birthYear);
  };
};

const jonas = new Person("Jonas", 1991);
jonas.calcAge(); // Logs the age of Jonas

const matilda = new Person("Matilda", 2017);
matilda.calcAge(); // Logs the age of Matilda

const jack = new Person("Jack", 1975);
jack.calcAge(); // Logs the age of Jack
```

The above code is not a good practice because the calcAge method is created for each instance of the Person object. This is not efficient because the method is the same for all instances, **and it would be better to have the method in the prototype chain**.

## Prototypes

Each and every function in JavaScript automatically has a property called `prototype`, and that includes constructor functions. Every object that is created by a certain constructor function will get access to all the methods and properties that we define on the constructor's prototype property.

And this property is an object. We can attach methods and properties to the prototype property, and these methods and properties will be inherited by all the objects created from the constructor function.

```javascript
Person.prototype.calcAge = function () {
  console.log(2023 - this.birthYear);
};
// Now, the calcAge method is not created for each instance of the Person object, but it is inherited from the prototype chain.
console.log(Person.prototype); // {calcAge: ƒ, constructor: ƒ}

const jonas = new Person("Jonas", 1991);
jonas.calcAge(); // Logs the age of Jonas

// The prototype property is not on the object itself, but on the constructor function
console.log(jonas); // Person {firstName: "Jonas", birthYear: 1991} -- No calcAge method
console.log(jonas.__proto__); // Person {calcAge: ƒ, constructor: ƒ} -- calcAge method is in the prototype chain

console.log(jonas.__proto__ === Person.prototype); // true
console.log(Person.prototype.isPrototypeOf(jonas)); // true

//Same for other instances
const matilda = new Person("Matilda", 2017);
matilda.calcAge(); // Logs the age of Matilda
```

The above code works because any object always has access to the methods and properties of its prototype. And the prototype of jonas is Person.prototype.

**We can also add properties to the prototype chain.**

```javascript
//from previous code
Person.prototype.species = "homo sapiens";

console.log(jonas.species, matilda.species); //homo sapiens, homo sapiens

console.log(jonas);
/*
Person {firstName: "Jonas", birthYear: 1991} __proto__: {calcAge: ƒ, species: "homo sapiens" constructor: ƒ}
*/
//to check own properties
console.log(jonas.hasOwnProperty("firstName")); //true
console.log(jonas.hasOwnProperty("species")); //false
```
