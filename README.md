---
title: 'plotly'
author: "The Johns Hopkins Data Science Lab"
always_allow_html: yes
---

## What is Plotly?

Plotly is a web application for creating and sharing data visualizations. Plotly
can work with several programming languages and applications including R,
Python, and Microsoft Excel. We're going to concentrate on creating different
graphs with Plotly and sharing those graphs.

## Installing Plotly

Installing Plotly is easy:

```r
install.packages("plotly")
library(plotly)
```

If you want to share your visualizations on https://plot.ly/ you should make an
account on their site.

## Basic Scatterplot

A basic scatterplot is easy to make, with the added benefit of tooltips that
appear when your mouse hovers over each point. Specify a scatterplot by
indicating `type = "scatter"`. Notice that the arguments for the `x` and `y` 
variables as specified as formulas, with the tilde operator (`~`) preceding the
variable that you're plotting.


```{r, eval=FALSE}
library(plotly)
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter")
```

## Basic Scatterplot

```{r, echo=FALSE, message=FALSE}
library(plotly)
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter")
```

## Scatterplot Color

You can add color to your scatterplot points according to a categorical variable
in the data frame you use with `plot_ly()`.

```{r, eval=FALSE}
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter", color = ~factor(cyl))
```

## Scatterplot Color

```{r, echo=FALSE, message=FALSE}
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter", color = ~factor(cyl))
```

## Continuous Color

You can also show continuous variables with color in scatterplots.

```{r, eval=FALSE}
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter", color = ~disp)
```

## Continuous Color

```{r, echo=FALSE, message=FALSE}
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter", color = ~disp)
```

## Scatterplot Sizing

By specifying the `size` argument you can make each point in your scatterplot a
different size.

```{r, eval=FALSE}
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter", 
        color = ~factor(cyl), size = ~hp)
```

## Scatterplot Sizing

```{r, echo=FALSE, message=FALSE}
plot_ly(mtcars, x = ~wt, y = ~mpg, type = "scatter", 
        color = ~factor(cyl), size = ~hp)
```

## 3D Scatterplot

You can create a three-dimensional scatterplot with the `type = "scatter3d"`
argument. If you click and drag these scatterplots you can view them from
different angles.

```{r, eval=FALSE}
set.seed(2016-07-21)
temp <- rnorm(100, mean = 30, sd = 5)
pressue <- rnorm(100)
dtime <- 1:100
plot_ly(x = ~temp, y = ~pressue, z = ~dtime,
        type = "scatter3d", color = ~temp)
```

## 3D Scatterplot

```{r, echo=FALSE, message=FALSE}
set.seed(2016-07-21)
temp <- rnorm(100, mean = 30, sd = 5)
pressue <- rnorm(100)
dtime <- 1:100
plot_ly(x = ~temp, y = ~pressue, z = ~dtime,
        type = "scatter3d", color = ~temp)
```

## Line Graph

Line graphs are the default graph for `plot_ly()`. They're of course useful for
showing change over time:

```{r, eval=FALSE}
data("airmiles")
plot_ly(x = ~time(airmiles), y = ~airmiles, type = "scatter", mode = "lines")
```

## Line Graph

```{r, echo=FALSE, message=FALSE}
data("airmiles")
plot_ly(x = ~time(airmiles), y = ~airmiles, type = "scatter", mode = "lines")
```

## Multi Line Graph

You can show multiple lines by specifying the column in the data frame that
separates the lines:

```{r, eval=FALSE}
library(plotly)
library(tidyr)
library(dplyr)
data("EuStockMarkets")
stocks <- as.data.frame(EuStockMarkets) %>%
  gather(index, price) %>%
  mutate(time = rep(time(EuStockMarkets), 4))
plot_ly(stocks, x = ~time, y = ~price, color = ~index, type = "scatter", mode = "lines")
```

## Multi Line Graph

```{r, echo=FALSE, message=FALSE}
library(plotly)
library(tidyr)
library(dplyr)
data("EuStockMarkets")
stocks <- as.data.frame(EuStockMarkets) %>%
  gather(index, price) %>%
  mutate(time = rep(time(EuStockMarkets), 4))
plot_ly(stocks, x = ~time, y = ~price, color = ~index, type = "scatter", mode = "lines")
```

## Histogram

