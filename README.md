# Plotting_Intro_R
This is an intro to plotting with R, and will introduce basic ggplot techniques as well as how to intregrate with Plotly.

<img src="/pics/plot_examples.png" width="800">  
     
       
       

&nbsp;   
&nbsp; 
### There are a lot of Intros on ggplot2 available, I put together a brief overview that takes no more than 1 hour to work through, but I highly reccomend also considering the following tutorials for a more in depth training:
1. http://r-statistics.co/Complete-Ggplot2-Tutorial-Part1-With-R-Code.html  
2. https://rpubs.com/arvindpdmn/ggplot2-basics  
3. https://bookdown.org/skaltman/visualization-book/the-basics-of-ggplot2.html  

*Some information and examples described below are from the above links*   
  
&nbsp;  
&nbsp;  

<img src="/pics/logo_ggplot2.png" width="200">  

## The Basics: What is ggplot2?

ggplot2 is an R package that allows for customizable data visualization. It is part of the tidyverse package suite and is a very well maintained and continuously updated package. https://ggplot2.tidyverse.org/

To use ggplot2, you will need to have access to `R`, and I recommend following along with `RStudio`. You can use a local installation, a server installation (https://igm-rstudio.crii.org/).

To install ggplot2 you can do the following:

#### The easiest way to get ggplot2 is to install the whole tidyverse:
```
install.packages("tidyverse")
```
*tidyverse includes popular R packages such as dplyr, readr, tibble, and others. They follow a tidy data philosophy: https://www.tidyverse.org/*

#### Alternatively, install just ggplot2:
```
install.packages("ggplot2")
```

#### As always, you must load the package

The easiest way is to just `library` the package
```
library("ggplot2")
```

#### You are now ready to work with ggplot2, see cheat sheets below to get started with the basics

ggplot2 cheat sheets: &nbsp;  
<img src="/pics/datavis_1.png" width="400">  
<img src="/pics/datavis_2.png" width="400">  
https://ggplot2.tidyverse.org/index.html



# Basic ggplot2 concepts:

ggplot2 was developed by Hadley Wickham (developer of the Tidyverse).    
<img src="/pics/hadley_wickham.jpeg" width="600">  

ggplot2 is different than most graphics packages, it uses underlying grammar that is based on the ["Grammar of Graphics"](https://link.springer.com/chapter/10.1007/978-3-642-21551-3_13). This allows the user to compose graphs by combining independent components. One can think about the plots as having layers of data.   
&nbsp;   

### The Grammar of Graphics  
<img src="/pics/grammar_of_graphics.png" width="800">
The Grammar of Graphics. Visual by Thomas de Beus
&nbsp; 
  
All plots are composed of the __data__, the information you want to visualise, and a __mapping__, the description of how the data’s variables are mapped to __aesthetic__ attributes. There are five mapping components:

- A __layer__ is a collection of geometric elements and statistical transformations. Geometric elements, geoms for short, represent what you actually see in the plot: points, lines, polygons, etc. Statistical transformations, stats for short, summarise the data: for example, binning and counting observations to create a histogram, or fitting a linear model.

- __Scales__ map values in the data space to values in the aesthetic space. This includes the use of colour, shape or size. Scales also draw the legend and axes, which make it possible to read the original data values from the plot (an inverse mapping).

- A __coord__, or coordinate system, describes how data coordinates are mapped to the plane of the graphic. It also provides axes and gridlines to help read the graph. We normally use the Cartesian coordinate system, but a number of others are available, including polar coordinates and map projections.

- A __facet__ specifies how to break up and display subsets of data as small multiples. This is also known as conditioning or latticing/trellising.

- A __theme__ controls the finer points of display, like the font size and background colour. While the defaults in ggplot2 have been chosen with care, you may need to consult other references to create an attractive plot. A good starting place is Edward Tufte’s early works.  

From: https://ggplot2-book.org/introduction.html


```diff
- The hardest part may be forgetting the preconceptions you brought over from using other graphics tools
```

### There are three main components to every ggplot graph:
#### 1. Data (usually as a datafram)
#### 2. Plot Aesthetics (indicates x and y variables, colors, shapes, sizes, etc.)
#### 3. Geometry (boxplot, scatter plot, violin plot, etc.)

### The main ggplot2 function is:
`ggplot()`

### Generated plots can be kept as variables, printed out at anytime, or saved using:
`ggsave(“plot.png”, width = 5, height = 5)` which saves the last plot in the current working directory.

### ggplot2 can generate the following types of plots (which are dependant on your type of data):
1. One variable - x: continuous or discrete
2. Two variables - x & y: continuous and/or discrete
3. Continuous bivariate distribution - x & y (both continuous)
4. Continuous function
5. Error bar
6. Maps
7. Three variables

## Lets try a few basic plots using a dataset that is part of the ggplot2 package
The below examples are modified from: https://ggplot2-book.org/getting-started.html

load the packages
```
library(ggplot2)
```
Check the following `variable`

```
mpg
```

printed to screen:
```
#> # A tibble: 234 × 11
#>   manufacturer model displ  year   cyl trans      drv     cty   hwy fl    class 
#>   <chr>        <chr> <dbl> <int> <int> <chr>      <chr> <int> <int> <chr> <chr> 
#> 1 audi         a4      1.8  1999     4 auto(l5)   f        18    29 p     compa…
#> 2 audi         a4      1.8  1999     4 manual(m5) f        21    29 p     compa…
#> 3 audi         a4      2    2008     4 manual(m6) f        20    31 p     compa…
#> 4 audi         a4      2    2008     4 auto(av)   f        21    30 p     compa…
#> 5 audi         a4      2.8  1999     6 auto(l5)   f        16    26 p     compa…
#> 6 audi         a4      2.8  1999     6 manual(m5) f        18    26 p     compa…
#> # … with 228 more rows
```

The variables are mostly self-explanatory:

`cty` and `hwy` record miles per gallon (mpg) for city and highway driving.

`displ` is the engine displacement in litres.

`drv` is the drivetrain: front wheel (f), rear wheel (r) or four wheel (4).

`model` is the model of car. There are 38 models, selected because they had a new edition every year between 1999 and 2008.

`class` is a categorical variable describing the “type” of car: two seater, SUV, compact, etc.

This dataset suggests many interesting questions. __How are engine size and fuel economy related?__ __Do certain manufacturers care more about fuel economy than others?__ __Has fuel economy improved in the last ten years?__ We will try to answer some of these questions, and in the process learn how to create some basic plots with ggplot2.


First, lets look at the mpg as our `data`, where our x is engine size and our y is fuel economy

To do so, lets create a scatterplot defined by:

__Data__: mpg.

__Aesthetic mapping__: engine size mapped to x position, fuel economy to y position.
__Layer__: points.
  
__Pay attention to the structure of this function call__: data and aesthetic mappings are supplied in ggplot(), then layers are added on with `+`. *This is an important pattern*, and as you learn more about ggplot2 you’ll construct increasingly sophisticated plots by adding on more types of components.

``
ggplot(mpg, aes(x = displ, y = hwy)) + geom_point()
``


__Almost every plot maps a variable to x and y, and naming these aesthetics is tedious, so the first two unnamed arguments to aes() will be mapped to x and y.This means that the following code is identical to the example above:__


``
ggplot(mpg, aes(displ, hwy)) + geom_point()
``

When you run either of the above commands, you will see this:  
<img src="/pics/example1.png" width="400">  
  
So lets dig into our data a bit more... 

To learn more about those outlying variables in the previous scatterplot, we could map the class variable to colour:

``
ggplot(mpg, aes(displ, hwy, colour = class)) + geom_point()
``

<img src="/pics/example2.png" width="500">  

This gives each point a unique colour corresponding to its class. The legend allows us to read data values from the colour, showing us that the group of cars with unusually high fuel economy for their engine size are two seaters: cars with big engines, but lightweight bodies.

If you want to set an aesthetic to a fixed value, without scaling it, do so in the individual layer outside of aes(). Compare the following two plots:
  
``
ggplot(mpg, aes(displ, hwy)) + geom_point(aes(colour = "blue"))
``   
<img src="/pics/example3.png" width="500">  

Oh no what happened?! Try the following instead...  


``
ggplot(mpg, aes(displ, hwy)) + geom_point(colour = "blue")
``     
<img src="/pics/example4.png" width="500">  
  
In the first plot, the value `blue` is scaled to a pinkish colour, and a legend is added. In the second plot, the points are given the R colour blue. This is an important technique: __Setting vs. mapping__. 
  
Instead of mapping an `aesthetic` property to a `variable`, you can set it to a single value by specifying it in the `layer parameters`. *We map an aesthetic to a variable (e.g., aes(colour = cut)) or set it to a constant (e.g., colour = "red").* If you want appearance to be governed by a variable, __put the specification inside aes()__; if you want override the default size or colour, __put the value outside of aes()__.

The above plots are created with similar code, but have rather __different outputs__. 

The first plot `maps` (not `sets`) the colour to the value ‘blue’. This effectively creates a __new variable__ containing only the value ‘blue’ and then scales it with a colour scale. Because this value is discrete, the default colour scale uses evenly spaced colours on the colour wheel, and since there is only one value this colour is pinkish.

Equivalently to the second plot, one could do the following by mapping to the value, but override the default scale:

``
ggplot(mpg, aes(dslp, hwy)) + 
  geom_point(aes(colour = "blue")) + 
  scale_colour_identity()
``
  
<img src="/pics/example4.png" width="500">  
  
see vignette("ggplot2-specs") for the values needed for colour and other aesthetics.

*Different types of aesthetic attributes work better with different types of variables.* For example, __colour__ and __shape__ work well with *categorical variables*, while __size__ works well for *continuous variables*. The amount of data also makes a difference: if there is a lot of data it can be hard to distinguish different groups. An alternative solution is to use `faceting`, as described next.

When using `aesthetics` in a plot, __less is usually more__. It’s difficult to see the simultaneous relationships among colour and shape and size, so *exercise restraint when using aesthetics*. Instead of trying to make one very complex plot that shows everything at once, see if you can create a series of simple plots that tell a story, leading the reader from ignorance to knowledge.


## Lets add a smoother to our plot!

```
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() + 
  geom_smooth()
```

An important argument to geom_smooth() is the method, which allows you to choose which type of model is used to fit the smooth curve:

method = "loess", the default for small n, uses a smooth local regression (as described in ?loess). The wiggliness of the line is controlled by the span parameter, which ranges from 0 (exceedingly wiggly) to 1 (not so wiggly).

```
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() + 
  geom_smooth(span = 0.2)
```  
<img src="/pics/example6.png" width="500">  




```
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() + 
  geom_smooth(span = 1)
```  
<img src="/pics/example7.png" width="500">  

  
It’s sometimes useful to map aesthetics to constants. For example, if you want to display multiple layers with varying parameters, you can “name” each layer:

```
ggplot(mpg, aes(dslp, hwy)) + 
  geom_point() +
  geom_smooth(aes(colour = "loess"), method = "loess", se = FALSE) + 
  geom_smooth(aes(colour = "lm"), method = "lm", se = FALSE) +
  labs(colour = "Method")
```
<img src="/pics/example5_u.png" width="500">  


## Lets take a look at boxplots and jitterplots

When a set of data includes a categorical variable and one or more continuous variables, you will probably be interested to know how the values of the continuous variables vary with the levels of the categorical variable. Say we’re interested in seeing how fuel economy varies within cars that have the same kind of drivetrain. We might start with a scatterplot like this:


```
ggplot(mpg, aes(drv, hwy)) + 
  geom_point()
```
<img src="/pics/example8.png" width="500">  

There are few unique values for `drv` and `hwy`, __so__ there is a lot of overplotting.   
  
Many points are plotted in the same location, making it difficult to see the distribution.   
  
There are three useful techniques that help alleviate the problem:

- __Jittering__, `geom_jitter()`, adds a little random noise to the data which can help avoid overplotting.

- __Boxplots__, `geom_boxplot()`, summarise the shape of the distribution with a handful of summary statistics.

- __Violin plots__, `geom_violin()`, show a compact representation of the “density” of the distribution, highlighting the areas where more points are found.

*These are illustrated below:*

```
ggplot(mpg, aes(drv, hwy)) + geom_jitter()
ggplot(mpg, aes(drv, hwy)) + geom_boxplot()
ggplot(mpg, aes(drv, hwy)) + geom_violin()
```
<img src="/pics/example9.png" width="500">  

For jittered points, `geom_jitter()` offers the same control over aesthetics as `geom_point()`: `size`, `colour`, and `shape`. For `geom_boxplot()` and `geom_violin()`, you can control the outline colour or the internal fill colour.
  
## Histograms and frequency polygons

Histograms and frequency polygons show the distribution of a single numeric variable. They provide more information about the distribution of a single group than boxplots do, at the expense of needing more space.

```
ggplot(mpg, aes(hwy)) + geom_histogram()
ggplot(mpg, aes(hwy)) + geom_freqpoly()
```
<img src="/pics/example10.png" width="500"> 

To compare the distributions of different subgroups, you can map a categorical variable to either `fill` (for `geom_histogram()`) or `colour` (for `geom_freqpoly()`). It’s easier to compare distributions using the frequency polygon because the underlying perceptual task is easier. You can also use faceting: this makes comparisons a little harder, but it’s easier to see the distribution of each group.  

```
ggplot(mpg, aes(displ, colour = drv)) + 
  geom_freqpoly(binwidth = 0.5)
ggplot(mpg, aes(displ, fill = drv)) + 
  geom_histogram(binwidth = 0.5) + 
  facet_wrap(~drv, ncol = 1)
```
<img src="/pics/example11.png" width="500"> 

## Lets talk colors!

There have been many attempts to come up with colours spaces that are more perceptually uniform. We’ll use a modern attempt called the __HCL colour space__, which has three components of hue, chroma and luminance:

- __Hue__ ranges from 0 to 360 (an angle) and gives the “colour” of the colour (blue, red, orange, etc).
- __Chroma__ is the “purity” of a colour, ranging from 0 (grey) to a maximum that varies with luminance.
- __Luminance__ is the lightness of the colour, ranging from 0 (black) to 1 (white).
  
The three dimensions have different properties. Hues are arranged around a colour wheel and are not perceived as ordered: e.g. green does not seem “larger” than red, and blue does not seem to be “in between” green or red. In contrast, both chroma and luminance are perceived as ordered: pink is perceived as lying between red and white, and grey is seen to fall between black and white.
   
   
*It’s best to avoid red-green contrasts, and to check your plots with systems that simulate colour blindness. Visicheck is one online solution, another alternative is the dichromat package, which provides tools for simulating colour blindness, and a set of colour schemes known to work well for colour-blind people.*

  
### Continuous Color Scales

Colour gradients are often used to show the height of a 2d surface. The plots in this section use the surface of a 2d density estimate of the faithful dataset,35 which records the waiting time between eruptions and during each eruption for the Old Faithful geyser in Yellowstone Park. I hide the legends and set expand to 0, to focus on the appearance of the data. Remember: although I use the erupt plot to illustrate concepts using with a fill aesthetic, the same ideas apply to colour scales. Any time I refer to scale_fill_*() in this section there is a corresponding scale_colour_*() for the colour aesthetic (or scale_color_*() if you prefer US spelling).

```
erupt <- ggplot(faithfuld, aes(waiting, eruptions, fill = density)) +
  geom_raster() +
  scale_x_continuous(NULL, expand = c(0, 0)) + 
  scale_y_continuous(NULL, expand = c(0, 0)) + 
  theme(legend.position = "none")
  
erupt
erupt + scale_fill_viridis_c()
erupt + scale_fill_viridis_c(option = "magma")
```
<img src="/pics/example12.png" width="500"> 


```
erupt + scale_fill_distiller()
erupt + scale_fill_distiller(palette = "RdPu")
erupt + scale_fill_distiller(palette = "YlOrBr")
```
<img src="/pics/example13.png" width="500"> 

### Discrete Color Scales

Discrete colour and fill scales occur in many situations. A typical example is a barchart that encodes both position and fill to the same variable.

```
df <- data.frame(x = c("a", "b", "c", "d"), y = c(3, 4, 1, 2))
bars <- ggplot(df, aes(x, y, fill = x)) + 
  geom_bar(stat = "identity") + 
  labs(x = NULL, y = NULL) +
  theme(legend.position = "none")
```

The default scale for discrete colours is scale_fill_discrete() which in turn defaults to scale_fill_hue() so these are identical plots:

```
bars
bars + scale_fill_discrete()
bars + scale_fill_hue()
```
<img src="/pics/example14.png" width="500"> 
  

`scale_colour_brewer()` is a discrete colour scale that—along with the continuous analog `scale_colour_distiller()` and binned analog `scale_colour_fermenter()`—uses handpicked “ColorBrewer” colours taken from http://colorbrewer2.org/. These colours have been designed to work well in a wide variety of situations, although the focus is on maps and so the colours tend to work better when displayed in large areas. There are many different options:

``RColorBrewer::display.brewer.all()``

The first group of palettes are sequential scales that are useful when your discrete scale is ordered (e.g., rank data), and are available for continuous data using scale_colour_distiller(). For unordered categorical data, the palettes of most interest are those in the second group. ‘Set1’ and ‘Dark2’ are particularly good for points, and ‘Set2’, ‘Pastel1’, ‘Pastel2’ and ‘Accent’ work well for areas.

```
bars + scale_fill_brewer(palette = "Set1")
bars + scale_fill_brewer(palette = "Set2")
bars + scale_fill_brewer(palette = "Accent")
```
<img src="/pics/example15.png" width="500"> 

## Lets get festive! One more fun plot!

See original publication here: https://t-redactyl.io/blog/2016/12/a-very-ggplot2-christmas.html

```
read.csv("/igm/projects/intro_to_ggplot/ChristmasTree.csv")
library(ggplot2)

tree <- ggplot() + 
          geom_tile(data = ChristmasTree, aes(x = Tree.X, y = Tree.Y, fill = Tree.Colour)) +       
          scale_fill_identity() + 
          theme_bw() +
          scale_x_continuous(breaks = NULL) + 
          scale_y_continuous(breaks = NULL) +
          labs(x = "", y = "")
tree
```
