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

# Week 04: Filling in some details

### Learning outcomes

After completing this lesson, you will be able to

* customize many aspects of plotnine plots
* make 2d plots displaying 3 variables
* prepare your own dataset for an analysis
* understand the basics of file paths
* read your own dataset into Python

### Preparation

To follow along with this Lesson, please open the Colab notebook
[Week-04-Notes](#TODO).  The first code cell of this notebook calls to
the remote computer, on which the notebook is running, and installs
the necessary packages.  For practice, you are repsonible for
importing the necessary packages.

## Graphing details

In this section, we cover some details about customizing a plotnine plot.  This
lesson will not explore all available options, nor are you expected to memorize
all the details we cover.  This lesson is intended to be an introduction to the
types of things you can easily change on a plotnine plot.  For details and/or
reference material on computer software, you should consult the plotnine
documentation.

Let's continue developing the plot we started at the end of [Week
03](../week-03.md).  Our goal is to display information about the mean total
amount of sleep mamals (in our dataset) get, where the data are grouped by their
`vore` status.  This is a task for aggregating: group by `vore` and summarize
with the function `np.mean`.  Since the mean of any dataset is random, as part
of the random sampling of the data, we should also calculate and display the
error in our estimate associated with the mean.  That is, we should display a
confidence interval for the mean.

```{code-cell}
import numpy as np
import pandas as pd
import plotnine as pn

from plotnine.data import msleep
```

After loading the appropriate Python packages, we aggregate our dataset: group
by `vore` and calculate means, by groups, of the numeric variable `sleep_total`.
Since `vore` has some missing data, we'll drop rows of the DataFrame where
`vore` is missing.

```{code-cell}
df = msleep.dropna(subset = "vore")

odf = (df
    .groupby("vore", as_index = False)
    .aggregate(
        mean = ("sleep_total", np.mean),
        sd = ("sleep_total", np.std),
        count = ("sleep_total", np.size)))
```

Next, we'll calculate the lower and upper bound of the confidence interval.

```{code-cell}
odf["se"] = odf["sd"] / np.sqrt(odf["count"])
odf["ub"] = odf["mean"] + 1.96 * odf["se"]
odf["lb"] = odf["mean"] - 1.96 * odf["se"]
```

* title
* labels
* other geoms: errorbar, jitter
* change tick labels
* re-consider bar charts
* multivariate plotting

Since plotnine plots are created by adding layers of information, let's get
started by creating the variable `p` to hold our plot as we build it up piece by
piece.  Below is the code we used from [Week 03](../week-03.md).


```{code-cell}
p = (pn.ggplot(odf)
    + pn.geom_point(pn.aes(x = odf["vore"].cat.codes, y = "mean"), color = "blue", shape = "X", size = 2.5))
```

