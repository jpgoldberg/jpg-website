---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Three things Python doesn't teach"
subtitle: "Distinguishing public vs private attributes, using the logic of types, and being mindful of mutability"
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

{{< epigraph
  author="Roman Jacobson"
  cite="On Linguistic Aspects of Translation"
  detail= "(1959)" >}}
Languages differ essentially in what they must convey and not in what they may convey.
{{< /epigraph >}}

When you first learn to program with a particular particular language
you are learning two things (among others):

1. How to program;
2. How to use that specific programming language for programming.
  
These, of course, are intertwined,
but it is important to keep in mind (1) is about learning how to
think about and solve certain sorts of puzzle.
Python is a fine choice as first language to learn for many of the reasons people say,
in particular it doesn't get in the way of learning how to program as much as many alternative do.
Indeed, Python allows the programmer to get on with things without
forcing them the to make or specify certain distinctions
that other programming languages do confront users with.
This leaves people unaware of the distinctions and good practices
that help avoid bugs.

Quite simply most people who only learn Python will not even be aware of very important concepts
of good software design.
There are practices one can follow using those concepts
that help avoid large categories of nasty bugs,
but they typical Python-only path for learning to program
is more likely to conceal the importance of these concepts than prepare learners to use them.

Understanding these concepts and following the kinds of practices I describe below
will help the developer avoid whole classes of subtle bugs and make it easier
for them to reason about their own code.

## Who are you?

This article is roughly aimed at two audiences.
You may be a Python programmer whose only programming experience is with Python
and you have reached a stage where they are comfortable with defining functions,
and you have understanding classes as a way to keep data and methods.
You do not need to be familiar with class inheritance.

Or perhaps you are coming to Python from some other language
and you find yourself struggling to make use of certain important concepts
that Python lacks.
Here I will point you to Pythonic ways to get some of what you seek while still,
in the words of a very wise friend of mine, “letting Python be Python.”

{{% callout title="Advanced note" %}}
For the experienced programmer, it is important to note that the Python interpreter
does not make use of the mechanisms described below,
but these still have value in that they can still play a large role in reducing
human error when programming.
{{% /callout %}}

### Some terminology

In much of what follows I will talk about the “user” of a class or function.
That user not only can be some other person using a library or module that
you share,
but that user can be you.
Even though you might know the details of a function or class you create when you
create it, you will still need to communicate to yourself at a later time how
your creations are expected to be used.
I will also be lax in my use of the terms “function” versus ”method”, often using ”function” to include both.
Similarly, I will be lax in my use of “interpreter” versus “compiler”.
The distinction matters for understanding why Python is the way that it is,
but it doesn't matter for my discussion here.

## Respect for privacy {#sec-privacy}

I will start with something that may be familiar with.
Many Python-only developers have learned when to use "`_`" at the start
of a variable name and when to use the `@property` decorator,
but using this more familiar example helps illustrate the kind of thing
I am discussing.
Many other programming languages force users to specify which attributes
of a class are public and which are private,
and the privacy is often enforced by the compiler.
But Python itself does not prevent you from accessing and changing any attribute of a class.
As far as the language is concerned all parts are public.

Let's see how that can cause trouble.
I will be defining various classes for a point on a plane to illustrate things.

```python {title = "A point with public attributes" id ="code-Pnt1"}
class Pnt1:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

        # Polar coordinates
        self.r = (self.x ** 2 + self.y ** 2) ** (1/2)
        self.theta = math.atan2(y, x)
```

