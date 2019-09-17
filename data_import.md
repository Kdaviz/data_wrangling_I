Data import
================
Keyanna Davis
9/17/2019

Load in a dataset
-----------------

``` r
litters_data = read.csv(file = "./FAS_litters.csv")
litters_data = janitor:: clean_names(litters_data)
```
