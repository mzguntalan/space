# space

WIP programming language of guarded types, pattern matching, and dependent arguments. Nothing has been done yet. Design-only for now.

## Main Features

- Type membership through spcification checking
- Guarded Types
- Dependent Arguments

## Type System

### Types

An a is of type A iff all specifications of A is met by a .
Specifications are conditional expressions in a and are either True or False .
If a type A has all the specifications (verbatim) of B then values of type A will pass the specifications of B and is therefore also of type B .

### Guarded Types

A consequence of this type system would be the ability to guard types (similar to guards in FP). This means that I could add boolean checks directly into the type.
Example:

- If I have a type Int , I could create a type called PositiveInt with specifications of Int and an additional x > 0
- I could then create a construct called Positive x : x > 0 such that PositiveInt = Positive Int
- : is read as such that
- Positive is then a guard for Int
- If i declare Small x : x < 256 , then I could have Small Positive Int type which I could use directly to annotate functions without having to use pattern guards

## Function

### Functions

- Functions are first class
- Functions are curried
- Functions can have dependent arguments (will be syntax sugar and desugared into guarded types)
- When a function call can fail, the compiler will require the author to write ? to mark where the failure will occur - right now, no failure handling will be offered - it the function fails, (i have no idea yet)
  Example

Overview of function definition

- functions are defined like those Haskell. A type Signature and implmentation(which could be several)
- functions are curried, `_` are used to skip giving an argument

```haskell
// function definition
func Int Int Int
func x y (x+y)

func 4 5 // output: 9

func 4 // output: \y -> ( 4 + y )

func 4 5 9 // compiler will complain because the last argument is dependent on the first two arguments

func 4 5 9? // compiler will not complain; will fail silently (for now. Will think about failure handling in the future)

func 4 5 11? // it will fail and cause an error. this will eventually evolve as the main way of doing tests

func _ 2 // _ will skip the argument; output : \x -> (x + 2); _ will also act as a way to do partial application (together with currying)

func _ 5 6 // output: \x -> (x+5) -> 6; the two -> means that it evaluate both (x+5) and 6, and they have to be equal. If not, the resulting function fails
f Int Int
f x (func x? 5 6) // ? is required because it can fail

f? 5 // ? is at f, because it is at `f` that it could fail; output: fails
f? 1 // output: succeeds
```
