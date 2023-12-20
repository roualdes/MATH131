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

# Week 03: Wrangling data and intro to graphing

### Learning outcomes

After completing this lesson students will be able to

* create basic data visualizations

### Preparation

To follow along with this Lesson, please open the Colab notebook
[Week-03-Notes](#TODO).  The first code cell of this notebook calls to
the remote computer, on which the notebook is running, and installs
the necessary packages.  For practice, you are repsonible for
importing the necessary packages.

### The syntax of `ggplot`

The reason we use the functions in plotnine is for consistency in the structure
of it’s arguments. Here is the bare bones plotting syntax.  All capital letter
variables are meanto be filled in, but note that not all variables are required
for any given plot type.  It will take some time and practice to understand
which plot types require which variables.

```{code-cell}
import pandas as pd
import plotnine as pn
```

```{code-cell}
---
tags: [raises-exception, remove-output]
---

pn.ggplot(data = DF) + pn.geom_PLOTTYPE(pn.aes(x = "X_VAR", y = "Y_VAR", fill = "FILL_VAR", group = "GROUP_VAR"))
```

* `DF` is the name of the DataFrame in which the variables for your plot live
* `PLOTTYPE` is the plot type you want to make e.g. histogram, density, boxplot, point
* `*_VAR` are variables from the DataFrame; don't forget the quotes

### `msleep` DataFrame

We'll use the `msleep` DataFrame from the package plotnine.

```{code-cell}
from plotnine.data import msleep

msleep
```

### Univariate (one variable)

##### categorical variable

For one categorical variable, possibly the simplest way to present data is in a
table.  A table for a categorical variables displays the counts of the unique
categories in the variable of interest.  Consider the variable conservation in
the DataFrame `msleep`.  The easiest way to get counts of the categories in the
variables conservation is with `.value_counts`.

```{code-cell}
msleep["conservation"].value_counts()
```

Alternatively, you could make a bar chart.  Remember, if you want to exclude the
missing data, you have to remove them first from the DataFrame, specifying the
column of interest.

```{code-cell}
p = pn.ggplot(data = msleep) + pn.geom_bar(pn.aes(x = "conservation"))
p.draw()
```

If you want to order the categories in a certain way, you are in charge of
creating a new variable that has the specified order of interest.  Here's some
code to order the categories in decreasing order of their counts.  Note that
missing values are always drawn last, even if this seems to mess up the
ordering.


```{code-cell}
conservation_cats_ord = msleep["conservation"].value_counts().index.tolist()
msleep["conservation_cat_ord"] = pd.Categorical(msleep["conservation"], categories = conservation_cats_ord)

p = pn.ggplot(data = msleep) + pn.geom_bar(pn.aes(x = "conservation_cat_ord"))
p.draw()
```

You could also use list indexing to order the categories in increasing order.

```{code-cell}
msleep["conservation_cat_ord"] = pd.Categorical(msleep["conservation"], categories = conservation_cats_ord[::-1]) # notice the indexing

p = pn.ggplot(data = msleep) + pn.geom_bar(pn.aes(x = "conservation_cat_ord"))
p.draw()
```

##### numeric variable

Rather than showing the value of each observation of a numeric variable, it is
common to display the value as belonging to a bin. The height of each bar in a
histogram records the frequency of values that fall into each bin.  The x-axis
for a histogram is different from a bar chart, because a histogram has a numeric
x-axis, whereas a bar chart has a categorical x-axis.

```{code-cell}
p = pn.ggplot(data = msleep) + pn.geom_histogram(pn.aes(x = "sleep_total"))
p.draw()
```

Notice that plotnine issued a warning about choosing the number of bins.  If you
don't specify the number of bins, plotnine will choose for you, and also yell at
you.

There is no one right choice for the number of bins.  Generally, between 7 and
51 are good first guesses.

```{code-cell}
p = pn.ggplot(data = msleep) + pn.geom_histogram(pn.aes(x = "sleep_total"), bins = 9)
p.draw()
```

A density plot is like a histogram that has been smoothed over.

```{code-cell}
p = pn.ggplot(data = msleep) + pn.geom_density(pn.aes(x = "sleep_total"))
p.draw()
```

The benefit of adding `+` layers of `geom_PLOTTYPE`s is that we can combine
plots to help us understand better the data.  The code below layers a histogram
and a density plot, and specifies two other options.  First, to force the
histogram onto the samw scale as a density plot, use `y =
pn.after_state("density")`.  Second, we make transparent the histogram by
specifying `alpha = 0.5`.  The keyword argument `alpha` ranges from `1`, by
default, to `0` completely transparent.

```{code-cell}
p = (pn.ggplot(data = msleep) 
    + pn.geom_density(pn.aes(x = "sleep_total"))
    + pn.geom_histogram(pn.aes(x = "sleep_total",  y = pn.after_stat("density")), alpha = 0.5, bins = 9))
p.draw()
```

### Bivariate (two variables)

##### categorical variables

More sophisticated tables of categorical variables can be with the Pandas
function `pd.crosstab`.  One of the benefits of `pd.crosstab` is that you cross
tabulate multiple Series.

```{code-cell}
pd.crosstab(msleep["conservation"], msleep["vore"])
```

You can also request proportions, instead of counts in the cross tabulated
table.  If you want to get proportions relative to the rows, you specify the
keyword argument `normalize` equal to `"index"`.  If you want proportions
relative to the columns, use `"columns"`.  And if you want proportions relative
to the entire table, use `"all"`.

```{code-cell}
pd.crosstab(msleep["conservation"], msleep["vore"], normalize = "all").round(3)
```

The plot equivalent of a cross tabulated table might be a grouped bar chart.  As
of 2023-12-20, the plotting package will yell at you if there are any missing
data, so let's remove the missing data from our two variables of interest first.

```{code-cell}
df = msleep.dropna(subset = ["conservation_cat_ord", "vore"])
p = pn.ggplot(data = df) + pn.geom_bar(pn.aes(x = "conservation_cat_ord", fill = "vore"))
p.draw()
```

```{code-cell}
df = msleep.dropna(subset = ["conservation_cat_ord", "vore"])
p = pn.ggplot(data = df) + pn.geom_bar(pn.aes(x = "conservation_cat_ord", fill = "vore"), position = "dodge")
p.draw()
```

##### numerical variables


##### numerical and categorical
