---
author: Cole
authorLink: https://colebaril.ca
categories:
- R
date: "2022-09-18T21:29:01+08:00"
description: A package containing functions to scrape recipes off the web. 
draft: false
images: []
lastmod: "2022-09-18T21:29:01+08:00"
lightgallery: true
resources:
- name: 
  src: 
tags:
- R
- RStudio
- Package
- Function
- Code
- Recipe
- Webscraping
title: rrecipes R Package
toc: no
keep_md: yes
weight: 1
---

A hobby-project package containing functions to scrape recipes off the web. 

<!--more-->

## Premise <img src='logo.png' align="right" height="139" />

As an avid amateur baker, I often search the web for the best recipes. Often times, I want to compare multiple recipes and paste them into an easier-to-access format such as a personal recipe list or send them to a friend. In a lot of cases, this involves a substantial amount of scrolling to the bottom of the page in order to find the recipe. 

Furthermore, another issue I encountered is that if I wish to paste the recipe to my own list, the format used by the site results in a lot of post-paste reformatting. I thought this would be the perfect time to create an R package that not only allows you to search popular recipe sites straight from R, but also allows you to extract the recipe without even having to visit the site. 

This project, while also useful for when I want to search for recipes, is also a way for me to practice using additional R packages and functions in addition to practice making a package. 

## The Package 

