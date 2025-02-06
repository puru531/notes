# Numbers

## Converting and checking numbers

In JavaScript, by default numbers are always floating point numbers. There is no separate integer type. So, 1 and 1.0 are the same number.

```js
console.log(1 === 1.0); // true
```

Numbers are stored in 64-bit format IEEE-754, also known as "double precision". This format stores numbers in binary form, which can only represent numbers as a sum of fractions of powers of 2. So they are composed of 0 and 1 bits only.

```js
console.log(0.1 + 0.2); // 0.30000000000000004 --> because of the binary representation, 0.1 + 0.2 is not exactly 0.3. It's a tiny bit more than that.

console.log(0.1 + 0.2 === 0.3); // false --> error in javascript floating point representation
```

Converting strings to numbers

```js
let str = "123";
console.log(typeof str); // string

let num = Number(str); // becomes a number 123
console.log(typeof num); // number
```

Using `+` operator to convert strings to numbers

Just like `Number(str)`, the unary plus `+str` converts `str` to a number.

```js
//conversion
let str = "123";
let num = +str; // becomes a number 123
```

Parsing numbers from strings

```js
console.log(Number.parseInt("100px")); // 100 --> parseInt will read from the beginning of the string until it encounters a character that is not a number, and then return the number value it has reached. If the first character is not a number, then it will return NaN.

//using a second argument to specify the base, second argument is the base for the numeral system
console.log(Number.parseInt("100px", 10)); // 100

//parseFloat
console.log(Number.parseFloat("12.5em")); // 12.5

//parseInt and parseFloat ignore leading and trailing whitespaces
console.log(Number.parseInt("12.3")); // 12
console.log(Number.parseInt("12.3.4")); // 12
console.log(Number.parseInt("  12.3  ")); // 12

//parseInt and parseFloat will return NaN if the first character can't be converted to a number
console.log(Number.parseInt("a123")); // NaN

//parseInt and parseFloat will return NaN if the string is empty
console.log(Number.parseInt("")); // NaN

//parseInt and parseFloat will return NaN if the string has only whitespaces
console.log(Number.parseInt("  ")); // NaN

//parseInt and parseFloat can be used without class name
console.log(parseInt("100px")); // 100
console.log(parseFloat("12.5em")); // 12.5
```

#### isNaN

```js
//isNaN --> checks if a value is not a number
console.log(isNaN("12")); // false
console.log(isNaN("12px")); // true
console.log(isNaN(23 / 0)); // false --> Infinity is a special numeric value that represents mathematical infinity
```

#### isFinite

```js
//isFinite --> checks for a regular number
console.log(isFinite("12")); // true
console.log(isFinite("12px")); // false
console.log(isFinite(23 / 0)); // false
```

#### isInteger

```js
// isInteger --> checks if a number is an integer
console.log(Number.isInteger(1)); // true
console.log(Number.isInteger(1.0)); // true
console.log(Number.isInteger(1.1)); // false
console.log(Number.isInteger("1")); // false
```

## Math and rounding

### Math.sqrt()

```js
//Math.sqrt --> returns the square root of a number
console.log(Math.sqrt(9)); // 3
```

### Exponentiation

```js
// Square and cubic roots
console.log(16 ** (1 / 2)); // 4 --> 4 is the square root of 16
console.log(8 ** (1 / 3)); // 2 --> 2 is the cubic root of 8

//Square root of negative numbers
console.log(Math.sqrt(-1)); // NaN
```

### Math.max()

```js
//Math.max --> returns the maximum of the arguments
console.log(Math.max(3, 5, -10, 0, 1)); // 5
console.log(Math.max(5, 10, 2, "25", 18)); // 25 --> the string '25' is converted to a number through type coercion
console.log(Math.max(5, 10, 2, "25px", 18)); // NaN --> the string '25px' can't be converted to a number, does not do parseInt or parseFloat
```

### Math.min()

```js
//Math.min --> returns the minimum of the arguments
console.log(Math.min(3, 5, -10, 0, 1)); // -10
console.log(Math.min(5, 10, 2, "25", 18)); // 2 --> the string '25' is converted to a number through type coercion
console.log(Math.min(5, 10, 2, "25px", 18)); // NaN --> the string '25px' can't be converted to a number, does not do parseInt or parseFloat
```

### Constants in Math

```js
//constants in Math
console.log(Math.PI); // 3.141592653589793
console.log(Math.E); // 2.718281828459045
```

