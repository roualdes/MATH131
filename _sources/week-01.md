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

* [Week 01 Notes](https://colab.research.google.com/drive/1VQhUmSxM6WfSw1ZZeKfhkRhkfM9JPXQx?usp=sharing)
* [Week 01 Assignment](https://colab.research.google.com/drive/1h9Ck7kWNN9_I2Yun9Yc4uBoI2lgv6chi?usp=sharing)

## Learning objectives

This lesson is designed to explain the basics of programming in Python.

* learn and practice computer science terms: variable, comment, call,
  function, arguments, keyword argument, positional argument, default
  values, float, int, string, boolean, list, dict, object, package,
  array, DataFrame, Series
* calculate arithmetic operations
* call functions and change default values
* indexing
* logical statements
* array
* DataFrame, Series, some details

## Preparation

To follow along with this Lesson, please open the Colab notebook [Week
01
Notes](https://colab.research.google.com/drive/1VQhUmSxM6WfSw1ZZeKfhkRhkfM9JPXQx?usp=sharing).
The first code cell of this notebook calls to the remote computer, on
which the notebook is running, and installs the necessary packages.
For practice, you are repsonible for importing the necessary packages.

## Variable

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
more and more on the details surrounding why the code is written as it is.
This is a fine line.

## Functions

This section will focus on using Python functions.  We will look at
writing functions later.  A Python **function** is a named and bundled
sequence of actions to perform on one or more variables.  The inputs
are called arguments and the outputs (if there are any) are called
return values.

Consider the function `abs`, which computes the absolute value of its argument.

```{code-cell}
x = -3.1415
abs(x)
```

The input is the variable `x`, which holds the value `-3.1415`, and the output is the
value `3.1415`, which itself is not assigned to any variable.

The general pattern in Python to call a function named `f` on a single
argument `x` is `f(x)`.  If there is more than one argument, say `x1`,
`x2`, and, `xN`, the the arguments are separated by commas, `f(x1, x2,
..., xN)`.

The function `round` accepts as the first argument a number and a
second, named, argument the number of digits to round the first
argument to.  If a function argument is named, it is called a
**keyword arguments**.  If a function argument is not named, it is
called a **positional argument**.

```{code-cell}
round(x, ndigits = 1)
```

Keyword arguments have a few benefits.  Keyword arguments can make
code easier to read, so that you and/or the reader of your code is
(somewhat) reminded what the argument `1` is doing.  Keyword arguments
also can have **default values**.  The argument `ndigits` of the
function `round` defaults to `None`, if no other value is specified,
which in this case instructs `round` to round the first positional
argument to the nearest integer.

```{code-cell}
round(x)
```

If you put arguments in the correct order, you do not need to specify
the name of the keyword arguments, but this isn't always good
practice.

```{code-cell}
round(x, 3)
```

## Variable types

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

There are many other Python types.  Here's an incomplete list of
Python specific types: `str`, `bool`, `list`, and `dict`.  We'll look
at `str` and `bool` immediately, and save `list` and `dict` for the
next section.

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
the two specific values, which are spelled `True` and `False` in Python.

```{code-cell}
y = True
z = "True"
y == z
```

Spend a second with the code above.  The variable `y` is of type `bool` because it
references one of the special Boolean values, specifically `True`.  The variable
`z` is of type `str` because it references a string which contains the word
True.  Python considers `y` and `z` as different, since they are different types and
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

The `TypeError` is a friendly reminder that you re-assigned the
variable `x` to hold a `str` value and the function `abs` has no
meaning when applied to a `str` type.  If nothing else, we must get
used to dealing with types appropriately.

There are in fact many more variable types in Python and in programming more
generally.  For now, we'll skip to variable types that store (plural) data.

## Data structures

Variable types that contain more than one datum, data, are generally referred to
as data structures.  Two important Python types for storing data are `list` and `dict`.

Both `list` and `dict` are types that contain multiple elements.  For both
types, the contained elements themselves do not need to be of the same type.
What differentiates a `list` from a `dict` is how the contained elements are
indexed (and also how the data structures are stored in memory).

### List

A `list` indexes its elements with `int`s or slices of integers.  The code below
creates a variable named `l` which references a list of heterogeneous types, and
then indexes the first element of the list `l`.

```{code-cell}
l = [1, 2.1, "Chico State", True]
l[1]
```

To define the list `l` the expression on the right hand side of `=`
surrounds, with square brackets, a comma separated list of values.
Any left square bracket `[` must be followed by a right square bracket
`]` to complete a list-defining expression.

The syntax `l[1]` indexes the list and retreives the first (not the zero-th)
element of the list.  Lists in Python are said to be zero indexed, since the
zero-th element of the list `l` is the value `1`.

One can also index a list with a slice of integers, e.g. `0:2`.

```{code-cell}
[l[0:2], l[1:], l[:1], l[0:3:2]]
```

Indexing lists with slices takes a fair bit of practice.  The above code creates
a new unreferenced list, which containes as elements sub-lists of the list `l`.  The zero-th
sub-list consists of the zero-th and first elements of `l`.  Notice though,
despite the slice `0:2`, the second element of `l` is not retreived.  The slice
`a:b` indexes from a list all elements at position `a` up to but not
including `b`.

The slice `1:` indexes the first element up to and including the last element.

The slice `:1` indexes the zero-th element up to but not includes the first
element.

The slice `0:3:2` indexes the zeros-th and the second element, since the
`:2` indicates steps of size `2` starting from index `0` and going up to
index `3`.

One can also index a list in reverse order.  Imagine wrapping the
indices around the back of the list starting from the zero-th element.
Thus the `-1`-th element is the last element.

```{code-cell}
l[-1]
```

And for fun, even crazier indexing expressions exist.

```{code-cell}
l[::-1]
```

The slice `::-1` says to start at index `0`, go up to the last index, and take
steps of size `-1`.  Hence, one gets a list in reverse order.

### Dictionary

A dictionary indexes its elements with keys.  The type of a dictionary
in Python is `dict`.  In other computer science worlds, a Python
`dict` might be called an associative container because it associates
to each key a value.

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


The `dict` referenced by the name `d` is created using curcly braces,
instead of square brackets like for a list.  Between the curly braces
the pattern repeats `key: value` pairs separated by commas, where the
colon `:` distinguishes the key from the value.  There are lots of
options for the key types, but it will be easiest if we think of the
keys as specifically type `str` for now.

The dict `d` associates with key `"pi"` the value stored in the variable
`x`, that we created earlier.  The key `"list"` associates the list
`l`.

The options for indexing `dict`s are, in some sense, more restrictive than for
`list`s.  There are no slices with `dict`s.  However, one can mutate (think
edit) a `dict` by indexing into a key that doesn't exist in order to mutate the
dict `d` and associate the new key with a specified value.  If the key happens
to exist already, the equals sign below will instead over-write the value
previously associated with the key.

```{code-cell}
d["python"] = "seems fancy"
d["python"]
```

These data structures show up all over Python code.  Python `list`s
are helpful when you want to reference data of not necessarily the
same type, with one variable name, in an order that makes sense to
index by sequential integers.  Python `dict`s are helpful when the
order of the contained elements does not necessarily make sequential
sense and instead it is easier to associate the contained elements by
key/name.

## Packages

There are many more types built-in to Python and many more types not directly
built-in.  Communities of programmers, like that of the data science community
and before them the numerical computing community, have built types into
**packages** for very specific use-cases.  Think of packages as additional
features one can add-on to Python when you need/want.

In the last sections of Week 01, we'll introduce the type `ndarray` from the
Python packages NumPy and the types `DataFrame` and `Series` from the package
Pandas.  We'll explore the type `DataFrame` by example, using a dataset that is
bundled with the plotting package plotnine.  All of these packages are important
for any data analysis using the programming language Python.

One must install and then import a package in order to use it.  At the
top of each Colab notebook, I will have placed code to install the necessary
packages into the notebook environment.  Once installed, to import the
packages plotnine, NumPy, and Pandas, the following code is common.

```{code-cell}
import plotnine

import numpy as np
import pandas as pd
```

The general code used to import a package is `import package_name`, like shown
for plotnine, but for some packages it is common to rename the packages.  NumPy
is often imported as `np`, so that anytime you need to reference any part of the
NumPy package, you lead with `np`, e.g. `np.array`, `np.mean`, or `np.size`.
Similarly, the package Pandas is often renamed as `pd`.


## Array

From the Python package NumPy, the type `ndarray` is used to contain multiple
elements, all of the same type.  The name of the array type, `ndarray` is meant
to stand for n-dimensional array.  The most common element types of NumPy
`ndarray`s are `bool`, `int`, or `float`.  Although, for reasons we won't cover, NumPy
uses its own types, analogous to the ones just mentioned.  The table below lists
some of the Python types we discussed above and the analogous NumPy types that
are often contained within a NumPy `ndarray`.

| Python | NumPy |
|--------|-------|
| `bool` | `bool_` |
| `int`  | `int_` or `int64` |
| `float` | `double` or `float64` |

The code below creates a NumPy array of `np.float64`s, since at least
one element of our input list contains a `float`.

```{code-cell}
import numpy as np # this line shows up once at the top of each notebook,
# not once per code cell

a = np.array([1., 2, 3])
a
```

With a NumPy array, like `a` above, you can calculate the mean or the
sum of the number of elements in the array with code like the
following.

```{note}

Note that only the last line of code actually prints, even if all lines of code are executed.

```

```{code-cell}
np.mean(a)
np.sum(a)
np.size(a)
```

The array `a` above is only one dimensional.  `np.ndarray`s, read as NumPy
arrays, can be multidimensional, hence the name `ndarray`.  The array `A` below
is initialized to hold 2 dimensions of ones; two rows and three columns of ones,
for a total of 6 ones.  Then `A` is multiplied by the mean of `a`.  Notice that
the multiplication happens element-wise, that is each element of `A` is
multiplied by `np.mean(a)`, and the output is a new array with the same shape
that `A` was initialized to.

```{code-cell}
A = np.ones(shape = (2, 3))
A * np.mean(a)
```

The shape of `A`, and this newly created, un-referenced array, is effectively 2
rows, each of size 3.

The function `np.shape` returns the shape of the array, while
`np.size` returns the total number of elements in the array.

```{code-cell}
print(np.shape(A)) # print(...) forces printing
np.size(A)
```

There is a lot more to say about NumPy's type `ndarray`.  For now, this is
enough and we'll move on to Pandas `DataFrame` and `Series` types, since we'll
spend most time in this class using dataframes.

(week-01-dataframes)=
## DataFrames

```{note}

To use the package Pandas, you normally need to execute `import
Pandas as pd`.  Such a line of code only needs to happen once per
notebook, and it should occur near the top of the notebook.  We will
instead import a dataframe from the package plotnine for our example.

```

The Python package Pandas has two important types for data analysis:
`pd.Series` and `pd.DataFrame`.  The `pd.DataFrame` type is used for tabular or
rectangular data; think data that would fit well in a spreadsheet.

The type `pd.Series` is intended to contain multiple elements, all of the same
type.  In this aspect, `pd.Series` is similar to `np.ndarray`.  Pandas created
their own homogeneous type container, `pd.Series` to more naturally wrap
multiple `pd.Series`s into a DataFrame.  And that's exactly what `pd.DataFrame`
is, one container of multiple Series, each of which has its own element type and
makes up one column of the tabular data.  So `pd.DataFrame` is a heterogeneous
type container, made up of multiple homogeneous type columns.  A further
requirement of `pd.DataFrame` is that each contained Series must have the same
size, hence `pd.DataFrame` holds rectangular data.

Let's see an example.  We'll import the DataFrame `diamonds` from the
Python package plotnine.

```{code-cell}
from plotnine.data import diamonds

diamonds
```

Each of the columns

```{code-cell}
diamonds.columns
```

has a name and each row corresponds to a row index, starting at index 0, just
like a NumPy arrray.  Further, each column has one specific type for all the
elements contained in that column.  The non-numeric type `category` is for
columns which contain names or labels.

```{code-cell}
diamonds.dtypes
```

Notice that I accessed a property `.columns` of the dataset
`diamonds`, just like we previously accessed types and functions from
the packages NumPy or Pandas, e.g. `np.array`, `np.mean`,
`pd.DataFrame`.  We'll cover the details of such `.` access later.
For now, let's continue to use properties associated with a
DataFrame to learn about the type `pd.DataFrame`.

Each DataFrame is two dimensional, so has a shape, a numer of rows and
number of columns, and a size.  There are 53,940 observations and 10
variables in the `diamonds` dataset.

```{code-cell}
print(diamonds.shape)
diamonds.size
```

To index one column from a DataFrame, use the associated property `.loc`.
Because the associated property `.loc` is not a function, but instead a property
for which you index rows and/or columns, you should use squre brackets as we did
for indexing `list`s and `dict`s.  Inside the square brackets goes an index for
rows and then columns, separated by a comma, e.g. `pd.DataFrame.loc[rows,
cols]`.

```{code-cell}
diamonds.loc[:, "depth"]
```

The colon `:` acts as a slice of `int`s indexing rows 0 up to and including the
last row.  Check for yourself that the type of the retrieved column is
`pd.Series`.

Indexing a DataFrame by column is so common, that you can even
retreive a column by indexing a DataFrame like a `dict`, where the key
is the name of the column.  Check for yourself that the type of the
retrived columns is `pd.Series`.

```{code-cell}
diamonds["depth"]
```

You can also retreive more than one column at once.  The key in this
case is a list of column names, and the return value is a subset of
the DataFrame you are indexing.

```{code-cell}
diamonds[["depth", "cut"]]
```

Since each column is a Series, which is analogous to `np.ndarray`, you can
calculate the size, mean, standard deviation, minimum, percentile/quantiles, or
maximum of a DataFrame column.  Or you can use the function `describe`
which is associated with both `pd.Series` and `pd.DataFrame` types.

```{code-cell}
depth = diamonds["depth"]
print(np.size(depth))
print(np.mean(depth))
print(np.std(depth))
print(np.quantile(depth, [0.25, 0.5, 0.75]))
depth.describe()
```

Only the columns with numeric types, `float64` or `int64` are
`describe`d.  The columns of type `category` are automatically removed
before the numeric summaries are calculated.

```{code-cell}
diamonds.describe()
```

You can index a subset of rows with slices, but with `pd.DataFrame`s, a slice
like `start:stop` will include the value `stop`.  The following code therefore
retreives rows 2 up to **and including** 10, and both columns `"depth"` and
`"clarity"`.  Notice that to get multiple columns, you must wrap the column
names in a list.  Check for yourself that the type of the object returned is
`pd.DataFrame`.

```{code-cell}
diamonds.loc[2:10, ["depth", "clarity"]]
```

```{note}

Quoted straight from the Pandas documentation: "Note that contrary to
usual python slices, both the start and the stop are included"

```

The options for indexing `pd.DataFrame`s and `pd.Series`s are, in some sense,
more powerful than for `list`s.  Let's consider indexing by a Series of
booleans.  The variable `idx` is a boolean Series that indexes all the rows
where the logical statement is true, that is when a diamond's cut is equal to
"Very Good".  All rows where a diamond's cut is not equal to "Very Good" are
indexed with False.

```{code-cell}
idx = diamonds.loc[:, "cut"] == "Very Good"
idx
```

We can then use the variable `idx` to index another column of
`diamonds`.  Say we want to calculate the mean of the column price,
for only the "Very Good" cut diamonds.

```{code-cell}
np.mean(diamonds.loc[idx, "price"])
```

One can also quickly flip all of `idx`'s `True`s to `False`s and all
`False`s to `True`, so as to compare the mean price of not "Very Good" cut
diamonds.  Notice the `~`, read tilde, in front of `idx`.

```{code-cell}
np.mean(diamonds.loc[~idx, "price"])
```

```{seealso}
For more about indexing in Pandas see [Selection by
label](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-label)
and [Selection by
position](https://pandas.pydata.org/docs/user_guide/indexing.html#indexing-integer).
Such tools have a steep learning curve and a huge payoff.
```

```{seealso}
[Week 01 Assignment](https://colab.research.google.com/drive/1h9Ck7kWNN9_I2Yun9Yc4uBoI2lgv6chi?usp=sharing)
```
