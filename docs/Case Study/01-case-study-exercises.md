Day 1 Case Study Exercises
================

# Prerequisites

All of the exercises can be solved using the `tidyverse` and
`completejourney` packages. The `completejourney` package is an R data
package that has been created so the full suite of Complete Journey
datasets can be loaded as a library. In order to use the data you must
first install the package following these steps:

``` r
devtools::install_github('bradleyboehmke/completejourney')
```

**NOTE**: Installing packages from GitHub requires the installation of
the devtools package, which can be installed by running the following
command from the R console: `install.packages('devtools')`.

Go ahead and load the `tidyverse` and `completejourney` packages:

``` r
library(_______)
library(_______)
```

The exercises that follow will use various data sets included in the
`completejourney` package to include:

``` r
transactions
products
```

# Data Transformation

The following five questions are based on concepts covered in the data
transformation (`dplyr`) slides. Answer them using `transactions` in the
Complete Journey data package.

``` r
transactions <- transactions %>% 
  select(
    quantity,
    sales_value, 
    retail_disc, coupon_disc, coupon_match_disc,
    household_id, store_id, basket_id, product_id, 
    week, transaction_timestamp
  ) %>%
  mutate(date = lubridate::as_date(transaction_timestamp))
```

-----

**Question 1**: Change the discount variables (i.e., `retail_disc`,
`coupon_disc`, `coupon_match_disc`) from negative to positive.

**Hint:** Use the `abs()` function within `mutate()`.

This question is designed to strengthen your ability to use the `dplyr`
verb `mutate()` to overwrite an existing variable.

-----

**Question 2**: Create three new variables named `regular_price`,
`loyalty_price`, and `coupon_price` according to the following
logic:

``` r
regular_price = (sales_value + retail_disc + coupon_match_disc) / quantity
loyalty_price = (sales_value + coupon_match_disc) / quantity
coupon_price  = (sales_value - coupon_disc) / quantity
```

This question is designed to strengthen your ability to use the `dplyr`
verb `mutate()` to create new variables from existing ones. It should
also help you develop a better understanding of the discount variables
in `transactions`.

-----

**Question 3**: `transactions` includes 68,509 unique product IDs. How
many of these products (not transactions\!) had a regular price of one
dollar or less? What does this count equal when loyalty price is one
dollar or less? How about when coupon price is one dollar or less?

**Hint:** After filtering, select the `product_id` column and count
unique products using the `n_distinct()` function.

This question is designed to strengthen your ability to use the `dplyr`
verbs `filter()` and `select()`.

-----

**Question 4**: What proportion of baskets are over $10 in sales value?

**Hint:** You need to use `group_by()` and `summarize()`. Depending on
your approach you may or may not use `mutate()`.

This question is designed to strengthen your ability to use the `dplyr`
verbs `group_by()`, `summarize()` and `ungroup()`.

-----

**Question 5**: Which store with over $10K in total `sales_value`,
discounts its products the most for loyal customers?

**Hint:** You can calculate loyalty discount as a percentage of regular
price using the following logic:

``` r
pct_loyalty_disc = 1 - (loyalty_price / regular_price)
```

This question is designed to strengthen your ability to use the `dplyr`
verbs `filter()`, `mutate()`, `group_by()`, `summarize()`, and
`arrange()`.

-----

# Data Visualization

The following five questions are based on concepts covered in the data
visualization (`ggplot2`) slides. They can be answered using the
`transactions` and `products` datasets from the `completejourney`
package.

-----

**Question 1**: Create a histogram of the `quantity` variable in the
`transactions` data. What, if anything, do you find unusual about this
visualization?

This question is designed to strengthen your ability to use
`geom_histogram()`.

-----

**Question 2**: Use a line graph to plot total sales value by `date`.
What kind of patterns do you find?

This question is designed to strengthen your ability to use `dplyr`
verbs in combination with `geom_line()`.

-----

**Question 3**: Use a bar graph to compare the total sales values of
national and private-label brands.

**Hint**: Because `transactions` does not contain product metadata, run
the code below to create a new dataset with additional product
information in it. Use `my_transaction_data` for your
answer.

``` r
my_transaction_data <- left_join(transactions, products, by = 'product_id')
```

This question is designed to strengthen your ability to use `dplyr`
verbs in combination with `geom_col()`.

-----

**Question 4**: Building on Question 3, suppose you want to understand
whether the retailer’s customers’ preference for national brands
(compared to private-label brands) is stronger in the soft drink
category than it is in the cheese category. Examine this supposition by
using a stacked bar graph to compare the split between national and
private-label brands for soft drinks and cheeses.

**Hint**: Follow these three steps to create your plot:

  - Filter `my_transaction_data` to include only transactions with
    `product_category` equal to “SOFT DRINKS” or “CHEESE”
  - Calculate total sales value by `product_category` and `brand`
  - Create the bars using `geom_col` and include `fill = brand` within
    `aes()`

-----

**Question 5**: The code below filters `my_transaction_data` to include
only peanut better, jelly, and jam transactions. Then it creates a new
variable named `package_size` equal to product size in ounces. Create a
bar graph with `pb_and_j_data` to visualize the distribution of the
retailer’s PB\&J transactions by product size. Which two product sizes
are the most popular?

``` r
pb_and_j_data <- my_transaction_data %>% 
  filter(commodity_desc == 'PNT BTR/JELLY/JAMS') %>%
  mutate(
    product_size = as.factor(as.integer(gsub('([0-9]+)([[:space:]]*OZ)', '\\1',
                                             curr_size_of_product)))
  )
```
