
<!-- README.md is generated from README.Rmd. Please edit that file -->

# dsdataprep

<!-- badges: start -->
<!-- badges: end -->

<br>

## Overview

dsdataprep helps assisting data visualization packages by offering
functions to aggregate, summarise, sort and wrap data; adds color to the
data columns and adds rows information for displaying in interactive
plots

### Main functions:

- `data_aggregation()`

- `wrap_sort_data()`

- `prep_tooltip()`

- `add_data_colors()`

## Installation

Install the development version of makeup from GitHub with:

``` r
# install.packages("devtools")
remotes::install_github("datasketch/dsdataprep")
```

## Example

This is a basic example which shows you how this packages work:

Let´s load `dsdataprep` package

``` r
library(dsdataprep)
```

### Aggregate and summarie data with `aggregation_data()` function

`aggregation_data()` allows you to summaries information (e.g. count,
sum, mean) and gives you options to do it by a (or more) grouping
variable(s), adding custom names for the new columns, among other
options.

#### Counting values

``` r
# Load data
data <- ggplot2::diamonds

# Aggregate
data_result <- aggregation_data(data = data,
                                 agg = "count",
                                 to_agg = NULL,
                                 agg_name = "Conteo",
                                 group_var = c("cut", "color"))

data_result
#> # A tibble: 35 × 3
#> # Groups:   cut [5]
#>    cut   color Conteo
#>    <ord> <ord>  <int>
#>  1 Fair  D        163
#>  2 Fair  E        224
#>  3 Fair  F        312
#>  4 Fair  G        314
#>  5 Fair  H        303
#>  6 Fair  I        175
#>  7 Fair  J        119
#>  8 Good  D        662
#>  9 Good  E        933
#> 10 Good  F        909
#> # … with 25 more rows
```

#### Sum values by a grouping variable and add custom names for new columns

``` r
data_result <- aggregation_data(data = data,
                                agg = "sum",
                                to_agg = c("x", "y"),
                                agg_name = c("Sum x", "Sum y"),
                                group_var = c("cut"),
                                percentage = TRUE)

data_result
#> # A tibble: 5 × 5
#>   cut       `Sum x` `Sum y` `..percentageSum x` `..percentageSum y`
#>   <ord>       <dbl>   <dbl>               <dbl>               <dbl>
#> 1 Fair       10058.   9954.                3.25                3.22
#> 2 Good       28645.  28704.                9.27                9.28
#> 3 Very Good  69359.  69713.               22.4                22.5 
#> 4 Premium    82386.  81986.               26.7                26.5 
#> 5 Ideal     118691. 118963.               38.4                38.5
```

### Sort and wrap data by a categorical variable and numeric variable with `wrap_sort_data()`:

``` r
# Load data
data <- ggplot2::diamonds

# Checking
data[!duplicated(data$cut), "cut"]
#> # A tibble: 5 × 1
#>   cut      
#>   <ord>    
#> 1 Ideal    
#> 2 Premium  
#> 3 Good     
#> 4 Very Good
#> 5 Fair


# Sort data
data_result <- wrap_sort_data(data, 
                              col_cat = "cut", 
                              order = c("Good", "Ideal"))

# Checking
data_result[!duplicated(data_result$cut), "cut"]
#> # A tibble: 5 × 1
#>   cut      
#>   <ord>    
#> 1 Good     
#> 2 Ideal    
#> 3 Premium  
#> 4 Very Good
#> 5 Fair
```

### Generate a tooltip string for each row of a data frame or matrix with `prep_tooltip()`. Tooltips are typically used to display additional information about the data when the user hovers over a data point in a plot or table:

``` r
# Load data
data <- ggplot2::diamonds

# Add tooltip
v <- prep_tooltip(data,
                    format_num = "1345.78",
                    opts_format_num = list(si_prefix = TRUE),
                    format_cat = "UPPER",
                    tooltip = "Precio: {price} <br/> Tipo: {cut}")
```

Learn about the many ways to work with formatting dates values in
`vignette("intro-to-dsdataprep")`
