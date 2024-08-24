# Basics

## Type Conversion and Coersion

### Type Conversion

Conversion done explicitely by user :

```js run
//String + number = string
const inputYear = "1997";
console.log(inputYear + 18); //by default JS will not add but concatenate (converting number (18) to string when other part is also string)

//converting string into number
console.log(Number(inputYear)); //--> Conversion
console.log(Number(inputYear) + 18);

console.log(Number("Puru")); //will give NaN (Not a number) which means invalid number as 'Puru' is not a number (typeof NaN is number)
```

### Type Coersion

Automatically done by JavaScript

```js run
console.log("I'm " + 23 + " years old"); //23 will be automatically converted to string
console.log("23" + "10" + 3); // (+) converted number to string
console.log("23" - "10" - 3); // (-) converted string to number
console.log("23" * 3); // (*) converted string to number
console.log("23" / 2); // (/) converted string to number
```

```js run
let n = "1" + 1; // 11
n = n - 1; //11-1
console.log(n); //10
```

```js run
console.log(2 + 3 + 4 + "5"); // (2+3+4)+'5' --> 9+'5' --> 95
console.log("10" - "4" - "3" - 2 + "5"); //(10-4-3-2)+'5' --> 1+'5' --> 15
```

## Truthy and Falsy Values

**5 falsy values :** 0, ' ', undefined, null, NaN

```js run
console.log(Boolean(0)); //false
console.log(Boolean("")); //false
console.log(Boolean(undefined)); //false
console.log(Boolean(null)); //false
console.log(Boolean(NaN)); //false
```

**Truthy values :**

```js run
console.log(Boolean({}));
console.log(Boolean(24));
console.log(Boolean("ahsg"));
```

## Equality Operators: == vs. ===

=== is strict equality operator, becuase it does not perform type coersion

```js run
console.log(2 === 2); //true
console.log(2 === "2"); //false
```

== is loose equality operator, becuase it does perform type coersion

```js run
console.log(2 == 2); //true
console.log(2 == "2"); //true
```

## Statements and Expressions

Statement doesn't produce a value. ends witha ;
Expressions produce a value. In string literals, we can only inser expressions and not statements.

## Ternary Operator

```js run
const age = 23;
age >= 18
  ? console.log("I like to drink wine")
  : console.log("I like to drink juice");
```
