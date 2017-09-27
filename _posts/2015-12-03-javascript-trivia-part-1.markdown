---
layout: post
title: JavaScript Trivia - Part 1 - Types
date: '2015-12-03 04:16:25'
tags:
- javascript
- trivia
---

After reading the series of books *You Don't know JS*, I wanted to sum up all the useful information contained in the book about the JS language (in fact mainly the EcmaScript 2015 language) in the format of a short paragraph about pitfalls or technical concepts, followed by questions & responses.

The posts of the series can be used as a reference to prepare any technical interview about the JS language, or as a simple remainder.

This part 1 deals with **Types**.

---
## Types and the *typeof* operator pitfalls

ES6 is composed of **7 types**: **null, undefined, boolean, string, symbol and object (the only type that is not a primitive)**. *Typeof* is the operator that inspect the type of a given **value**. *Typeof* is really useful but its usage comport many pitfalls: 

- First, there is not a direct 1 to 1 match between the real type of a value and the result of using typeof on that value:
  
```javascript
typeof null == 'object'; // returns true, typeof null not returns 'null' !!!
```

**Q1**: Can you write a function isNull that returns true if the given value is null ?

```javascript
function isNull(obj) { return !obj && typeof obj === 'object'; }
// !obj is true for false, NaN, undefined, null, 0
// only the test typeof obj == 'object' discriminate the null value
```

The other non direct match is the function:

```javascript
typeof function() {} === 'function'; // returns true
```
Why 'object' is not returned ? That's a pitfall again, a function is in fact a **subtype of an object**, that's why you can attach properties to functions as normal objects. So, on the same way, as arrays are subtypes of objects, typeof will returns 'array' ? In fact no:

```javascript
typeof [] === 'object' // Rrrrhhh, really not consistent !
```

**Q2**: Can you write a function isArray that detect that the given object is an array ?

```javascript
function isArray(obj) { return toString.call(obj) === '[object Array]'; }
isArray([1,2,3]) // returns true
``` 

**Q3**: Can you write a function isObject that returns true when the provided value is an object, at the sense of a Js type?

```javascript
function isObject(obj) {
  var type = typeof obj;
  return type === 'function' || (type === 'object' && !!obj); // null is recognizes as an object but is falsy
}

isObject([1,2,3]) // true
isObject(function(){}) // true
isObject(null) // false
```

To finish on a good note for *typeof*, it comes to our rescue for 'undeclared' variables, where it can be used to avoid ReferenceError if a variable is undeclared:

```javascript
if(typeof c === 'undefined' ) {
  c = function() { ... };
} 
// This is used a lot inside modules.
```

---
## Arrays & Strings pitfalls

Arrays can support string based properties, but they are not participating to the array length. You must beware in the case that the string provided as a property correspond to a base 10 number, because you can create empty array slots:

```javascript
var a = [];
a['13'] = 4;
a.length; // 14
a['foo'] = 'bar';
a.length; // 14 !!!
```

String are not arrays of characters and are **immutable**, that means that all the String functions not alter the string in place but returns a new string. Many of the arrays functions can be useful in String processing and can be *borrowed* from the Array prototype, but beware to only borrow functions that do not realize mutations !

```javascript
var a = 'hello';
Array.prototype.join.call(a, '-') // returns 'h-e-l-l-o'
```

**Q4**: Write a simple function that returns a boolean indicating whether or not a string is a palindrome.

```javascript
function isPalindrome(str) {
  var str = str.replace(/\W/g, '').toLowerCase(); // do not change the value of str in place
  return (str == str.split('').reverse().join(''));
}
``` 

---
## Numbers

We know well that there is no specific Integer type in JS and that all the numbers are represented as floating numbers.

**Q5**: Write a function that determines if a number is an integer:

```javascript
function isInteger(x) { return typeof x === 'number' && x % 1 === 0; }
``` 

**NaN** is a special number that represents an *invalid number* or a *failed number in computation*. This is a better definition than *Not a Number*, because this is one !!

```javascript
typeof NaN === 'number'; // true
```

**Q6**: Write a polyfill of the ES6 function Number.isNaN that test if the value given is NaN

```javascript
if (!Number.isNaN) {
  Number.isNaN = function(x) {
    return x !== x; // That's magic, NaN is the only value that is never equal to itself 
  }
}
```

JS has a positive zero but a ***negative zero* too:

```javacript
var a = 0 / -3; // returns -0
```

The JSON API is strange on how it treats -0, because JSON.stringify(-0) returns "0", but at the inverse, JSON.parse("-0") returns -0 !!

**Q7**: Write a function that tests if a given number is a negative zero

```javascript
function isNegZero(n) {
  var n = Number(n);
  return (n === 0) && (1 / n === -Infinity);
}
```
<br/>
---
This close our first part of the serie. If the next part, we will speak about the JS grammar tricks.