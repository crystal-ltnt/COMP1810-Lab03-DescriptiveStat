# COMP1810 Lab 03 — Descriptive Statistics

Practical lab for **COMP1810: Data Analytics**, focused on R programming fundamentals: importing data, working with packages, data manipulation with **dplyr**, and **descriptive statistical analysis**.

Full step-by-step instructions are in:

`Lecture3 Tutorial Descriptive statistics for Data Analytics and R programming.docx`

------------------------------------------------------------------------

## Learning objectives

By the end of this lab you should be able to:

- Set up R and RStudio (or another R IDE) for data analysis
- Import datasets into R from CSV and Excel files
- Install packages, load libraries, and explore built-in datasets
- Use **dplyr** to filter, select, and transform data
- Compute descriptive statistics and create summary visualisations

------------------------------------------------------------------------

## Prerequisites

Install one of the following before starting:

- [R](https://cran.r-project.org/) and [RStudio](https://posit.co/download/rstudio-desktop/)

Many steps in the tutorial expect you to look up concepts online (e.g. how to import Excel files in R, how to install **dplyr**).

------------------------------------------------------------------------

## Project structure

```         
COMP1810-Lab03-DescriptiveStat/
├── Data/
│   ├── data.csv       # Numeric dataset (columns: x, x2, x3, y)
│   ├── hflights.xlsx  # Flight data (Excel)
│   └── mpg.xlsx       # Vehicle fuel economy data (Excel)
├── main.Rmd           # R Notebook for your lab work
├── Lecture3 Tutorial Descriptive statistics for Data Analytics and R programming.docx
└── README.md
```

Open `main.Rmd` in RStudio to write and run your code. Execute chunks with **Cmd+Shift+Enter** (macOS) or **Ctrl+Shift+Enter** (Windows/Linux).

------------------------------------------------------------------------

## Getting started

1.  Clone or download this repository.
2.  Open the project folder in RStudio.
3.  Set your working directory to the project root (adjust the path for your machine):

``` r
setwd("/path/to/COMP1810-Lab03-DescriptiveStat")
```

> **Note:** `setwd()` may not work on some online/cloud environments. Use RStudio’s **Session → Set Working Directory → To Source File Location** instead.

------------------------------------------------------------------------

## Task 1 — Import datasets into R

Download and import the three datasets provided for this lab: **data**, **hflights**, and **mpg** (already included in `Data/`).

### CSV

``` r
data <- read.csv(file = "Data/data.csv", header = TRUE, sep = ",")
data
```

### Excel

Reading `.xlsx` files requires an additional package such as **readxl** or **openxlsx**:

``` r
install.packages("readxl")
library(readxl)

hflights <- read_excel("Data/hflights.xlsx", sheet = 1)
mpg      <- read_excel("Data/mpg.xlsx", sheet = 1)
```

### Select columns

Select specific columns (e.g. `x` and `x3`) using base R or **dplyr**:

``` r
df <- data[, c("x", "x3")]

# Or with dplyr:
library(dplyr)
df <- select(data, x, x3)
```

------------------------------------------------------------------------

## Task 2 — Install and load packages

### Install packages

``` r
install.packages("dplyr")
install.packages("tidyverse")   # Meta-package for data analytics
install.packages("vioplot")
install.packages("babynames")
install.packages(c("vioplot", "MASS"))
```

### Load packages

Installing a package is not enough — you must load it before use:

``` r
library(dplyr)
library(babynames)
```

### List functions in a loaded package

``` r
ls("package:babynames")
ls("package:dplyr")
```

### Remove a package

``` r
remove.packages("vioplot")
```

> **Tip:** **dplyr** (part of **tidyverse**) can overwrite some base R functions. If you need the original behaviour, be aware of namespace conflicts.

------------------------------------------------------------------------

## Accessing datasets

| Source                | How to access                                 |
|-----------------------|-----------------------------------------------|
| Course materials      | Files in `Data/`                              |
| Built-in R datasets   | `data()`                                      |
| Datasets in a package | `data(package = "ggplot2")`                   |
| UCI ML Repository     | <https://archive.ics.uci.edu/ml/datasets.php> |
| Kaggle                | <https://www.kaggle.com/datasets>             |

### Example: flights data with nycflights13

``` r
install.packages("nycflights13")
library(nycflights13)

filter(flights, month == 1, day == 1)
dec25 <- filter(flights, month == 12, day == 25)
```

### Getting help

``` r
?mean
help(datasets)
```

------------------------------------------------------------------------

## dplyr — data manipulation

The core **dplyr** verbs (remember **filter → arrange → select → mutate → summarize**):

| Function      | Purpose                       |
|---------------|-------------------------------|
| `filter()`    | Keep rows matching conditions |
| `arrange()`   | Sort rows                     |
| `select()`    | Choose columns                |
| `mutate()`    | Create new variables          |
| `summarize()` | Aggregate to summary values   |

### Comparison operators

`>`, `>=`, `<`, `<=`, `!=`, `==`

### Filter examples

``` r
# Flights in November or December
filter(flights, month == 11 | month == 12)
filter(flights, month %in% c(11, 12))

# Flights not delayed by more than 120 minutes (arrival or departure)
filter(flights, !(arr_delay > 120 | dep_delay > 120))
filter(flights, arr_delay <= 120, dep_delay <= 120)
```

------------------------------------------------------------------------

## Descriptive analysis in R

Work with columns from `data` (e.g. `x` and `x3`):

``` r
df <- data[, c("x", "x3")]
x  <- df$x
x3 <- data$x3
```

### Measures of central tendency

``` r
# Mean (remove NA values)
xmean <- mean(x, na.rm = TRUE)

# Median
xmedian <- median(x, na.rm = FALSE)

# Mode — R has no built-in mode function
getmode <- function(v) {
  uniqv <- unique(v)
  uniqv[which.max(tabulate(match(v, uniqv)))]
}
xmode <- getmode(x)
```

### Measures of spread

``` r
var(x3)              # Sample variance
sd(x3)               # Standard deviation
summary(x3)          # Five-number summary + mean
max(x3) - min(x3)    # Range
IQR(x3)              # Interquartile range
```

### Visualisation

``` r
boxplot(x3, horizontal = TRUE)
```

------------------------------------------------------------------------

## Lab workflow

```         
Setup R/RStudio
      ↓
Import data (CSV & Excel)
      ↓
Install & load packages (dplyr, tidyverse, …)
      ↓
Explore & manipulate data (dplyr)
      ↓
Descriptive statistics & box plots
```

Record your work and outputs in `main.Rmd`.

------------------------------------------------------------------------

## Resources

- [R Documentation](https://www.rdocumentation.org/)
- [dplyr cheat sheet](https://posit.co/learn/cheatsheets/)
- [Project Jupyter — Try Jupyter](https://jupyter.org/try)
- Tutorial document: `Lecture3 Tutorial Descriptive statistics for Data Analytics and R programming.docx`

------------------------------------------------------------------------

## Course

**COMP1810** — Data Analytics\
Lab 03: Descriptive Statistics for Data Analytics and R Programming
