Data Manipulation
================
Calvin Sibarani, edited by Yurham Afif
4/20/2021

## Data Manipulation Homework

Stack Overflow dataset – Solved by Afif

Goal 1 : Hitung berapa banyak tiap question\_id muncul pada dataset
`answers.csv`. Join hasil tersebut ke kolom dataset `questions.csv`

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.0.5

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.3     v purrr   0.3.4
    ## v tibble  3.1.0     v dplyr   1.0.5
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   1.4.0     v forcats 0.5.1

    ## Warning: package 'tidyr' was built under R version 4.0.5

    ## Warning: package 'readr' was built under R version 4.0.5

    ## Warning: package 'dplyr' was built under R version 4.0.5

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
answer <- read_csv("answers.csv")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   id = col_double(),
    ##   creation_date = col_date(format = ""),
    ##   question_id = col_double(),
    ##   score = col_double()
    ## )

``` r
question <- read_csv("questions.csv")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   id = col_double(),
    ##   creation_date = col_date(format = ""),
    ##   score = col_double()
    ## )

``` r
answer
```

    ## # A tibble: 380,643 x 4
    ##          id creation_date question_id score
    ##       <dbl> <date>              <dbl> <dbl>
    ##  1 39143713 2016-08-25       39143518     3
    ##  2 39143869 2016-08-25       39143518     1
    ##  3 39143935 2016-08-25       39142481     0
    ##  4 39144014 2016-08-25       39024390     0
    ##  5 39144252 2016-08-25       39096741     6
    ##  6 39144375 2016-08-25       39143885     5
    ##  7 39144430 2016-08-25       39144077     0
    ##  8 39144625 2016-08-25       39142728     1
    ##  9 39144794 2016-08-25       39043648     0
    ## 10 39145033 2016-08-25       39133170     1
    ## # ... with 380,633 more rows

``` r
question <- answer %>% 
  count(question_id) %>% 
  inner_join(question, by = c("question_id" = "id"))
question
```

    ## # A tibble: 243,930 x 4
    ##    question_id     n creation_date score
    ##          <dbl> <int> <date>        <dbl>
    ##  1       77434    11 2008-09-16      255
    ##  2       79709     7 2008-09-17        5
    ##  3       95007     2 2008-09-18       63
    ##  4      102056    21 2008-09-19      127
    ##  5      103312     2 2008-09-19        5
    ##  6      127137    15 2008-09-24       24
    ##  7      255697     1 2008-11-01        4
    ##  8      359438     7 2008-12-11        8
    ##  9      420296    12 2009-01-07       25
    ## 10      439526     2 2009-01-13       25
    ## # ... with 243,920 more rows

Goal 2 : Dari hasil nomor 1, join ke dataset `questions_tags.csv` dan ke
dataset `tags.csv`

``` r
questions_tags <- read_csv("question_tags.csv")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   question_id = col_double(),
    ##   tag_id = col_double()
    ## )

``` r
tags <- read_csv("tags.csv")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   id = col_double(),
    ##   tag_name = col_character()
    ## )

``` r
tags <- question %>% 
  inner_join(questions_tags, by = "question_id") %>% 
  inner_join(tags, by = c("tag_id" = "id"))
tags
```

    ## # A tibble: 407,165 x 6
    ##    question_id     n creation_date score tag_id tag_name        
    ##          <dbl> <int> <date>        <dbl>  <dbl> <chr>           
    ##  1       77434    11 2008-09-16      255  46457 dataframe       
    ##  2       77434    11 2008-09-16      255   5852 vector          
    ##  3       79709     7 2008-09-17        5     61 memory          
    ##  4       79709     7 2008-09-17        5   5569 function        
    ##  5       79709     7 2008-09-17        5   5992 global-variables
    ##  6       79709     7 2008-09-17        5   5991 side-effects    
    ##  7       95007     2 2008-09-18       63    259 math            
    ##  8       95007     2 2008-09-18       63    402 statistics      
    ##  9      102056    21 2008-09-19      127    816 search          
    ## 10      102056    21 2008-09-19      127  69142 r-faq           
    ## # ... with 407,155 more rows

Goal 3 : Dari hasil nomor 2, agregasi berdasarkan kolom `tag_name` untuk
mendapatkan berapa kali pertanyaan muncul. Urutkan hasil akhir dari
`tag_name` yang paling sering muncul.

``` r
tags %>% 
  count(tag_name, sort = TRUE)
```

    ## # A tibble: 7,110 x 2
    ##    tag_name       n
    ##    <chr>      <int>
    ##  1 ggplot2    23647
    ##  2 dataframe  17245
    ##  3 dplyr      12684
    ##  4 shiny      10675
    ##  5 plot        9613
    ##  6 data.table  7663
    ##  7 matrix      5519
    ##  8 regex       4664
    ##  9 loops       4538
    ## 10 for-loop    4216
    ## # ... with 7,100 more rows
