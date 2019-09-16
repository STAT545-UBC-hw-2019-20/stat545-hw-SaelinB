HW01\_Gapminder
================

``` r
library(gapminder)
library(tibble)
library(DT)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

From the package gapminder, there is a built-in tibble called gapminder,
which I’ll be exploring here using dyplr

First, lets see how many countries are in this tibble:

``` r
Count <- (count(gapminder,country))
print(nrow(Count))
```

    ## [1] 142

Another way you could count the countries:

``` r
countries <- gapminder$country
countries <- (unique(countries))
print(length(countries))
```

    ## [1] 142

What is the average life expectancy in Canada throughout all years
recorded?

``` r
life_expect <- select(gapminder, country, lifeExp)
life_expect <- filter(gapminder, country=='Canada', !is.na(lifeExp))
average <- mean(life_expect$lifeExp)
  print(mean(average))
```

    ## [1] 74.90275

Which country has the highest population in 2002, and what was the
population?

``` r
population <- select(gapminder, year, country, pop)
population <- filter(population,year==2002)
population <- arrange(population, desc(pop))
Country <- population$country
Country <- Country[1]
max_pop <- population$pop
max_pop <- max_pop[1]
print(Country)
```

    ## [1] China
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

``` r
print(max_pop)
```

    ## [1] 1280400000
