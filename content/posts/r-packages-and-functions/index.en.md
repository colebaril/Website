---
author: Cole
authorLink: https://colebaril.netlify.app
categories:
- R
date: "2022-07-30T21:29:01+08:00"
description: Useful R packages, functions and use-cases. 
draft: false
images: []
lastmod: "2020-03-06T21:29:01+08:00"
lightgallery: true
resources:
- name: featured-image
  src: featured-image.jpg
tags:
- R
- RStudio
- Package
- Function
- Code
- RStats
title: Useful R Packages and Functions 
toc:
  auto: false
weight: 1
---

This post serves as a repository of R packages, functions and use-cases I come across that I do not want to forget and may be useful for others as well. It will be organized by package, then function and/or use/example. This is mainly so that I only have to look in one place to quickly find out what I need to know.

<!--more-->

## Tidyverse

[tidyverse](https://www.tidyverse.org/) is a collection of R packages that work together to form the 'tidyverse'. These include:

-   dplyr (manipulation)
-   ggplot2 (visualization)
-   tibble (easily view data frames)
-   readr (import)
-   purrr
-   tidyr
-   stringr
-   (NEW) Lubridate (easy date manipulation)

## dplyr

[dplyr](https://dplyr.tidyverse.org/) is a grammar for manipulating data and is one of my most-used packages. It loads in with the tidyverse package.

### Mutate

`mutate()` adds new variables that are *functions* of existing variables. For example if you wish to square all values in column x within your dataframe:

``` {.r}
df %>% 
  mutate(x_squared = x * x)

ndw <- read_excel(here("Data/Raw/USW19-21.xlsx")) %>% 
  mutate(date = date(date)) 
```

The beauty of `mutate()` is that it leaves the original column alone and creates a new column.

### Select

`select()` enables selection or dropping of specific columns within your data frame. This is useful for data sets where there is a lot of irrelevant information (e.g., weather station data).

``` {.r}
ndw <- read_excel(here("Data/Raw/USW19-21.xlsx")) %>%
  select(-latitude, -longitude, -longitude, -elevation, -max_temp_flag, -min_temp_flag, -avg_temp_flag, -avg_bare_soil_temp_flag,
         -avg_wind_speed_flag, -total_solar_rad_flag, -rainfall_flag, -dew_point_flag) 
```

### Filter

`filter()` allows you to filter your data set by names or values. For example, here I use `filter()` to identify outliers within a dataset inside of a `ggplot2` function. I apply the `filter()` function to the weather1 dataset and only view data where dsi \> 10. This can also be used with logical operators `==` (equals) and `!=` (does not equal) to filter out or in variables.

``` {.r}
ggplot(data = filter(weather1, dsi > 10), aes(x = dsi, fill = site)) +
  theme_bw() +
  geom_histogram(position = "dodge") +
  facet_wrap(~year) +
  scale_fill_viridis_d(end = 0.8) +
  facet_wrap(~trialnumber) +
  scale_y_continuous() +
  geom_vline(xintercept = c(10, 20), linetype = "dotted")
```

## Janitor

[Janitor](https://www.rdocumentation.org/packages/janitor/versions/2.1.0) is a package that helps with data cleaning. Useful functions I use include:

-   `clean_names()` makes "R friendly" column names (e.g., it will turn `Average Mass` into `average_mass`).
-   `row_to_names()` is useful if there is unnecessary rows of information between your data and a column name. For example:

```{r}
row_to_names(df, row_number = 5) will bring the 5<sup>th</sup> row up to the column names and remove rows in between. 
```

## Lubridate

[Lubridate](https://lubridate.tidyverse.org/) makes dealing with dates (usually a nightmare) a little bit easier. This package has recently been integrated into the tidyverse.

## tidyr

[tidyr](https://tidyr.tidyverse.org/) (part of the tidyverse) helps with tidying data. My favourite functions include:

-   `unite()` to unite split columns (common with dates, where, for example, Y/M/D are in separate columns).
-   `separate()` and `extract()` to split columns into multiple columns (e.g., split a full name column into separate first and last name columns)
-   `pivot_longer()` and `pivot_wider()` for extending data long and wide.
-   `drop_na` to remove NA values.
-   `replace_na` to replace a known value with NA.

## assertr

[assertr](https://www.rdocumentation.org/packages/assertr/versions/2.8) includes many functions used for *assertive programming* and acts as a quality check before you start data analysis.

For example, if any of the assertions are violated in this code, the function throws an error and terminates the code.

```{R}
mtcars %>%
      verify(has_all_names("mpg", "vs", "am", "wt")) %>%
      verify(nrow(.) > 10) %>%
      verify(mpg > 0) %>%
      insist(within_n_sds(4), mpg) %>%
      assert(in_set(0,1), am, vs) %>%
      assert_rows(num_row_NAs, within_bounds(0,2), everything()) %>%
      assert_rows(col_concat, is_uniq, mpg, am, wt) %>%
      insist_rows(maha_dist, within_n_mads(10), everything()) %>%
      group_by(cyl) %>%
      summarise(avg.mpg=mean(mpg))
```

## ggplot2

[ggplot2](https://ggplot2.tidyverse.org/), part of the tidyverse, allows for elegant data visualization. The functions and customization within are endless, so I will share some examples of plots below.

This code is for plotting climate norms. See text within plot for descriptions.

```{r}
ggplot(data = ndw_norm, aes(x = month, y = avg_air_temp)) + # sets data frame, axes
  stat_smooth(colour = "black", fill = "grey70", size = 1.5) + # calls the stat smooth function (stats) and specifies line colours.
  geom_line(data = ndw_bound, aes(x = month, y = avg_temp, colour = factor(year)), size = 1) + # calls a new data set and function for new data set
  labs( # labelling axes
    x = "Month",
    y = "Mean Air Temperature (\u00B0C)"
  ) + 
  facet_wrap(~station_name, labeller = as_labeller(c("Carrington" = "Carrington", # facet wrap by station and re-labels each chart
                                                  "Kempton" = "Northwood",
                                                  "Langdon" = "Langdon",
                                                  "Little Falls" = "St. Cloud"))) +
  scale_x_continuous(limits = c(4, 10), # setting scale limits
                     breaks = c(4, 5, 6, 7, 8, 9, 10), 
                     labels = c("Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct")) + # re-label month number as month
  scale_y_continuous(limits = c(0, 25)) + # upper and lower limits of the chart
  scale_colour_discrete(name = "Year") + # discrete colour palette based on the year of the data 
  theme_bw() # a nice theme
```

## Patchwork

[patchwork](https://www.rdocumentation.org/packages/patchwork/versions/1.1.1) allows for the easy combining of multiple ggplots into a singlular graphic. Simply turn the ggplots into objects, and then add or divide objects together.

```{r}
plot1 + plot2 # side by side

plot1 / plot2 # top and bottom 

(plot1 + plot2) / (plot3 + plot4) + # 1 and 2 upper side by side, 2 and 4 lower side by side 
  plot_layout(guides = `collect`) & # collects any legends that are similar (e.g., 1 legend rather than 4)
  theme(legend.position = `bottom`) # specifies legend position
```