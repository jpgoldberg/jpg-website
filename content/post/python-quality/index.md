---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Evaluating Python code"
subtitle: "An answer to the question “How do you evaluate the quality of [a] Python package?”"
summary: "Don't use a check list for evaluating code quality, but there are still things I look at. Some of them are things that many people who only know Python may struggle with."
highlight: true
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

In many sections I have gone well beyond answering how I evaluate into why I feel
that the things I look for are important.
In particular I try to communicate some of the importance of a
[trio of good practices](#sec-trinity) that Python doesn't teach.
This involves concepts of good software design that individuals who only learn Python
might be totally unaware of.

{{< toc >}}

## Don’t use check lists to evaluate

The original question was presented in a way that could lead people to think in terms
of check lists of features and tools.
Despite the fact that there are things that I will notice (or notice missing)
that will inform my judgement of the quality of some Python project,
it is a mistake to largely think in terms of check lists.
Sometimes there are good reasons why some project is lacking something
I might generally expect of a high quality project,
and there are times when the things I like to see are present but are done poorly.

The biggest problem with establishing check lists isn’t for the person evaluating the code.
The problem with check lists is for the novice programmer attempting to check all the boxes
without good understanding of what they are about.
If the novice developer goes down that path, they are likely to

- take on too much at one time;
- miss what they should be focusing on in their particular stage of their learning;
- find themselves extremely frustrated in their attempt;
- and quite possibly end up producing worse code than if they hadn't tried to satisfy such a list.

Another important thing for anyone using this or any other similar list of better practices
to improve their code is that pretty much everything I discuss can be done partially.
These are not all-or-nothing practices.
Doing a bit of testing is better than doing no testing.
Doing more testing is better than doing just a bit of testing.
That isn't just true of testing. It is to varying degrees of everything
I mention.
If you are are reading this to help you improve your own
projects,
then focus on the word “improve”.

### Don't insist on all the newest and shiniest tools {#sec-shiny}

The original question listed use of some relatively new shiny tools,
many of which I strongly recommend.
But using those tools is more of a reflection of when the project was created than of code quality itself.
So I would not rate those too highly.
While I have my preferences regarding
[unittest](https://docs.python.org/3/library/unittest.html#module-unittest)
or [pytest](https://docs.pytest.org/en/stable/),
which tool developers use is often driven by mere accidents of which tool they got up and running first.

## Understand your own code {#sec-bad-vibes}

{{< quote source="Fowler et al. (1999)" >}}
Any fool can write code that a computer can understand.
Good programmers write code that humans can understand.
{{< /quote >}}

It really should go without saying that a developer must understand
their code and are responsible for the design choices.
And so I will say little more about this here beyond stating
that I don't care what tools someone used to develop their software
as long as the code itself is understandable and understood by the developer.

## Test {#sec-test}

Testing is essential, but see more about this in the [next section](#sec-defensively).

## Code defensively {#sec-defensively}

Defensive coding is thinking about and preparing for ways that things can go wrong,
and this has implications for, among other things, the kinds of tests you write.

Too often people only test that their functions and methods work in the normal case.
I am certainly guilty of this because during development
I write a test just to see that I have the thing basically working
and tell myself that I will flesh out the tests later.
But, hypocrite that I am,
what I really like to see is unhappy path unit testing.
Proper tests check what happens with edge cases and with unusual inputs.

Suppose we have a function that expects a probability as an argument.
What happens when it is passed a value that is not between 0 and 1?
What happens when it is exactly 1 or 0?
It is important to test these boundary conditions (where things often go wrong)
and certainly test when the input values clearly don't meet expectations.

Or more simply, let's look at the typical
first introduction to recursion function that people may encounter.

```python { title="Things go bad when we are negative" verbatim=false }
def factorial(n: int) -> int:
  if n == 0:
    return 1
  return n * factorial(n - 1)
```

What happens if you try `factorial(-5)`?
(You get a [`RecursionError`](https://docs.python.org/3/library/exceptions.html#RecursionError), that's what.)

Both the probability and factorial examples are typically addressed by
checking the value passed to the function
and raising
a [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)
if it is not sensible input.
But that isn't the only design choice.
What is important that anyone (including yourself at some point in the future)
know what to expect.

If you develop the habit of testing with peculiar inputs, you will find yourself
writing better thought out functions in the first place.

### Run time type checking? {#sec-isinstance}

When I first started using Python a few years ago, I used run-time enforcement
of the types of arguments passed to a function.
That is my hypothetical factorial function might look something like

```python { title="factorial with run time type enforcement" verbatim=false hl_lines = "2-3" }
def factorial(n: int) -> int:
  if not isinstance(n, int):
    raise TypeError("n must be an integer")
  if < 0:
    raise ValueError("n can't be negative")
  if n == 0:
    return 1
  return n * factorial(n - 1)
```

I no longer do that.
My increased understanding and grudging acceptance of Python type system
along with the substantial improvements in support type type hinting
has led me to prefer static type checking to help me use
the [logic of types](#sec-types).
There are varying and often strongly held opinions about heavy use of run time type checking in Python.
Indeed, my opinion has varied over time,
and I do not wish to try to persuade anyone of my current view.
I am merely stating it.

## Documentation {#sec-docs}

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

## Three things Python doesn't teach {#sec-trinity}

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

### The logic of types {#sec-types}

Type annotations (also called “type hints”) are a must.
This, along with writing unit tests, is what I would consider the top priorities.
I recognize that the ability to do this well is relatively recent,
but at this writing (September 2025) Python 3.9 has only a month to live,
so one can start by using what is available for Python 3.10.

{{% callout note %}}
I will not be discussing how to run type checkers here,
as this is already getting too long.
My focus is to give people unfamiliar with it an idea of what it does for you.
The good news is that for use IDE's it is easily available within popular Python extensions.
{{% /callout %}}


The single greatest gain from type annotations is that they serve as important documentation for functions and methods.
They tell the people using your functions
what data types/classes your function expects its arguments to be and the type of the data returned.
They code hand-in-hand with docstrings.

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

### Mindfulness about mutability

Python doesn’t offer (much less insist on) ways to say whether some object is mutable or not.
And attempting to enforce such things in Python leads to deeply messy and un-pythonic code
and those attempts don't really work anyway.
That did not stop me from trying when I first started using Python.

But that doesn’t mean that there aren’t Pythonic ways reduce the chances
of bugs involving unexpected data mutation.
One such mechanism, in conjunction with type annotations,
is to limit mutation of function parameters to functions that return None.

Clearly documenting which arguments might be changed by the activity of a function is important.
And this, too, can be done with type annotations.
If a function parameter is listed as, say, type `dict`,
the user calling that function doesn’t know if the dict they pass to a function will change the dict.
But if it is annotated as `Mapping`,
the user (and the type checker) know that the function is not expected changing the contents of the dict.
If the type in the functions parameters call it a `MutableMapping`
that tells the user that the dictionary they pass is expected to be modified
as a consequence of being passed to the function.

### Respect for privacy {#sec-privacy}

Every part of a Python object can be inspected or modified when the object is in scope. There are no truly private attributes. But we do have the conventions of naming things that should be treated as private with “_” as the leading character.

In the class Point, users can change the value of x after the point is created. We might not want that.