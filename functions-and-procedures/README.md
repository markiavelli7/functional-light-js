# Functions and Procedures

``` javascript
function addNumbers(x = 0, y = 0, z = 0, w = 0) {
  var total = x + y + z + w;
  console.log(total);
}
```

addNumbers IS NOT A FUNCTION. It is a procedure. A procedure is a collection of operations (things that we need to do in a program). Procedures can take inputs, return outputs, print things to the console, be called multiple times, etc.

A function has to take some input and return an output. If it doesn't have a return keyword, it is not a function.

``` javascript
function extraNumbers(x = 2, ...args) {
  return addNumbers(x, 40, ...args);
}
```

extraNumbers is not a function because functions can only call other functions. functions can't call procedures, or they also become procedures.

``` javascript
extraNumbers();  // 42

extraNumbers(3, 8, 11);  // 62

function tuple(x, y) {
  return [x + 1, y - 1];
}

var [a, b] = tuple(...[5, 10]);

a; // 6
b; // 9
```

In our code above, as a programmer we are saying that we care about two outputs from the tuple call, var a and var b.

A function is a relationship between the input and the output. The goal of functional programming is to create relationships between the input and the output of our code that are obvious to the readers.

Function: the semantic relationship between the input and computed output.

``` javascript
function shippingRate(size, weight, speed) {
  return ((size + 1) * weight) + speed;
}
```

The name of the function shippingRate tells us the semantic relationship between the input and the computed output. If we put in size, weight, and speed as inputs, we are going to get a shipping rate back out.

Every function that we create should have a semantic name that describes the relationship between the input and the output.

## Side Effects

function shippingRate() {
  rate = ((size + 1) * weight) + speed;
}

```javascript
var rate;
var size = 1;
var weight = 4;
var speed = 5;
shippingRate();
rate;  // 57

size = 8;
speed = 6;
shippingRate();
rate;  // 42
```

shippingRate is a procedure, it doesn't have any parameters listed. It also doesn't have a return keyword.

For a function to truly be a function, not only does there have to be a relationship between the inputs and the output, the inputs and the outputs have to be direct. If the inputs or the output are indirect as they are above, then we don't have a true function.

When we say side effects, we mean indirect inputs or an indirect output, or both.

Functions have to take direct inputs (arguments passed to parameters) and they have to compute and return a value without assigning to anything outside of itself. It can't access anything outside of itself and it can't assign to anything outside of itself.

```javascript

function shippingRate(size, weight, speed) {
  return ((size + 1) * weight) + speed
}

var rate;

rate = shippingRate(12, 4, 5);   // 57
rate = shippingRate(8, 4, 6);   // 42 

```

shippingRate is a function, we have direct inputs and we are not assigning to anything outside of the function within the function itself. The return value is assigned to the rate variable outside of the function.

In JavaScript, there is no such as a function, there is only a function call. The characteristics of the function call is what matters, we need direct inputs and a direct output.

### Avoid side effects with function calls... if possible.

Side Effects
- I/O (console, files, etc)
- Database Storage
- Network Calls
- DOM
- Timestamps
- Random Numbers
- CPU Heat
- CPU Time Delay

### Minimize Side Effects

Side effects take away the benefits of why we do functional programming in the first place. They nullify the benefits of functional programming. We want to make it very obvious in the cases where we use side effects that we are doing so.

### Avoid side effects where possible, otherwise make them obvious.


# Pure Functions

``` javascript

// pure
function addTwo(x,y) {
  return x + y;
}

// impure
function addAnother(x,y) {
  return addTwo(x,y) + z
}

```

addAnother takes two inputs and then uses a z variable that is outside of itself. That is an indirect input. Using an indirect input invalidates that nature of what thought we would be expecting from this function.

``` javascript

const z = 1;

function addTwo(x, y) {
  return x + y;
}

function addAnother(x,y) {
  return addTwo(x,y) + z;
}

addAnother(20, 21);   // 42

```

It's a little more complicated than just saying that if a function accesses something from outside of itself, it is no longer a function.

In the above example, if inside of return addAnother, we were to say:

``` javascript
return addTwo(x,y) + 1  
```

We wouldn't have a problem with addAnother and would say that it is a function. So when we declare a constant above named z and assign the value 1 to it, we are almost doing exactly the same thing. We could have even used the keyword var, the result is the same. We are not re-assigning z, so it is acting as a constant.

The important key thing to keep in mind is this question:

### Is the variable being reassigned?

#### Not, can it be reassigned?

## Even more important: is it obvious to the reader that it can't be reassigned or that it doesn't get reassigned?

We want the reader of our code to understand that they don't need to worry about if our variables changed.

### Reducing Surface Area

``` javascript

function addAnother(z) {
  return function addTwo(x,y) {
    return x + y + z;
  };
}

addAnother(1)(20, 21);   // 42
```

When the addTwo function runs, it is referencing a variable outside of itself, it is referencing z, which is passed in from the addAnother function. This reduces the surface area of z being changed/altered. It can only be tampered with inside of the start of the addAnother function or the start of the addTwo function.

This code has a smaller surface area that we need to worry about with respect to whether z is reassigned or not. 

By reducing surface area, we are increasing readability.