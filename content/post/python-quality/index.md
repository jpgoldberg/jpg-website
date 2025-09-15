---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Evaluating Python code"
subtitle: "An answer to the question “How do you evaluate the quality of [a] Python package?”"
summary: "Don't use a check list for evaluating code quality, but there are still things I look at. Some of them are things that many people who only know Python may struggle with."
authors: []
tags: []
categories: []
date: 2025-09-12T14:06:36-05:00
lastmod: 2025-09-12T14:06:36-05:00
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

I had started to write a reply to a
[question on Reddit](https://www.reddit.com/r/learnpython/s/5YPXp28hiB),
which offered an initial check list of things to look for to evaluate the quality
of a Python project and solicited additional items.
The response that I started to draft grew in length and complexity,
and so I am posting that here instead.
Like most who responded, I am treating the question about how one evaluates the
quality of Python projects in general instead of as about evaluating ones own
projects.

## Don’t use check lists to evaluate

There definitely are things that I will notice or notice missing that will inform my judgement
of the quality of some Python project, but it is mistake to think in terms of checklists.
Some times there are good reasons why some project is lacking something
I might generally expect of a high quality project,
and there are times when the things I like to see are present but are done poorly.

The biggest problem with establishing checklists isn’t for the person evaluating the code.
The problem is for the novice programmer attempting to satisfying check lists
without a good understanding of what they are about.
If they take on too much at one time,
it will take away from what they really need to be focusing on in that stage of the learning,
it will be extremely frustrating for them, and it will lead to terrible code.

So if you are a novice programmer reading this and wish to improve your code,
take this as an opportunity to start learning more about a topic I raise instead
of as a list of things you need to do in your project at this time.

## Don't insist on all the newest and shiniest tools

The original question listed use of some relatively new shiny tools,
many of which I strongly recommend.
But using those tools is more of a reflection of when the project was created than of code quality itself.
So I would not rate those too highly.
While I have my preferences regarding
[unittest](https://docs.python.org/3/library/unittest.html#module-unittest)
or [pytest](https://docs.pytest.org/en/stable/),
which tool developers use is often driven by mere accidents of which tool they got up and running first.

## A developer must understand their own code

{{< quote source="Martin Fowlder (1999)" >}}
Any fool can write code that a computer can understand.
Good programmers write code that humans can understand.
{{< /quote >}}

It really should go without saying that a developer must understand
their code and are responsible for the design choices.
And so I will say little more about this here beyond stating
that I don't care what tools someone used to develop their software
as long as the code itself is understandable and understood by the developer.

## Think about how things can go wrong

Tests are essential,
but too often people only test that their functions and methods work in the normal case.
I am certainly guilty of this because during development
I write a test just to see that I have the thing basically working
and tell myself that I will flesh out the tests later.
But, hypocrite that I am, whatI really like to see
is unhappy path unit testing. Proper tests check what happens with edge cases and with unusual inputs.

Suppose we have a function that expects a probability as an argument.
What happens when it is passed a value that is not between 0 and 1?
What happens when it is exactly 1 or 0?
It is important to test these boundary conditions (where things often go wrong)
and certainly test when the input values clearly don't meet expectations.

Or more simply, let's look at the typical
first introduction to recursion function that people may encounter.

```python { title="Things go bad when we are negative" verbatim=false style=vim }
def factorial(n: int) -> int:
  if n == 0:
    return 1
  return n * factorial(n - 1)
```

What happens if you try `factorial(-5)`?
(You get a [`RecursionError`](https://docs.python.org/3/library/exceptions.html#RecursionError), that's what.)

Both the probability and factorial examples can be addressed by
checking the value passed to the function
and raising
a [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)
if it is not sensible input.
But that is a design decision and whatever behavior you want should be tested.

Testing for such things gets you in the habit of building your functions defensively in the first place.

## Documentation

At the very least every (public) function and method should have useful docstrings.
This not only makes those available through `help` but they are often displayed in IDEs.

Here is an example of
[one of mine](https://jpgoldberg.github.io/toy-crypto-math/modules/utils.html#toy_crypto.utils.digit_count),

```python { title="Function definition with docstring" }
def digit_count(n: int, base: int = 10) -> int:
    """returns the number of digits (base b) of integer n.

    :raises ValueError: if base < 1
    """
    if base < 1:
        raise ValueError("base must be at least 1")
    ... # rest of code snipped
```

{{< figure
  src="vscode-docstring-reveal.png"
  title="Docstring popup"
  caption="Docstring popup when hovered over `utils.digit_count`"
  height=200px
>}}

Ideally the code should be consistent in its use and style of docstrings, but missing docstrings leaves a bad smell.

I can’t blame anyone for not wanting to use Sphinx to generate documentation in various formats. It is definitely not something a novice programmer should have to worry about. But more mature projects by mature developers, I would expect some complete documentation.

## Three things Python doesn't teach

This is where I am going to say things that may irritate some Python advocates.
That is ok, I will also say things in this section that will irritate some of its
fiercest critics.

Python is a fine choice as first language to learn for many of the reasons people say,
but it leads to bad habits.
What's worse is that those bad habits are habits of omission.
Quite simply most people who only learn Python will not even be aware of very important concepts
of good software design.

Python itself doesn’t provide enforceable means enforce better habits,
nor does the Python interpreter itself make any use of the good practices I advocate in this section.
But there are Pythonic conventions, tools, and practices will very much help developers avoid bugs
as well as produce cleaner, more maintainable, and more readable code.

These practices will help the developer reason more clearly about their own code.

### Use the logic of types

Type annotations (also called “type hints”) are a must.
This, along with writing unit tests, is what I would consider the top priorities.
I recognize that the ability to do this well is relatively recent,
but at this writing (September 2025) Python 3.9 has only a month to live,
so one can start by using what is available for Python 3.10.

The single greatest gain from type annotations is that they serve as important documentation for functions and methods.
They tell the people using your functions
what data types/classes your function expects its arguments to be and the type of the data returned.
They code hand-in-hand with docstrings.

Consider two function signatures

```python { title="Two functions" verbatim=false }
def func1(x: str) -> int: ...

def func2(x: int) -> float: ...
```

Using the type hints immediately tells you what kind of input and output you should use and expect
from these functions.

It also tells you how you can combine those functions functions.
Those who studied some physics in high school or beyond
will have learned “dimensional analysis” as a way to help you
avoid error and see what should be applied to what by keeping track of the units.
Good type annotations and checking do the same thing for
you when coding and for others using what you have produced.

So let's look at this with respect to what we know about `func1()` and `func2()` declared above.


```python { title = "A few type checker warnings" hl_lines = "4 10" }
text = "abc"
a = func1(text)  # Type checker will know that “a” is an int
b: float = func2(a)  # This is correct
c: str = func2(a)  # Type checker will report an error

d = func2(5)  # Type checker knows that d is a float

# This might result in a hard to debug run time error
# depending on what funct1 does internally
e = func1(d)  # Type checker will report error
```

It's worth noting that in many other languages, type information helps the compiler produce
more efficient and safer binaries.
Even though Python does not do this, using type annotations and
running a static type checker will help you catch potential and subtle bugs early.

### Mindfulness about mutability

Python doesn’t offer (much less insist on) ways to say whether some object is mutable or not.
And attempting to enforce such things in Python leads to deeply messy and un-pythonic code
and those attempts don't really work anyway.
That did not stop me from trying when I first started using Python.

But that doesn’t mean
that there aren’t Pythonic ways reduce the changes of bugs involving unexpected data mutation.  One such mechanism, in conjunction with type annotations, is to limit mutation of function parameters to functions that return None.

Clearly documenting which arguments might be changed by the activity of a function is important. And this, too, can be done with type annotations. If a function parameter is listed as, say, type dict, the user calling it doesn’t know if the dict they pass to a function will change the dict. But if it is annotated as Mapping, the user (and the type checker) know that the function should not be changing the contents f the dict. While the type in the functions parameters call it a MutableMapping that tells the user that the dictionary they pass is likely to be modified as a consequence of being passed to the function. 

### Respecting privacy

Every part of a Python object can be inspected or modified when the object is in scope. There are no truly private attributes. But we do have the conventions of naming things that should be treated as private with “_” as the leading character. 

In the class Point, users can change the value of x after the point is created. We might not want that.



       