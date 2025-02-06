# Working with Arrays

## Slice and Splice

- Slice: It returns a new array with the elements from the start index to the end index.
- Splice: It returns the removed elements from the array.

## slice()

```js run
//slice

const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.slice(4)); // [5, 6, 7, 8, 9, 10] --> start index 4 to end, index 4 is included
console.log(arr.slice(4, 7)); // [5, 6, 7] --> start index 4 to end index 7, index 7 is not included
console.log(arr.slice(-4)); // [7, 8, 9, 10] --> start index -4 to end, index -4 is included
console.log(arr.slice(-4, -1)); // [7, 8, 9] --> start index -4 to end index -1, index -1 is not included
console.log(arr.slice()); // copy of the array
console.log(arr.slice(0)); // copy of the array
console.log(arr.slice(0, arr.length)); // copy of the array
console.log(arr.slice(-1)); // [10] --> last element
```

## splice()

```js run
//splice --> 2nd argument is the number of elements to remove

const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.splice(4)); // [5, 6, 7, 8, 9, 10] --> these items are removed from array,  start index 4 to end, index 4 is included
console.log(arr); // [1, 2, 3, 4] --> original array

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.splice(4, 3)); // [5, 6, 7] --> 3 items are removed from array, start index 4 to end index 7, index 7 is not included
console.log(arr); // [1, 2, 3, 4, 8, 9, 10] --> original array

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.splice(-4)); // [7, 8, 9, 10] --> last 4 items are removed from array.
console.log(arr); // [1, 2, 3, 4, 5, 6] --> original array

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.splice(-4, 3)); // [7, 8, 9] --> last 3 items are removed from array.
console.log(arr); // [1, 2, 3, 4, 5, 6, 10] --> original array

//splice with 3rd argument
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.splice(4, 3, 11, 12, 13)); // [5, 6, 7] --> 3 items are removed from array, start index 4 to end index 7, index 7 is not included and 11, 12, 13 are added
```

## reverse()

- It reverses the array in place. It mutates the original array.

```js run
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr.reverse()); // [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
console.log(arr); // [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

## concat()

- It is used to merge two or more arrays. It does not mutate the original array.

```js run
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [6, 7, 8, 9, 10];
const concatArr = arr1.concat(arr2);
console.log(concatArr); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
//same as : const concatArr = [...arr1, ...arr2];
```

## join()

- It is used to join the elements of an array into a string. It does not mutate the original array.
- It returns a string.

```js run
const arr = [1, 2, 3, 4, 5];
console.log(arr.join("-")); // 1-2-3-4-5
console.log(arr.join()); // 1,2,3,4,5 -->default separator is comma
console.log(arr.join("")); // 12345
```

## at()

- It is used to get the element at a specific index.
- It returns undefined if the index is out of range.
- We can use negative index to get the element from the end of the array.

```js run
const arr = [1, 2, 3, 4, 5];
console.log(arr.at(2)); // 3
console.log(arr.at(5)); // undefined
console.log(arr.at(-1)); // 5
```

## forEach() --> Looping through an array

- It is used to loop through the array.
- It takes a callback function as an argument.
- The callback function takes three arguments: element, index, and the array itself.
- It does not return anything.
- It does not mutate the original array.
- foreach() is not suitable for breaking the loop. If you want to break the loop, you should use for loop.

```js run
const arr = [1, 2, 3, 4, 5];

arr.forEach((element, index, array) => {
  console.log(`Element at index ${index} is ${element}`);
  element = element * 2; // This will not change the original array
  console.log(`Modified element: ${element}`);
  console.log(`Original array: ${array}`);
});

/*
Element at index 0 is 1
Modified element: 2
Original array: 1,2,3,4,5

Element at index 1 is 2
Modified element: 4
Original array: 1,2,3,4,5
...
*/

console.log(`Final array: ${arr}`); // Final array: 1, 2, 3, 4, 5 -->  The original array remains unchanged
```

```js run
//program to find the sum of all elements in an array using forEach
const arr = [1, 2, 3, 4, 5];
let sum = 0;
arr.forEach((element) => {
  sum += element;
});
console.log(`Sum of all elements: ${sum}`); // Sum of all elements: 15

//OR
let sum = 0;
arr.forEach((element) => (sum += element));
console.log(`Sum of all elements: ${sum}`); // Sum of all elements: 15

//OR
let sum = 0;
arr.forEach(function (element) {
  sum += element;
});
console.log(`Sum of all elements: ${sum}`); // Sum of all elements: 15
```

## map() filter() reduce()

### map()

- Unlike forEach(), It is used to create a new array by applying a function to each element of the array.
- It returns a new array and does not mutate the original array.
- In arguments, we can pass the element, index, and the array itself.

```js run
const arr = [1, 2, 3, 4, 5];
const newArr = arr.map((element) => element * 2);
console.log(newArr); // [2, 4, 6, 8, 10]
console.log(arr); // [1, 2, 3, 4, 5] --> original array remains unchanged

