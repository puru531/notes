# Built-in Data Structure, Modern Operators and Strings

## Destructuring Arrays

It is an ES6 feature, it a way of unpacking values from an array or an object into separate variables.

```js run
const arr = [2, 3, 4];
const a = arr[0];
const b = arr[1];
const c = arr[2];

//destructuring
const [x, y, z] = arr; //single elements saved to individual variables. and original array is not affected
```

```js run
const restaurant = {
  name: "Classico Italiano",
  location: "Via Angelo Tavanti 23, Firenze, Italy",
  categories: ["Italian", "Pizzeria", "Vegetarian", "Organic"],
  starterMenu: ["Focaccia", "Bruschetta", "Garlic Bread", "Caprese Salad"],
  mainMenu: ["Pizza", "Pasta", "Risotto"],

  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },
};

let [first, second] = restaurant.categories; //takes in order : first = 'Italian', second= 'Pizzeria'
let [first2, , third] = restaurant.categories; //will skip the second one
console.log(first, third);

// swap two values using this
[first, second] = [second, first];

//function returning an array can be destructured
console.log(restaurant.order(2, 0));
const [starter, main] = restaurant.order(2, 0);
console.log(starter, main); //Garlic Bread, Pizza
```

### Destructuring in nested array (array inside an array)

```js run
const nested = [2, 4, [3, 9]];
const [i, , j] = nested;
console.log(i, j); // 2, [3,9]

//destructuring inner array
const [p, , [q, r]] = nested;
console.log(p, q, r); //2,3,9
```

### Setting default values while destructuring

We can set default value to the variables while destructuring

```js run
const [p, q, r] = [8, 9];
console.log(p, q, r); // 8,9, undefined
//In order to not get undefined for values which are not found, we can set default values
const [p = 1, q = 3, r = 2] = [8, 9];
console.log(p, q, r); // 8,9,2
```

## Destructuring Objects

```js run
const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },

  openingHours : {
    thu: {
      open: 12,
      close: 22,
    },
    fri: {
      open: 11,
      close: 23,
    },
    sat: {
      open: 0,
      close: 24,
    },
  };

  //ES6 Creating methods inside objects
  orderPasta(ing1, ing2, ing3) {
    console.log(`Here is your delicious pasta with ${ing1}, ${ing2} and ${ing3}.`);
  },

  orderPizza(mainIngredient, ...OtherIngredients) {
    console.log(mainIngredient);
    console.log(OtherIngredients);
  }
};
```

To destructure objects we use { }, we need to write exact property name to extract varibles from object, Order of property does not matter in objects like Arrays <br>

```js run
const { name, openingHours, categories } = restaurant; //this creates three new variables

//if we want to keep different variable name from property name
const { name: restaurantName, openingHours: hours, categories } = restaurant;
console.log(restaurantName, hours, categories);
```

### setting default values :

```js run
const { menu = [], starterMenu: starters = [] } = restaurant; // if the property is not present, then default value is set
```

### mutating variables

```js run
let a = 111;
let b = 999;
const obj = { a: 23, b: 7, c: 14 };
//if variable is already declared then we cannot use const or let, and if we just keep {a, b} = obj;, it will throw error because JS understands it as code block and expects some code, so we need to wrap in ()
({ a, b } = obj);
```

### Destructuring nested objects

```js run
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};

const {
  fri: { open: o, close: c },
} = openingHours;
//first desturcturing 'fri', which gives {open: 11, close: 23}, further we destructure this and then set another variable name.
```

### Destructuring function parameters

```js run
function orderDelivery({
  starterIndex = 1,
  mainIndex = 2,
  time = "20:00",
  address,
}) {
  console.log(starterIndex, mainIndex, time, address);
}

const obj = {
  starterIndex: 2,
  mainIndex: 4,
  time: "22:00",
  address:
    "Flat 202 (front side), Plot no. 179, TNGOs Conly, Gachibowli, Hyderabad",
};
orderDelivery(obj);
```

## The Spread operator (...)

Used to expand all iterables into its all its elements, (Unpacking all the array elements at one.)<br>
Iterables : arrays, string, map, sets. (except objects)

