---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Week 01: Python basics

[Homework 01 (Colab notebook)](https://colab.research.google.com/drive/1hh49hQRkhxcE1ldXCB7QKNkr3RkCiA4J?usp=sharing)

### Learning objectives

This lesson is designed to explain the basics of programming in Python.

* learn and practice computer science terms: variable, comment, call, function, arguments, default values, float, int, string, boolean, list, dict, object, packages, array, DataFrame, Series
* calculate arithmetic operations
* call functions and change default values
* indexing
* logical statements
* control flow
* DataFrame, some details

### Variable

A **variable** consists of a name and value, where the name references the
value.  The equals sign `=` assigns to a name a specific value.  The code below
creates a variable named `weight_kg` which holds the value `55`.

```{code-cell}
weight_kg = 55
```

The code above follows the general pattern in Python for creating variables:
`variable = expression`.  The left hand side of the `=` specifies a name and the
right hand side can be any valid Python expression.

```{code-cell}
weight_lb = weight_kg * 2.2
```

Notice that the right hand side is now a Python/mathematical expression that
uses the variable `weight_kg` to create a new value (55 * 2.2) and creates a new
variable, named `weight_lb`.  We have thus assigned to the variable `weight_lb`
the value as determined by the expression `weight_kg * 2.2`.

If we want to leave future readers of our code a note, within the code, to
remind them of what this code is doing, we can leave a **comment**.  Comments
are ignored by Python, and are intended to help readers of your code.

```{code-cell}
weight_lb = weight_kg * 2.2 # convert kg to pounds
```

It is easy to get carried away with comments.  When you are starting, this is
not necessarily a bad thing.  As your programming skills progress, your comments
should focus less and less on how to read Python code and should instead focus
more and more on the details surrounding why the code is was written as it is.
This is a fine line.

### Functions

This section will focus on using Python functions.  We will look at writing
functions later.  A Python **function** is a named and bundled sequence of
actions to perform on a variable.  The inputs are called arguments and the
outputs (if there are any) are called return values.

Consider the function `abs`, which computes the absolute value of its argument.

```{code-cell}
x = -3.1415
abs(x)
```

The input is the variable `x`, which holds the value `-3.1415`, and the output is the
value `3.1415`, which itself is not assigned to any variable.

The general pattern in Python is to call a function named `f` as `f(x)`.

Some functions take multiple arguments.  For instance, the function `round`
accepts as the first argument a number and a second, named, argument the number
of digits to round the first argument to.

```{code-cell}
round(x, ndigits = 1)
```

Named arguments have a few benefits.  Named arguments can make code easier to
read, so that you and/or the reader of your code is (some what) reminded what
the argument `1` is doing.  Named arguments also can have **default values**.
The argument `ndigits` of the function `round` defaults to `None`, if no other
value is specified, which in this case instructions `round` to round the first
argument to the nearest integer.

```{code-cell}
round(x)
```

If you follow the correct order of the arguments, you do not need to specify the
name of the named arguments, but this isn't always good practice.

```{code-cell}
round(x, 3)
```

### Variable types

Each variable in Python is of a specific **type**.  Although we may think of the
numbers `3` and `3.0` as mathematically equivalent, these numbers are of
different types in Python (and most other programming languages).

```{code-cell}
type(x)
```

```{code-cell}
type(3)
```

The Python type `float` is for variables that reference (something like) real
valued numbers, numbers with decimals, like the value `-3.1415`.  The type `int`
is for integer values.

There are many other Python types: `str`, `bool`, `list`, and `dict`.  We'll
look at `str` and `bool` immediately, and save `list` and `dict` for the next
section.

The type `str` stands for string.  Strings are any sequence of characters
between double `"` or single `'` quotes.

```{code-cell}
x = "MATH 131"
type(x)
```

Notice that I re-assigned the variable `x` to now reference the value `"MATH
131"`.  Most programming languages allow re-assignment of variables.  Dynamic
languages, such as Python, go futher and allow re-assignment of variables even
across two different types: `float` -> `str`.

The type `bool` stands for boolean.  Boolean is the computer science term for
the two specific values `True` or `False`.

```{code-cell}
y = True
z = "True"
y == z
```

Spend a second with the code above.  The variable `y` is of type `bool` because
references one of the special Boolean values, specifically `True`.  The variable
`z` is of type `str` because it references a string which contains the word
True.  Python considers these different, since they are different types and
have different values.  The last line indicates that Python treats `y` and `z`
as not equal.  The Python code `y == z` is like asking the question, is `y`
equal to `z`?, to which Python returns the Boolean value `False`.

Novice programmers sometimes think the different types are burdensome.  With
some practice though, you will learn that types help programmers write good and
logical code.  For instance, if you write code like

```{code-cell}
---
tags: [raises-exception]
---
abs(x)
```

The `TypeError` is a friendly reminder that you re-assigned the variable `x` to
hold a `str` value and the function `abs` has no meaning when applied to a `str`
type.  I insist this is a friendly reminder, even if the error message itself is
hard to parse :) If nothing else, we must get used to dealing with types
appropriately.

