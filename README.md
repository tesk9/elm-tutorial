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

Please follow along and type things!

### [Ellie](https://ellie-app.com/new) ELm LIve Editor

#### Hello, World

Let's start with the imports. We're importing an Html library and exposing `Html` and `text`. `Html` signifies a type and `text` is a function that takes a string and returns an html text node.

Change the import to be `(..)` and we'll be importing everything from `Html`.

`main` is special, and you'll see a `main` in every Elm app. For now, just notice that it's producing `Html`.

- Hit Compile, "Hello, World!"

Yay app is done!

#### Bird Counter

Not really. We're going to make a mini bird-counting application. Specifically, we'll be counting the mighty lesser prairie chicken.

Let's build out a view first.

Apart from `text`, most functions in the `Html` library take a list of attributes as their first argument and a list of children as their second.


```
main : Html msg
main =
    view


view : Html msg
view =
    div []
        [ h1 [] [ text "Prairie Chicken, Lesser" ]
        , h2 [] [ text "An Accounting of Hens" ]
        , div []
            [ text "We have seen: "
            , text "0 chickens."
            ]
        ]
```

Okay, so how do we avoid hardcoding the number of chickens we've seen? `view` is a function, and right now it doesn't take any arguments. Let's have it take the number of chickens we've seen:


```
main : Html msg
main =
    view lesserPrairieChickenCount

lesserPrairieChickenCount : Int
lesserPrairieChickenCount =
    0

view : Int -> Html msg
view count =
    div []
        [ h1 [] [ text "Prairie Chicken, Lesser" ]
        , h2 [] [ text "An Accounting of Hens" ]
        , div []
            [ text "We have seen: "
            , text (toString count ++ " chickens.")
            ]
        ]

```

Note that we need to use `toString` in order to go from our integer value to a string value.

Let's add a button that we'll click whenever we see a lesser prairie chicken.

```
main : Html msg
main =
    view lesserPrairieChickenCount

lesserPrairieChickenCount : Int
lesserPrairieChickenCount =
    0

view : Int -> Html msg
view count =
    div []
        [ h1 [] [ text "Prairie Chicken, Lesser" ]
        , h2 [] [ text "An Accounting of Hens" ]
        , div []
            [ text "We have seen: "
            , text (toString count ++ " chickens.")
            ]
        , viewCountButton
        ]


viewCountButton : Html msg
viewCountButton =
    button [] [ text "I see a new lesser prairie chicken!!!" ]
```

Okay, so we've got a button that doesn't do anything and a count that never changes. How do we wire it all up?

----TODO: add a graph of some kind here----

This is our first taste of the Elm Architecture.


```
main : Program Never Int msg
main =
    beginnerProgram
        { model = 0
        , update = -- do something???
        , view = view
        }

view : Int -> Html msg
view count =
    div []
        [ h1 [] [ text "Prairie Chicken, Lesser" ]
        , h2 [] [ text "An Accounting of Hens" ]
        , div []
            [ text "We have seen: "
            , text (toString count ++ " chickens.")
            ]
        , viewCountButton
        ]


viewCountButton : Html msg
viewCountButton =
    button [] [ text "I see a new lesser prairie chicken!!!" ]
```

Let's use our `incrementCounter` function again. We're going to assume that all the prairie chickens we see stay alive so we don't need to add a decrement function yet.

```
main : Program Never Int Msg
main =
    beginnerProgram
        { model = 0
        , update = update
        , view = view
        }

type Msg = Increment

update : Msg -> Int -> Int
update msg model =
    case msg of
        Increment ->
            incrementCounter model


incrementCounter : Int -> Int
incrementCounter counter =
    counter + 1


view : Int -> Html msg
view count =
    div []
        [ h1 [] [ text "Prairie Chicken, Lesser" ]
        , h2 [] [ text "An Accounting of Hens" ]
        , div []
            [ text "We have seen: "
            , text (toString count ++ " chickens.")
            ]
        , viewCountButton
        ]


viewCountButton : Html msg
viewCountButton =
    button [] [ text "I see a new lesser prairie chicken!!!" ]
```

Now there's a lot of new stuff in this step! For one, now we've got this `Msg` thing.
This is a Union Type--it's a way of defining an enumerable list of allowed values
that we'll consider to be of the type `Msg`. For now, note that now we have a
discrete list of allowed actions in our application--within our app, we can
Increment and that's it.


