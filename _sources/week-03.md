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

# Week 03: Graphing and aggregating

## Learning outcomes

After completing this lesson students will be able to

* create basic data visualizations

## Preparation

To follow along with this Lesson, please open the Colab notebook
[Week-03-Notes](#TODO).  The first code cell of this notebook calls to
the remote computer, on which the notebook is running, and installs
the necessary packages.  For practice, you are repsonible for
importing the necessary packages.

## Graphing

### The syntax of `ggplot`

The reason we use the functions in plotnine is for consistency in the structure
of it’s arguments. Here is the bare bones plotting syntax.  All capital letter
variables are meanto be filled in, but note that not all variables are required
for any given plot type.  It will take some time and practice to understand
which plot types require which variables.

```{code-cell}
import numpy as np
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

#### categorical variable

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

#### numeric variable

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

#### categorical variables

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
p = (pn.ggplot(data = df)
    + pn.geom_bar(pn.aes(x = "conservation_cat_ord", fill = "vore")))
p.draw()
```

```{code-cell}
df = msleep.dropna(subset = ["conservation_cat_ord", "vore"])
p = pn.ggplot(data = df) + pn.geom_bar(pn.aes(x = "conservation_cat_ord", fill = "vore"), position = "dodge")
p.draw()
```

##### numerical variables

The most common method of visualizing the relationship between two numeric
variables is by using a scatterplot.  With `ggplot` we specify both the x and y
variables, and the geometry `geom_point`.

```{code-cell}
p = (pn.ggplot(msleep, pn.aes(x = "brainwt", y = "bodywt"))
    + pn.geom_point())
p.draw()
```

The most common trend line added to a scatterplot is the so called best fit, or
least squares line. This is done by adding a `geom_smooth()` layer.  A least
squares line should only be used for linearly related numerical variables, and
sometimes you can help linearize variables with log transformations.

```{code-cell}
p = (pn.ggplot(msleep, pn.aes(x = "brainwt", y = "bodywt"))
    + pn.geom_point()
    + pn.geom_smooth(se = False, color = "blue")
    + pn.scale_x_log10()
    + pn.scale_y_log10()
    + pn.labs(x = "log of brain weight", y = "log of body weight"))
p.draw()
```

Look back at the slightly changed in syntax in the last two plots.  Both the
dataset and the aesthetic specification showed up as positional arguments,
instead of the dataset as a keyword argument and the aesthetic specification in
the geometry layer.  With the syntax as in the last two examples, the aesthetic
specification is shared across both the `geom_point` and the `geom_smooth`
layers.  All other examples have provided a syntax where the aesthetic is
specific to a single layer and not all subsequenty layers.

##### numerical and categorical

One can combine plots for numeric variables with categorical information by
repeating each plot type above for each category of a categorical variable,
where each category is drawn in a unique color.  When coloring across a
categorical variable like this, a legend becomes necessary.

```{code-cell}
p = (pn.ggplot(data = msleep)
    + pn.geom_density(pn.aes(x = "sleep_total", color = "vore")))
p.draw()
```

Alternatively, you can put the categorical variable on an axis and summarize the
numerical variable on the other axis.

```{code-cell}
p = (pn.ggplot(data = msleep)
    + pn.geom_boxplot(pn.aes(x = "vore", y = "sleep_total")))
p.draw()
```

Sometimes it is helpful to add a violin plot layer, if the box plots are not
capturing all of the information in the density of the data.  Otherwise, it can
just be fun.

```{code-cell}
df = msleep.dropna(subset = "vore")
p = (pn.ggplot(df, pn.aes(x = "vore", y = "sleep_total", fill = "vore"))
    + pn.geom_boxplot(width = 0.2)
    + pn.geom_violin(alpha = 0.1))
p.draw()
```

It seems as of 2023-12-23 that `fill`ing by a categorical variable does not work
appropriately if there is any missing data.  The plot above works, if you first
drop the NAs.


## Aggregating

In Week 02, we learned how to summarize numerical variables by themselves.  The
latter plots above provide **graphical** summaries of numerical variables across
a categorical variable.  Aggregating, or aggregation, is all about
**calculating** summaries of numerical across a categorical variable.

The first step in aggregating is to group a DataFrame by the categories, or
levels, of some categorical variables.  The Pandas function `groupby` carries
out this task for us.  You can group by a single categorical variable, as we do
below, or group by a list of categorical variables.

```{code-cell}
gdf = msleep.groupby("vore", as_index = False)
gdf
```

By itself, `groupby` is pretty boring and doesn't obviously do much.  The real
power of `group` by is when you couple it with the function `aggregate`, which
can be abbreviated to `agg`.

```{code-cell}
odf = gdf.aggregate(
    mean = ("sleep_total", np.mean),
    sd = ("sleep_total", np.std),
    count = ("sleep_total", np.size))
odf
```

The code above can be a lot to take in at first.  And to be honest, it is more
complex than necessary.  The structure of the `aggregate` code above is the most
explicit, in the hope that you can see all the necessary pieces of the puzzle
and understand how they fit together.

The basic pattern of the keyword arguments to `aggregate` go like this.  You
specify as a keyword argument the name of the output variable of interest.
These output variables will show up as columns of the output DataFrame, along
side the grouping variable(s).  So we actively chose the output variable names
`mean`, `sd`, and `count`.  We could have just as well chosen the output
variables names to be `horse`, `octopus`, and `badger`, but obviously more
meaningful variables are better.

On the right hand side of each equals sign, for each keyword argument, you put
within a left/open and right/close parenthesis the name of the numerical
variable you want to act on, and then the function that will act on the
numerical variable you specified.  The numerical variable must live in the
DataFrame you are aggregating, and the function to be applied to your numerical
variable must take only one numerical variable as its first argument, as does
`np.mean`, `np.std`, and `np.size`.

The output DataFrame can now be acted on just like anyother DataFrame we've
worked with previously.  For instance, we can create a Series based on variables
from `odf` and insert this new Series into the DataFrame `odf` or we can make
plots from `odf`.  We'll spend some more time on the details of the plot below
in [Week 04](../week-04.md).

```{code-cell}
odf["se"] = odf["sd"] / np.sqrt(odf["count"])

p = (pn.ggplot(odf)
    + pn.geom_point(pn.aes(x = odf["vore"].cat.codes, y = "mean"), color = "blue"))
p.draw()
```
