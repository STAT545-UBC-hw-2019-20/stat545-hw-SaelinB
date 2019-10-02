---
title: "HW03"
author: "Saelin Bjornson"
output: html_document
---

<style>
  .col2 {
    columns: 2 200px;         /* number of columns and width in pixels*/
  }
  .col3 {
    columns: 3 100px;
  }
</style>

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

```{r load, include=FALSE}
library(dplyr)
library(gapminder)
library(ggplot2)
library(knitr)
library(DT)
```
# The Data
## I will be working with the gapminder data set:


```{r Data}
datatable(gapminder)

```


# Task Option 1
## What countries had a life expectancy below the global median for each year?

```{r Task 1.1}
gapminder %>%
  group_by(year) %>%
  mutate(median_lifeExp = median(lifeExp)) %>%
  filter(lifeExp < median_lifeExp) %>%
  select(country, year, lifeExp, median_lifeExp) %>%
  arrange(year, lifeExp, country) %>%
  rename(Country = country, Year = year, "Life Expectancy" = lifeExp, "Global Median Life Expectancy" = median_lifeExp) %>%
  as_tibble() %>%
  datatable()
```

## To summarize, how many countries on each contient had a life expectancy below the global median for each year?

<div class="col2">
```{r Task 1.2}
gapminder %>%
  group_by(year) %>%
  mutate(median_lifeExp = median(lifeExp)) %>%  # find global median life expectancy for each year
  filter(lifeExp < median_lifeExp) %>%  #filter out rows where life expectancy is less than the global mean
  group_by(continent) %>%
  mutate(num_countries = length(unique(country))) %>%  # find number of countries per continent  
  distinct(year, continent, .keep_all =TRUE) %>%  # filter to rows with distinct combinations of continents and year (becuase otherwise they are duplicated for some reason),keeping all other columns as well
  select(continent, year, median_lifeExp, num_countries) %>%  # making the table
  rename(Year = year, "Global Median Life Expectancy" = median_lifeExp, "Number of Countries" = num_countries) %>%
  as_tibble() %>%
  datatable()
```

## Graphically:
```{r Task 1.3}
gapminder %>%
  group_by(year) %>%
  mutate(median_lifeExp = median(lifeExp)) %>%
  group_by(continent) %>%
  filter(lifeExp < median_lifeExp) %>%
  mutate(num_countries = length(unique(country))) %>%
  distinct(year, continent, .keep_all =TRUE) %>%  
  select(continent, year, num_countries) %>%
  ggplot(aes(factor(year), num_countries, fill = continent, group = continent)) + #group by and color by continent, change year to discrete categories
  geom_bar(position="stack", stat="identity") +
  ylab("Number of Countries") + 
  xlab("Year") +
  scale_fill_discrete("Continent") +
  theme_light()
  
```

</div>


# Task Option 2
## What is the maximum and minimum GDP per capita for all continents?
<div class="col2">
```{r Task 2.1}
gapminder %>%
  group_by(continent) %>%
  summarize("Maximum GDP Per Capita" = max(gdpPercap), "Minimum GDP Per Captia" = min(gdpPercap)) %>%
  select(continent, "Maximum GDP Per Capita", "Minimum GDP Per Captia") %>%
  as_tibble()

```
## Graphically:
```{r Task 2.2}
gapminder %>%
  group_by(continent) %>%
  summarize(max = max(gdpPercap), min = min(gdpPercap)) %>%
  ggplot(aes(continent)) +
  geom_point(aes(continent, max, color = "Maximum",size = 5)) +
  geom_point(aes(continent, min,color= "Minimum",size = 5))  +
  scale_color_discrete("") +
  xlab("Continent") +
  scale_y_continuous("GDP Per Capita", breaks=seq(0,120000,10000), labels=waiver()) +  #set y axis breaks
  guides(size = FALSE)  #remove size legend

```
</div>


# Task Option 4
## Reporting the global mean and median life expectancy,and life expectancy weighted by population
<div class="col2">
```{r Task 4.1}
gapminder %>%
  summarize(Mean = mean(lifeExp),
            Median = median(lifeExp),
            "Weighted Mean" = weighted.mean(lifeExp, pop)) %>%
  as_tibble()
```
## Graphically:
```{r Task 4.2} 
gapminder %>%
  ggplot(aes(x=lifeExp)) +
  geom_histogram(aes(y=..density..),colour="black", fill="white") + # have to change "counts" to "density" to overlay density plot
  geom_density(alpha=0.1,fill="Red") +
  geom_vline(aes(xintercept=mean(lifeExp), color ="Mean")) +
  geom_vline(aes(xintercept=median(lifeExp), color ="Median"))+
  geom_vline(aes(xintercept=weighted.mean(lifeExp, pop), color ="Mean Weighted by Population")) +
  ylab("") +
  xlab("Life Expectancy") +
  scale_color_discrete("") +
  theme_light()

```
</div>

<div class="clearer"></div>