A histogram is great for showing how counts of data are distributed. Use the
`type = "histogram"` argument.

```{r, eval=FALSE}
plot_ly(x = ~precip, type = "histogram")
```

## Histogram

```{r, echo=FALSE, message=FALSE}
plot_ly(x = ~precip, type = "histogram")
```

## Boxplot

Boxplots are wonderful for comparing how different datasets are distributed.
Specify `type = "box"` to create a boxplot.

```{r, eval=FALSE}
plot_ly(iris, y = ~Petal.Length, color = ~Species, type = "box")
```

## Boxplot

```{r, echo=FALSE, message=FALSE}
plot_ly(iris, y = ~Petal.Length, color = ~Species, type = "box")
```

## Heatmap

Heatmaps are useful for displaying three dimensional data in two dimensions,
using color for the third dimension. Create a heatmap by using the
`type = "heatmap"` argument.

```{r, eval=FALSE}
terrain1 <- matrix(rnorm(100*100), nrow = 100, ncol = 100)
plot_ly(z = ~terrain1, type = "heatmap")
```

## Heatmap

```{r, echo=FALSE, message=FALSE}
terrain1 <- matrix(rnorm(100*100), nrow = 100, ncol = 100)
plot_ly(z = ~terrain1, type = "heatmap")
```

## 3D Surface

Why limit yourself to two dimensions when you can render three dimensions on a
computer!? Create moveable 3D surfaces with `type = "surface"`.

```{r, eval=FALSE}
terrain2 <- matrix(sort(rnorm(100*100)), nrow = 100, ncol = 100)
plot_ly(z = ~terrain2, type = "surface")
```

## 3D Surface

```{r, echo=FALSE, message=FALSE}
terrain2 <- matrix(sort(rnorm(100*100)), nrow = 100, ncol = 100)
plot_ly(z = ~terrain2, type = "surface")
```

## Choropleth Maps: Setup

Choropleth maps illustrate data across geographic areas by shading regions with
different colors. Choropleth maps are easy to make with Plotly though they
require more setup compared to other Plotly graphics.

```{r, eval=FALSE}
# Create data frame
state_pop <- data.frame(State = state.abb, Pop = as.vector(state.x77[,1]))
# Create hover text
state_pop$hover <- with(state_pop, paste(State, '<br>', "Population:", Pop))
# Make state borders white
borders <- list(color = toRGB("red"))
# Set up some mapping options
map_options <- list(
  scope = 'usa',
  projection = list(type = 'albers usa'),
  showlakes = TRUE,
  lakecolor = toRGB('white')
)
```

## Choropleth Maps: Mapping

```{r, eval=FALSE}
plot_ly(z = ~state_pop$Pop, text = ~state_pop$hover, locations = ~state_pop$State, 
        type = 'choropleth', locationmode = 'USA-states', 
        color = state_pop$Pop, colors = 'Blues', marker = list(line = borders)) %>%
  layout(title = 'US Population in 1975', geo = map_options)
```

## Choropleth Maps

```{r, echo=FALSE, message=FALSE}
# Create data frame
state_pop <- data.frame(State = state.abb, Pop = as.vector(state.x77[,1]))
# Create hover text
state_pop$hover <- with(state_pop, paste(State, '<br>', "Population:", Pop))
# Make state borders white
borders <- list(color = toRGB("red"))
# Set up some mapping options
map_options <- list(
  scope = 'usa',
  projection = list(type = 'albers usa'),
  showlakes = TRUE,
  lakecolor = toRGB('white')
)
plot_ly(z = ~state_pop$Pop, text = ~state_pop$hover, locations = ~state_pop$State, 
        type = 'choropleth', locationmode = 'USA-states', 
        color = state_pop$Pop, colors = 'Blues', marker = list(line = borders)) %>%
  layout(title = 'US Population in 1975', geo = map_options)
```

## More Resources

- [The Plolty Website](https://plot.ly/)
- [The Plotly R API](https://plot.ly/r/)
- [The Plotly R Package on GitHub](https://github.com/ropensci/plotly)
- [The Plotly R Cheatsheet](https://images.plot.ly/plotly-documentation/images/r_cheat_sheet.pdf)
- ["Plotly for R" book by Carson Sievert](https://cpsievert.github.io/plotly_book/)