### Spread in Arrays

```js run
const arr = [5, 6, 7];
//if we want ro add few items in starting and then all items of arr :
const badNewArr = [1, 2, arr[0], arr[1], arr[2]]; //wrong way

//using spread operator
const goodNewArr = [1, 2, ...arr];
const arrCopy = [...arr]; //here items returned by ...arr will not point to same object or arr, instead it is a new array and point to different memory reference.

const arr1 = [2, 5, 6];
const arr2 = [5, 6, 9, 5];
const combinedArr = [...arr1, ...arr2];

//String
const name = "Purushottam";
const letters = [...name, " ", "K."];

//should be used to build a new array or pass arguments to functions
console.log(...name);
console.log(`${...name} Kumar`); //this will throw error

//spread in function arguments

function orderPasta(ing1, ing2, ing3) {
    console.log(`Here is your delicious pasta with ${ing1}, ${ing2} and ${ing3}.`);
  }

  orderPast(['a', 'b', 'c']);
```

### Spread in objects

Since ES2018 spread operator also works for objects even though objects are not iterables.

```js run
const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },

  openingHours : {
    thu: {
      open: 12,
      close: 22,
    },
    fri: {
      open: 11,
      close: 23,
    },
    sat: {
      open: 0,
      close: 24,
    },
  };
};


const newRestaurant = {...restaurant, founder: 'Puru', foundedIn: 2025}; //new object
```

## The Rest Operator (...)

Does opposite of the **Spread operator**, spread expands the array, Rest collects multiple elements and pack them into an array. It is used in the **Left** side of assignment operator unlike Spread Operator in the Right.

### REST pattern in Arrays

```js run
//SPREAD because in right side of assignment operator (=), Although sometime can be in lef side, together with destructuring
const arr = [1, 3, [4, 5]];

//REST because on the left of assignment operator
const [a, b, ...others] = [1, 2, 3, 4, 5, 6, 7];
/*
this will save 1 in variable a, 2 in variable b, and rest all items in array 'others'
 a = 1
 b = 2
 others = [3,4,5,6,7]
*/
```

```js run
const arr1 = [2, 5, 6, 7];
const arr2 = [5, 6, 9, 5];
//Rest pattern must be the last element, There can only be one REST pattern in a destructuring assignment
const [a, , b, ...others] = [...arr1, arr2];
/*
 a= 2
 b= 6
 others = [7,5,6,9,5]
*/
```

```js run
const add = function (...numbers) {
  let sum = 0;
  for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
  }
  console.log(sum);
};

add(2, 3);
add(5, 3, 6, 7, 8);
add(6, 2, 6, 8, 7, 8, 9, 6, 5, 4, 4);
const x = [6, 5, 9, 7];
add(...x);
```

### REST pattern in Objects

```js run
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};

const { sat, ...weekdays } = openingHours;
/*
sat = {
    open: 0,
    close: 24
  }

weekdays = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  }
}
*/
```

## Short Circuiting ( && and || )

&& and || can use any data type, return any data type, Short circuiting.

```js run
// short circuiting with OR (||)
//if the first operand is truthy then it will immediately return it, without checking the next. If all operands are falsy then it returns the last operand
console.log(3 || "Puru"); //3
console.log("" || "Puru"); //Puru
console.log(true || 0); //true
console.log(undefined || null); //null //if all falsy then last one is returned
console.log(undefined || null || false); //false //if all falsy then last one is returned
console.log(undefined || 0 || "" || "hello" || 23 || null); //hello
```

```js run
// short circuiting with AND (&&)
//If first operand is falsy then it immediately returns it. Otherwise if the first operand is truthy then it will check for next one, Once it finds falsy value, that one is returned, otherwise in case of all truthy last operand is returned.
console.log(0 && "Puru"); //0
console.log(3 && "Puru"); //Puru
console.log("" && "Puru"); //''
console.log(true && 0); //0
console.log("Puru" && undefined && null); //undefined
```

## The Nullish Coalescing Operator (??)