I present [rrecipes](https://github.com/colebaril/rrecipes), hosted on my [Github](https://github.com/colebaril) page. 

## rrecipes

This package contains some simple functions to scrape recipes from the web using the [`rvest`](https://rvest.tidyverse.org/) package. I got this idea as I was scrolling through paragraphs of mostly useless text on recipe websites trying to find the actual recipe. I also became frustrated when trying to copy and paste recipes to other applications and the website was less than ideal for doing so. This package is useful if you wish to extract multiple recipes from the web at once. It can also be used to convert recipe text on a website into a easily usable format (e.g., if you wish to copy and paste into another document or recipe book, as I am doing). 

Adding more sites and functionality in my spare time, as this is a hobby project.

# Download

To download and load the package, run the following code in R: 

```{R, rrecipes download}
devtools::install_git("https://github.com/colebaril/rrecipes")
library(rrecipes)
```

# Functionality


## `scrape()`

Use the `scrape()` function employs a variety of other functions to extract recipes from the web. Internet connection required. This function prints recipes to the console and saves a file called `scraped_recipes.txt` in your working directory. 

### Arguments

`recipe_urls = `:  Takes in 1 or more recipe URLs. Make sure the URL is for a single recipe, not a list.

For example:

```{R}
scrape(recipe_urls = c(
  "https://www.allrecipes.com/recipe/25080/mmmmm-brownies/?internalSource=hub%20recipe&referringContentType=Search",
  "https://www.foodnetwork.ca/recipe/the-pioneer-woman-bbq-pork-walking-tacos-are-the-ultimate-snack/",
  "https://tasty.co/recipe/slow-cooker-ribs")) 
```
## Search Recipe Websites 

For each supported website, a search feature has been or is being implemented.

The `search_recipes()` function works by taking in an argument `query =` which can be any food you want to search for and `site =`, wwhich can be any of the supported sites, and will return the top 10 URLs for your query by default. You can also specify the number of URLs returned as demonstrated below. The number returned may be less than the number requested where there are no more recipes. 

### Arguments

`query =`: The search term (e.g., "apple pie"). 

`site =`: The site you wish to search (e.g., "allrecipes"). 

**The following commands may be entered for `site`**

- Foodnetwork.ca: `foodnetwork`
- allrecipes.com: `allrecipes`
- thepioneerwoman.com: `pioneerwoman`


`number =`: The number of URLs you wish to return. There may be less results than requested.

### Example

For example, running this:

```{R}
search_recipes(query = "apple pie",
               site = "allrecipes",
               number = 10)
```
Yields this:
```
 [1] "https://www.allrecipes.com/recipe/283215/salted-caramel-apple-pie/"             
 [2] "https://www.allrecipes.com/recipe/15806/chemical-apple-pie-no-apple-apple-pie/" 
 [3] "https://www.allrecipes.com/recipe/235346/apple-pie-moonshine/"                  
 [4] "https://www.allrecipes.com/recipe/261468/apple-pie-dip/"                        
 [5] "https://www.allrecipes.com/recipe/234166/apple-pie-liquor/"                     
 [6] "https://www.allrecipes.com/recipe/213569/grandmas-iron-skillet-apple-pie/"      
 [7] "https://www.allrecipes.com/recipe/255040/awesome-apple-pie-cookies/"            
 [8] "https://www.allrecipes.com/recipe/239918/apple-jam-apple-pie-in-a-jar/"         
 [9] "https://www.allrecipes.com/recipe/15683/dutch-apple-pie-with-oatmeal-streusel/" 
[10] "https://www.allrecipes.com/recipe/218330/grandmas-apple-pie-ala-mode-moonshine/"     
 ```

## Use Pipes for Efficiency

The functions work with `magrittr` pipes. For example, see below as we search for brownie recipes and scrape the code. In this example, The outputs are printed in the console and the brownie URLs are piped through the `scrape()` function which retrieves the complete recipes from the site and saves the recipes in a file called `scraped_recipes.txt` in your file directory.

```
apple_pie <- search_recipes(query = "apple pie",
                           site = "allrecipes") %>% 
  scrape()
```

# Supported Sites

1. allrecipes.com
2. foodnetwork.ca (Canadian version)
3. tasty.co
4. thepioneerwoman.com

> More sites will be added as I feel like it, for this is a hobby project.

# In Development 

Support for more sites for both the `scrape()` and search functions are being implemented. Work is being done to group all search functions into one search function, although I am working out the kinks on this. 

# References 

Wickham H, Fran??ois R, Henry L, M??ller K (2022). _dplyr: A Grammar of Data Manipulation_. R package version 1.0.10,
<https://CRAN.R-project.org/package=dplyr>.

Wickham H (2022). _rvest: Easily Harvest (Scrape) Web Pages_. R package version 1.0.3, <https://CRAN.R-project.org/package=rvest>.

Wickham H, Girlich M (2022). _tidyr: Tidy Messy Data_. R package version 1.2.1, <https://CRAN.R-project.org/package=tidyr>.

Bache S, Wickham H (2022). _magrittr: A Forward-Pipe Operator for R_. R package version 2.0.3,
<https://CRAN.R-project.org/package=magrittr>.

Dowle M, Srinivasan A (2021). _data.table: Extension of `data.frame`_. R package version 1.14.2,
<https://CRAN.R-project.org/package=data.table>.

Yu G (2020). _hexSticker: Create Hexagon Sticker in R_. R package version 0.4.9, <https://CRAN.R-project.org/package=hexSticker>.

Qiu Y, details. aotifSfAf (2022). _sysfonts: Loading Fonts into R_. R package version 0.8.8, <https://CRAN.R-project.org/package=sysfonts>.

Ooms J (2021). _magick: Advanced Graphics and Image-Processing in R_. R package version 2.7.3, <https://CRAN.R-project.org/package=magick>.

References code:

```{R}
library(purrr)
c("dplyr", "rvest", "tidyr", "magrittr", "data.table", "hexSticker", "sysfonts", "magick") %>%
  map(citation) %>%
  print(style = "text")
```

Logo Code:

```{R}
library(hexSticker)

sysfonts::font_add_google("Zilla Slab", "pf", regular.wt = 500)

hexSticker::sticker(
  subplot = ~ plot.new(), s_x = 1, s_y = 1, s_width = 0.1, s_height = 0.1,
  package = "rrecipes", p_x = 1, p_y = 1, p_size = 30, h_size = 1.2, p_family = "pf",
  p_color = "#00738c", h_fill = "#FFF9F2", h_color = "#00738c",
  dpi = 320, filename = "man/figures/logo.png"
)

magick::image_read("man/figures/logo.png")
```