In our [definition of `Pnt1`](#code-Pnt1), we have a bunch of attributes,
including `x`, `y`, `r`, and `theta`.
All of these can be accessed and manipulated from outside of the the class.

```python { title = "Inconsistent internal state" hl_lines ="6" linenos = "true" id = "code-inconsistant" }
p = Pnt1(3, -4)
print(p.r)  # approximately 5

p.x = 0
...  # imagine there were many lines of other code here
print(p.r)  # Still 5, were you expecting 4?
```

Nothing about how `Pnt1` is presented to the user of the class tells us
that setting `x` (as we do on line 4) to a different value will
leave the point in an inconsistent state.
In this small examples, it is usually easy to see where things go wrong,
but keep in mind that where you might manipulates `p.x` (our line 4)
and where you might make use of the radius in polar coordinates (our line 6)
may be be in distant parts of your code.

To fix this, we should either prevent (well discourage) the user from messing with
the coordinates directly
or, if we allow such manipulation, we make sure that the polar coordinates get updated
when the Cartesian coordinates change.
There are Pythonic ways to do either, but I will focus my examples on the first.
We will discourage the user of the class from manipulating the `x` and `y` values of a point after
it has been created.

```python {title = "A point with public properties" id="code-private-point" }
class Point:
    def __init__(self, x: float, y: float):
        self._x = x
        self._y = y

        # Polar coordinates
        self._r = (self._x ** 2 + self._y ** 2) ** (1/2)
        self._theta = math.atan2(y, x)

    @property
    def x(self):
        """Cartesian X coordinate."""
        return self._x
    ...
    @property
    def r(self):
        """Polar radius"""
        return self._r
```

Here we have used names beginning with "`_`" for those attributes of a point
that we don't want to user to access or manipulate directly.
We can't prevent users from doing so,
but the naming convention tells the user that
they really shouldn't be accessing those attributes that way.
Many tools used with Python can work to reinforce that convention,
but it is really on the user to understand use of attributes whose names
begin with "`_`" from outside the module where those are defined is just asking for trouble.

```python {title = "Disrespecting privacy" hl_lines ="3"}
p2 = Point(-5, 12)
print(p2.r)  # Approximately 13
p2._x = 0  # Don't do this outside class or module where Point is defined
print(p2.r)  # Still approximately 13, not 12
```

You are expected to use these private attributes within the class or module
where you have defined them,
but when you are the user of the class or module, you are setting yourself up for trouble
when you do so.

Furthermore the user can now access, but not change, the X value through `.x`

```python {title = "Setting a property is prevented" hl_lines = "5"}
p2 = Point(-5, 12)
print(p2.r)  # Approximately 13
print(p2.x)  # .x is still readable
try:
    p2.x = 0  #  This will be an error
except AttributeError:
    print("There was an error. Can't use .x to change a point")
```

If you are not yet familiar with `try` and `except` ignore that.
I just wrapped this error in that so that my sample code still runs.

Let me give another example of the kind of trouble that direct access to such attributes
can lead to.
For those of you who learned and recall anything about polar coordinates,
you may know that angle theta (θ) could be stated in either degrees or radians.
In what I have above it happens to be radians.
But suppose you want the freedom to change your mind about the units to use internally
in the non-public parts of your class without messing things up for the user.[^-273]

[^-273]: The example of radians vs degrees is more than a bit contrived, but consider a class in which it is very useful to perform all computations regarding temperature in degrees Kelvin, while degrees Celsius is what makes the most sense for the user of the class.

You might add this property to your class.

```python
...  # continue defining Point class

    @property
    def theta(self):
        """Degrees from positive X axis."""
        return math.degrees(self._theta)

```

As long as the user stays away from accessing `._theta` they will not have to
worry or know what you use internally.
Indeed, it might be better and safer to not offer `.theta` at all, but just have

```python
...  # continue defining Point class

    @property
    def angle_degree(self):
        """Degrees from positive X axis."""
        return math.degrees(self._theta)

    @property
    def angle_radian(self):
        """Radians from positive X axis."""
        return self._theta)

```

As I hinted above, there are Pythonic ways in which we could have allowed user to
modify a point after it is created and still keep the internals of the point consistent,
but my primary goal in this section was to illustrate why it is important to
be mindful of of the public/private distinction even though it isn't built into Python.
Additionally I wanted to illustrate the fact that there are Pythonic ways to
make use of the distinction to help you reduce often subtle error errors.
The same sort of goals drive the following sections.

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
and the term for something that is changeable is "mutable".

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
But there are still a number of Pythonic practices we should do
to reduce the kinds of bugs that this leads to.

1. Functions that mutate their arguments should not also return a value.
2. Follow function naming conventions that that provide some hint about this behavior,
   using a verb, "`spamify()`" for a mutating variant
   and a de-verbal adjective,  "`spamified()`", for a non-mutating one.
   This is similar to Python's "`reverse`" vs "`reversed`" distinction.
3. Use type annotations that indicate mutability.

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

### The ABCs of distinguishing mutability using types

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

Because we said when we created `f` that we did not expect it to be mutable
we were warned by the type checker that something was amiss.
First we were told the `extend` method is not something that makes
sense for something immutable.
And then we were warned that trying to assign an immutable thing
to something mutable isn't quite right either.

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
