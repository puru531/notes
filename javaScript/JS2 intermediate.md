# Intermediate

## use strict

helps us to avoid possible errors.

```js run
let hasDriversLicense = false;
const passTest = true;

if (passTest) hasDriverLicense = true; //spelling mistack, s is not after driver, but it will not throw an error, just silently fails
if (hasDriversLicense) console.log("I can drive"); //will not printed because variable is still false and we didn't get error also.

// If we use 'use strict', we can avoid this error.
```

## Functions

### Function Fundamentals

Function declaration :

```js run
//a function is a piece of code that can be re-used multiple times
function logger() {
  console.log("My name is Purushottam");
}
```

Calling / running / invoking function

```js run
//we can call same function multiple times
logger();
logger();
logger();
```

Functions can also recieve (parameters) and return data:

```js run
function fruitProcessor(apples, oranges) {
  //function recieving value - apples and oranges are 'parameters'
  const juice = `Juice with ${apples} apples and ${oranges} oranges.`;
  return juice; // function returning value
}
```

Passing values to the function (arguments) and and recieving returned values

```js run
const juice1 = fruitProcessor(5, 2); //passing values to function, 5, 2 and arguments;
//returned value from the function is stored in juice1 variable

console.log(juice1); //Juice with 5 apples and 2 oranges.

//And, we can call that function multiple times with different arguments:
const juice2 = fruitProcessor(3, 6);
const juice3 = fruitProcessor(15, 12);
```

### Function Declarations vs Expressions

Function declaration :

```js run
function calcAge1(birthYear) {
  return 2037 - birthYear;
}

//calling
const age1 = calcAge1(1999);
console.log(age1);
```

Function expression :

```js run
//An Anonymous function stored in a variable
const calcAge2 = function (birthYear) {
  return 2037 - birthYear;
};

const age2 = calcAge2(1999);
console.log(age2);
```

Difference :
We can call function declaration function before it is defined.

```js run
console.log(calcAge3(1999)); // this will work even if we are calling function before it is initialized.

function calcAge3(birthYear) {
  return 2037 - birthYear;
}
```

Calling a function exoression before it is initialized will cause a error.

```js run
console.log(calcAge4(1999)); //ReferenceError : Cannot access 'calcAge4' before initialization.

const calcAge4 = function (birthYear) {
  return 2037 - birthYear;
};
```

### Arrow Functions

```js run
const calcAge5 = (birthYear) => 2037 - birthYear; // parentheses and return keywords are not need for one liner function

const age5 = calcAge5(1999);
console.log(age5);

//when more lines of code is needed then parentheses and return keywords are needed.
const yearUntilRetirement = (birthYear, firstName) => {
  // when parameters are more than one, they should be enclosed in parentheses
  const age = 2037 - birthYear;
  const retirement = 65 - age;
  return `${firstName} will retire in ${retirement} years`;
};

console.log(yearsUntilRetiment(1999, "Abc")); //Abc will retire in 22 years
```

Note : Arrow function does not get 'this' keyword.

### Function calling other functions

```js run
function getGreetingMessage(name) {
  return `Hello ${name}!`;
  console.log(name); //this will not execute as function has already exited at return
}

function greet(name) {
  const message = getGreetingMessage(name);
  console.log(message);
}
```

## Arrays

### Array methods

```js run
arr.push(someValue); //adds element at the end of the array, and returns length of modified array
arr.unshift(someValue); //adds element at the beginning of the array, and returns length of modified array

//push() and unshift() methods returns length of modified array
arr = [1, 2, 3, 4, 5];
const len = arr.push(8);
console.log(len); //6

arr.pop(); //removes last element of the array, and returns the removed element
arr.shift(); //removes first element of the array, and returns the removed element

//pop() and shift() return the removed element
arr = [1, 2, 3, 4, 5];
const poppedElement = arr.pop();
console.log(poppedElement); //5

arr.indexOf(3); //Knowing position of an element in an array, if it is not there, it will return -1
arr.includes(3); //Knowing if an element is present in an array or not, returns true,/fals, (strict equality, no type coersion)
```

## Objects

A data structure which combines key-value pairs in one variable.

```js run
//Object literal syntax
const obj1 = {
  firstName: "Purushottam",
  lastName: "Kumar",
  age: 2024 - 2015,
  friends: ["Motu", "Lambu", "Kullu"],
};
```

Each key in the object is called a property. So, we can say tha object obj1 has 4 properties.

Retrieving and changing data in objects:

```js run
console.log(obj1); //whole object, order does not matter in objects

//Dot notation (.) :
console.log(obj1.firstName); //Purushottam
console.log(obj1.job); //undefined
obj1.job = "Programmer"; //adding new property
console.log(obj1.job); //Programmer

//Bracket notation :
console.log(obj1["firstName"]); //Purushottam
obj1["location"] = "India"; //adding new property

//we can use expressions in bracket notation
const nameKey = "Name";
console.log(obj1["first" + nameKey]); //Purushottam
```

### Object Methods

As functions are just values, we can store them in Objects as key value pairs.

```js run
const obj2 = {
  firstName: "Purushottam",
  lastName: "Kumar",
  birthYear: 2010,
  job: "Programmer",
  friends: ["Motu", "Lambu", "Kullu"],
  hasDriversLicense: true,

  //function inside object
  calcAge: function () {
    console.log(this); //whole obj2 object
    this.age = 2037 - this.birthYear; //this means same object
  },
  getSummary: function () {
    return `${this.firstName} is a ${this.age} year old ${this.job}, and he has ${this.hasDriversLicense ? "a" : "no"} driver's license`;
  },
};

//Accessing the method inside the object
obj2.calcAge();
//or
obj2["calcAge"]();
console.log(obj2.age);
console.log(obj2.getSummary());
```

## Loops

### For loop

for loop keeps running while the condition is true \
Best for use when number of iteration is know

```js run
//initial;  condition; increment
for (let num = 1; num <= 10; num++) {
  if (num === 5) {
    continue; //will skip fifth iteration
  }
  if (num === 9) {
    break; //will break the loop at 9 and not execute further
  }
  console.log(num);
}
```

```js run
//loop inisde a loop
for (let exercise = 1; exercise < 4; exercise++) {
  console.log(`----------- Starting exercise ${exercise}`);

  for (let rep = 1; rep < 6; rep++) {
    console.log(`Lifting weight repetition ${rep}`);
  }
}
```

### While loop

Best in use when we do not depend on any counter, and it is not known that how many times the loop is going to run. \
In while loop we only specify condition, unlike we specify initial and increment/decrement too in for loop

```js run
let rep = 0; //initial is declared outside
while (rep <= 10) {
  console.log(`Lifting weight repetition ${rep}`);

  rep++; //increment/decrement done at the end
}
```

```js run
let dice = Math.trunc(Math.random() * 6) + 1;

while (dice !== 6) {
  console.log(`You rolled a ${dice}`);

  dice = Math.trunc(Math.random() * 6) + 1;
  if (dice === 6) console.log("Loop is about to end...");
}
```