There are in fact many more variables types in Python and in programming more
generally.  For now, we'll skip to variable types that store (plural) data.

### Data structures

Variable types that contain more than one datum, data, are generally referred to
as data structures.  Two important Python types for storing data are `list` and `dict`.

Both `list` and `dict` are types that contain multiple elements.  For both
types, the contained elements themselves do not need to be of the same type.
What differentiates a `list` from a `dict` is how the contained elements are
indexed or referenced.

##### List

A `list` indexes its elements with `int`s or slices of integers.  The code below
creates a variable named `l` which references a list of heterogeneous types, and
then indexes the first element of the list `l`.

```{code-cell}
l = [1, 2.1, "Chico State", True]
l[1]
```

To define the list `l` the expression on the right hand side of `=` surrounds a
comma separated list of values by square brackets.  Any left square bracket `[`
must be followed by a right square bracket `]` to complete a list-defining
expression.

The syntax `l[1]` indexes the list by requesting the first (not the zero-th)
element of the list.  Lists in Python are said to be zero indexed, since the
zero-th element of the list `l` is the value `1`.

One can also index a list with a slice on integers, e.g. `0:2`.

```{code-cell}
[l[0:2], l[1:], l[:1], l[0:3:2]]
```

Indexing lists with slices takes a fair bit of practice.  The above code creates
a new list, which containes as elements sub-lists of the list `l`.  The zero-th
sub list consists of the zero-th and first elements of `l`.  Notice though,
despite the slice `0:2`, the second element of `l` is not retreived.  The index
`a:b` thus indexes from a list all elements at position `a` up to but not
including `b`.

The slice `1:` indexes the first element up to and including the last element.

The slice `:1` indexes the zero-th element up to but not includes the first
element.

The slice `0:3:2` indexes the zeros-th and the second element, since the
`:2` indicates steps of size `2` starting from index `0` and going up to
index `3`.

One can also index a list in reverse order.  Imagine wrapping the indices around
the back of the list starting from the zero-th element.  Thus the `-1`-element
is the last element.

```{code-cell}
l[-1]
```

And for fun, even crazier indexing expressions exist.

```{code-cell}
l[::-1]
```

The slice `::-1` says to start at index `0`, go up to the last index, and take
steps of size `-1`.  Hence, one gets a list in reverse order.

##### Dict

A `dict` indexes its elements with keys.  In other computer science worlds, a
Python `dict` might be called an associative container because it associates to
each key a value.

```{code-cell}
d = {
  "MATH 131": "Introduction to Python", 
  "MATH 450": True, 
  "pi": x,
  "list": l
  # key: value,
}
d["pi"]
```


The `dict` referenced by the name `d` is created using curcly braces, instead of
square brackets like for a list.  Between the curly braces the pattern goes
`key: value`, where the colon `:` distinguishes the key from the value.  There's
lost of options for the key types, but it'll be easiest if we think of they keys
as specifically type `str` for now.

The dict `d` associates key `"pi"` the value stored in the variable `x`, that we
we created earlier.  The key `"list"` associates the list `l`.

The options for indexing `dict`s are more restrictive than for `list`s.  There's
no slices here.  But one can mutate (think edit) a `dict` by indexing into a key
that doesn't previously exist.

```{code-cell}
d["python"] = "sometimes too fancy"
d["python"]
```

These data structures show up all over Python code.  Python `list`s are helpful
when you want to contain (possibly multiple) data of not necessarily the same
type, in an order that makes sense to index by sequential integers, and be able
to reference the one container with one variable name.  Python `dict`s are
helpful when the order of the contained elements does not necessarily make
sequential sense and instead it is easier to associate the contained elements by
key/name.

### Packages

There are many more types built-in to Python and many more types not directly
built-in.  Communities of programmers, like that of the data science community
and before them the numerical computing communities, have built types into
**packages** for very specific use-cases.  Think of packages as additional
features one can add-on to Python when you need/want.  

TODO more about packages

In the last sections of Week 01, we'll introduce the Python packages Numpy and
Pandas.

The Python package numpy contains the type `array`, for containing homogenous
types (different than `list`s and `dict`s which store heterogeneous types), and
the package Pandas contains the type `DataFrame` and `Series`.  A `DataFrame` is
for tabular data, think spreadsheet data, where each column is a `Series`.

### Array




### DataFrames



```{seealso}
[Pandas help page: DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)
```