### Random numbers (Math.random())

```js
//Math.random --> returns a random number between 0 and 1
console.log(Math.random()); // random number between 0 and 1
console.log(Math.trunc(Math.random() * 10) + 1); // random number between 0 and 10 --> trunc removes the decimal part --> Math.trunc(Math.random() * 10) will give a number between 0 and 9, adding 1 will give a number between 1 and 10
```

### Rounding integers

```js
console.log(Math.round(3.1)); // 3 --> rounds to the nearest integer
console.log(Math.round(3.6)); // 4

console.log(Math.ceil(3.1)); // 4 --> rounds up
console.log(Math.ceil(3.6)); // 4

console.log(Math.floor(3.1)); // 3 --> rounds down
console.log(Math.floor(3.6)); // 3
console.log(Math.floor(-3.1)); // -4 --> rounds down
console.log(Math.floor(-3.9)); // -4

console.log(Math.trunc(3.1)); // 3 --> removes the decimal part
console.log(Math.trunc(3.6)); // 3
console.log(Math.trunc(-3.1)); // -3 --> unlike floor, trunc does not round down
console.log(Math.trunc(-3.9)); // -3

//all above functions also support type coercion
console.log(Math.trunc("23.9")); // 23
console.log(Math.floor("23.9")); // 23
console.log(Math.ceil("23.9")); // 24
console.log(Math.round("23.9")); // 24
```

### Random integer between min and max

```js
const randomInt = (min, max) => {
  return Math.floor(Math.random() * (max - min) + 1) + min;
};
/*
1. Math.random() will give a number between 0 and 1
2. if we multiply it by (max - min), we will get a number between 0 and (max - min)
3. if we add min to it, we will get a number between min and max

Example:
randomInt(10, 20) will give a random number between 10 and 20
1. Math.random() will give a number between 0 and 1 (0 ... 1)
2. if we multiply it by (20 - 10), we will get a number between 0 and 10 (0 ... (20 - 10)) --> 0 ... 10
3. if we add 10 to it, we will get a number between 10 and 20 -> (10 ... (20 - 10) + 10) --> (10 ... 20)
*/
```

### Rounding to fixed number of decimal places

**Returns a string representation of the number**

```js
// toFixed --> rounds the number to the given number of decimal places and returns a string representation of the number
let num = 12.345;
console.log(num.toFixed(2)); // 12.35 --> rounds to 2 decimal places
console.log(num.toFixed(0)); // 12 --> rounds to 0 decimal places
console.log(num.toFixed(5)); // 12.34500 --> rounds to 5 decimal places

//all above functions returns a string representation of the number, to convert it to a number, use the unary plus operator
console.log(+(12.345).toFixed(2)); // 12.35 --> converts the string to a number

// with negative numbers
console.log((-12.345).toFixed(2)); // -12.35

// with numbers in string format --> supports type coercion
console.log("12.345".toFixed(2)); // 12.35
```

### Remainder

```js
console.log(5 % 2); // 1 --> 5 divided by 2 is 2 with a remainder of 1
console.log(8 % 3); // 2 --> 8 divided by 3 is 2 with a remainder of 2
console.log(6 % 3); // 0 --> 6 divided by 3 is 2 with a remainder of 0

// with negative numbers
console.log(-5 % 2); // -1 --> -5 divided by 2 is -2 with a remainder of -1
console.log(5 % -2); // 1 --> 5 divided by -2 is -2 with a remainder of 1

// with floating point numbers
console.log(6.1 % 3); // 0.1
console.log(6.1 % 3.1); // 0

// with strings
console.log("6" % "3"); // 0

//example to return the last digit of a number
let num = 123;
console.log(num % 10); // 3

//example to check if a number is even or odd
let num = 123;
console.log(num % 2 === 0 ? "even" : "odd");

//example to check if a number is a multiple of 3 and 5
let num = 123;
console.log(
  num % 3 === 0 && num % 5 === 0
    ? "multiple of 3 and 5"
    : "not a multiple of 3 and 5",
);
```

## Numeric separators

