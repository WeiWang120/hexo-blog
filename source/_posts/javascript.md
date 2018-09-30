---
title: javascript
date: 2018-09-30 00:49:13
tags: javascript
---


Javasciprt is not a class-based language even there are classes since ES5.
It's a prototype-based scripting language


### Types in JavaScript

Numbers, Strings, Booleans, null, undefined and objects


### this and invocation pattern

* The __function invocation pattern__, __this__ will be bound to global object

```javascript
  const func = function () {
    return this
  }
```

* The __constructor invocation pattern__, with __new__ prefix will create a new object, in this case, __this__ will be bound to that new object.

```javascript
  const Quo = function (string) {
    this.status = string
  }

  const obj = new Quo("test")
  // obj.status = test
  // if invoked without new, this will be bound to the global object
```

If the function is defined by arrow function, you can't invoke it with __new__ prefix.

* The __apply invocation pattern__

The __apply__ method lets us construct an array of args to use to invoke a function, it takes two params.

The first is the value that should be bound to __this__, the second is an array of params.

### Currying

There is a way to reduce functions of more than one argument to functions of one argument

```javascript
  multiply = (n, m) => (n * m)
  multiply(3, 4) === 12 // true

  curryedMultiply = (n) => (m) => multiply(n, m)
  triple = curryedMultiply(3)
  // triple = (m) => multiply(3, m)
  triple(4) === 12 // true
```

Some examples:

* javascript bind method. bind() return a new function

* React and Redux and HOC

```javascript
export default connect(mapStateToProps)(TodoApp)
```