In short circuitin with OR ( || ), we have a problem that it considers 0 as a false value but sometimes we want zero as a value.

```js run
const count = 0;
const totalCount = count || 10; //0  --> here we want totalCount to be 0 but, it getting set as 10
```

Nullish Coalescing operator solves this problem. It checks only **null** or **undefined** and (0 or '') is considred as a value.

```js run
const count = 0;
const totalCount = count || 10; //0
const totalCount = newValue || 10; //10
```

## Logical Assigment Operators

Introduced in ES2021

```js run
const rest1 = {
  name: "Capri",
  numGuests: 20,
};

const rest2 = {
  name: "La Piazza",
  owner: "Giovanni Rossi",
};
// OR assignment operator ( ||= )
rest1.numGuest ||= 10;
rest2.numGuest ||= 10;

//same as :
rest1.numGuest = rest1.numGuest || 10;
rest2.numGuest = rest2.numGuest || 10;

//Nullish coalescing assignment operator ( ??= )
rest1.numGuest ??= 10;
rest2.numGuest ??= 10;

// AND assignment operator ( &&= )
rest1.numGuest &&= 10; //only works when property is available, for false values it does not add/change property
rest2.numGuest &&= 10;
```

## New way of looping arrays (for of loop)

```js run
const arr = [1, 2, 3, 4, 5, 6];
for (const item of arr) console.log(item);

//arr.entries() --> return an array of arrays containing element and index
const arr2 = ["apple", "ball", "cat"];
console.log(arr2.entries()); // [[0, 'apple'], [1, 'ball'], [2, 'cat']]
```

## Enhanced Object Literals

Before ES6 :

```js run
const obj2 = {
  some: "value",
};

const obj1 = {
  someOther: "value",
  obj2: obj2, //property name and variabke obj2 as value
  greet: function (name) {
    //have to give property name and specify function
    return `Hello ${name}`;
  },
};
```

From ES6 :

```js run
const obj2 = {
  some: "value",
};

const arr = [1,2,3,4]

const obj1 = {
  someOther: "value",
  arr[1]: 'two',
  `day-${1+2}`: 'some value', //we can do computations also

  obj2, //directly specify variable name if we want to keep same name.
  greet(name) {
    //explicit property name is not needed for function
    return `Hello ${name}`;
  },
};
```

## Optional Chaining

Helps prevent errors when some property is not ther and we want to do some action on it or want to access data further:

```js run
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};

console.log(openingHours.mon); //undefined
//now with this undefined, I try to access openning hours
console.log(openingHours.mon.open); // TypeError : Cannot read property 'open' of undefined.
//or do any action
console.log(openingHours.name.toLowercase()); //TypeError : Cannot find method 'toLowercase' of undefined

//The application breaks in above cases and does not execute code further

//to overcome this we can have below solution :

//before ES6
console.log(openingHours.mon && openingHours.mon.open);
console.log(openingHours.name && openingHours.name.toLowercase());
//this will return undefined in first operand only and will not check further, hance does break application and continues execution

//After ES6 (use optional chaining ?.)
console.log(openingHours.mon?.open); //undefined --> if mon property is there then only executes further otherwise returns undefined
console.log(openingHours.name?.toLowercase()); //undefined --> if name property is there then only executes further otherwise returns undefined
```

## Looping Objects : Object Keys, Values, and Entries

Looping over property names of object:

```js run
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};

// Object.keys(objectName); give array of names of all the properties
const properties = Object.keys(openingHours);
console.log(properties); //["thu", "fri", "sat"];
let openStr = `We are open ${properties.length} days : `; //We are open 3 days

for (const day of properties) {
  console.log(day); //thu fri sat
  openStr += ` ${day},`;
}
console.log(openStr); //We are open 3 days : thu, fri, sat
```

Looping over property values of object:

```js run
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};
const values = Object.values(openingHours);
console.log(values); // [{open: 12, close: 22}, {open: 11, close: 23}, {open: 0, close: 24}]
```

Looping over both keys and values of object:

