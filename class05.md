---
author:
- "Rachel Galleta (PID:A16859649)"
authors:
- "Rachel Galleta (PID:A16859649)"
title: "Class 5: Data Viz with ggplot"
toc-title: Table of contents
---

## introduction

Today we are exploring the **gggplot** package and how to make nice
figures in R.

There are lots of ways to make figures and plot in R. These include:

-   so called "base" R
-   and add on packages like **ggplot2**

Here is a simple "base" R plot.

:::: cell
``` {.r .cell-code}
head(cars)
```

::: {.cell-output .cell-output-stdout}
      speed dist
    1     4    2
    2     4   10
    3     7    4
    4     7   22
    5     8   16
    6     9   10
:::
::::

We can simply pass to to the `plot()` function.

:::: cell
``` {.r .cell-code}
plot(cars)
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-2-1.png)
:::
::::

> Key-point: Base R is quick but not so nice looking in some folks eyes.

Let's see how we can plot this with **ggplot2**...

1st I need to install this add-on package. For this we use the
`install.packages()` function - **WE DO THIS IN THE CONSOLE, NOT our
report**. This is a one time only deal.

2nd We need to load the package with the `library()` function every time
we want to use it.

::::: cell
``` {.r .cell-code}
library(ggplot2)
```

::: {.cell-output .cell-output-stderr}
    Warning: package 'ggplot2' was built under R version 4.4.3
:::

``` {.r .cell-code}
ggplot(cars)
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-3-1.png)
:::
:::::

Every ggplot is composed of at least 3 layers:

-   **data** (i.e a data.frame with the things you want to plot),
-   aesthitics **aes()** that map the columns of data to your plot
    features (i.e. aesthitics)
-   geoms like **geom_point()** that srt how the plot appears

:::: cell
``` {.r .cell-code}
ggplot(cars) +
  aes(x=speed, y=dist) +
  geom_point()
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-4-1.png)
:::
::::

> Key point: For simple "canned" graphs base R is quicker but as things
> get more custom and elobrate then ggplot wins out...

Let's add more layers to our ggplot

Add a line showing the relationship between x and y Add a title Add
custom axis labels "Speed (MPH)" and "Distance (ft)" Change the theme...

::::: cell
``` {.r .cell-code}
ggplot(cars) +
  aes(x=speed, y=dist) +
  geom_point() +
  geom_smooth(method="lm", se=FALSE) +
  labs(title="Silly plot of Speed vs Stoping distance",
       x="Speed (MPH)",
       y="Distance (ft)") +
  theme_bw()
```

::: {.cell-output .cell-output-stderr}
    `geom_smooth()` using formula = 'y ~ x'
:::

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-5-1.png)
:::
:::::

## Going further

Read some gene expresion data

:::: cell
``` {.r .cell-code}
url <- "https://bioboot.github.io/bimm143_S20/class-material/up_down_expression.txt"
genes <- read.delim(url)

head(genes)
```

::: {.cell-output .cell-output-stdout}
            Gene Condition1 Condition2      State
    1      A4GNT -3.6808610 -3.4401355 unchanging
    2       AAAS  4.5479580  4.3864126 unchanging
    3      AASDH  3.7190695  3.4787276 unchanging
    4       AATF  5.0784720  5.0151916 unchanging
    5       AATK  0.4711421  0.5598642 unchanging
    6 AB015752.4 -3.6808610 -3.5921390 unchanging
:::
::::

> Q1. How many genes are in this wee dataset?

:::: cell
``` {.r .cell-code}
nrow(genes)
```

::: {.cell-output .cell-output-stdout}
    [1] 5196
:::
::::

:::: cell
``` {.r .cell-code}
ncol(genes)
```

::: {.cell-output .cell-output-stdout}
    [1] 4
:::
::::

> Q2. How many "up" regulated genes are there?

:::: cell
``` {.r .cell-code}
sum( genes$State == "up" )
```

::: {.cell-output .cell-output-stdout}
    [1] 127
:::
::::

A useful function for counting up occurances of things in a vector is
the `table()` function.

:::: cell
``` {.r .cell-code}
table( genes$State )
```

::: {.cell-output .cell-output-stdout}

          down unchanging         up 
            72       4997        127 
:::
::::

Make a v1 figure

:::: cell
``` {.r .cell-code}
p <- ggplot(genes) +
  aes(x=Condition1,
      y=Condition2,
      col=State) +
  geom_point() 

