Data Cleansing
================
Calvin Sibarani, edited by Yurham Afif
5/9/2021

``` r
# Load the necessary packages
library(readr)
```

    ## Warning: package 'readr' was built under R version 4.0.5

``` r
# Import CSV for prices
(airbnb_price <- read_csv('airbnb_price.csv'))
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   listing_id = col_double(),
    ##   price = col_character(),
    ##   nbhood_full = col_character()
    ## )

    ## # A tibble: 25,209 x 3
    ##    listing_id price       nbhood_full               
    ##         <dbl> <chr>       <chr>                     
    ##  1       2595 225 dollars Manhattan, Midtown        
    ##  2       3831 89 dollars  Brooklyn, Clinton Hill    
    ##  3       5099 200 dollars Manhattan, Murray Hill    
    ##  4       5178 79 dollars  Manhattan, Hell's Kitchen 
    ##  5       5238 150 dollars Manhattan, Chinatown      
    ##  6       5295 135 dollars Manhattan, Upper West Side
    ##  7       5441 85 dollars  Manhattan, Hell's Kitchen 
    ##  8       5803 89 dollars  Brooklyn, South Slope     
    ##  9       6021 85 dollars  Manhattan, Upper West Side
    ## 10       6848 140 dollars Brooklyn, Williamsburg    
    ## # ... with 25,199 more rows

``` r
# What is the average price of total listings?

# identify the column data type
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.0.5

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.3     v dplyr   1.0.5
    ## v tibble  3.1.0     v stringr 1.4.0
    ## v tidyr   1.1.3     v forcats 0.5.1
    ## v purrr   0.3.4

    ## Warning: package 'tidyr' was built under R version 4.0.5

    ## Warning: package 'dplyr' was built under R version 4.0.5

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
glimpse(airbnb_price)
```

    ## Rows: 25,209
    ## Columns: 3
    ## $ listing_id  <dbl> 2595, 3831, 5099, 5178, 5238, 5295, 5441, 5803, 6021, 6848~
    ## $ price       <chr> "225 dollars", "89 dollars", "200 dollars", "79 dollars", ~
    ## $ nbhood_full <chr> "Manhattan, Midtown", "Brooklyn, Clinton Hill", "Manhattan~

``` r
is.numeric(airbnb_price$price)
```

    ## [1] FALSE

``` r
# create a new column which price are in numeric
airbnb_price <- airbnb_price %>% 
  mutate(price_dollars = as.numeric(str_remove(airbnb_price$price, " dollars")))
airbnb_price
```

    ## # A tibble: 25,209 x 4
    ##    listing_id price       nbhood_full                price_dollars
    ##         <dbl> <chr>       <chr>                              <dbl>
    ##  1       2595 225 dollars Manhattan, Midtown                   225
    ##  2       3831 89 dollars  Brooklyn, Clinton Hill                89
    ##  3       5099 200 dollars Manhattan, Murray Hill               200
    ##  4       5178 79 dollars  Manhattan, Hell's Kitchen             79
    ##  5       5238 150 dollars Manhattan, Chinatown                 150
    ##  6       5295 135 dollars Manhattan, Upper West Side           135
    ##  7       5441 85 dollars  Manhattan, Hell's Kitchen             85
    ##  8       5803 89 dollars  Brooklyn, South Slope                 89
    ##  9       6021 85 dollars  Manhattan, Upper West Side            85
    ## 10       6848 140 dollars Brooklyn, Williamsburg               140
    ## # ... with 25,199 more rows

``` r
# the average price of total listings? 
mean(airbnb_price$price_dollars)
```

    ## [1] 141.7779
