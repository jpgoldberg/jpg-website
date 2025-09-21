---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Three things Python doesn't teach"
subtitle: "That developers ought to learn"
summary: "Someone learning software development through Python alone might never learn how the power of types, attention to mutability, and the private/public distinction can be used to prevent many nasty bugs. This article tries to present some idea of how important those practices are and how to make use of those concepts while still letting “Python be Python.”"
authors: []
tags: []
categories: []
date: 2025-09-21T17:34:51-05:00
lastmod: 2025-09-21T17:34:51-05:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

When you learn to program with a particular particular language you are learning (at least)
two things:
(1) how to program,
and (2) how to use the specific programming language that you are starting to program with.
These, of course are intertwined. 

Python is a fine choice as first language to learn for many of the reasons people say,
but it leads to bad habits.
What's worse is that those bad habits are habits of omission.
Quite simply most people who only learn Python will not even be aware of very important concepts
of good software design.
There are practices one can follow using those concepts
that help avoid large categories of nasty bugs,
but they typical Python-only path for learning to program
is more likely to conceal the importance of these concepts than prepare learners to use them.


This article is roughly aimed at two audiences.
The first is the Python programmer whose only programming experience is with Python and has reached a stage where they are comfortable with
defining functions and has some sense of what classes are for.
If you are first learning programming in Python but have not yet learned the basics,
you may wish to take a look at this to understand that there are important practices
that you might not be aware of 

Python itself doesn’t provide enforceable means enforce better habits,
nor does the Python interpreter itself make any use of the good practices I advocate in this section.
But there are Pythonic conventions, tools, and practices will very much help developers avoid bugs
as well as produce cleaner, more maintainable, and more readable code.

These practices will help the developer reason more clearly about their own code.

## The logic of types {#sec-types}

Type annotations (also called “type hints”) are a must.
This, along with writing unit tests, is what I would consider the top priorities.
I recognize that the ability to do this well is relatively recent,
but at this writing (September 2025) Python 3.9 has only a month to live,
so one can start by using what is available for Python 3.10.

The most immediate gain from type annotations is that
they serve as important documentation for functions and methods.
They tell the people using your functions what data types/classes your function
expects its arguments to be and the type of the data returned.
They work hand-in-hand with docstrings in this respect.

Consider two function signatures

```python { title="Two functions" verbatim=false }
def f1(x: str) -> int: ...
def f2(x: int) -> float: ...
```

Using the type hints immediately tells you what kind of input and output you should use and expect
from these functions.

```python { title = "A type checking example" hl_lines = "6" }
def f1(x: str) -> int: ...
def f2(x: int) -> float: ...
text = "abc"
a = f1(text)  # Type checker infers that “a” is an int
b: float = f2(a)  # Type checker is happy here
c: str = f2(a)  # Type checker will report an error
```

