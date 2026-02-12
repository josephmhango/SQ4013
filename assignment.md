---
title: "R Problem Set: Introductory Statistics and Data Handling"
author: "Joseph Mhango"
format:
  html:
    toc: true
execute:
  echo: true
  warning: false
  message: false
---

# Instructions

- Use **base R** unless a question explicitly allows another package.
- Write clear, commented code.
- Where explanations are requested, use full sentences.

---

# Part A: Basic Objects and Indexing

## 1. Constructing vectors

Create the following vectors:

```r
temperatures <- c(12.4, 14.1, 13.8, 15.6, 14.9)
sites <- c("north", "south", "east", "west", "north")
flagged <- c(FALSE, TRUE, FALSE, FALSE, TRUE)
```

**Tasks:**

1. Print each vector.
2. Use `class()` to identify its type.
3. Report the length of each vector.

---

## 2. Extracting elements

Using the `temperatures` vector:

**Tasks:**

1. Extract the third value.
2. Extract the final value.
3. Extract the first three values.
4. Extract all values above 14.

---

## 3. Creating a data frame

Create a data frame called `plots`:

```r
plots <- data.frame(
  plot_id = 101:105,
  yield = c(3.2, 4.1, 3.8, 5.0, 4.3),
  irrigation = c("low", "high", "low", "high", "low")
)
```

**Tasks:**

1. Display the data frame.
2. Inspect it using `str()`.
3. Extract:
   - The `yield` column
   - Rows 2 to 4
   - The value in row 5, column 2

---

## 4. Logical filtering

Using `plots`:

**Tasks:**

1. Select rows where irrigation is `"low"`.
2. Select rows where yield is greater than 4.
3. Count how many rows meet each condition.

---

## 5. Grouped means (base R)

Using the `plots` data:

**Tasks:**

1. Compute the mean yield for each irrigation level.
2. Report the results.

---

# Part B: Data Types and Conversion

## 6. Turning text into factors

Using the `plots` data:

**Tasks:**

1. Convert `irrigation` into a factor.
2. Confirm using `str()`.
3. List the levels of the factor.

---

## 7. Character numbers

Create:

```r
rainfall <- c("45", "52", "38", "60")
```

**Tasks:**

1. Check the class.
2. Attempt to calculate the mean.
3. Convert to numeric.
4. Recalculate the mean.

---

## 8. Mixed input vector

Create:

```r
values <- c(8, 10, "12", 15)
```

**Tasks:**

1. Check the class.
2. Explain why this class was assigned.
3. Convert to numeric.
4. Compute the mean.

---

## 9. Conversion warnings

Create:

```r
readings <- c("5.2", "4.8", "error", "6.1")
```

**Tasks:**

1. Convert to numeric.
2. Observe and explain the warning.
3. Count how many missing values appear.

---

# Part C: Cleaning and Reshaping

## 10. Cleaning messy variable names

You are given:

```r
names_vec <- c(" Soil Temp ", "Air(%)", "pH-level",
               "3rd_sample", "device.ID")
```

**Tasks:**

Using only base R:

1. Trim spaces.
2. Convert to lowercase.
3. Replace punctuation with `_`.
4. Collapse multiple `_`.
5. Ensure names do not start with a number.
6. Save result as `clean_names`.

Explain your steps briefly.

---

## 11. Reshaping measurements (wide → long, base R)

```r
set.seed(7)
df_measure <- data.frame(
  sample = 1:4,
  reading_A = rnorm(4, 20, 2),
  reading_B = rnorm(4, 22, 2),
  reading_C = rnorm(4, 24, 2)
)
```

**Tasks:**

1. Convert to long format using base R only.
2. Final columns should be:
   - `sample`
   - `reading_type`
   - `value`

---

## 12. Summaries from long data

Using the long data from Question 11:

**Tasks:**

1. Compute the mean value per reading type.
2. Report the results.
3. Explain why long format is often better for plotting.

---

## 13. Long to wide (tidyverse allowed)

Using the long dataset:

**Tasks:**

1. Convert it back to wide format using `pivot_wider()`.
2. Confirm it matches the original structure.

---

# Part D: Practical Data Handling

## 14. Reproducible randomness

```r
set.seed(99)
scores <- rnorm(15, mean = 60, sd = 5)
scores[sample(1:15, 3)] <- NA
```

**Tasks:**

1. Explain what `set.seed(99)` does.
2. Replace missing values with the median.
3. Show that `mean(scores)` now returns a number.
4. State one drawback of this approach.

---

## 15. Writing a summary function

Write a function called `quick_stats()` that:

- Accepts a data frame
- Accepts a character vector of column names
- Checks that columns are numeric
- Returns a data frame with:
  - n
  - mean
  - sd

Test it on `mtcars` using:

```r
c("mpg", "disp", "wt")
```

Explain why `[[` is safer than `$` inside functions.

---

## 16. Working with a list of datasets

Create:

```r
a <- mtcars[1:8, c("mpg", "wt")]
b <- mtcars[9:16, c("mpg", "wt")]
c <- mtcars[17:32, c("mpg", "wt")]
lst <- list(a, b, c)
```

**Tasks:**

1. Use `lapply()` to compute the mean `mpg` in each data frame.
2. Combine the results into one numeric vector.
3. Compute the overall mean across all rows.
4. Explain what `do.call(rbind, ...)` does.

---

# Part E: Merging, Structures, and Modelling

## 17. Correcting a faulty join and plot

You are given:

```r
df_main <- mtcars
df_groups <- data.frame(
  car_id = rownames(mtcars),
  power_band = ifelse(mtcars$hp > 150, "high", "low"),
  stringsAsFactors = FALSE
)

joined <- merge(df_main, df_groups,
                by.x = "row.names",
                by.y = "vehicle")   # incorrect key
```

**Tasks:**

1. Correct the merge so rows match by car name.
2. Plot `hp` vs `mpg`, coloured by power band.
3. Add a legend with two clear labels.
4. Explain how `by.x` and `by.y` control the join.

---

## 18. Matrix versus data frame

```r
mat <- matrix(2:13, nrow = 3, byrow = TRUE)
colnames(mat) <- c("X1", "X2", "X3", "X4")
```

**Tasks:**

1. Convert the matrix into a data frame.
2. Add a factor column with levels `"small"` and `"large"`.
3. Show how arithmetic behaves differently.
4. Explain why matrices must be homogeneous.

---

## 19. Changing the reference level

Using the `iris` dataset:

**Tasks:**

1. Set `"versicolor"` as the reference species.
2. Fit the model:

```r
Sepal.Width ~ Species
```

3. Show the coefficient table.
4. Explain how the interpretation changes.

---

## 20. One-sample t-test

```r
set.seed(5)
weights <- rnorm(30, mean = 102, sd = 8)
```

**Tasks:**

1. Test whether the mean weight differs from 100.
2. Report:
   - t-statistic
   - p-value
3. State the decision at α = 0.05.
4. Explain why a t-test is used instead of a Z-test.