//with three arguments
const newArr = arr.map((element, index, array) => {
  console.log(`Element at index ${index} is ${element}`);
  console.log(`Original array: ${array}`);
  return element * 2;
});
```

### filter()

- It is used to create a new array with elements that pass the test implemented by the provided function. (filter out the elements based on the condition)
- It returns a new array and does not mutate the original array.
- In arguments, we can pass the element, index, and the array itself.

```js run
const arr = [1, 2, 3, 4, 5];
const newArr = arr.filter((element) => element % 2 === 0);
console.log(newArr); // [2, 4]
```

### reduce()

- It is used to reduce the array to a single value.
- It takes a callback function as an argument.
- The callback function takes four arguments: accumulator, element, index, and the array itself.
- The accumulator is the value that is returned by the function.
- The accumulator stores the accumulated value.
- The initial value of the accumulator is the first argument of the reduce() method.
- If the initial value is not provided, the first element of the array is used as the initial value.
- It does not mutate the original array.

```js run
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((accumulator, element, index) => {
  console.log(`Accumulator at index ${index} is ${accumulator}`);
  return accumulator + element;
}, 0); // 0 is the initial value of the accumulator
console.log(sum); // 15
/*
Accumulator at index 0 is 0
Accumulator at index 1 is 1
Accumulator at index 2 is 3
Accumulator at index 3 is 6
Accumulator at index 4 is 10
*/
```

```js run
//program to sort the array in ascending order using reduce
const arr = [5, 2, 8, 1, 4];
const sortedArr = arr.reduce((accumulator, element) => {
  if (accumulator.length === 0) {
    accumulator.push(element);
  } else {
    let i = 0;
    while (i < accumulator.length && element > accumulator[i]) {
      i++;
    }
    accumulator.splice(i, 0, element);
  }
  return accumulator;
}, []);
console.log(sortedArr); // [1, 2, 4, 5, 8]
```

## chaining methods

```js run
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];

movement
  .filter((mov) => mov > 0)
  .map((mov) => mov * 2)
  .reduce((acc, mov) => acc + mov, 0); // 6500 --> 200*2 + 450*2 + 3000*2 + 70*2 + 1300*2 --> filter out positive numbers, double them and sum them
```

## find()

It returns the first element that satisfies the condition. If no element satisfies the condition, it returns undefined.

The arguments of the callback function are element, index, and the array itself.

```js run
const arr = [1, 2, 3, 4, 5];
const result = arr.find((element) => element > 2);
console.log(result); // 3

const result = arr.find((element) => element > 5);
console.log(result); // undefined
```

## findIndex()

It returns the index of the first element that satisfies the condition. If no element satisfies the condition, it returns -1.

The arguments of findIndex() are element, index, and the array itself.

```js run
const arr = [1, 2, 3, 4, 5];
const result = arr.findIndex((element) => element > 2);
console.log(result); // 2

const result = arr.findIndex((element) => element > 5);
console.log(result); // -1

//with all three arguments
const result = arr.findIndex((element, index, array) => {
  console.log(`Element at index ${index} is ${element}`);
  console.log(`Original array: ${array}`);
  return element > 2;
});
```

## some()

It returns true if at least one element satisfies the condition. Otherwise, it returns false.
The arguments of some() are element, index, and the array itself.

```js run
const arr = [1, 2, 3, 4, 5];
const result = arr.some((element) => element > 2);
console.log(result); // true

const result = arr.some((element) => element > 5);
console.log(result); // false

//with all three arguments
const result = arr.some((element, index, array) => {
  console.log(`Element at index ${index} is ${element}`);
  console.log(`Original array: ${array}`);
  return element > 2;
});
```

## every()

It returns true if all elements satisfy the condition. Otherwise, it returns false. The arguments of every() are element, index, and the array itself.

```js run
const arr = [1, 2, 3, 4, 5];
const result = arr.every((element) => element > 2);
console.log(result); // false

const result = arr.every((element) => element > 0);
console.log(result); // true
```

## flat()

Flat() is used to flatten the nested arrays. It returns a new array.

It takes an argument that specifies the depth of the nested arrays. The default value is 1.

```js run
const arr = [1, 2, [3, 4, [5, 6]]];
console.log(arr.flat()); // [1, 2, 3, 4, [5, 6]]