Passing an argument of an unexpected type can lead to hard to debug errors
depending on things that may be deep inside the called function
(including things that that function calls.
But using type annotations and a type checker saves you and your users
from many of those sorts of bugs.

```python { title = "Catching bugs early" hl_lines = "2" }
b = f2("abc")
d = f1(b)  # Type checker will report an error, as b us a float
```

In the example above, we have one intermediate variable, `b`
and everything happens to be set close to each other,
which makes it relatively easier for the human developer avoid this
kind of error without the help of a type checker.
But this is also a compact example.
When variables are set in distant parts of code and functions
defined in separate modules, you won't have the luxury of seeing
everything defined within the space of a few lines.

The example also starts to illustrate how the type system use useful for
combining (composing) functions.
Those who studied some physics in high school or beyond
will have learned something akin to
[dimensional analysis](https://en.wikipedia.org/wiki/Dimensional_analysis)
as a way to help you
avoid error and see what should be applied to what by keeping track of the units.
Good type annotations and checking do the same thing for
you when coding and for others using what you have produced.

If you have provided proper type annotations and use type checking
you can have some confidence that the following is properly constructed
if the type checker is happy with it.

```python { title="Type checking function composition" }
(numerator, denominator) = f2(f1("abc")).as_integer_ratio()

# Or build a function from that
def f3(text: str) -> str:
    n, d = f2(f1(text)).as_integer_ratio()
    return f"{n}/{d}"
```

{{% callout note %}}
In many other languages, type consistency is enforced by the compiler
and the compiler uses that information to produce
more efficient and safer binaries.
Even though Python does not do this, using type annotations and
running a static type checker will help the developer
catch and prevent potential and subtle bugs early.
{{% /callout %}}

### Some tools

My goal has been to introduce the concept and benefits of static type checking in Python,
instead writing a how-to guide,
but here are a few things that might help some people to get started
with at least the things that I happen to use.

- [Getting started with mypy](https://mypy.readthedocs.io/en/stable/getting_started.html),
  which in addition to providing a guide to installing and running the mypy static type checker contains
  a brief introduction to adding type annotations to your code.
- The [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)
  Visual Studio Code extension is installed as part of Microsoft's excellent
  [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  for VSCode.

Both of those can be configured with respect to how strict they are.
And each recommends that you start out with not very strict settings.

## Mindfulness about mutability

Everyone learning Python is taught something like the fact
that the "`=`" in lines 2 and 8 behave differently.

```python {hl_lines = "2 8", linenos = true }
a = "abc"
b = a  # Assignment copies the *value*
b += "xyz"  
print(b) # abcxyz
print(a) # abc

d = ['a', 'b', 'c']
e = d  # Assignment copies the *reference*
e.extend(['x', 'y', 'z']) 
print(''.join(e)) # abcxyz
print(''.join(d)) # abcxyz
```

Changing `e` changed `d`,
and the term for something that is changable is "mutable".

Although this lesson is taught, it is hard to build up the habit of
remaining mindful of this sort of thing,
and failure to be mindful of the consequences of mutation can lead
subtle and difficult to identify bugs.

Those bugs often arise because it is sometimes unclear whether whether a function
changes any of its arguments.

Consider the function `more_spam()`, which aims to double the
amount of spam in some meal.

```python { title = "more_spam()" verbatim = true }
def more_spam(ingredients: list[str]) -> list[str]:
    """Doubles the amount of spam in ingredients."""
    for ingredient in list(ingredients):
        if ingredient.upper() == "SPAM":
            ingredients.append("SPAM")
    return ingredients
```

The type annotations for the `more_spam()` function just tell
us that the argument should be a list of strings.
It tells us nothing about whether that list might be
be mutated.

```python {hl_lines = "6"}
ingredients = ["SPAM", "eggs", "bacon", "spam"]
print(f"len(ingredients): {len(ingredients)}")  # 4. As expected

doubled = more_spam(ingredients)
print(f"len(doubled): {len(doubled)}")  # 6. As expected
print(f"len(ingredients): {len(ingredients)}")  # 6 (is this expected?)
```

Programming languages differ in the degree and manner in which they force
the programmer to be mindful of mutability.
Python itself doesn't force you to think about it until you are deep in
debugging something that has gone wrong.
But there are still a number of Pythonmic practices we should do
to reduce the kinds of bugs that this leads to.

1. Functions that mutate their arguments should not also return a value.
2. Follow function naming conventions that that provide some hint about this behavoir,
   using a verb, "`spamify()`" for a mutating varient
   and a de-verbal adjective,  "`spamified()`", for a non-mutating one.
   This is similar to Python's "`reverse`" vs "`reversed`" distinction.
3. Use type annoations that indicate mutability.

Much of the remainder of this section talks about (3),
but to illustrate methods 1 and 2, we would have definitions like

```python
def spamify1(ingredients: list[str]) -> None:
    for ingredient in list(ingredients):
        if ingredient.upper() == "SPAM":
            ingredients.append("SPAM")

def spamified1(ingredients: list[str]) -> list[str]:
    doubled: list[str] = []
    for ingredient in list(ingredients):
        if ingredient.upper() == "SPAM":
            doubled.append("SPAM")
    return doubled
```

### The ABCs of distinguishing mutatabilty using types

We can use abstract types to give us early warning of potential
mutation bugs
often referred to as  Abstract Base Classes ({{< abbr "ABC" >}})s
in the Python world.
These are just as we used more concrete types
the [section on type hints](#sec-types).
This are just, well, more abstract.

We we will import two {{< abbr "ABC" >}}s from
[`collections.abc`](https://docs.python.org/3/library/collections.abc.html).

```python
from collections.abc import Sequence, MutableSequence
```

`Sequence`
: List-like things that are not expected to be mutated.

`MutableSequence`
: List-like things that are expected to be mutated.

Here is a simple example of them in play.

```python {hl_lines = "3 4" }
f: Sequence[str] = ['a', 'b', 'c']
g = f
g.extend(['x', 'y', 'z'])  # Type error "Sequence has not attribute 'extend'
h: MutableSequence[str] = f  # Type error "Incompatible types ..."
```

Because we said when we created `f` that we did not expect it to be mutatable
we were warned by the type checker that something was amiss.
First we were told the `extend` method is not something that makes
sense for something immutable.
And then we were warned that trying to assign an immutable thing
to someting mutable isn't quite right either.

We can, however, make a mutable copy of our sequence

```python {title = "Copy to mutable type" id="code-copy-2-mutable" }
f: Sequence[str] = ['a', 'b', 'c']
j: MutableSequence[str] = list(f)
j.extend(['x', 'y', 'z'])
print(''.join(j)) # abcxyz
print(''.join(f)) # abc
```

Now that we have some understanding of `Sequence` and `MutableSequence`
we can annotate our functions properly.

```python
def spamify(ingredients: MutableSequence[str]) -> None:
    for ingredient in list(ingredients):
        if ingredient.upper() == "SPAM":
            ingredients.append("SPAM")

def spamified(ingredients: Sequence[str]) -> Sequence[str]:
    doubled: list[str] = []
    for ingredient in list(ingredients):
        if ingredient.upper() == "SPAM":
            doubled.append("SPAM")
    return doubled
```

Once again, the Python compiler doesn't make any use of the naming conventions
and type annotations.
But, once again, communicating intent to humans and to type checkers
does prevent us from introducing many nasty bugs.

### About that Base

I ducked a problem by using a list comprehension to copy the list in
my [copy example](#code-copy-2-mutable)
instead of the `copy()` method defined for lists.
This is because `copy` is not an attribute that is declared for `Sequence`
even though it is defined for lists.
So the type checker would have treated `f.copy()` as a type error.

This serves as a reminder that `Sequence` is not only an abstract class,
but it is meant as a *base* class from which more specific classes can be created.
I will not go into doing so here.

## Respect for privacy {#sec-privacy}

Every part of a Python object can be inspected or modified when the object is in scope. There are no truly private attributes. But we do have the conventions of naming things that should be treated as private with “_” as the leading character.

In the class Point, users can change the value of x after the point is created. We might not want that.
