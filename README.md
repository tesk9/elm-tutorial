# Elm Tutorial

## Elm: An Introduction

### What is it?

Elm is a nice-to-write and nice-to-read language that compiles to JavaScript. Using core language ecosystem features, like the html library, Elm lets developers write the bulk of their frontend code in a single language. Developers writing Elm also follow The Elm Architecture, which is a baked-in strategy for modeling state across the application.

### Why do people like it?

#### Functional Style

Functional/Object oriented is to some degree a question of taste. Using either strategy well can produce code with clear separation of concerns, encapsulation, and ease in developer understanding. The centrality of functions in Elm, however, makes code predictable and consistent. Fundamentally, it's nice to know, with 100% certainty, that every time a function is invoked with the same arguments, it'll return the same values.

Consider:
```
var counter = 0;
function incrementCounter () {
    counter++
}
```

Against:
```
function incrementCounter (counter) {
    return ++counter
}
```

#### Type Safety

Types describe values. Developers can specify what kind of values their functions can use, and not allow the function to be used with any other values. Consider the `incrementCounter` example from before:

```
function incrementCounter (counter) {
    return ++counter
}
```

If we call this function without an argument (or with a string or object), we get back `NaN`--aka code for future sorrow. Plus, for some arrays (`[]`, `["100"]`), we actually do get back a number.

Let's write a type-signature the Elm way:

```
incrementCounter : number -> number
```
```
function incrementCounter (counter) {
    if (typeof counter === 'number') {
        return ++counter
    } else {
        throw "type error: called incrementCounter with non-number argument."
    }
}
```

Okay! So in JS, to make sure incrementCounter is called just with numbers, we make our callee handle an exception.

But wait--our function is named `incrementCounter`. The "counter" part of that implies that we're talking about integers, not floats. Right now, that's not encapsulated by our Elm type signature or by our JS code.

Let's finish writing out the Elm code first:


```
incrementCounter : Int -> Int
incrementCounter counter =
    counter + 1
```

Note that now we can only call `incrementCounter` with an integer argument, and that it will always hand back an integer too.


In JS:

```
// Polyfill for IE and Opera Mobile from [MDN Number.isInteger entry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger)
Number.isInteger = Number.isInteger || function(value) {
  return typeof value === 'number' &&
    isFinite(value) &&
    Math.floor(value) === value;
};

function incrementCounter (counter) {
    if (Number.isInteger(counter)) {
        return ++counter
    } else {
        throw "type error: called incrementCounter with non-integer argument."
    }
}
```

Note that we still may have some trouble! `1.0000000` is considered an integer, which is probably fine but maybe won't be fine.


#### The Compiler

Type safety works through the compiler. The compiler checks that the types of various values and functions line up, so that there aren't surprises when code is live. The Elm Compiler in particular is known for helpful error messages. Let's revisit the counter example.

If I try to increment `1.000000`, I'll get an error message like this, plus details about line number:

```
The argument to function incrementCounter is causing a mismatch.
Function incrementCounter is expecting the argument to be:

Int

But it is:

Float

Hint: Elm does not automatically convert between Ints and Floats. Use toFloat
and round to do specific conversions.
```

Oops, `1.000000` isn't an Int, but we can make it one by rounding!

```
ourFloat : Float
ourFloat =
    1.000000


counter : Int
counter =
    round ourFloat

incrementedCounter : Int
incrementedCounter =
    incrementCounter counter
```

Or, more succinctly:

```
incrementedCounter : Int
incrementedCounter =
    incrementCounter (round 1.000000)
```

The compiler excels at providing easy-to-read and helpful error messages for type mismatches. It has a harder time with syntactic errors, so we'll spend some time making sure we've all got basic syntax down. Please also don't hesitate to ask questions about surprising or confusing compiler messages.

### Should I learn or use it?

Yes! We'll talk more about adding Elm to an existing application and about using JavaScript alongside Elm at the end. Even if work constraints and considerations prevent you from using Elm at this time, there are still advantages to learning Elm. Writing Elm encourages writing safe code in any language, and illustrates the benefits of limiting side effects and having simple, unidirectional data flow.

## Getting Started
## Just Enough Syntax
## Joyful Tools
## Finding and Using Packages
## Writing and Publishing a Package
## Showing off Work
## Going Further: JavaScript & Elm together
