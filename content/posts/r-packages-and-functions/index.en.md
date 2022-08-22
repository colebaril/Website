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
Usually when I run into problems while using R, cannot think of a solution off the top of my head, or forget something I have used in the past, I end up searching the internet for answers. This often results in a headache and wasted time searching for something I have already used before. 

This post serves as a repository of R packages, functions and use-cases I come across that I do not want to forget and may be useful for others as well. It will be organized by package, then function and/or use/example. This is mainly so that I only have to look in one place to quickly find out what I need to know.

<!--more-->

## Package Summary

The packages table links to each package documentation. Links to my description and use-case in the notable functions column.  
<table>
  <tr>
    <th>Package</th>
    <th>Description</th>
    <th>Functions</th>
  </tr>
  <tr>
    <td><a href="https://www.tidyverse.org/">Tidyverse</a></td>
    <td>Collection of grammar and data structure packages</td>
    <td>dplyr, ggplot2, tibble, readr, purrr, tidyr, stringr, Lubridate</td>
  </tr>
  <tr>
  <td><a href="https://dplyr.tidyverse.org/">dplyr</a></td>
    <td>Manipulation & organisation</td>
    <td>mutate, select, filter</td>
  </tr>
  </tr>
    <tr>
  <td><a href="https://tidyr.tidyverse.org/">tidyr</a></td>
    <td>Manipulation & organisation</td>
    <td>unite, separate, pivot_longer, pivot_wider, replace_na</td>
  </tr>
  <tr>
  <td><a href="https://lubridate.tidyverse.org/">Lubridate</a></td>
    <td>Date & Time manipulation</td>
    <td>as_Date</td>
  </tr>
    <tr>
  <td><a href="https://www.rdocumentation.org/packages/janitor/versions/2.1.0">Janitor</a></td>
    <td>Data cleaning</td>
    <td>clean_names, row_to_number</td>
  </tr>
  <tr>
  <td><a href="https://www.rdocumentation.org/packages/assertr/versions/2.8">assertr</a></td>
    <td>Quality control</td>
    <td>assert, verify, insist</td>
  </tr>
  <tr>
  <td><a href="https://ggplot2.tidyverse.org/">ggplot2</a></td>
    <td>Visualization</td>
    <td>Elegant charts and figures</td>
  </tr>
      <tr>
  <td><a href="https://www.rdocumentation.org/packages/patchwork/versions/1.1.1">patchwork</a></td>
    <td>Visualization</td>
    <td>Combining ggplot2 plot objects</td>
  </tr>
  <tr>
  <td><a href="https://www.rdocumentation.org/packages/ggVennDiagram/versions/1.2.0">ggVennDiagram</a></td>
    <td>Visualization</td>
    <td>Pretty and informative Venn Diagrams</td>
  </tr>
  <tr>
  <td><a href="https://gt.rstudio.com/">gt & gtExtras</a></td>
    <td>Visualization</td>
    <td>Elegant customizable tables</td>
  </tr>
  <tr>
  <td><a href="https://www.rdocumentation.org/packages/knitr/versions/1.39/topics/kable">kable</a></td>
    <td>Visualization</td>
    <td>Simple table generator</td>
  </tr>
  <tr>
  <td><a href="https://here.r-lib.org/">here</a></td>
    <td>Functionality</td>
    <td>Upload files from current working directory</td>
  </tr>
    <tr>
  <td><a href="https://www.rdocumentation.org/packages/webshot/versions/0.5.3">webshot</a></td>
    <td>Functionality</td>
    <td>Takes screenshots of rendered figures/documents</td>
  </tr>
  <tr>
  <td><a href="https://www.rdocumentation.org/packages/zoo/versions/1.8-10">zoo</a></td>
    <td>Math/stats</td>
    <td>Functions for dealing with time-series data (e.g., rolling mean)</td>
  </tr>
</table>

## Data Wrangling

### Tidyverse

