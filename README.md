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

If we call this function without an argument, we get back `NaN`--aka code for future sorrow.


#### The Compiler


### Should I learn or use it?

## Getting Started
## Just Enough Syntax
## Joyful Tools
## Finding and Using Packages
## Writing and Publishing a Package
## Showing off Work
## Going Further: JavaScript & Elm together
