Lab 02 - Plastic waste
================
Ben Waggener
01/21/2025

## Load packages and data

``` r
library(tidyverse) 
```

``` r
plastic_waste <- read.csv("data/plastic-waste.csv")
```

## Exercises

### Exercise 1

Here is the plastic-waste-continent data plotted as a frequency graph.
This is data from 2010.

``` r
ggplot(data = plastic_waste, aes(x = plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.2)
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

Here is the dataset filtered so that it only shows countries with
plastic waste per capita that is greater than 3.5kg per person

``` r
plastic_waste %>%
  filter(plastic_waste_per_cap > 3.5)
```

    ##   code              entity     continent year gdp_per_cap plastic_waste_per_cap
    ## 1  TTO Trinidad and Tobago North America 2010    31260.91                   3.6
    ##   mismanaged_plastic_waste_per_cap mismanaged_plastic_waste coastal_pop
    ## 1                             0.19                    94066     1358433
    ##   total_pop
    ## 1   1341465

This shows that the outlier from the previous graph is Trinidad and
Tobago. This means that this country has a much much higher rate of
plastic waste per capita compared to the other countries in this
dataset. Quickly looking online, Trinidad and Tobago is known to have
issues with waste management however, this is a very extreme outlier so
I feel like it might be an error.

1.1

Below shows each continent as a density plot.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = plastic_waste_per_cap,
    color = continent
  )
) +
  geom_density()
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](lab-02_files/figure-gfm/desnsityplotbycontinent-1.png)<!-- -->

This graph is similar but has all the continents filled in.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = plastic_waste_per_cap,
    color = continent,
    fill = continent
  )
) +
  geom_density(alpha = 0.7)
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](lab-02_files/figure-gfm/unnamed-chunk-1-1.png)<!-- --> Most of the
data is clustered around (0,0) but there are serveral humps from South
America, north America, and Europe between .5 and .75 on the x axis (at
least i think so, i am colorblind). This graph also shows the outlier,
likely Trinidad and Tobago.

### Exercise 2

“2.1. Recreate the density plots above using a different (lower) alpha
level that works better for displaying the density curves for all
continents.”

Here is a density plot with a lower alpha (at .25) to better display the
data.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = plastic_waste_per_cap,
    color = continent,
    fill = continent
  )
) +
  geom_density(alpha = 0.25)
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

“2.2. Explain why we defined the color and fill of the curves by mapping
aesthetics of the plot but we defined the alpha level as a
characteristic of the plotting geom.”

My answer: ‘aes’ is used to communicate to R what data to use whereas
‘geom’ says how to display that data. In the graph above, the x axis
displays plastic waste per capita while color and fill display
continent. These are under the ‘aes’ function in R. Alpha refers to the
color transparency which is a quality of the data visualization rather
than identifying the data itself.

“And yet another way to visualize this relationship is using
side-by-side box plots.”

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = continent,
    y = plastic_waste_per_cap
  )
) +
  geom_boxplot()
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](lab-02_files/figure-gfm/sidebysideboxplots-1.png)<!-- -->

### Exercise 3

“Convert your side-by-side box plots from the previous task to violin
plots. What do the violin plots reveal that box plots do not? What
features are apparent in the box plots but not in the violin plots?”

Here is the side-by-side box plots as violin plots. The violin plots
show the distribution of data better. The violin plots make the
differences in data distribution (especially the shape near the mean)
easier to see compared to the box plots.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = continent,
    y = plastic_waste_per_cap
  )
) +
  geom_violin(
    mapping = NULL,
  data = NULL,
  stat = "ydensity",
  position = "dodge",
  draw_quantiles = NULL,
  trim = TRUE,
  bounds = c(-Inf, Inf),
  scale = "area",
  na.rm = FALSE,
  orientation = NA,
  show.legend = NA,
  inherit.aes = TRUE
  )
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

### Exercise 4

Remove this text, and add your answer for Exercise 4 here.

“4.1. Visualize the relationship between plastic waste per capita and
mismanaged plastic waste per capita using a scatter plot. Describe the
relationship.”

This plot shows the relationship between plastic waste per capita and
mismanaged plastic waste per capita through a scatter plot. This
relationship is generally positive however, there seems to be strict
boundaries where if a country has a certain amount of plastic waste,
they WILL have a minimum amount of mismanaged plastic waste.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = plastic_waste_per_cap,
    y = mismanaged_plastic_waste_per_cap)
  ) +
  geom_point()
```

    ## Warning: Removed 51 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

“4.2. Color the points in the scatterplot by continent. Does there seem
to be any clear distinctions between continents with respect to how
plastic waste per capita and mismanaged plastic waste per capita are
associated?”

``` r
ggplot(
  data = plastic_waste,
  mapping = aes(
    x = plastic_waste_per_cap,
    y = mismanaged_plastic_waste_per_cap)
  ) +
  geom_point(aes(color = continent))
```

    ## Warning: Removed 51 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-continent-1.png)<!-- -->

“4.3. Visualize the relationship between plastic waste per capita and
total population as well as plastic waste per capita and coastal
population. You will need to make two separate plots. Do either of these
pairs of variables appear to be more strongly linearly associated?”

This graph visualizes the relationship between plastic waste per capita
and total population.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes (
    x = plastic_waste_per_cap,
    y = total_pop)
  ) +
  geom_point (aes(color = continent))
```

    ## Warning: Removed 61 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

This graph visualizes the relationship between plastic waste per capita
and costal population.

``` r
ggplot(
  data = plastic_waste,
  mapping = aes (
    x = plastic_waste_per_cap,
    y = coastal_pop)
  ) +
  geom_point (aes(color = continent))
```

    ## Warning: Removed 51 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

Honestly, neither of these graphs appear much more linear than the
other. The first is more clustered near the bottom left of the graph but
other than a positive correlation and some outliers, there is not a lot
to say about these graphs.

### Exercise 5

The graph below (which removed one country with waste per capita over
3kg/day) shows that there is a positive trend between coastal population
proportion and plastic waste per capita until the coastal population
proportion reaches around 1.

``` r
filtered_plastic_waste <- plastic_waste %>%
  filter(plastic_waste_per_cap < 3)

ggplot(
  data = filtered_plastic_waste,
  mapping = aes (
    x = (coastal_pop / total_pop),
    y = plastic_waste_per_cap)
  ) +
  geom_point (aes(color = continent)) +
  
  geom_smooth (color = "black") +
  labs(
    title = "Plastic waste vs. coastal population proportion",
    subtitle = "by continent",
    x = "Coastal population proportion (Coastal / total population)", y = "Plastic waste per capita",
    
  )
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 10 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/recreate-viz-1.png)<!-- -->
