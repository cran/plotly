[![Build Status](https://travis-ci.org/ropensci/plotly.png?branch=master)](https://travis-ci.org/ropensci/plotly)

# plotly

An R package for creating interactive web graphics via the open source JavaScript graphing library [plotly.js](https://github.com/plotly/plotly.js).

## Installation

Install from CRAN:

```r
install.packages("plotly")
```

Or install the latest development version (on GitHub) via devtools:

```r
devtools::install_github("ropensci/plotly")
```

**NOTE:** The CRAN version of **plotly** is designed to work with the CRAN version of **ggplot2**, but at least for the time being, we recommend using the development versions of both **plotly** and **ggplot2** (`devtools::install_github("hadley/ggplot2")`).

## Getting started

### Web-based ggplot2 graphics

If you use [ggplot2](https://github.com/hadley/ggplot2), `ggplotly()` converts your static plots to an interactive web-based version!

```r
library(plotly)
g <- ggplot(faithful, aes(x = eruptions, y = waiting)) +
  stat_density_2d(aes(fill = ..level..), geom = "polygon") + 
  xlim(1, 6) + ylim(40, 100)
ggplotly(g)
```

![http://i.imgur.com/G1rSArP.gifv](http://i.imgur.com/G1rSArP.gif)

By default, `ggplotly()` tries to replicate the static ggplot2 version exactly (before any interaction occurs), but sometimes you need greater control over the interactive behavior. The `ggplotly()` function itself has some convenient "high-level" arguments, such as `dynamicTicks`, which tells plotly.js to dynamically recompute axes, when appropriate. The `style()` function also comes in handy for _modifying_ the underlying [traces attributes](https://plot.ly/r/reference/#scatter-hoveron) used to generate the plot: 

```r
gg <- ggplotly(g, dynamicTicks = "y")
style(gg, hoveron = "points", hoverinfo = "x+y+text", hoverlabel = list(bgcolor = "white"))
```

![http://i.imgur.com/qRvLgea.gifv](http://imgur.com/qRvLgea.gif)

Moreover, since `ggplotly()` returns a plotly object, you can apply essentially any function from the R package on that object. Some useful ones include `layout()` (for [customizing the layout](https://cpsievert.github.io/plotly_book/extending-ggplotly.html#customizing-the-layout)), `add_traces()` (and its higher-level `add_*()` siblings, for example `add_polygons()`, for [adding new traces/data](https://cpsievert.github.io/plotly_book/extending-ggplotly.html#leveraging-statistical-output)), `subplot()` (for [combining multiple plotly objects](https://cpsievert.github.io/plotly_book/merging-plotly-objects.html)), and `plotly_json()` (for inspecting the underlying JSON sent to plotly.js).

The `ggplotly()` function will also respect some "unofficial" **ggplot2** aesthetics, namely `text` (for [customizing the tooltip](https://cpsievert.github.io/plotly_book/a-case-study-of-housing-sales-in-texas.html#fig:ggsubplot)), `frame` (for [creating animations](https://cpsievert.github.io/plotly_book/key-frame-animations.html)), and `ids` (for ensuring sensible smooth transitions). 

### Using plotly without ggplot2

The `plot_ly()` function provides a more direct interface to plotly.js so you can leverage more specialized chart types (e.g., [parallel coordinates](https://plot.ly/r/parallel-coordinates-plot/) or [maps](https://plot.ly/r/maps/)) or even some visualization that the ggplot2 API won't ever support (e.g., surface, [mesh](https://plot.ly/r/3d-mesh/), [trisurf](https://plot.ly/r/trisurf/), or sankey diagrams). The [cheatsheet](https://images.plot.ly/plotly-documentation/images/r_cheat_sheet.pdf) is a nice quick reference for this interface, but the [plotly cookbook](https://cpsievert.github.io/plotly_book/the-plotly-cookbook.html) has more complete overview of the philosophy behind this "non-ggplot2" approach.

```r
plot_ly(z = ~volcano, type = "surface")
```

![https://plot.ly/~brnvg/1134](https://plot.ly/~brnvg/1134.png)


## Crosstalk support

The R package has special support for linking/highlighting/filtering views that is not (yet) available outside of the R package. This functionality is built upon the [**crosstalk** package](https://rstudio.github.io/crosstalk/), which distinguishes between two event classes: *select* and *filter*. The **plotly** package interprets these classes in the following way:

1. **Select**: add new "selection" trace(s) (i.e., graphical marks) and *dim* the other traces. Some people refer to this as "brushing" or "highlighting".
2. **Filter**: retain "selection" trace(s), but *remove* other traces, and update the layout accordingly. Some people refer to this as "crossfilter" or "drill-down". 

The following gif helps to demonstrate the difference -- see [here](http://rpubs.com/cpsievert/275511) for the code used to generate it. 

![http://rpubs.com/cpsievert/275511](http://i.imgur.com/cYsuNQn.gif)

Like other [crosstalk enabled widgets](https://rstudio.github.io/crosstalk/widgets.html), **plotly** responds to filter events, but you can't (yet) emit a filter event via direct manipulation of a plotly graph. Unlike (some) other crosstalk enabled widgets, **plotly** has advanced support for select (a much more broad class than filter) events, like [persistent/dynamic brushing](https://cpsievert.github.io/plotly_book/linking-views-without-shiny.html#transient-versus-persistent-selection), [brushing via indirect manipulation](https://cpsievert.github.io/plotly_book/linking-views-without-shiny.html#selection-via-indirect-manipulation), and even ways to [completely control the appearance new selection traces](https://cpsievert.github.io/plotcon17/workshop/day2/#21). In other words, these "special" brushing features work when linking multiple **plotly** graphs, but may not when linking to other crosstalk enabled widgets. 

To date, [this slide deck](https://cpsievert.github.io/plotcon17/workshop/day2) is the most comprehensive, yet somewhat thorough, walk-through of this framework, but the [linking views without shiny](https://cpsievert.github.io/plotly_book/linking-views-without-shiny.html) chapter of the [plotly book](https://cpsievert.github.io/plotly_book/) provides even more background. There are also numerous demos shipped with the package which provide nice examples (list all the demos via `demo(package = "plotly")`).

**plotly**'s **crosstalk** functionality aims to provide tools for interactively exploring subsets of your data with a fixed definition from data to plot. If you need more flexibility, you can always embed **crosstalk** plots within a larger **shiny** app, or even [access and respond to any plotly event within shiny](https://cpsievert.github.io/plotly_book/linking-views-with-shiny.html), but adding **shiny** into the equation comes with a cost -- the result is no longer standalone HTML (harder to share/host) and linking views requires fairly sophicated knowledge/use of shiny's reactive programming framework.

## Learn more

We have lots of examples on <https://plot.ly/r/> and <https://plot.ly/ggplot2/>, but a more comprehensive review is also available at <https://cpsievert.github.io/plotly_book/>. I also have a number of [slide decks](http://cpsievert.github.io/talks/) that also have useful demos.

## Contributing

Please read through our [contributing guidelines](https://github.com/ropensci/plotly/blob/master/CONTRIBUTING.md). Included are directions for opening issues, asking questions, contributing changes to plotly, and our code of conduct. 

---

![http://ropensci.org](http://www.ropensci.org/public_images/github_footer.png)