The keyword arguments `color` and `shape`, when they appear inside a `geom_`
layer and outside of `aes`, take just one color and one shape.  Because plotnine
is built on top of other Python packages, [matplotlib](https://matplotlib.org/stable/) specifically, finding the
available colors and shapes can be tricky.  Here are links to the [matplotlib
named
colors](https://matplotlib.org/stable/gallery/color/named_colors.html#base-colors)
and the [matplotlib available
shapes](https://matplotlib.org/stable/api/markers_api.html#module-matplotlib.markers).

```{code-cell}
p.draw()
```

So far, we have "X"s for means for each group.  No confidence intervals, no
group names (just numbers representing their group names), no original data, and
no plot title.  And maybe we want to change the axis labels.  Let's take each of
these in turn.  We leave it to the reader to display the plot after each update,
if they so choose.  We'll show the complete plot only at the end of all of our
updates.

Let's add the confidence interval, useing the `geom_errorbar` layer.

```{code-cell}
p += pn.geom_linerange(pn.aes(x = odf["vore"].cat.codes, ymin = "lb", ymax = "ub"), color = "blue")
```

The syntax `p += expression` is equivalent to `p = p + expression`.  It
certainly takes some time to get used to, but it also very convenient.  For all
of the layers we are interested in adding on to our plot `p`, we just want to
update `p` to hold the latest version of our plot.  There is no sense in keeping
around the previous version of our plot.  So this syntax is just what we want.

Next, let's add the original data.  Putting the original data on a plot is
increasingly popular and always recommended.  It helps you and your reader
understand the variation in your data, and will often help explain why one (or
more) groups may appear different.

```{code-cell}
p += pn.geom_jitter(df,              # notice the change in dataset
    pn.aes(x = df["vore"].cat.codes, # unique numeric codes of the labels in vore
    y = "sleep_total",
    color = "vore"), 
    width = 0.25,                    # the maximal width of horizontal noise/jitter
    alpha = 0.5
    )
```

The layer `geom_jitter` is like `geom_point` but adds a bit of horizontal noise/jitter
in the x-axis.  Since the x-axis of this plot is `vore` status, and not actually
numbers adding horizontal noise doesn't change the information in the data at
all.  For each category of `vore`, the numeric values on the y-axis is where the
information in the data are contained.

By specifying a categorical variable for the keyword argument `color` within
`aes`, the jittered points will show up in unique colors for each category of
the variable specified, `vore` in this case.  Further, a legend will
automatically be displayed.  Change the category names of the variable `vore` is
best done by renaming the categories, as in [Week 02: Categorical
variables](categorical-variables), but we can easily change the
legend title from plotnine.

```{code-cell}
p += pn.guides(color = pn.guide_legend(title = "Vore status"))
```

To remind your readers, and future-you, that the x-axis here is a categorical
variable, let's change the tick labels to better represent what information they
actually contain.

```{code-cell}
p += pn.scale_x_continuous(                  # continuous x-axis scale based on numeric codes of vore
    breaks = df["vore"].cat.codes.to_list(), # breaks dictate where on the x-axis, a list of numeric values
    labels = df["vore"].to_list()            # list of labels at breaks
)
```



Next, let's change the axis labels and title.

```{code-cell}
p += pn.labs(
    x = "Vore status",
    y = "Sleep (hours)", 
    title = "Hours of sleep by vore status"
)
```

Last, let's change the plot theme and font size, just to show that you can.
Actually, I much prefer simpler themes, so as to highlight the data instead of
the plot theme.

```{code-cell}
p += pn.theme_minimal(base_size = 12)
p.draw()
```

Some final thoughts on the plot above.  I'd prefer the confidence intervals not
be blue.  The default black would probably be better, but then I wouldn't have
been able to tell you more about the keyword argument `color`.

The units of your variables show up in your plots, to inform you reader, but do
now show up in your variable names.

The categories for `vore` should be changed.  Using abbreviations in your
writing and/or plots is usually not helpful to anyone but you.  There are ways
to change the categories through plotnine, instead of by changing your
DataFrame.  I don't recommend this.  There's little to no reason to use
abbreviations for label names.

Some people would have reached for `geom_errorbar` before `geom_linerange`.  The
difference is a pair of horizontal lines on the top and bottom of the confidence
intervals.  I see the appeal of this, but prefer the minimalism of
`geom_linerange`.

For a hot minute, people were debating whether or not standard deviations or
standard errors were better, when drawing lines extending from a mean as we have
done here.  The correct answer is it depends.  If you want to talk about means
across groups, then use standard errors.  If you want to talk about population
distributions across groups, then your data should at least be roughly
symmetric, and then use standard deviations.  If on the other hand you want to
show off your plotting skills, use both.

```{code-cell}
odf["sd_ub"] = odf["mean"] + odf["sd"]
odf["sd_lb"] = odf["mean"] - odf["sd"]

p = (p + pn.geom_linerange(pn.aes(x = odf["vore"].cat.codes, ymin = "lb", ymax = "ub"), size = 1.5)
    + pn.geom_linerange(pn.aes(x = odf["vore"].cat.codes, ymin = "sd_lb", ymax = "sd_ub")))
p.draw()
```


## Your own data

In this section, we'll learn about **file paths**.  File paths are not Python
specific.  A file path is like the URL of a specific file on your computer.
Each file on your computer has an address
and that address is called a file path.  

So far we've relied on a dataset that already exists on your machine.  The main
reason behind this is because file paths can be confusing at first.  And so
we've delayed this topic until near the end.  Invariably, though, you will need
to load your own dataset.

To help you learn about file paths, we have set up a Colab notebook specific for
practicing file paths.  Please follow along in the Colab notebook
[file-paths](https://colab.research.google.com/drive/12q0ostmi7b7LUFOv6srC2xBuN7y7Gz9g?usp=sharing).

#### directories

All computers organize their files in **directories**, which are sometimes
called **folders**.  Think of directories as a specific drawer in which
logically grouped files go.

Whenever you login to a computer, you should imagine that you are immediately
(as if standing) within one of the many directories of that machine.  Whichever
directory you are currently in is called the **current working directory**.
Sometimes the current working directory is abbreviated **cwd** or even just
**wd**.

From the current working directory, you can move up or down the directory
hierachry.  Which is to say directories can contain other directories, and
chances are good the current working directory is located within another
directory.  The root of the directory tree is called the **root directory**.

Each user on a computer has their own directory, which is a (not necessarily
immediate) child of the root directory.  Such a directory, unique to each user
of a computer, is called the **home directory**.

#### visualizing directory structure

From the Colab notebook [file-paths](https://colab.research.google.com/drive/12q0ostmi7b7LUFOv6srC2xBuN7y7Gz9g?usp=sharing), run the cell with the command 

```{code-cell}
!pwd
```

The band `!` calls to the underlying machine on which the `pwd`


#### file paths

The current working directory is written `.`.  From the current working
directory, you can move up, to a parent directory, or down to a child directory.
The file path written as `..` references the parent directory from the current
working directory.  Since there is only one parent directory, `..` is uniquely
the parent directory of the current working
directory.


To move up or down more than one directory at a time, you need to specify a file
path.  From the current working directory, imagine