Finally, let's add an onClick handler to finish wiring everything up.


```
import Html.Events exposing (onClick)

...

viewCountButton : Html Msg
viewCountButton =
    button [ onClick Increment ] [ text "I see a new lesser prairie chicken!!!" ]
```


Versions:
- [Without notes](https://ellie-app.com/Z79YDXgtMSa1/0)
- [With notes](https://ellie-app.com/Z79YDXgtMSa1/1)

EXERCISE: Add decrement functionality. (Sad lesser prairie chicken!)
[SOLUTION](https://ellie-app.com/Z79YDXgtMSa1/2)

Speaker notes:
- walk through solution
- take questions

Hmm, seems like we've got a grammar mistake. "There are: 1 chickens" doesn't sound great!
Heads up--any if branch MUST have an else branch.

```
formatChickenCount count =
    if count == 1 then
        "1 chicken."
    else
        toString count ++ " chickens."
```

EXERCISE : Add this logic to the app!
EXERCISE: Prevent users from decrementing more chickens that is possible (
don't show the decrement button if there aren't any chickens).
[SOLUTION](https://ellie-app.com/Z79YDXgtMSa1/3)

### Installation

Let's get this killer application up and running locally!

Run `elm` on the command line. If the output includes "Elm Platform 0.18.0 - a way to run all Elm tools" then you're good to go!

Otherwise, follow the [instructions for your operating system](https://guide.elm-lang.org/install.html) or run `npm install elm`.

Speaker notes:
- Check to be sure everyone is ready to proceed.

Let's get a project set up.

```
> mkdir prairie-chicken-counter
> cd priarie-chicken-counter
> touch Main.elm
> elm-reactor
```

Now let's plop our bird-watching code in that Main file and refresh our browser. Tada!

## Just Enough Syntax

At this point, we've seen some code and we've written some code. We're going to briefly go
some syntax in the interest of getting confident and having a handy-dandy reference.

### Strings

Strings are denoted with double-quotation-marks, never single-quotation marks.

Strings are concatenated with the `(++)` operator, which can be used as an infix operator,
as we've already seen, or as a prefix operator.

```
exclaim : String -> String
exclaim comment =
    comment ++ "!"

noSeriouslyExclaimTho : String -> String
noSeriouslyExclaimTho comment =
    (++) comment "!!!!"
```

More complex operations with Strings can be explored in the [String documentation](http://package.elm-lang.org/packages/elm-lang/core/5.1.1/String).

### Numbers

```
float : Float
float =
    99.99


integer : Int
integer =
    round float

```

### Lists

```
anEmptyList : List a
anEmptyList =
    []


aListOfStrings : List String
aListOfStrings =
    List (List.intersperse " " [ "Just", "some", "strings!" ])


hasMyName : List String -> Bool
hasMyName nameList =
    List.member "Tessa" nameList
```

### Functions

We've already seen a bunch of functions!

```
iAmAFunction : String -> String
iAmAFunction argument =
    "This is the body of the function, and we can add the argument: " ++ argument


iAmAFunctionWithLocalScope : Int -> Int
iAmAFunctionWithLocalScope argument =
    let
        iAmALocalVariable : Int
        iAmALocalVariable =
            7

        localFunction localArgument =
            9 + localArgument
    in
        argument + localFunction 10


exclaimList : List String -> List String
exclaimList =
    -- Notice  that we haven't explicitly named the argument to this function
    List.map (\item -> item ++ "!!!")
```

### Some Special Operators

`(|>)`, `(<|)`, -- pipe operators!

These operators pass the result of whatever operation happened previously to the arrow side:

```
helloWithPipeOperator : String
helloWithPipeOperator =
    "!!!" |> (++) "Hello" -- this will result as "Hello!!!"


helloWithParens : String
helloWithParens =
    ((++) "Hello") "!!!" -- this will result as "Hello!!!"
```

`(>>)`, `(<<)` -- compose functions!

```
{-| mathItUp [ 0, 1, 2  ] == [ 0, 2, 5 ]
-}
mathItUp : List Int -> List Int
mathItUp =
    List.map ((*) 2 >> (+) 1)


mathItUpWithoutFunctionComposition : List Int -> List Int
mathItUpWithoutFunctionComposition =
    List.map (\value -> val * 2 + 1)
```

## Joyful Tools
## Finding and Using Packages
## Writing and Publishing a Package
## Showing off Work
## Going Further: JavaScript & Elm together