[tidyverse](https://www.tidyverse.org/) is a collection of R packages that work together to form the 'tidyverse'. These include:

-   dplyr (manipulation)
-   ggplot2 (visualization)
-   tibble (easily view data frames)
-   readr (import)
-   purrr
-   tidyr
-   stringr
-   (NEW) Lubridate (easy date manipulation)

### dplyr

[dplyr](https://dplyr.tidyverse.org/) is a grammar for manipulating data and is one of my most-used packages. It loads in with the tidyverse package.



`mutate()` adds new variables that are *functions* of existing variables. For example if you wish to square all values in column x within your dataframe:

``` {.r}
df %>% 
  mutate(x_squared = x * x)

ndw <- read_excel(here("Data/Raw/USW19-21.xlsx")) %>% 
  mutate(date = date(date)) 
```

The beauty of `mutate()` is that it leaves the original column alone and creates a new column.



`select()` enables selection or dropping of specific columns within your data frame. This is useful for data sets where there is a lot of irrelevant information (e.g., weather station data).

``` {.r}
ndw <- read_excel(here("Data/Raw/USW19-21.xlsx")) %>%
  select(-latitude, -longitude, -longitude, -elevation, -max_temp_flag, -min_temp_flag, -avg_temp_flag, -avg_bare_soil_temp_flag,
         -avg_wind_speed_flag, -total_solar_rad_flag, -rainfall_flag, -dew_point_flag) 
```


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

### Janitor

[Janitor](https://www.rdocumentation.org/packages/janitor/versions/2.1.0) is a package that helps with data cleaning. Useful functions I use include:

-   `clean_names()` makes "R friendly" column names (e.g., it will turn `Average Mass` into `average_mass`).
-   `row_to_names()` is useful if there is unnecessary rows of information between your data and a column name. For example:

```{r}
row_to_names(df, row_number = 5) will bring the 5<sup>th</sup> row up to the column names and remove rows in between. 
```

### Lubridate

[Lubridate](https://lubridate.tidyverse.org/) makes dealing with dates (usually a nightmare) a little bit easier. This package has recently been integrated into the tidyverse.

### tidyr

[tidyr](https://tidyr.tidyverse.org/) (part of the tidyverse) helps with tidying data. My favourite functions include:

-   `unite()` to unite combine columns (common with dates, where, for example, Y/M/D are in separate columns).
-   `separate()` and `extract()` to split columns into multiple columns (e.g., split a full name column into separate first and last name columns)
-   `pivot_longer()` and `pivot_wider()` for extending data long and wide.
-   `drop_na` to remove NA values.
-   `replace_na` to replace a known value with NA.

## Quality Control 

### assertr

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
## Data Visualization

### ggplot2

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

### Patchwork

[patchwork](https://www.rdocumentation.org/packages/patchwork/versions/1.1.1) allows for the easy combining of multiple ggplots into a singlular graphic. Simply turn the ggplots into objects, and then add or divide objects together.

```{r}
plot1 + plot2 # side by side

plot1 / plot2 # top and bottom 

(plot1 + plot2) / (plot3 + plot4) + # 1 and 2 upper side by side, 2 and 4 lower side by side 
  plot_layout(guides = `collect`) & # collects any legends that are similar (e.g., 1 legend rather than 4)
  theme(legend.position = `bottom`) # specifies legend position
```

### gt

[`gt`](https://gt.rstudio.com/) allows for elegant and custom tables suitable for publication using easy-to-understand functions and commands. Furthermore, the `gtExtras` package contains functions for additional table customization. 

Here is an example of a `gt` table I created:

```{r, include = FALSE, echo = FALSE}
library(readxl)
library(here)
library(tidyverse)
library(gt)
library(gtExtras)
library(webshot)

pools <- read_excel(here("data/raw/sequencepools.xlsx")) 

gtpools <- gt(pools) %>% 
  tab_options(
    table.border.top.color = "white",
    column_labels.border.top.width = 3,
    column_labels.border.top.color = "black",
    column_labels.border.bottom.width = 3,
    column_labels.border.bottom.color = "black",
    table_body.border.bottom.color = "black",
    table.width = pct(75)) %>% 
  tab_style(
    style = cell_text(style = "italic"),
    locations = cells_body(
      columns = Species
    )
  ) %>% 
  tab_header(
    title = md("**Table 2-1: Sequenced Mosquito RNA Pools**"),
    subtitle = md("The year, location, species of mosquito and number of specimens comprising each mosquito pool that was sent for RNA Sequencing. A total of 2 pools, 19 pools and 23 RNA pools were sequenced from mosquitoes caught in 2019, 2020 and 2021, respectively. 1 pool (*Oc. triseriatus*) was comprised of specimens caught in Manitoba in 2019 and 2020.")
  ) %>%
  gtsave(filename = "table2-1.png", zoom = 1)
```

## Math/Stats

### zoo

The [zoo](https://www.rdocumentation.org/packages/zoo/versions/1.8-10) package contains many functions for dealing with time series data.

`rollapplyr()` calculates the rolling means. 

For example, in the following code, the rolling mean for max and min temperature are calculated in 30-day intervals (assuming dates are consecutive). 

{{< admonition note "Note" >}}
If data has `NA` values, include the argument `na.rm = T` and `rollapplyr` will include the NA value in the 30 days. If not, any `NA` within a 30-day period will result in `NA`. 
{{< /admonition >}}

```{r}
weather_rolled <- {weather %>% 
  mutate(maxt30 = rollapplyr(max_temp, 30, mean, partial = TRUE, na.rm=T)) %>% 
  mutate(mintt30 = rollapplyr(min_temp, 30, mean, partial = TRUE, na.rm=T)) 
```