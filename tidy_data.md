Tidy Data
================
Keyanna Davis
9/24/2019

Wide to long
------------

``` r
pulse_data=
  haven::read_sas("./public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()

pivot_longer(
  pulse_data,
  bdi_score_bl :bdi_score_12m,
  names_to = "visit",
  names_prefix = "bdi_score",
  values_to = "bdi"
) %>% 
  mutate(
    visit = recode(visit, "bl" ="00m")
  )
```

    ## # A tibble: 4,348 x 5
    ##       id   age sex   visit   bdi
    ##    <dbl> <dbl> <chr> <chr> <dbl>
    ##  1 10003  48.0 male  _bl       7
    ##  2 10003  48.0 male  _01m      1
    ##  3 10003  48.0 male  _06m      2
    ##  4 10003  48.0 male  _12m      0
    ##  5 10015  72.5 male  _bl       6
    ##  6 10015  72.5 male  _01m     NA
    ##  7 10015  72.5 male  _06m     NA
    ##  8 10015  72.5 male  _12m     NA
    ##  9 10022  58.5 male  _bl      14
    ## 10 10022  58.5 male  _01m      3
    ## # … with 4,338 more rows

separate in litters
-------------------

``` r
litters_data = 
  read_csv("./FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  
  separate( col = group, into = c("dose", "day_of_tx"), 3)
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

go untidy
---------

``` r
analysis_result = tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)


pivot_wider(
  analysis_result,
  names_from = time,
  values_from = mean
)
```

    ## # A tibble: 2 x 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4