//with depth
console.log(arr.flat(2)); // [1, 2, 3, 4, 5, 6]
```

## flatMap()

flatMap() is used to map each element using a mapping function and then flatten the result into a new array.

It is equivalent to using map() and then flat(). It returns a new array.

```js run
const arr = [1, 2, [3, 4], [5, 6]];
const newArr = arr.flatMap((element) => element * 2); // [2, 4, 6, 8, 10, 12]
```

```js run
const accounts = [
  { name: "John Doe", movements: [200, 450, -400, 3000, -650, -130, 70, 1300] },
  { name: "Jane Doe", movements: [500, 340, -150, 3000, -650, -130, 70, 1300] },
];

//total deposits in all accounts using map and then flat and then reduce
const totalDeposits = accounts
  .map((acc) => acc.movements) // [[200, 450, -400, 3000, -650, -130, 70, 1300], [500, 340, -150, 3000, -650, -130, 70, 1300]]
  .flat() // [200, 450, -400, 3000, -650, -130, 70, 1300, 500, 340, -150, 3000, -650, -130, 70, 1300]
  .reduce((acc, mov) => acc + mov, 0); // 8620

//using flatMap()
const totalDeposits = accounts
  .flatMap((acc) => acc.movements) // [200, 450, -400, 3000, -650, -130, 70, 1300, 500, 340, -150, 3000, -650, -130, 70, 1300]
  .reduce((acc, mov) => acc + mov, 0); // 8620
```

## sort()

The `sort()` method sorts the elements of an array in place and returns the sorted array. The default sort order is according to string Unicode code points.

**Default Sorting** By default, sort() converts elements to strings and sorts them in ascending order.

```js run
const fruits = ["banana", "apple", "cherry"];
fruits.sort();
console.log(fruits); // Output: ['apple', 'banana', 'cherry']

const numbers = [40, 1, 5, 200];
numbers.sort();
console.log(numbers); // Output: [1, 200, 40, 5] (sorted as strings)
```

**Sorting with a Compare Function** To sort numbers in ascending order, you can use a compare function.

```js run
const numbers = [40, 1, 5, 200];
numbers.sort((a, b) => a - b);
console.log(numbers); // Output: [1, 5, 40, 200]
```

**Compare function** The compare function compares all the values in the array, two values at a time (a, b).
The compare function should return:

- A negative value if the first argument is less than the second (a < b).
- Zero if the two arguments are equal (a === b).
- A positive value if the first argument is greater than the second (a > b).

```js run
const numbers = [40, 1, 5, 200];
numbers.sort((a, b) => b - a); // sort in descending order, if b is less than a, then b comes first
console.log(numbers); // Output: [200, 40, 5, 1]

//OR using compare function
const compare = (a, b) => {
  if (a < b) return -1;
  if (a > b) return 1;
  return 0;
};
// here, if a is less than b, then a comes first in the sorted array
```

Sort an array of objects by a property value.

```js run
const items = [
  { name: "apple", price: 50 },
  { name: "banana", price: 30 },
  { name: "cherry", price: 20 },
];

items.sort((a, b) => a.price - b.price); // here if a.price is less than b.price, then a comes first
console.log(items);
// Output:
// [
//   { name: 'cherry', price: 20 },
//   { name: 'banana', price: 30 },
//   { name: 'apple', price: 50 }
// ]
```

## Create and fill arrays

We can create and fill arrays using the `Array()` constructor and the `fill()` method.

```js run
const arr = Array(5); // creates an array with 5 empty slots
//OR
// const arr = new Array(5); //both are same
console.log(arr); // Output: [ <5 empty items> ]

const arr = Array(5).fill(0); // creates an array with 5 elements, all elements are 0
console.log(arr); // Output: [0, 0, 0, 0, 0]

const arr = Array(5).fill("hello"); // creates an array with 5 elements, all elements are "hello"
console.log(arr); // Output: ['hello', 'hello', 'hello', 'hello', 'hello']

//array with value 1 to 5 using fill and map
const arr = Array(5)
  .fill(0)
  .map((_, index) => index + 1); // creates an array with 5 elements, [1, 2, 3, 4, 5]
```

Using Array.from() method, from() takes two arguments, the first argument is an object with a length property, and the second argument is a map function.

```js run
const arr = Array.from({ length: 5 }); // creates an array with 5 empty slots
console.log(arr); // Output: [ <5 empty items> ]

const arr = Array.from({ length: 5 }, () => 0); // creates an array with 5 elements, all elements are 0
console.log(arr); // Output: [0, 0, 0, 0, 0]
```

Using the map() method

```js run
const arr = Array.from({ length: 5 }, (_, index) => index + 1); // creates an array with 5 elements, [1, 2, 3, 4, 5]
console.log(arr); // Output: [1, 2, 3, 4, 5]
```
