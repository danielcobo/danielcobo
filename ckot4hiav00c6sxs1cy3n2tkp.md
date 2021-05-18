## Bulletproof type checks in JavaScript

## Using typeof

To check what type is a variable you would most likely use the `typeof` function. You can even skip the parenthesis (`typeof someVariable` is same as `typeof(someVariable)`).

`typeof` works great for:
- strings
- numbers (not if checking integer vs. float)
- [big integers][1]
- [symbols][2]
- functions 
- booleans
- checking if a variable is undefined


```javascript
typeof "hello"; //string

typeof 1; //number
typeof 0.1; //number

typeof 10000000000000000000000000n; //bigint

typeof Symbol("SomeSymbol"); //symbol

typeof true; //boolean
typeof false; //boolean

typeof function(){}; //function
``` 

It kinda sucks if you need to check:
- arrays
- integer
- floating number
- `null`
- `NaN`
- regex
- a date object
- a custom object

👑 Keep calm and check the solutions below 👍

## Check if a variable is an array

The use built-in `.isArray` method will return true/false if the value is an array. This is way better than the old school method of duck typing when you had to check if the variable is an object and has the `.length` property.

```javascript
Array.isArray([]); // true
``` 

## Check if a variable is an integer

The `Number.isInteger` method returns true if the variable is an integer.

```javascript
Number.isInteger(1); //true
Number.isInteger(0.1); //false
Number.isInteger(1000000000000000000000n); //false
Number.isInteger(""); //false
``` 

## Check if a variable is a floating number
It's a two step process:
1. Check if the value is a numbe
2. Check if the value is not an integer

```javascript
// Solution
const isFloat = function isFloat(val){
  return (typeof val === 'number' && !Number.isInteger(val));
}

// Test
const floatNumber = 0.1;
console.log(isFloat(floatNumber)); //true
``` 

## Check if a variable is `null`

Best way is to do a comparison.

```javascript
const iamnull = null; 
const zero = 0;

iamnull === null; //true
zero === null; //false
```

## Check if a variable is `NaN`

The `Number.isNaN` method returns true if the variable is an integer. 

It's an improvement of the older `isNaN` function, since in that case you should also combine it with `typeof` check for number, otherwise something like `isNaN(undefined)` would also return true.

```javascript
Number.isNaN(NaN); //true
Number.isNaN(undefined); //false
```

## Check if a variable is a regular expression
To verify a variable is a regex simply check if it's an instance of the `RegExp` object.

```javascript
/I am a regex/ instanceof RegExp //true
true instanceof RegExp //false
```

## Check if a variable is a `Date` object

Similar to regex, for dates we can also check if the value is an instance of the `Date` object.

```javascript
new Date() instanceof Date //true
true instanceof Date //false
```

## Check if a variable is a custom object
`typeof` will return `"object"` not just for objects but also for things like arrays and `null`. Your best option is to combine `typeof` with elimination. 

```javascript
const isObject = function isObject(val) {
  return (
    typeof val === 'object' &&
    !Array.isArray(val) &&
    val !== null &&
    val instanceof RegExp === false &&
    val instanceof Date === false
  );
};
```
You can adjust this based on what you want to qualify as an object, however the objects I would like to validate are non-builtin JavaScript objects aka. objects defined by the developer.


[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt
[2]: https://developer.mozilla.org/en-US/docs/Glossary/Symbol