```js run
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};
const entries = Object.entries(openingHours);
console.log(entries);
/* Array of properties and values
 [
  ["thu", { open: 12, close: 22 }], 
  ["fri", { open: 11, close: 23 }], 
  ["sat", { open: 0, close: 24 }]
  ]
 */

for (const [day, { open, close }] of entries) {
  console.log(`On ${day} we open at ${open} and close at ${close}`);
}
/*
On thu we open at 12 and close at 22
On fri we open at 11 and close at 23
On sat we open at 0 and close at 24
*/
```

## Sets

Initally JS had only Arrays and Objects as built in data structures, but ES6 introduced Sets and Maps.

a Set is a built-in object that allows you to store unique values of any type, whether primitive values or object references. Here's a brief overview of how Set works:

**Key Features of Set:**

- **Unique Values:** A Set automatically removes duplicate values.
- **Any Type:** You can store any type of value, including objects and primitives.
- **Order:** Elements are iterated in the order of insertion.
- Sets does not have indexes, so we cannot access elements by index.

**Common Methods and Properties:**

- **add(value):** Adds a new element with the given value to the Set.
- **delete(value):** Removes the specified value from the Set.
- **has(value):** Returns true if the Set contains the specified value.
- **clear():** Removes all elements from the Set.
- **size:** Returns the number of elements in the Set.

```js run
// Creating a new Set
const mySet = new Set();

// Adding values to the Set
mySet.add(1);
mySet.add(5);
mySet.add(5); // Duplicate value, will be ignored
mySet.add("hello");
mySet.add({ a: 1, b: 2 });

// Checking the size of the Set
console.log(mySet.size); // Output: 4

// Checking if a value exists in the Set
console.log(mySet.has(5)); // Output: true
console.log(mySet.has(10)); // Output: false

// Iterating over the Set
for (const item of mySet) {
  console.log(item);
}

// Removing a value from the Set
mySet.delete(5);
console.log(mySet.has(5)); // Output: false

// Clearing the Set
mySet.clear();
console.log(mySet.size); // Output: 0

// set with string
console.log(new Set("Puru")); //Set(4) {'P', 'u', 'r', 'u'}

//accessing with index
console.log(mySet[0]); //undefined, NO index in set
```

**Use Cases:**

- Removing Duplicates: Quickly remove duplicate values from an array.
- Tracking Unique Items: Keep track of unique items in a collection.

**Example: Removing Duplicates from an Array**

```js run
const numbers = [1, 2, 2, 3, 4, 4, 5];
const uniqueNumbers = [...new Set(numbers)];
console.log(uniqueNumbers); // Output: [1, 2, 3, 4, 5]
```

Set is a powerful and efficient way to handle collections of unique values in JavaScript.

### forEach method in Set

```js run
const orderSet = new Set([
  "Pasta",
  "Pizza",
  "Risotto",
  "Pasta",
  "Pizza",
  "Pizza",
]);
orderSet.forEach((value, _, set) => {
  console.log(value);
});
/*
Pasta
Pizza
Risotto
*/
```

## Maps

A map is a data structure that allows you to store key-value pairs. It is similar to an object, but with a few key differences:

- **Key Type:** In a map, the key can be of any type, including objects and primitives.
- **Order:** Elements are iterated in the order of insertion.
- **Size Property:** A map has a size property that returns the number of key-value pairs.
- **Iteration:** Maps have built-in methods for iterating over keys, values, and entries.

```js run
// Creating a new Map
const myMap = new Map();
myMap.set("name", "Classico Italiano");
myMap.set(1, "Firenze, Italy");
console.log(myMap.set(2, "Lisbon, Portugal")); //setting value also return the map
/*
Map(3) {
  'name' => 'Classico Italiano',
  1 => 'Firenze, Italy',
  2 => 'Lisbon, Portugal'
}
*/

//Since adding values also returns the map, we can chain the set method
myMap
  .set("categories", ["Italian", "Pizzeria", "Vegetarian", "Organic"])
  .set("open", 11)
  .set("close", 23)
  .set(true, "We are open :D")
  .set(false, "We are closed :(");

// Getting values from the Map
console.log(myMap.get("name")); //Classico Italiano
console.log(myMap.get(true)); //We are open :D

const time = 21;
myMap.get(time > myMap.get("open") && time < myMap.get("close")); //We are open :D

// Checking if a key exists in the Map
console.log(myMap.has("categories")); //true

// Deleting a key from the Map
myMap.delete(2);

// checking size
console.log(myMap.size); //6

// Clearing the Map
myMap.clear();

//Arrays or objects can be used as keys
const arr = [1, 2];
myMap.set(arr, "test data");
console.log(myMap.get(arr)); //test data
console.log(myMap.get([1, 2])); //undefined, because it is a different array, even though it has same values, it is different object in memory reference.
```

