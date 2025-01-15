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

(week-05)=
# Week 05: Puting it all together

Time to put everything you learned in this class into action.  You
will prepare an Exploratory Data Analysis Project.  In an exploratory
data analysis project, you are just looking at (hence exploring) the
data and learning about the data and the possible relationships
between variables.

This is not meant to be a formal statistical analysis.  You cannot
make any claims about groups being statistically different or causally
related.  This is just an excuse to practice what you learned by
describing a dataset of your choice.  You are allowed and encouraged
to hypothesize why you observe certain relationships between variables
in your chosen dataset.  Just try to resist making causal claims from
what is most likely an observational dataset.

## Instructions

Using your data set of choice, pose a brief research question that
explores the relationship between 2 - 3 variables. Use Markdown
headers to make the following sections.

1. Introduction: A short introduction/description of the data.

    * Specifically mention the 2-3 variables you are going to explore.

    * What is your research question? What are you interested in
      finding out more about?

2. Univariate Exploration: Describe each of the variables under consideration.

    * This means calculate some summary statistics (N(%) or mean(sd))
      and make a graphic

3. Bivariate Exploration: Comparison between two variables of interest.

    * Calculate grouped summary statistics as appropriate. This is
      often the most often forgotten part

    * You can go further and explore more than two variables at a time
      using paneling, but be sure to explain what you learn from each
      graph.

4. Conclusion: What did you find? If you had a prior hypothesis, does
   the data seem to support it? Remember this is NOT a statistical
   analysis.

All descriptions (univariate and bivariate) must be done using
graphics, summary statistics, and sentences.

This is a very vague set of instructions for a reason. You are to
choose variables and explore anything that you find interesting.
Create tables, graphics, grouped summary statistics (mean of the
continuous variable across levels of the categorical variable).
Whatever you need to do to understand the relationship between these
two measures.

Use the grading rubric at the bottom of this page for guidance as to
what you should present, in what order, and the level of detail you
need to present.

## Data

You have a choice here. If you are currently working on some data that
you would like to explore, talk with your instructor to get your data
set approved. As long as it has more than a few variables in it, and
at least 30 observations it should be fine.

If you do not have your own data, you can choose from one of the
following data sets, all of which can be downloaded from the Data page
of Dr. D’s teaching course website. Here are some viable choices:

* [Email Spam](https://www.norcalbiostat.com/data/#Email): Characteristics of emails used to predict if the email
  is spam or not.

* [HIV](https://www.norcalbiostat.com/data/#Parental_HIV): Data on adolescent children living with HIV positive parents.

* [Depression](https://www.norcalbiostat.com/data/#Depression): Level of depression (cesd), health care, and demographic
  characteristics.  High School and Beyond: Educational, vocational,
  and personal development of elementary and high school students.

* [Police Shootings](https://www.norcalbiostat.com/data/#Police_Shootings): Characteristics of individuals killed by police in
  2015.

Any other datasets, and/or any that we've already used in class
require instructor approval.

## How to submit

Each student is to create a Google Colab notebook and set the Share
permissions to General access Anyone with the link.

A Google spreadsheet will be created and shared with everyone.  Each
student will have their own row.  You are to paste the URL to your
Colab notebook in your row.

## Deadline

To allow for adequate time for peer grading the submission deadline is
a strict cutoff.  Late assignments won’t be accepted.  It is better to
submit something, than nothing.


## Guidelines

* Run all, early and often. To ensure all of the code you develop in
  your Colab notebook works as expected, it is highly recommended to
  click Runtime > Run all, essentially after you finish code for each
  next code cell.

* Proof read your report prior to submission.  Please read and re-read
  your report, editing for clarity and removing any duplicate content
  (sentences and/or code).

* Hide output of any code that is not necessary for your reader.  Put
  a semi-colon `;` on the last line of code within a code cell to
  reduce excessive printing.  Nobody needs to read through raw data
  afterall.

* This is to be independent work. Papers that are too similar will
  receive no credit.  Look at the grading rubric to help you decide
  the level of detail required.

## Grading rubric

The criteria below is what you will be graded on.

* **Data Description**. Provide a description of the data set and the
  variables of interest.

    * *Does not meet expectations*. There is no description or the
      description is a copy of the data metadata.

    * *Meets expectations*. There is a minor description of the data but not
      enough to understand what is being measured or compared.

    * *Exceeds expectations*. The data description is clear and
      concise and it is clear to me what data is being analyzed and
      where it was obtained.

* **Univariate Description**. Fully describe the distribution of each variable by itself.

    * *Does not meet expectations*. There are no numerical or graphical
      summaries provided.

    * *Meets expectations*. Only numeric or only graphical summaries were
      created, but no textual description.

    * *Exceeds expectations*. The variable was compared using
      appropriate numerical and/or graphical methods and short textual
      explanations immediately followed.

* **Bivariate Comparison**. Describe the relationship between the two
  chosen variables.

    * *Does not meet expectations*. No comparisons were made, or the
      comparisons were inappropriate/incorrect.

    * *Meets expectations*. The variables were compared using appropriate
      graphical methods and grouped summary statistics were created,
      but nothing was discussed.

    * *Exceeds expectations*. The variables were compared using
      appropriate numerical and/or graphical methods and short textual
      explanations immediately followed.

* **Organization / Grammar**. How well does the report read? How well
  organized was the report? Was it checked for grammar and spelling
  mistakes?

    * *Does not meet expectations*. Only Python code and output are
      present.  There is no discussion of results.  Lots of extraneous
      code that is not relevant to the discussion is present. Markdown
      was used inappropriately.

    * *Meets expectations*. An attempt was made to discuss the
      results, but the explanations are not in a report format or
      there are some large grammar and/or spelling problems. Some
      Python code that is not relevant to the analysis question at
      hand is being displayed. Markdown headers were used to create
      sections.

    * *Exceeds expectations*. The report was spell checked and written
      well, with full English sentences. The report flowed well and
      followed the required order of discussion topics with Markdown
      headers used successfully.