p
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-11-1.png)
:::
::::

:::: cell
``` {.r .cell-code}
p + 
  scale_colour_manual( values=c("blue","gray","red") ) +
  labs(title="Expresion changes upon drug treatment",
       x="Control (no drug)",
       y="Treatment (with drug)") +
  theme_bw()
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-12-1.png)
:::
::::

## More Plotting

Read in the gapminder dataset

::: cell
``` {.r .cell-code}
# File location online
url <- "https://raw.githubusercontent.com/jennybc/gapminder/master/inst/extdata/gapminder.tsv"

gapminder <- read.delim(url)
```
:::

Lets have a wee peak

:::: cell
``` {.r .cell-code}
head( gapminder, 3)
```

::: {.cell-output .cell-output-stdout}
          country continent year lifeExp      pop gdpPercap
    1 Afghanistan      Asia 1952  28.801  8425333  779.4453
    2 Afghanistan      Asia 1957  30.332  9240934  820.8530
    3 Afghanistan      Asia 1962  31.997 10267083  853.1007
:::
::::

:::: cell
``` {.r .cell-code}
tail(gapminder, 3)
```

::: {.cell-output .cell-output-stdout}
          country continent year lifeExp      pop gdpPercap
    1702 Zimbabwe    Africa 1997  46.809 11404948  792.4500
    1703 Zimbabwe    Africa 2002  39.989 11926563  672.0386
    1704 Zimbabwe    Africa 2007  43.487 12311143  469.7093
:::
::::

> Q4. How many different country values are in this dataset?

:::: cell
``` {.r .cell-code}
nrow(gapminder)
```

::: {.cell-output .cell-output-stdout}
    [1] 1704
:::
::::

:::: cell
``` {.r .cell-code}
length( table(gapminder$country) )
```

::: {.cell-output .cell-output-stdout}
    [1] 142
:::
::::

> Q5. How many different continent values are in this dataset.

:::: cell
``` {.r .cell-code}
unique(gapminder$continent)
```

::: {.cell-output .cell-output-stdout}
    [1] "Asia"     "Europe"   "Africa"   "Americas" "Oceania" 
:::
::::

:::: cell
``` {.r .cell-code}
ggplot(gapminder) +
  aes(gdpPercap, lifeExp, col=continent) +
  geom_point()
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-19-1.png)
:::
::::

:::: cell
``` {.r .cell-code}
ggplot(gapminder) +
  aes(gdpPercap, lifeExp, col=continent, label=country) +
  geom_point() +
  geom_text()
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-20-1.png)
:::
::::

I can use the **ggrepl** package to make more sensible labels here.

:::::: cell
``` {.r .cell-code}
library(ggrepel)
```

::: {.cell-output .cell-output-stderr}
    Warning: package 'ggrepel' was built under R version 4.4.3
:::

``` {.r .cell-code}
ggplot(gapminder) +
  aes(gdpPercap, lifeExp, col=continent, label=country) +
  geom_point() +
  geom_text_repel()
```

::: {.cell-output .cell-output-stderr}
    Warning: ggrepel: 1697 unlabeled data points (too many overlaps). Consider
    increasing max.overlaps
:::

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-21-1.png)
:::
::::::

I want a seperate pannel per continent

:::: cell
``` {.r .cell-code}
ggplot(gapminder) +
  aes(gdpPercap, lifeExp, col=continent, label=country) +
  geom_point() +
  facet_wrap(~continent)