```js
//Numeric separators --> to make large numbers more readable
// let num = 1234567890; // without numeric separators, hard to read
let num = 1_234_567_890; // with numeric separators
console.log(num); // 1234567890 //no difference in the output

//not allowed :
// let num = _123; // error --> can't start with a separator
// let num = 123_; // error  --> can't end with a separator
// let num = 1._23; // error --> can't have a separator after a dot
// let num = 1_.23; // error --> can't have a separator before a dot
// let num = 1__23; // error --> can't have multiple separators together

// with strings
console.log("123_456_789_0"); // error --> can't have numeric separators in strings
```

## BigInt

```js
// Numbers are represented internally as 64-bit values, which can't represent integers larger than (2^53 - 1) (or less than -(2^53 - 1)) --> between (-9007199254740991 ... 9007199254740991)
// Only 53 bits are used to store the number, the rest are used to store the position of the decimal point, the sign, etc.

console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991

// BigInt is a special numeric type that can represent integers of arbitrary length, added in ES2020

const num1 = 1234567890123456789012345678901234567890;
console.log(num1); // 1.2345678901234568e+39 --> the number is rounded to 16 digits

const bigInt = 1234567890123456789012345678901234567890n; // n at the end of the number makes it a BigInt
console.log(bigInt); // 1234567890123456789012345678901234567890
//OR
const bigInt = BigInt("1234567890123456789012345678901234567890");
console.log(bigInt); // 1234567890123456789012345678901234567890n
```

### BigInt can't be mixed with regular numbers

```js
const bigInt = 1234567890123456789012345678901234567890n;
const num = 123;
console.log(bigInt + num); // error --> can't mix BigInt and regular numbers
```

### exception for comparison

```js
console.log(20n > 15); // true --> BigInt can be compared with regular numbers
console.log(20n === 20); // false --> BigInt can be compared with regular numbers
console.log(typeof 20n); // bigint
console.log(20n == "20"); // true --> BigInt can be compared with strings

// string concatenation
console.log(20n + "20"); // 2020
console.log(20n + " I am BigInt"); // 20 I am BigInt
```

### Math functions are not supported for BigInt

```js
Math.sqrt(16n); // error --> Math functions are not supported for BigInt
```

### Division

```js
console.log(10n / 3n); // 3n --> division returns the integer part of the result, removes the decimal part
console.log(10n % 3n); // 1n --> remainder
```

# Dates

## Creating a Date

```js
// 4 ways to create a date
const now = new Date();
console.log(now); // current date and time (Sun Aug 18 2024 02:15:01 GMT+0530 (India Standard Time))

console.log(new Date("August 18, 2024 08:00:00")); // Sun Aug 18 2024 08:00:00 GMT+0530 (India Standard Time) --> will parse the date

console.log(new Date("August 18, 2024")); // Sun Aug 18 2024 00:00:00 GMT+0530 (India Standard Time) --> will parse the date

console.log(new Date(2024, 7, 18, 8, 0, 0)); // Sun Aug 18 2024 08:00:00 GMT+0530 (India Standard Time) --> year, month, day, hour, minute, second (month is 0-based)

//autocorrecting the date
console.log(new Date(2024, 8, 32)); // Tue Oct 01 2024 00:00:00 GMT+0530 (India Standard Time) --> 32nd day of September is 1st October

console.log(new Date(0)); // Thu Jan 01 1970 05:30:00 GMT+0530 (India Standard Time) --> 0 is the timestamp for the beginning of the Unix epoch

//three days after the Unix epoch
console.log(new Date(3 * 24 * 60 * 60 * 1000)); // Sun Jan 04 1970 05:30:00 GMT+0530 (India Standard Time) --> 3 days * 24 hours * 60 minutes * 60 seconds * 1000 milliseconds
```

## Methods for dates

```js
const future = new Date(2037, 10, 19, 15, 23);
console.log(future); // Thu Nov 19 2037 15:23:00 GMT+0530 (India Standard Time)

console.log(future.getFullYear()); // 2037 --> returns the year
console.log(future.getMonth()); // 10 --> returns the month (0-based)
console.log(future.getDate()); // 19 --> returns the day of the month
console.log(future.getDay()); // 4 --> returns the day of the week (0-based)
console.log(future.getHours()); // 15 --> returns the hour
console.log(future.getMinutes()); // 23 --> returns the minutes
console.log(future.getSeconds()); // 0 --> returns the seconds

console.log(future.toISOString()); // 2037-11-19T09:53:00.000Z --> returns the date in ISO format --> follows the format YYYY-MM-DDTHH:mm:ss.sssZ --> International Standard for date and time representation

console.log(future.getTime()); // 2142248580000 --> returns the timestamp
console.log(new Date(2142248580000)); // Thu Nov 19 2037 15:23:00 GMT+0530 (India Standard Time) --> returns the date from the timestamp

console.log(Date.now()); // 1723928411358 --> returns the current timestamp

//setters
future.setFullYear(2040);
console.log(future); // Sun Nov 19 2040 15:23:00 GMT+0530 (India Standard Time) --> sets the year
//similarly, setMonth, setDate, setHours, setMinutes, setSeconds
```

