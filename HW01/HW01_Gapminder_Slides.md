---
title: "HW01 Gapminder Slides"
author: Saelin Bjornson
date: Sept 17 2019
output: 
  ioslides_presentation:
    widescreen: true
    bigger: true
---
## First, we have to load our packages 
```{r load,warning=FALSE, echo=FALSE}
library(gapminder)
library(tibble)
library(DT)
library(dplyr)
```

## From the package gapminder, there is a built-in tibble called gapminder, which I'll be exploring here using dyplr 

## lets see how many countries are in this tibble: 
```{r}
Count <- (count(gapminder,country))
nrow(Count)
```



## Another way you could count the countries:
```{r}
countries <- gapminder$country
countries <- (unique(countries))
length(countries)
```


## What is the average life expectancy in Canada throughout all years recorded? 

```{r}
life_expect <- select(gapminder, country, lifeExp)
life_expect <- filter(gapminder, country=='Canada', !is.na(lifeExp))
average <- mean(life_expect$lifeExp)
mean(average)
```

## Which country has the highest population in 2002, and what was the population?

```{r}
population <- select(gapminder, year, country, pop)
population <- filter(population,year==2002)
population <- arrange(population, desc(pop))
Country <- population$country
max_pop <- population$pop
Country[1]
max_pop[1]
```

## The End