```

::: cell-output-display
![](class05_files/figure-markdown/unnamed-chunk-22-1.png)
:::
::::

## Summary

The main advantages of ggplot over base R plot are:

1.  **Layered Grammar of Graphics**: ggplot uses a consistent, layered
    approach where you build plots by adding layers for data,
    aesthetics, and geometries. This makes complex plots easier to
    construct and modify, compared to base R which often requires
    different functions and many arguments for each plot type
    [\[1\]](https://drive.google.com/file/d/1BYSWJLROqxA1YpuDhJkzUolhiZqiOOKg/view?usp=drivesdk),
    [\[2\]](https://drive.google.com/file/d/1tFqKg9_nhVMmKYfiM1CQKDS2PmPwLh8n/view?usp=drivesdk),
    [\[3\]](https://drive.google.com/file/d/1Clw2_EJ_hY3USNwObiPnxpIQIfirxfW0/view?usp=drivesdk),
    [\[5\]](https://drive.google.com/file/d/15xXaaIcCWOc_x1gJLdySWOd_sfMXTiaw/view?usp=drivesdk),
    [\[4\]](https://drive.google.com/file/d/1FDBbIi2Rlw2In9mClB7Mub8oUPgx6y8h/view?usp=drivesdk).

2.  **Publication-Quality Output**: ggplot produces visually appealing,
    publication-ready figures by default, while base R plots often need
    extensive tweaking to look polished
    [\[1\]](https://drive.google.com/file/d/1BYSWJLROqxA1YpuDhJkzUolhiZqiOOKg/view?usp=drivesdk),
    [\[2\]](https://drive.google.com/file/d/1tFqKg9_nhVMmKYfiM1CQKDS2PmPwLh8n/view?usp=drivesdk),
    [\[3\]](https://drive.google.com/file/d/1Clw2_EJ_hY3USNwObiPnxpIQIfirxfW0/view?usp=drivesdk),
    [\[4\]](https://drive.google.com/file/d/1FDBbIi2Rlw2In9mClB7Mub8oUPgx6y8h/view?usp=drivesdk).

3.  **Declarative Syntax**: You specify what you want to show (data,
    mappings, geoms) rather than how to draw each element, making code
    more readable and maintainable
    [\[1\]](https://drive.google.com/file/d/1BYSWJLROqxA1YpuDhJkzUolhiZqiOOKg/view?usp=drivesdk),
    [\[2\]](https://drive.google.com/file/d/1tFqKg9_nhVMmKYfiM1CQKDS2PmPwLh8n/view?usp=drivesdk),
    [\[3\]](https://drive.google.com/file/d/1Clw2_EJ_hY3USNwObiPnxpIQIfirxfW0/view?usp=drivesdk),
    [\[5\]](https://drive.google.com/file/d/15xXaaIcCWOc_x1gJLdySWOd_sfMXTiaw/view?usp=drivesdk),
    [\[4\]](https://drive.google.com/file/d/1FDBbIi2Rlw2In9mClB7Mub8oUPgx6y8h/view?usp=drivesdk).

4.  **Easy Customization and Extension**: Adding titles, axis labels,
    legends, colors, and themes is straightforward and consistent. In
    base R, these customizations can be fiddly and require more manual
    control
    [\[1\]](https://drive.google.com/file/d/1BYSWJLROqxA1YpuDhJkzUolhiZqiOOKg/view?usp=drivesdk),
    [\[2\]](https://drive.google.com/file/d/1tFqKg9_nhVMmKYfiM1CQKDS2PmPwLh8n/view?usp=drivesdk),
    [\[3\]](https://drive.google.com/file/d/1Clw2_EJ_hY3USNwObiPnxpIQIfirxfW0/view?usp=drivesdk),
    [\[5\]](https://drive.google.com/file/d/15xXaaIcCWOc_x1gJLdySWOd_sfMXTiaw/view?usp=drivesdk),
    [\[4\]](https://drive.google.com/file/d/1FDBbIi2Rlw2In9mClB7Mub8oUPgx6y8h/view?usp=drivesdk).

5.  **Reproducibility**: ggplot code is easy to reuse and adapt for new
    datasets or visualizations, supporting reproducible research
    [\[1\]](https://drive.google.com/file/d/1BYSWJLROqxA1YpuDhJkzUolhiZqiOOKg/view?usp=drivesdk),
    [\[2\]](https://drive.google.com/file/d/1tFqKg9_nhVMmKYfiM1CQKDS2PmPwLh8n/view?usp=drivesdk),
    [\[3\]](https://drive.google.com/file/d/1Clw2_EJ_hY3USNwObiPnxpIQIfirxfW0/view?usp=drivesdk),
    [\[4\]](https://drive.google.com/file/d/1FDBbIi2Rlw2In9mClB7Mub8oUPgx6y8h/view?usp=drivesdk).

Which of these advantages do you think is most important for your work?
Why?
