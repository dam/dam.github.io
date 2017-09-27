---
layout: post
title: JavaScript Trivia - Part 2 - Grammar
date: '2015-12-04 19:12:05'
tags:
- javascript
- trivia
---

In this part, we are looking to the JS Grammar tricks.

---
## Statements

Each statements in JS returns a completion value (including *undefined*), that is easily visible in your console. The problem is that we cannot capture the completion value of a statement and assign it to a variable (at least without tricks).

One of the trick is to use eval (The EVIL):

```javascript
var a, b;
a = eval("if(true) { b = 4 + 38;}");
a === 42; // true
``` 

Thats' ugly but there is a proposal in **ES7** to create a *do* expression, which will execute the block and the final statement completion value will become the completion value of the *do* expression, that can be assigned:

```javascript
var a, b;
a = do {
  if(true) { b = 4 + 38; }
}
a === 42; // true
```

In JS, **labeled statements** exists, such as: 

```javascript
foo: for (var i=0; i<4; i++) {
  for (var j=0; j<4; j++) {
    if(label_jump) {
      continue foo;
    }
    // else, continue following code
  }
}
```

They can be useful and allow a special form of *goto*: *label jumps*. But there is a problem on how JSON is dealing with this labels, because trying to run JSON.stringify() on a piece of code that content this labeled statement, will generate an Error. So truly, **JSON is a subset of the JS syntax** !

---
## Expressions side effects

We start this section with some questions.

**Q1**: What are the values of a and b, given this piece of code ?

```javascript
var a = 42;
var b = (a++);
```
response: a = 43, b = 42. 

**Q2**:What are the outputs of the following lines of code ?

```javascript
(function(){
  var a = b = 3;
})();

console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined');
```

response: false and true, so a is not defined (referencing a generates a ReferenceError), b is defined.

This is a problem due to the misconception of chaining assignments. We can fix this problem, by using the **strict mode** or explicitly **declaring b as a variable**.

**Q3**: Can you explain this well-know JS Gotcha ?

```javascript
[] + {}; // '[object Object]'
{} + []; // 0
```

response: in the first line {} appears for the + operator to be an empty object, but in contrary, {} in interpreted as a standalone empty block.

## Special nuances of grammar

JavaScript doesn't have an else if clause, even if you can do:

```javascript
if(a) {}
else if(b) {}
else {}
``` 

This is an hidden characteristic, we are only using the fact that *if* and *else* statements are allowed to omit the {}