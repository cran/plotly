[![Build Status](https://travis-ci.org/ropensci/plotly.png?branch=master)](https://travis-ci.org/ropensci/plotly)

# plotly

An R package for creating interactive web graphics via [plotly.js](https://github.com/plotly/plotly.js).

## Installation

Install from CRAN:

```r
install.packages("plotly")
```

Or install the latest development version (on GitHub) via devtools:

```r
devtools::install_github("ropensci/plotly")
```

## Getting Started

### Web-based ggplot2 graphics

If you use [ggplot2](http://cran.r-project.org/package=ggplot2), `ggplotly()` converts your plots to an interactive, web-based version! It also provides sensible tooltips, which assists decoding of values encoded as visual properties in the plot.

```r
library(plotly)
g <- ggplot(faithful, aes(x = eruptions, y = waiting)) +
  stat_density_2d(aes(fill = ..level..), geom = "polygon") + 
  xlim(1, 6) + ylim(40, 100)
ggplotly(g)
```

![https://plot.ly/~cpsievert/9836](http://i.imgur.com/6G4zv7b.png)

If you'd like to see how `ggplotly()` does in converting different ggplot2 examples, we host a [plotly version](http://ropensci.github.io/plotly/) of the [official ggplot2 documentation](http://docs.ggplot2.org). We also have some of our own examples [here](https://plot.ly/ggplot2/).

### plotly's custom R interface

__plotly__ supports some chart types that ggplot2 doesn't (such as 3D [surface](https://plot.ly/r/3d-surface-plots/), [point](https://plot.ly/r/3d-scatter-plots/), and [line](https://plot.ly/r/3d-line-plots/) plots). You can create these (or any other plotly) charts using `plot_ly()`. 

```r
plot_ly(z = volcano, type = "surface")
```

![https://plot.ly/~brnvg/1134](https://plot.ly/~brnvg/1134.png)

For a more concise overview of the `plot_ly()` interface, see [here](https://cran.r-project.org/web/packages/plotly/vignettes/intro.html). You might also find our [examples](https://plot.ly/r/#basic-charts) and the [figure reference](https://plot.ly/r/reference/) to be helpful.

### Hooking onto plotly events

[plotly.js](https://github.com/plotly/plotly.js) exposes a number of 'standard' events that work consistently across plot types. It's easy to hook into these events using the `event_data()` function in shiny apps, as these examples demonstrate:

1. [2D events](http://104.131.111.111:3838/plotlyEvents/) ([source](https://github.com/ropensci/plotly/tree/master/inst/examples/plotlyEvents))
2. [Linked Clicks](http://104.131.111.111:3838/plotlyLinkedClick/) ([source](https://github.com/ropensci/plotly/tree/master/inst/examples/plotlyLinkedClick))
3. [Linked Brush](http://104.131.111.111:3838/plotlyLinkedBrush/) ([source](https://github.com/ropensci/plotly/tree/master/inst/examples/plotlyLinkedBrush))

![http://104.131.111.111:3838/plotlyLinkedBrush/](http://i.imgur.com/eVqsZma.gif)

You can also hook into these events without shiny using `htmlwidgets::onRender()` ([example](https://github.com/ropensci/plotly/tree/master/inst/examples/onRenderHover)). This, however, requires JavaScript knowledge and makes it much harder, if not impossible, to coordinate views between htmlwidgets.

## Contributing

Please read through our [contributing guidelines](https://github.com/ropensci/plotly/blob/master/CONTRIBUTING.md). Included are directions for opening issues, asking questions, contributing changes to plotly, and our code of conduct. 

---

[![](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
