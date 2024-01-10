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

# Week 06 and beyond

## Python w/out Google Colab

* <a href="https://github.com/jupyterlab/jupyterlab-desktop" target="_blank">JupyterLab Desktop</a>
* <a href="https://jupyterlab.readthedocs.io/en/latest/" target="_blank">JupyterLab</a>
* <a href="https://docs.python.org/3/library/venv.html">Virtual environment -> Emacs/Vim/VS Code/other</a>

## Creating you own dataset

When you get to the point that you start creating your own small to
medium sized datasets, then this section is for you.  This section
explains some general advice surrounding creating a dataset.

Entering data into a spreadsheet is easy.  And that's good.  But there
are some gotchas that you should avoid.  Below you'll find lists of
the dos and then explanations, and the don'ts and explanations, for
creating your own datasets.

### DOs

* be consistent
* use simple variable names
  * prefer all lower case letters
  * minimize numbers and special characters
  * use underscore `_` instead of space ` `
* organize files within directories

**Be consistent**. When programming, having to repeated look back at your
spreadsheet to figure out your variable names is beyond annoying.  It is beyond
annoying because it interrupts your programming.  Programming is hard enough,
try to minimize inconsistencies that can otherwise be settled by being
consistent.

**Use simple variable names**.  Consider two variables you might want
to name with multiple words, like miles per gallon and brain to body
weight ratio. It is easy to name one variable using camel case,
e.g. `MilesPerGallon`, and another capitalized,
e.g. `Brain(g)bodyWeight(KG)`.  The first name is fine, so long as you
are consistent and choose camel case for all of your variable names.
The second variable name is both not simple and inconsistent.  Camel
case would have you capitalize each new work, as in `BrainBodyWight`.
In this case, even the units are not capitalized the same.  This is a
recipe for frustration.  Also see below, *don't put units in variabl
names*.

It is recommended to make yourself a simple rule, like *prefer all
lowercase letters*.  Maybe that's not the rule for you, but don't get
caught up on the rule.  The rule itself doesn't matter.  Just be
simple and consistently so.

My go to rule is all lowercase letters, no numbers or special
characters other than `_`, and to separate words when there are
contiguous repeated letters, `ee` or `ss`, and otherwise don't
separate words.  The separator I prefer is underscore `_` instead of
space ` `, which is mostly a carry over rule from programming in R.
Remember, the rule matters less than consistency with the rule.

**Organize files within directories**.  When editing files, it is
tempting to write metadata into the file name.  For instance, it is
unfortunately common for people to write file names such as
`draft_manuscript.docx`, `draft2_manuscript.docx`,
`draft3_manuscript.docx`, `final_manuscript.docx`,
`final_final_manuscript.docx`.  File names are not intended to carry
the metadata associated with draft versions.

If you really need to maintain copies of drafts, and I guess you most
often do not need such copies, then you should create directories such as
`draft` and `final`.  Each directory should contain a (singular) copy
of the files you absolutely need with each and every copy of the file.
Any files, such as data, that are the same for all copies of the file
should have their own directory.  It might help future you to put a
separate notes file in each directory that reminds you of exact
purpose of the directory.

### DON'Ts

* don't start a variable name with a number
* don't use special character in variable names
* don't put units in variable names
* don't use abbreviations
* don't organize through file names
* don't put dates in your file names
* don't have multiple copies of your data

**Don't start a variable name with a number**.  In most programming
languages, you can't start a variable name with a number.  So it's
easiest to just avoid putting numbers in variable names altogether.
Occassionaly, it makes sense to use a number in a variable name.  Just
don't start your variable name with a number.

**Don't use special characters in variable names**.  This rule is much
like the rule above.  In my experience, special characters,
e.g. `~!@#$%^&*()+=,<>/|\`, only make remembering a variable name more
difficult.  The only special character that you should allow, when
necessary, in your variable names is underscore `_`.  See **Use simple
variable names** above.

**Don't put units in variable names**.  Units in variable names just
open the door for inconsistent variable names.  It is easiest to just
avoid putting units or other metadata into variable names.  Your data
should instead have a separate file of all the associated metadata.

**Don't use abbreviations**.  Abbreviated variable names are
attractive, because they save typing.  For instance, one could imagine
abbreviating micrograms as `ug`, `mg`, or `μg`.  This creates
opportunity for misremembering and inconsistency.  Such abbreviations
in variable names also breaks the rule **Don't put units in variable
names**.  Further, see **Use simple variable names above** above.
Instead, put such metadata in a separate file.

**Don't organize through file names**.  The only metadata a file name
should contain is the name of the file.  Instead, use directories to
organize your files.  See **Organize files within directories** above.

**Don't put dates in your file names**.  Dates are metadata, see
**Don't organize through file names** above.

**Don't have multiple copies of your data**.  Generally, you should
only have one copy of your dataset.  See **Don't put dates in your
file names** above.  If there are necessary edits to your data for a
specific analysis, then you should program those edits in Python code
and save that code for future re-use.  This way you can re-create data
changes as necessary, and you minimize introducing permanent errors
into your dataset.

### tidy data

The most complete reference containing the advice above, and more, is
from Hadley Wickham's paper <a
href="https://vita.had.co.nz/papers/tidy-data.pdf"
target="_blank">Tidy Data (pdf)</a>.  The paper lays out a framework
with the goal of making it easier to clean up (tidy) data, so that
subsequent analysis is easier.