## Operations with dates

```js
const future = new Date(2037, 10, 19, 15, 23);
console.log(+future); // 2142248580000 --> converts the date to a timestamp

// we can subtract dates to get the difference in milliseconds
const calcDaysPassed = (date1, date2) => {
  return Math.abs(date2 - date1) / (1000 * 60 * 60 * 24);
  // timestamp is divided by milliseconds to get date in seconds (1000 milliseconds in a second) --> seconds to minutes (60 seconds in a minute) --> minutes to hours (60 minutes in an hour) --> hours to days (24 hours in a day)
  // Math.abs is used to get the absolute value --> removes the negative sign
};

const days1 = calcDaysPassed(new Date(2037, 3, 14), new Date(2037, 3, 24)); // 10
const days2 = calcDaysPassed(
  new Date(2037, 3, 24),
  new Date(2037, 3, 14, 10, 8),
); // 10.4222222222 --> can be fixed by using Math.round
```

# Internationalization of Dates (Intl)

In JavaScript, the `Intl` object is used to format dates, numbers, and strings in a locale-sensitive way.

```js
const now = new Date(); //Sun Aug 18 2024 02:15:01 GMT+0530 (India Standard Time)
console.log(new Intl.DateTimeFormat("en-US").format(now)); // 8/18/2024 --> formats the date in US format --> DateTimeFormat function takes two arguments, the first argument is the locale, the second argument is an object that contains options --> .format() method is used to format the date with formatter given by the DateTimeFormat function

console.log(new Intl.DateTimeFormat("en-GB").format(now)); // 18/08/2024 --> formats the date in UK format

console.log(new Intl.DateTimeFormat("ar-SY").format(now)); // ١٨‏/٠٨‏/٢٠٢٤ --> formats the date in Arabic format

//options object for DateTimeFormat function
let options = {
  hour: "numeric",
  minute: "numeric",
};
console.log(new Intl.DateTimeFormat("en-US", options).format(now)); // 2:15 AM --> formats the date with hour and minute

options = {
  hour: "numeric",
  minute: "numeric",
  day: "numeric",
  month: "numeric", // numeric, long, 2-digit --> numeric = 8, long = August, 2-digit = 08
};
console.log(new Intl.DateTimeFormat("en-US", options).format(now)); // 8/18, 2:15 AM --> formats the date with hour, minute, day, and month

options = {
  hour: "numeric",
  minute: "numeric",
  day: "numeric",
  month: "long", // numeric, long, 2-digit --> numeric = 8, long = August, 2-digit = 08
  year: "numeric", // numeric, 2-digit --> numeric = 2024, 2-digit = 24
};
console.log(new Intl.DateTimeFormat("en-US", options).format(now)); // August 18, 2024, 2:15 AM --> formats the date with hour, minute, day, month, and year

options = {
  hour: "numeric",
  minute: "numeric",
  second: "numeric",
  day: "numeric",
  month: "long", // numeric, long, 2-digit --> numeric = 8, long = August, 2-digit = 08
  year: "numeric", // numeric, 2-digit --> numeric = 2024, 2-digit = 24
  weekday: "long", // numeric, long, short, narrow --> numeric = 1, long = Sunday, short = Sun, narrow = S
};
console.log(new Intl.DateTimeFormat("en-US", options).format(now)); // Sunday, August 18, 2024, 2:15:01 AM --> formats the date with hour, minute, second, day, month, year, and weekday

console.log(new Intl.DateTimeFormat("pt-PT", options).format(now)); // domingo, 18 de agosto de 2024 às 02:15:01 --> formats the date in Portuguese format
```

