# A closer look at functions

## Default parameters

We can set default values for parameters in a function. If a parameter is not provided, the default value will be used.

```js run
function greet(greeting, name = "friend") {
  //if name is not provided, it will be "friend"
  return `${greeting}, ${name}!`;
}
greet("Hello"); // "Hello, friend!"
greet("Hello", "reader"); // "Hello, reader!"
```

## How passing arguments works : value vs reference

When we pass a primitive value to a function, we pass a copy of the value. This means that if we change the value inside the function, the changes will not be reflected outside the function.

```js run
function changeValue(value) {
  value = 2;
}
let value = 1;
changeValue(value);
console.log(value); // 1
```

When we pass an object to a function, we pass a reference to the object. This means that if we change the object inside the function, the changes will be reflected original object outside the function.

JavaScript does not have pass-by-reference. Even though objects are passed by reference, the reference itself is passed by value. This means that when we pass an object to a function, we pass a copy of the reference to the object. This is why we can change the object inside the function and the changes will be reflected outside the function.

```js run
function changeValue(obj) {
  obj.value = 2;
}
let obj = { value: 1 };
changeValue(obj);
console.log(obj.value); // 2
```

## Fisrt-Class and Higher-Order Functions

JavaScript treats functions as first-class citizens. This means that functions can be assigned to variables, passed as arguments, and returned from other functions.

### First-Class Functions

```js run
const add = (a, b) => a + b; // function assigned to a variable

const counter = {
  value: 0,
  // function assigned to an object as a method
  increment: function () {
    this.value++;
  },
};

// function passed as an argument in another function
const greet = (name) => `Hello, ${name}!`;
const greetUser = (greet, name) => greet(name); // greet is a function passed as an argument to greetUser function and then called and returned inside greetUser function

// function returned from another function
const createAdder = (a) => (b) => a + b;
const addFive = createAdder(5); // createAdder returns a function that adds 5 to a number
addFive(3); // 8 - addFive is a function that adds 5 to a number

//call methods on functions
const someObject = { value: 20 };
counter.increment().bind(someObject);
```

### Higher-Order Functions

A higher-order function is a function that takes a function as an argument or returns a function or both. This is possible because functions are first-class citizens in JavaScript. Higher-order functions are used to abstract over actions, create reusable code, and create more readable code.

A function that is passed as an argument to another function is called a **callback function**.

Function accepting a callback function :

```js run
// Function that takes another function as an argument
const oneWord = function (str) {
  return str.replace(/ /g, "").toLowerCase();
};

const upperFirstWord = function (str) {
  const [first, ...others] = str.split(" ");
  return [first.toUpperCase(), ...others].join(" ");
};

//Higher order function
const transformer = function (str, fn) {
  console.log(fn(str));

  console.log(`Transformed by: ${fn.name}`); // function name property
};

transformer("JavaScript is the best", upperFirstWord); // Output: "JAVASCRIPT is the best" and "Transformed by: upperFirstWord"
transformer("JavaScript is the best", oneWord); // Output: "javascriptisthebest" and "Transformed by: oneWord"
```

Function that returns another function :

```js run
// Function that returns another function
function createMultiplier(multiplier) {
  return function (x) {
    return x * multiplier;
  };
}

// Example usage
const multiplyByThree = createMultiplier(3);
console.log(multiplyByThree(5)); // Output: 15
```

Function that takes a function as an argument and returns another function :

```js run
// Function that takes a function as an argument and returns another function
function createOperation(operation) {
  return function (a) {
    return function (b) {
      return operation(a, b);
    };
  };
}

// Example usage
const add = (x, y) => x + y;
const addOperation = createOperation(add);
const addFive = addOperation(5);
console.log(addFive(10)); // Output: 15
console.log(addOperation(5)(10)); // Output: 15
```

## The call, apply, and bind methods

