# Functional Programming Refresher

A repo to brush up on FP using Python.

![fp-languages](demo/fp-languages.png)

## In This Document:
  - [What is Functional Programming](#what-is-functional-programming)
  - [Why Functional Programming](#why-functional-programming)
  - [FP Concepts](#fp-concepts)

## What is Functional Programming
- It's a way to program using functions to transform data.
- It's declarative, focused on the **what**.
- Python is not a pure FP language, but we can use Python to learn the key concepts in FP.
- Purer FP languages include: Haskell, Clojure, OCamel, Scala.
- In FP, data is **immutable**. Tuples are immutable.
- Out of the 4 key concepts in OOP, only **inheritance** is not possible in FP. Inheritance requires mutable classes.
- Use OOP for modeling objects and concepts. It's why it's used in:
  - Game programming
  - Simulations
  - UI
 
## Why Functional Programming
- It's easier to debug.
- When debugging, break it down to pieces, print the outcome of each, then when fixed, put things back together.

## FP Concepts
### 1. Functions as First Class Citizens
- In Python, functions are first class citizens, i.e. they are treated as regular values tha can be assigned to variables.
- `add_one = lambda a: a + 1`

### 2. Higher Order Functions
- HOFs receive functions as parameter values and return functions as return values
- Key HOFs: map, filter, reduce. They take a funciton, and an iterable, and return an iterable.

### 3. Pure Functions
- PFs are deterministic. Given the same input, they produce the same output.
- They have no side effects. They do not impact the state outside of them. They do no I/O.
- No-op functionas return nothings.

### 4. Memoization
- Memoization is using a memo/cache to remember the results of past calculations. It's trading space for better speed.

### 5. Referential Transparency
- RT is to always be able to replace a function call with its return value(s).

### 6. Recursion
- It's a way to write functions such that the function calls itself, until a base case is met.
```
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)    
```
- Recursion is part of FP becasue it's a way to iterate while avoiding stateful loops.
- `isinstance(thing, type)`
- To merge two dictionaries, use the newly added merge operator, which is just a pipe, `|`. `merged = dict1 | dict2`.
- We use recursion with trees becasue they have unknown depth.
- You can use a loop in a recursion function.

#### Dangers of Recursion
1. Stack overflow
2. Infinite loop -> stack overflow
3. In Python it is slower than a regular loop, as each call requires some memory. Tail call optimization could have helped but Python doesn't have it.

### 7. Function Transformation
- It's a special kingd of HOF.
- It's a function that takes a function(s) and returns a function.
```
def transformer(fun):
    def inner_fun(x):
        return fun(x, x)
    return inner_fun
```

#### Why
1. Simplify your code
2. Reusability
3. Closure

### 8. Closure
- A function that closes on the enclosing scope.
- It remembers the state outside of it.
- It's a stateful function. That's the whole point.
- Closures are not pure functions. They change state that's outside of them, and they may have side effects.
- If the outside state is an immutable variable that you'll need to reassign, we need to use `nolocal`.
```
def keep_the_count(current_count):
    count = current_count
    def add(new_count):
        nonlocal count
        count += new_count
        return count
    return add
```
- You can only concatenate a tuple to a tuple. `added = old + (item, )`

### 9. Currying
- Translate a single function that accepts multiple parameters into multiple functions that each accepts a single parameter.
- We use it to change a function's signature to make it conform to a specific shape.
```
def sum(a):
    def inner_sum(b):
        return a + b
    return inner_sum

print(sum(1)(2))
```

### 10. Decorators
- This is syntactci sugar for High Order Functions.
- It's found in Python, but not in FP langauges like Haskell, OCaml, Scala, Clojure, etc.
```
def use_formatter(fun):
    def wrapper(word):
        return "* " + fun(word) + " *"
    return wrapper

@use_formatter
def add_dashes(word):
  return "--" + word

print(add_dashes("Hello"))
```    
- `*args` in a function signature collects positional args into a tuple. In the function call, it spreads a tuple into positional args.
- `**kwargs` in a function signature collects keyword/named/key-value pairs args into a dictionary. In the function call, it spreads a dictionary into keyword args.
```
def print_arguments(*args, **kwargs):
    for item in args:
        print(item)
    for key in kwargs:
      print(kwargs[key])

print_arguments("hello", "world", a=1, b=2)
``` 
- Syntactic sugar = more convenient syntax

### 11. Sum Types
- It's a fancy name for `enum`s.
- A sum type has a fixed number of possible values.
- A product type has many combinations.
- In FP we represent erors as values we return, i.e. data, rather than raised exceptions. Raising exceptions are side effects.
```
from enum import Enum

Color = Enum("Color", ["RED", "BLUE"]

red = Color.RED
black = Color.BLACK # raises exception
```
- Python doesn't support sum types as well as statically typed languages. They capture errors during compilation.
- Python has a `match` syntax that takes advantage of `enum`s.
```
def get_status(status):
    match status:
        case Status.PENDING:
            return "It's pending."
        case Status.PROCESSING:
            return "It's processing."
        case Status.DONE:
            return "It's done."
        case _:
            raise Exception("Incorrect status")
```