visit [ISO Language Code Table](http://www.lingoes.net/en/translator/langcode.htm) for locale codes

Getting locales from the user's browser

```js
console.log(navigator.language); // en-US --> returns the language of the user's browser

const now = new Date(); //Sun Aug 18 2024 03:10:28 GMT+0530 (India Standard Time)
console.log(new Intl.DateTimeFormat(navigator.language).format(now)); // 8/18/2024 --> formats the date in the user's browser language
```

# Internationalization of Numbers (Intl)

```js
const num = 3884764.23;
console.log(new Intl.NumberFormat("en-US").format(num)); // 3,884,764.23 --> formats the number in US format --> NumberFormat function takes two arguments, the first argument is the locale, the second argument is an object that contains options --> .format() method is used to format the number with formatter given by the NumberFormat function

console.log(new Intl.NumberFormat("de-DE").format(num)); // 3.884.764,23 --> formats the number in German format

console.log(new Intl.NumberFormat("ar-SY").format(num)); // ٣٬٨٨٤٬٧٦٤٫٢٣ --> formats the number in Arabic format

console.log(new Intl.NumberFormat("en-IN").format(num)); // 38,84,764.23 --> formats the number in Indian format

//options object for NumberFormat function
let options = {
  style: "unit", // decimal, percent, currency, unit
  unit: "mile-per-hour", // currency = USD, EUR, JPY, unit = mile-per-hour, kilometer-per-hour
};
console.log(new Intl.NumberFormat("en-US" options).format(num)); // 3,884,764.23 mph --> formats the number with unit

console.log(new Intl.NumberFormat("en-IN", options).format(num)); // 38,84,764.23 mph --> formats the number with unit in Indian format

// currency
options = {
  style: "currency",
  currency: "INR",
};
console.log(new Intl.NumberFormat("en-IN", options).format(num)); // ₹38,84,764.23 --> formats the number with currency in Indian format

// grouping
options = {
  style: "currency",
  currency: "INR",
  useGrouping: false, // whether to use grouping separators
};
console.log(new Intl.NumberFormat("en-IN", options).format(num)); // ₹3884764.23 --> formats the number with currency in Indian format without grouping
```

# setTimeout and setInterval

## setTimeout

`setTimeout` : executes a function or a piece of code once after a specified delay (in milliseconds)
Syntax :

```js
setTimeout(function, delay, [param1, param2, ...]);
```

```js
setTimeout(() => console.log("Hello World"), 2000); // Hello World --> after 2 seconds
```

Code execution does not stop while waiting for the timeout to finish. The function is executed after the delay, even if the delay is 0. And other code can run before the timeout.

example :

```js
console.log("Start");
setTimeout(() => console.log("Hello World"), 1000);
console.log("End");
/*
Start
End
Hello World --> after 1 second, even though the timeout is set to 1 second, the code execution does not stop, the function is executed after the delay
*/
```

### Arguments in setTimeout

All arguments after the delay are passed to the function

```js
setTimeout((text, num) => console.log(`${text} ${num}`), 2000, "Hello", 5); // Hello and 5 are passed as arguments to the function
```

```js
// setTimeout example
const timeout = setTimeout(
  (param1, param2) => {
    console.log("timeout", param1, param2);
  },
  2000,
  "param1", // arguments to the function
  "param2", // arguments to the function
); // Executes the function after 2 seconds
```

```js
setTimeout(console.log, 2000, "Hello", "World"); // Hello World --> console.log is a function, so it can be passed as an argument
```

### clearTimeout --> to cancel the timeout

```js
const timeout = setTimeout(() => console.log("Hello World"), 2000);
clearTimeout(timeout); // cancels the timeout
```

## setInterval

`setInterval` : executes a function or a piece of code repeatedly, with a fixed time delay between each call
Syntax :

```js
setInterval(function, delay, [param1, param2, ...]);
```

Basic example :

```js
setInterval(() => console.log("Hello World"), 2000); // Hello World --> every 2 seconds
```

### Arguments in setInterval

```js
const interval = setInterval(
  (param1, param2) => {
    console.log("interval", param1, param2);
  },
  2000,
  "param1", // arguments to the function
  "param2", // arguments to the function
); // Executes the function every 2 seconds
```

### clearInterval --> to cancel the interval

```js
const interval = setInterval(() => console.log("Hello World"), 2000);
clearInterval(interval); // cancels the interval
```

```js
// timer of 5 second for 2 minutes and cancel after completion
let i = 0;
const interval = setInterval(() => {
  console.log("Hello World");
  i++;
  if (i === 24) {
    clearInterval(interval);
  }
}, 5000);
```