Call, apply, and bind are methods that allow us to set the value of this in a function.
```js run
const lufthansa = {
  airline: "Lufthansa",
  iataCode: "LH",
  bookings: [],
  book(flightNum, name) {
    console.log(`${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`);
    this.bookings.push({flight: `${this.iataCode}${flightNum}`, name});
  } 
}

lufthansa.book(239, "Purushottam Kumar"); // Output: Purushottam Kumar booked a seat on Lufthansa flight LH239
lufthansa.book(235, "Parth"); // Output: Parth booked a seat on Lufthansa flight LH235

const eurowings = {
  airline: "Eurowings",
  iataCode: "EW",
  bookings: [],
};

const book = lufthansa.book;
// book(239, "Sachin"); // will throw an error because 'this' is undefined, and it is a regulaer function call, and in a regular function call, 'this' is undefined

// to overcome this, we need to pass value of 'this' explicitly using call, apply or bind method

// Call method --> it takes individual arguments
book.call(eurowings, 239, "Shweta"); // Output: Shweta booked a seat on Eurowings flight EW239, Here value of 'this' keyword is set to eurowings object.

// Apply method --> same as call method, but it takes an array of arguments, and not individual arguments like call method, not used much now with moodern JS
book.apply(eurowings, [235, "Shweta"]); // Output: Shweta booked a seat on Eurowings flight EW235

// Bind method --> it does not call the function immediately, but it returns a new function where 'this' keyword is set to the object passed as an argument
const bookEW = book.bind(eurowings);
bookEW(239, "Shweta"); // Output: Shweta booked a seat on Eurowings flight EW239

```
### bind method 
```js run
const lufthansa = {
  airline: "Lufthansa",
  iataCode: "LH",
  bookings: [],
  book(flightNum, name) {
    console.log(`${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`);
    this.bookings.push({flight: `${this.iataCode}${flightNum}`, name});
  } 
}


const book = lufthansa.book;

const bookEW = book.bind(eurowings);
bookEW(239, "Shweta"); // Output: Shweta booked a seat on Eurowings flight EW239

// Partial application using bind method --> setting some parameters in advance
const bookEW235 = book.bind(eurowings, 235); // here we are setting the flight number to 235, so we don't need to pass it again
bookEW235("Shweta"); // Output: Shweta booked a seat on Eurowings flight EW235

// With Event Listeners --> bind method is used to set the value of 'this' keyword in event listeners
lufthansa.planes = 300;
luftansa.buyPlane = function() {
  console.log(this);
  this.planes++;
  console.log(this.planes);
}
document.querySelector(".buy").addEventListener("click", lufthansa.buyPlane.bind(lufthansa)); // here we are setting the value of 'this' keyword to lufthansa object, by default value of 'this' keyword in event listeners is set to the element on which the event is triggered

const addTax = (rate, value) => value + value * rate;
console.log(addTax(0.1, 200)); // Output: 220

// Partial application using bind method
const addVAT = addTax.bind(null, 0.23); // setting the rate to 0.23
// addVAT = value => value + value * 0.23; // this is what bind method does internally
console.log(addVAT(100)); // Output: 123

```

## IIFE (Immediately Invoked Function Expression)
IIFE is a function that is executed immediately after it is created. It is used to create a new scope for variables and to avoid polluting the global scope.

```js run
(function () {
  console.log("This will never pollute the global scope");
})();

// IIFE with arrow function
(() => console.log("This will never pollute the global scope"))();
```
## Closures
Closures are functions that have access to variables from an outer function that has already returned. Closures are used to create private variables and to create higher-order functions.

A function has access to the variable environment of the execution context in which it was created, even after that execution context has already returned.

Closure : Variable Environment to the function is attached even after the function has returned.

```js run
const secureBooking = function () {
  let passengerCount = 0;

  return function () {
    passengerCount++;
    console.log(`${passengerCount} passengers`);
  };
};

const booker = secureBooking();
booker(); // Output: 1 passengers
// Here, booker function has access to passengerCount variable, even though secureBooking function has already returned.
// This is because of closures, which allows booker function to access variables from an outer function that has already returned.

// When code started execution, secureBooking function was called and it created a new execution context and passengerCount variable was created in that execution context. After that, secureBooking function returned a new function, and that function was assigned to booker variable. Now, booker function has access to passengerCount variable, even though secureBooking function has already returned. This is because of closures. 
```
```js run
// Example 2
let f;

const g = function () {
  const a = 23;
  f = function () {
    console.log(a * 2);
  };
};
g();
f(); // Output: 46, Here, f function has access to a variable, even though g function has already returned.
```
```js run
let f;

const g = function () {
  const a = 23;
  f = function () {
    console.log(a * 2);
  };
};

const h = function() {
  const b = 777;
  f = function () {
    console.log(b * 2);
  };
}
g();
f(); // Output : 46
//re-assigning f function
h();
f(); // Output : 1554

console.dir(f); //Output : f() {scope: {closure: {b: 777}}}
```
```js run
const boardPassengers = function(n , wait) {
  const perGroup = n / 3;
  setTimeout(function() {
    console.log(`We are now boarding all ${n} passengers`);
    console.log(`There are three groups each with ${n} passengers`);
  }, wait * 1000);
  console.log(`Will start boarding in ${wait} seconds`);
}

boardPassengers(180, 3); 
//here perGroup variable is not used inside the callback function, but it is still available to the callback function because of closures.
```
