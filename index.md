# How to Find and Handle Missing Values in R

## Introduction

Real datasets are rarely perfect. Sometimes information is missing because a person did not answer a question, a sensor failed, or data was not recorded correctly.

In R, missing values are usually shown as `NA`.

Missing values are important because they can affect calculations and analysis. For example, if a dataset has missing GPA values, the average GPA may not be calculated correctly unless we tell R how to handle the missing data.

In this tutorial, I will show how to find, count, and remove missing values in R. I will use simple functions that are useful when cleaning a dataset before analysis.

## Create a Simple Dataset

First, we can create a small dataset about students.

```r
students <- data.frame(
  name = c("Anna", "Ben", "Chris", "David", "Emma"),
  GPA = c(3.8, 3.5, NA, 3.9, 3.6),
  study_hours = c(5, NA, 4, 6, 3),
  sleep_hours = c(7, 6, 8, NA, 7)
)

students
```

This dataset has five students and three variables related to school and daily life.

Some values are missing. For example, Chris does not have a GPA value, Ben does not have a study hours value, and David does not have a sleep hours value.

Before doing an analysis, it is useful to check where the missing values are.

## Find Missing Values with is.na()

One simple function for finding missing values is `is.na()`.

```r
is.na(students)
```

This returns `TRUE` when a value is missing and `FALSE` when a value is available.

This can be useful for a small dataset, but it becomes difficult to read when the dataset has many rows and columns.

We can also check a single variable.

```r
is.na(students$GPA)
```

This shows which students have a missing GPA value.

Another useful method is to check if there are any missing values in the entire dataset.

```r
any(is.na(students))
```

If the result is `TRUE`, the dataset contains at least one missing value.

## Count Missing Values

Instead of looking at every row, we can count the number of missing values in each column.

```r
colSums(is.na(students))
```

The result tells us how many `NA` values appear in each variable.

For this dataset, `name` has no missing values, while GPA, study hours, and sleep hours each have one missing value.

This is especially useful when working with a larger dataset because it quickly shows which variables may need cleaning.

We can also count the total number of missing values in the dataset.

```r
sum(is.na(students))
```

This gives us one number representing all missing values.

## Remove Missing Values

Sometimes we may want to remove rows that contain missing values.

One option is `drop_na()` from the `tidyr` package.

```r
library(tidyverse)

students_clean <- students %>%
  drop_na()

students_clean
```

This removes every row that has at least one missing value.

However, this may remove more data than we want.

For example, imagine that we only care about GPA for our analysis. In that case, we can remove only the rows where GPA is missing.

```r
students_gpa <- students %>%
  drop_na(GPA)

students_gpa
```

This keeps students who have missing values in other variables as long as their GPA is available.

We can also do the same thing with `filter()`.

```r
students_gpa <- students %>%
  filter(!is.na(GPA))
```

The `!` means "not," so this code keeps rows where GPA is not missing.

## Should We Always Remove Missing Values?

No. Removing missing values is not always the best choice.

If only a few observations are missing, removing them may not have a large effect. However, if many observations are missing, deleting them can reduce the amount of useful data.

It can also create bias.

For example, imagine a survey about student study hours. If students who study very little are more likely to skip the question, removing those missing observations could make the average study time look higher than it really is.

Because of this, we should first understand how much data is missing and why it may be missing.

A good first step is to check the number of missing values before deciding what to do.

```r
colSums(is.na(students))
```

Then we can decide whether removing the rows makes sense for the analysis.

## Compare Before and After Cleaning

It is also useful to compare the size of the dataset before and after removing missing values.

```r
nrow(students)
nrow(students_clean)
```

The original dataset has five rows.

After using `drop_na()`, only the rows with complete information remain.

This comparison helps us understand how much data we are losing during the cleaning process.

If too many rows disappear, we may need to use another method instead of simply removing all missing values.

## Conclusion

Missing values are common in real data, so knowing how to handle them is an important data science skill.

In R, `is.na()` can help us find missing values, `colSums()` can count them, and `drop_na()` or `filter()` can remove observations when needed.

The important part is not just removing every `NA`. We should first check how much data is missing and think about why it is missing.

As a next step, try these functions on a dataset you have used in STAT 386. Check which columns contain missing values and compare the number of rows before and after cleaning the data.
