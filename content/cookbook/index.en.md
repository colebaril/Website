---
date: "2022-11-11T11:04:49+08:00"
description: Cookbook
draft: false
images:
- /Apple-Devices-Preview.png
lightgallery: true
math:
  enable: true
title: Cookbook
---

<iframe src = "Cookbook.pdf" height="1000"  width=125%></iframe>

## About

> **The raw code file for this document is available at https://github.com/colebaril/Cookbook**

### Made with R Markdown

This cookbook was made using [R Markdown](https://rmarkdown.rstudio.com/) and rendered via [Knitr](https://yihui.org/knitr/) using [XeLaTeX](https://www.overleaf.com/learn/latex/XeLaTeX), a [Tex](https://en.wikipedia.org/wiki/TeX) typesetting engine using Unicode and supporting modern font technologies. Combined, these tools make writing, formatting and rendering large documents easy and reproducible.

### YAML

The YAML, a human-readable data serialization language, is a snippet of code that appears before the entire document enclosed by a set of `---`. For this particular document, a number of LaTeX configuration settings was required in order to format it. The following is a breakdown of the YAML for this cookbook. 

```{YAML}
---
title: "Baril Family Cookbook"
subtitle: "A collection of beloved recipes from family, friends and Manitobans"
author:
- Written by Cole Baril
- Edited by Denis Baril
date: "`r Sys.Date()`"
output:
  pdf_document: 
    latex_engine: xelatex
header-includes: 
  \usepackage{titling} # Used the package {titling} for formatting titles.
  \pretitle{\begin{center}\LARGE\includegraphics[width=12cm]{title.jpg}\\[\bigskipamount]} # Formats the title image (center, width)
  \posttitle{\end{center}} # Ends the title page with the picture.
  \usepackage{geometry} \geometry{top=0.75in,left=1in,bottom=0.75in,right=1in} # Using the {geometry} package for setting page margins.
  \usepackage{sectsty} \sectionfont{\centering} \chapterfont{\centering} \subsectionfont{\centering \emph} # Using the {sectsy} package to format different levels of headings.
  \usepackage{multicol} # Using the {multicol} package to format columns of ingredients.
toc: yes
number_sections: yes
documentclass: report
papersize: a4
fontsize: 12pt
---
```

### Columns 

Each time I wish to customize a section of ingredients column-wise, I call the following function to start and stop multiple column formatting in the document:

```{LaTeX}
\begin{multicols}{3} # This appears before the section of columns and would specify 3 columns of text.

\end{multicols} # This appears after the section of columns.
```

### Recipe Authors 

I wanted the recipe authors to be prominently displayed below the recipe title. However, as a heading, it also appears in the table of contents, which is not necessary. To circumvent this, I include `{.unlisted .unnumbered}` after each author to hide the heading from appearing in the table of contents. 

For example:

```{LaTeX}
## Recipe Title

### Cole Baril {.unlisted .unnumbered}
```


## References

R Core Team (2022). _R: A Language and Environment for Statistical Computing_. R Foundation for Statistical Computing, Vienna, Austria.
<https://www.R-project.org/>.

Allaire J, Xie Y, McPherson J, Luraschi J, Ushey K, Atkins A, Wickham H, Cheng J, Chang W, Iannone R (2022). _rmarkdown: Dynamic Documents for
R_. R package version 2.16, <https://github.com/rstudio/rmarkdown>.

Xie Y, Allaire J, Grolemund G (2018). _R Markdown: The Definitive Guide_. Chapman and Hall/CRC, Boca Raton, Florida. ISBN 9781138359338,
<https://bookdown.org/yihui/rmarkdown>.

Xie Y, Dervieux C, Riederer E (2020). _R Markdown Cookbook_. Chapman and Hall/CRC, Boca Raton, Florida. ISBN 9780367563837,
<https://bookdown.org/yihui/rmarkdown-cookbook>.

Xie Y (2022). _knitr: A General-Purpose Package for Dynamic Report Generation in R_. R package version 1.40, <https://yihui.org/knitr/>.

Xie Y (2015). _Dynamic Documents with R and knitr_, 2nd edition. Chapman and Hall/CRC, Boca Raton, Florida. ISBN 978-1498716963,
<https://yihui.org/knitr/>.

Xie Y (2014). “knitr: A Comprehensive Tool for Reproducible Research in R.” In Stodden V, Leisch F, Peng RD (eds.), _Implementing Reproducible
Computational Research_. Chapman and Hall/CRC. ISBN 978-1466561595, <http://www.crcpress.com/product/isbn/9781466561595>.

Baril, Cole (2022). "rrecipes: scrape recipes from the web". <https://github.com/colebaril/rrecipes>'

Penner, Emmy (2022). Contributed recipes to the book. 

Derksen, Darlene (2022). Contributed recipes to the book. 

Sabourin, Lucette (2022). Contributed recipes to the book. 