### Iterating over Maps

```js run
const question = new Map([
  ["question", "What is the best programming language?"],
  [1, "C"],
  [2, "Java"],
  [3, "JavaScript"],
  ["correct", 3],
  ["true", "Correct"],
  ["false", "Try Again"],
]);

console.log(question);
/*
Map(7) {
  'question' => 'What is the best programming language?',
  1 => 'C',
  2 => 'Java',
  3 => 'JavaScript',
  'correct' => 3,
  'true' => 'Correct',
  'false' => 'Try Again'
}
*/

//Iterating over map
for (const [key, value] of question) {
  if (typeof key === "number") console.log(`Answer ${key} : ${value}`);
}

//Converting object to map
const openingHours = {
  thu: {
    open: 12,
    close: 22,
  },
  fri: {
    open: 11,
    close: 23,
  },
  sat: {
    open: 0,
    close: 24,
  },
};
const hoursMap = new Map(Object.entries(openingHours));
/*
  Map(3) {
  'thu' => { open: 12, close: 22 },
  'fri' => { open: 11, close: 23 },
  'sat' => { open: 0, close: 24 }
*/
```

### forEach method in Map

```js run
const question = new Map([
  ["question", "What is the best programming language?"],
  [1, "C"],
  [2, "Java"],
  [3, "JavaScript"],
  ["correct", 3],
  ["true", "Correct"],
  ["false", "Try Again"],
]);
question.forEach((value, key) => {
  console.log(`This is ${key}, and it's set to ${value}`);
});
/*
This is question, and it's set to What is the best programming language?
This is 1, and it's set to C
This is 2, and it's set to Java
This is 3, and it's set to JavaScript
This is correct, and it's set to 3
This is true, and it's set to Correct
This is false, and it's set to Try Again
*/
```

## String and its methods

Strings are primitive values in JavaScript, but they also have built-in methods that allow you to manipulate and work with strings. String is immutable. Here are some common string methods:

```js run
const airline = "TAP Air Portugal";
const plane = "A320";
console.log(plane[0]); //A
console.log("B737"[0]); //B
console.log(aireline.length); //16
console.log(airline.indexOf("r")); //6
console.log(airline.lastIndexOf("r")); //10
console.log(airline.indexOf("Portugal")); //8
console.log(airline.indexOf("portugal")); //-1, case sensitive, if not found then returns -1
```

### slice() method

```js run
console.log(airline.slice(4)); //Air Portugal, starts from 4th index, returns a new string, does not change original string
console.log(airline.slice(4, 7)); //Air, starts from 4th index and ends at 7th index, length of returned sub-string is 7-4 = 3
console.log(airline.slice(0, airline.indexOf(" "))); //TAP, starts from 0 and ends at first space
console.log(airline.slice(airline.lastIndexOf(" ") + 1)); //Portugal, starts from last space and goes till end
console.log(airline.slice(-2)); //al, starts from last 2 characters
console.log(airline.slice(1, -1)); //AP Air Portuga, starts from 1 and goes till last but excludes last 1 character

// function to check if a letter is present in the string
const checkMiddleSeat = function (seat) {
  const s = seat.slice(-1);
  if (s === "B" || s === "C") console.log("You got the middle seat");
  else console.log("You got lucky");
};
checkMiddleSeat("11B");
checkMiddleSeat("23C");
checkMiddleSeat("3E");
```

### toLowerCase(), toUpperCase(), and trim() methods

```js run
const airline = "TAP Air Portugal";
const plane = "A320";
console.log(airline.toLowerCase()); //tap air portugal
console.log(airline.toUpperCase()); //TAP AIR PORTUGAL

