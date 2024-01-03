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

# Week 02: Introduction to working with data

## Learning objectives

In this lesson we learn how to summarize data in a DataFrame, and do basic data
management tasks such as making new variables, recoding data, and dealing with
missing data.

After completing this lesson, you should be able to

* identify missing values in a dataset
* summarize variables contained in a DataFrame
* make new variables and insert them into a DataFrame
* edit variables contained in a DataFrame

Throughout this notebook, we'll use the dataset `msleep` from the packages
plotnine.

## Preparation

To follow along with this Lesson, please open the Colab notebook
[Week-02-Notes](#TODO).  The first code cell of this notebook calls to
the remote computer, on which the notebook is running, and installs
the necessary packages.  For practice, you are repsonible for
importing the necessary packages.

## Missing data

Missing data occurs when the value for a variable is missing.  Think
of it as a blank cell in an spreadsheet.  Missing values can cause
some problems during analysis, so let’s see how to detect missing
values and how to work around them.

Let's use the dataset `msleep` from the package plotnine.

```{code-cell}
from plotnine.data import msleep

import plotnine as pn
import numpy as np

msleep
```

We can see that a few variables contain the value `NaN`, which stands for
not-a-number.  These are the missing data.  Notice that only the first
five and last five rows of the DataFrame are printed.  The ellipsis,
..., in the middle row indicates not all rows are printed by default.
So we may not be able to see all the variables with `NaN`s.  Each
column/Series has a property `.hasnans` which returns `True` if the
Series on which it is called has any `NaN`s.

```{code-cell}
msleep["conservation"].hasnans
```

Missing values are tricky to deal with in general.  Much of the time
in Pandas, it seems like things just work out well for you.  For
instance, calculating a mean or a standard deviation automatically
ignores `NaN`s.

```{code-cell}
np.mean(msleep["brainwt"])
```

But other times, `NaN`s are not automatically ignored, and nothing
about the code or its output tells you when `NaN`s are ignored or not.

```{code-cell}
np.size(msleep["brainwt"])
```

The plotting package plotnine, by default, includes `NaN`s as its own category,
which can be undesirable.

```{code-cell}
p = pn.ggplot(data = msleep) + pn.geom_bar(pn.aes("conservation"))
p.draw()
```
To remove the missing data from a plot, create a new DataFrame by
dropping the mising data from the column of interest.  You should
always specify the keyword argument `subset`, lest you drop all
missing data, which might drop a row where the data is missing in a
column you care less about and subsequently drops data from the row
you do care about.

```{code-cell}
df = msleep.dropna(subset = "conservation")
p = pn.ggplot(data = df) + pn.geom_bar(pn.aes("conservation"))
p.draw()
```

The code equivalent of the last plot above is as follows.

```{code-cell}
msleep["conservation"].value_counts(dropna = True)
```

## Summarize data

In the subsection above, Missing data, we learned that accounting for
missing data can be tricky.  The Pandas property
`describe` which excludes `NaN` values.  Thus, you have a choice.  Use
`np.size` if you want a count of the number of elements **including**
missing data, `NaN`s.  Or use `.describe` if you want a count of the
number of elements **excluding** missing data.

Here, we call `.describe` on a Series of type `category` elements,
that is a categorical variable named conservation.

```{code-cell}
msleep["conservation"].describe()
```

Here, we call `.describe` on a Series of type `float64` elements, that
is a numeric variables named brainwt.

```{code-cell}
msleep["brainwt"].describe()
```

Since there is not much to be done with missing values other than
count them, here is a line of code you can use to count the number of
missing values.

```{code-cell}
nan_idx = msleep["conservation"].isna()
np.sum(nan_idx)
```

The property/function `.isna` returns a boolean Series where each
missing value is indexed by `True` and each non-missing value is
indexed by `False`.  Since `True` is equivalent to the number 1 and
`False` is equivalent to 0 in Python, we can simply sum up the number
of `True`s and `False`s to count the number of missing data.

## Creating/Editing new variables

Imagine you want to create a new variable `brnbdywt` in the DataFrame
`msleep`.  This new variable might tell you something about how smart
the animal is.  The bigger the brain to body weight ratio, maybe the
smarter the animal is.

Just like for a `dict`, you can index into a DataFrame with a column
name that does not yet exist, so long as you first assign to that
column with some expression.  For instance, let's create a new
variable `brnbdywt` inside the DataFrame `msleep`, and assign to it
the ratio of the columns `brainwt` and `bodywt`.

```{code-cell}
msleep["brnbdywt"] = msleep["brainwt"] / msleep["bodywt"]
```

The division on the right hand side of `=` is done element-wise.
Therefore, the Series created by the division of two Series has the
same size as every column in `msleep`.  We have thus assigned to the
new column `brnbdywt` a new variable that was created by the ratio of
`brainwt` to `bodywt`.

```{code-cell}
msleep["brnbdywt"].describe()
```

We can take this one step further.  Let's create a new boolean
variable named `smrt`, which will hold `True` whenever an animal is in
the top 25% of the brain to body weight ratio and `False` otherwise.
We start by creating the new column and making every value equal to
`False`.

```{code-cell}
msleep["smrt"] = False
```

Since the right hand side of the equals is scalar, just one
value, it is recycled as much as necessary to create a new column in
`msleep` to keep the requirement that every column of a DataFrame has
the same size.

Next, we'll create a boolean Series that indicates when an animal
qualifies as `smrt`.  Then, we over-write with the value `True` just
those rows for which we've identified a `smrt` animal.  Hence, we can
edit a variable in a DataFrame using the indexing strategy we learned
about in [Week 01: DataFrames](week-01-dataframes).

```{code-cell}
smrt_idx = msleep["brnbdywt"] >= 0.015
msleep.loc[smrt_idx, "smrt"] = True
```

(categorical-variables)=
## Categorical variables

A categorical variable is a variable that has names or labels as
values.  We basically created a categorical variable above, with only
two values `True` and `False`, and named it `smrt`.  The only thing we
haven't done is convinced Python/Pandas to treat the variable `smrt`
as if the elements are of type `category`.  Let's fix this.

```{code-cell}
msleep["smrt"] = msleep["smrt"].astype("category")
msleep["smrt"]
```

Invariably with categorical variables, the categories you have are
not what you want.  Let's change the categories from `False` and `True` to
something else.

```{code-cell}
msleep["smrt"] = msleep["smrt"].cat.rename_categories({False: "nope", True: "yup"})
```

There's two things to note here.  First, the property `.cat` can only
be called on a categorical Series; the elements must be recognized by
Python/Pandas as type `category`.  Second, the argument to
`.rename_categories` is a `dict` with keys equal to the categories
that you have, but don't want, and values equal to the categories you
want.  The pattern in pseudo-code might be written as `{"old":
"new"}`.  The problem with this is that the keys don't have to be of
type `str`, as we saw above.

We now have a variable with categories as names or labels,
instead of `False` and `True`, we can develop more categories.  Let's
break up the `"Nope"`s into two groups: `"meh"` for so-so smart
animals and `"Nope"`.  The `"meh"` group is in the middle.  In order
to add this new category, we need to prime the Series `smrt` for the
fact that we want to add a category that doesn't yet exist.

```{code-cell}
msleep["smrt"] = msleep["smrt"].cat.add_categories(["meh", "doh"])
```

Next, let's create a boolean Series which indicates the exact rows
which should be labeled as `"meh"`.  The variable `meh_idx` is one
boolean Series created with logical and `&` of two boolean Series.  In
an expression such as `a & b`, where `a` and `b` are both boolean
Series, each element of `a` is compared to each element of `b`.  When
both elements are `True`, the corresponding element of `meh_idx` is
`True`.  If either element is `False`, the corresponding element of
`meh_idx` id `False`.

```{code-cell}
meh_idx = (0.004 <= msleep["brnbdywt"]) & (msleep["brnbdywt"] < 0.015)
msleep.loc[meh_idx, "smrt"] = "meh"
msleep["smrt"]
```

Did you notice that I added the unused category `"doh"`?  On the
one hand, this shows that you can add multiple new categories at once,
just use a list.  On the other hand, you may very well end up with an
unsed category after renaming various categories.  Here's how you can
remove any unused categories.

```{code-cell}
msleep["smrt"] = msleep["smrt"].cat.remove_unused_categories()
msleep["smrt"]
```