const passenger = "  Puru  ";
console.log(passenger.trim()); //Puru, removes extra spaces from start and end

//trimStart, trimEnd
console.log(passenger.trimStart()); //Puru  , removes extra spaces from start
console.log(passenger.trimEnd()); //  Puru, removes extra spaces from end

// function to fix capitalization in name
const fixCapitalization = function (name) {
  const lowerName = name.toLowerCase();
  const correctName = lowerName[0].toUpperCase() + lowerName.slice(1);
  console.log(correctName);
};
fixCapitalization("pURU"); //Puru
```

### replace() and replaceAll() method

```js run
const priceGB = "288,97£";
const priceUS = priceGB.replace("£", "$").replace(",", "."); //288.97$

const announcement =
  "All passengers come to boarding door 23. Boarding door 23!";
console.log(announcement.replace("door", "gate")); //All passengers come to boarding gate 23. Boarding door 23! --> only first occurence is replaced
console.log(announcement.replaceAll("door", "gate")); ////All passengers come to boarding gate 23. Boarding gate 23!

//replace with regular expression
console.log(announcement.replace(/door/g, "gate")); //All passengers come to boarding gate 23. Boarding gate 23!
```

### includes(), startsWith(), and endsWith() methods

```js run
const plane = "A320neo";
console.log(plane.includes("A320")); //true, checks if the string is present in the original string
console.log(plane.includes("Boeing")); //false
console.log(plane.startsWith("A")); //true, checks if the string starts with the given string
console.log(plane.startsWith("a")); //false, case sensitive
console.log(plane.endsWith("neo")); //true, checks if the string ends with the given string
console.log(plane.endsWith("A")); //false
```

### split() method

Split method is used to split a string into an array of substrings based on a specified separator.

```js run
console.log("a+very+nice+string".split("+")); //["a", "very", "nice", "string"]
console.log("Purushottam Kumar".split(" ")); //["Purushottam", "Kumar"]

const [firstName, lastName] = "Purushottam Kumar".split(" ");
console.log(firstName); //Purushottam
console.log(lastName); //Kumar
```

### join() method

Join method is used to join all elements of an array into a string.

```js run
const [firstName, lastName] = "Purushottam Kumar".split(" ");
const newName = ["Mr.", firstName, lastName.toUpperCase()].join(" ");
console.log(newName); //Mr. Purushottam KUMAR
```

### padStart() and padEnd() methods

PadStart and PadEnd methods are used to add padding to a string. The padStart method adds given character to the start of a string, while the padEnd method adds given character to the end of a string.

```js run
const message = "Go to gate 23!";
console.log(message.padStart(25, "+")); //++++++++++Go to gate 23!, adds + to start of string to make it 25 characters long
console.log("Puru".padStart(25, "+")); //++++++++++++++++++++Puru

console.log(message.padEnd(25, "+")); //Go to gate 23!++++++++++, adds + to end of string to make it 25 characters long
console.log("Puru".padEnd(25, "+")); //Puru++++++++++++++++++++

console.log("Puru".padStart(25, "+").padEnd(30, "+")); //++++++++++++++++++++Puru+++++

//function to mask credit card number
const maskCreditCard = function (number) {
  const str = number + ""; //convert number to string
  const last = str.slice(-4);
  return last.padStart(str.length, "*");
};
console.log(maskCreditCard(1234567890)); //*********7890
console.log(maskCreditCard("1234567890")); //*********7890
```

### repeat() method

Repeat method is used to repeat a string a specified number of times.

```js run
const message2 = "Bad weather... All departures delayed... ";
message2.repeat(5);
/*
  Bad weather... Alldepartures delayed... Bad weather... Alldepartures delayed... Bad weather... Alldepartures delayed... Bad weather... Alldepartures delayed... Bad weather... Alldepartures delayed... 
*/
```
