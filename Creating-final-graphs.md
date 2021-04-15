---
title: "Creating final graphs using ggplot2"
author: "Jocelyn Mara PhD, University of Canberra"
output: 
  prettydoc::html_pretty:
    theme: hpstr
    highlight: github
    keep_md: true
---



## Overview

This lesson will teach you the elements of final graphs and how to produce these using R code. Let's start by defining what we mean by a **final graph**. Final graphs are those figures that we are going to use to communicate our findings (from our data analysis) to a variety of audiences. That is, these will be the *prettified* graphs that you include in final reports, infographics, and presentations. 

Earlier on in the course you learned how to use plots to visually explore your data. A number of the principles we spoke about will remain the same (e.g. the types of graphs you can use to show distributions, compare groups, show relationships etc depending on whether you have continuous or categorical data), however when we explore data we will usually be the only one who looks at it, and then we move on to creating the next graph.

Final graphs needs to be able to tell the story of our analysis to audiences that haven't been on the full data analysis journey like we have. They need to be able to communicate the key findings clearly, and an effective graph will be able to communicate these key findings in isolation of a textual description (with the exception of a figure caption).

That being said, there are a number of elements that final graphs need to include in order to be effective. We will be going through these elements in this lesson.


## The data

We will use AFL data from 1991-2020 which has been sourced from the [`fitzRoy`](https://github.com/jimmyday12/fitzRoy) package^[James Day, Robert Nguyen and Oscar Lane (2019). fitzRoy: Easily Scrape and Process AFL Data. R package version 0.2.0.9000. https://github.com/jimmyday12/fitzRoy]. You should remember this data from our lessons on Exploratory Data Analysis earlier on in the course. 

To kick off, load the tidyverse:


```r
library(tidyverse)
```

Next, read in the data and check the structure of it to re-familiarise yourself with this dataset. 


```r
dat <- read_csv("data/1991-2020_afl_team_data_tidy.csv")
str(dat)
```

```
## spec_tbl_df[,45] [5,483 × 45] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Season                 : num [1:5483] 1991 1991 1991 1991 1991 ...
##  $ Round                  : num [1:5483] 1 1 1 1 2 2 2 2 2 2 ...
##  $ Home.team              : chr [1:5483] "Adelaide" "St Kilda" "West Coast" "Western Bulldogs" ...
##  $ Away.team              : chr [1:5483] "Hawthorn" "Richmond" "Melbourne" "Collingwood" ...
##  $ Attendance             : num [1:5483] 44902 33192 26105 38861 43850 ...
##  $ HQ1G                   : num [1:5483] 6 2 3 7 3 2 1 1 3 10 ...
##  $ HQ1B                   : num [1:5483] 2 4 2 3 2 2 4 4 5 3 ...
##  $ HQ2G                   : num [1:5483] 11 7 3 9 5 4 3 3 7 13 ...
##  $ HQ2B                   : num [1:5483] 6 5 4 6 3 4 6 7 10 7 ...
##  $ HQ3G                   : num [1:5483] 17 10 10 11 9 6 5 4 13 17 ...
##  $ HQ3B                   : num [1:5483] 10 8 6 8 6 7 8 11 18 10 ...
##  $ HQ4G                   : num [1:5483] 24 16 14 11 12 8 8 6 18 25 ...
##  $ HQ4B                   : num [1:5483] 11 11 15 10 9 10 10 13 22 16 ...
##  $ Home.score             : num [1:5483] 155 107 99 76 81 58 58 49 130 166 ...
##  $ AQ1G                   : num [1:5483] 2 2 0 2 3 5 0 6 7 2 ...
##  $ AQ1B                   : num [1:5483] 0 2 0 3 4 4 1 5 0 2 ...
##  $ AQ2G                   : num [1:5483] 3 6 0 4 5 7 5 15 11 7 ...
##  $ AQ2B                   : num [1:5483] 3 3 1 11 9 7 2 10 4 6 ...
##  $ AQ3G                   : num [1:5483] 6 9 1 11 11 12 11 21 14 9 ...
##  $ AQ3B                   : num [1:5483] 8 8 5 15 10 11 4 17 5 8 ...
##  $ AQ4G                   : num [1:5483] 9 12 2 21 15 15 13 27 16 10 ...
##  $ AQ4B                   : num [1:5483] 15 10 8 20 14 14 7 18 8 15 ...
##  $ Away.score             : num [1:5483] 69 82 20 146 104 104 85 180 104 75 ...
##  $ Kicks                  : num [1:5483] 449 420 433 416 394 446 433 413 398 393 ...
##  $ Marks                  : num [1:5483] 168 151 115 143 116 129 130 150 118 125 ...
##  $ Handballs              : num [1:5483] 261 214 190 251 286 154 173 195 181 209 ...
##  $ Goals                  : num [1:5483] 33 28 16 32 27 23 21 33 34 35 ...
##  $ Behinds                : num [1:5483] 21 17 19 25 17 20 16 30 25 31 ...
##  $ Hit.Outs               : num [1:5483] 24 23 59 26 36 27 35 31 41 50 ...
##  $ Tackles                : num [1:5483] 33 44 31 40 77 28 39 30 46 60 ...
##  $ Rebounds               : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Inside.50s             : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Clearances             : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Clangers               : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Frees.For              : num [1:5483] 48 51 56 39 48 45 62 51 53 51 ...
##  $ Frees.Against          : num [1:5483] 48 51 56 39 48 45 62 51 53 51 ...
##  $ Brownlow.Votes         : num [1:5483] 6 6 6 6 6 6 6 6 6 6 ...
##  $ Contested.Possessions  : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Uncontested.Possessions: num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Contested.Marks        : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Marks.Inside.50        : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ One.Percenters         : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Bounces                : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Goal.Assists           : num [1:5483] 0 0 0 0 0 0 0 0 0 0 ...
##  $ match_outcome          : chr [1:5483] "Win" "Win" "Win" "Loss" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Season = col_double(),
##   ..   Round = col_double(),
##   ..   Home.team = col_character(),
##   ..   Away.team = col_character(),
##   ..   Attendance = col_double(),
##   ..   HQ1G = col_double(),
##   ..   HQ1B = col_double(),
##   ..   HQ2G = col_double(),
##   ..   HQ2B = col_double(),
##   ..   HQ3G = col_double(),
##   ..   HQ3B = col_double(),
##   ..   HQ4G = col_double(),
##   ..   HQ4B = col_double(),
##   ..   Home.score = col_double(),
##   ..   AQ1G = col_double(),
##   ..   AQ1B = col_double(),
##   ..   AQ2G = col_double(),
##   ..   AQ2B = col_double(),
##   ..   AQ3G = col_double(),
##   ..   AQ3B = col_double(),
##   ..   AQ4G = col_double(),
##   ..   AQ4B = col_double(),
##   ..   Away.score = col_double(),
##   ..   Kicks = col_double(),
##   ..   Marks = col_double(),
##   ..   Handballs = col_double(),
##   ..   Goals = col_double(),
##   ..   Behinds = col_double(),
##   ..   Hit.Outs = col_double(),
##   ..   Tackles = col_double(),
##   ..   Rebounds = col_double(),
##   ..   Inside.50s = col_double(),
##   ..   Clearances = col_double(),
##   ..   Clangers = col_double(),
##   ..   Frees.For = col_double(),
##   ..   Frees.Against = col_double(),
##   ..   Brownlow.Votes = col_double(),
##   ..   Contested.Possessions = col_double(),
##   ..   Uncontested.Possessions = col_double(),
##   ..   Contested.Marks = col_double(),
##   ..   Marks.Inside.50 = col_double(),
##   ..   One.Percenters = col_double(),
##   ..   Bounces = col_double(),
##   ..   Goal.Assists = col_double(),
##   ..   match_outcome = col_character()
##   .. )
```



## Labels

One key element that differentiates an exploratory graph from a final graph is the addition of good labels. Graphic labels include titles, subtitles, captions, and axes titles. Without labels the audience has no idea what the data is, what the variables are, and in what units of measurement these variables are being presented in. 

Lets start by creating a boxplot of the variable `Home.score` by `match_outcome` without adjusting the labels. 


```r
gg <- ggplot(data = dat, aes(x = match_outcome, y = Home.score, 
                             colour = match_outcome)) +
  geom_boxplot()

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

One way we can add a title, subtitle and/or caption to the plot is by adding the `labs()` function as another "layer" to the ggplot. 
Note that you need to use the `+` to add the labs layer.


```r
gg + 
  labs(title = "Point distribution when home teams win, lose and draw", 
       subtitle = "Teams tend to win when they score more than 100 points", 
       caption = "Data sourced from the Lahman package")
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

You can also use the `labs()` function to adjust the axis titles. 


```r
gg <- 
  gg + 
  labs(title = "Point distribution when home teams win, lose and draw", 
       x = "Match Outcome",
       y = "Points scored when playing at home")

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

Note that because we originally used the `colour = match_outcome` aesthetic, the colours have been mapped to the variable `match_outcome`. This means that each level of the `match_outcome` variable will now have a different colour. This also results in a legend, where the legend title is "match_outcome". We can change the legend title using `labs()` as well.


```r
gg <-
  gg + 
  labs(colour = "Match Outcome") 

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

Note the argument we use to change the legend title is `colour = `  because we originally mapped the aesthetic 'colour' to `match_outcome` in our original code to produce the plot.

If, on the other hand, we mapped the aesthetic 'fill' to `match_outcome`, we would then use `labs(fill = "Match Outcome")` to change the legend title. 

For example,


```r
ggplot(data = dat, aes(x = match_outcome, y = Home.score, 
                       fill = match_outcome)) +
  geom_boxplot() +
  labs(fill = "Match Outcome") 
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
# this argument name needs to match the argument in aes()
```

If we put it altogether now it looks like this:


```r
gg <- ggplot(data = dat, aes(x = match_outcome, y = Home.score, 
                             colour = match_outcome)) +
  geom_boxplot() +
  labs(title = "Point distribution when home teams win, lose and draw", 
       subtitle = "Teams tend to win when they score more than 100 points", 
       caption = "Data sourced from the Lahman package", 
       x = "Match Outcome", 
       y = "Points scored when playing at home",
       colour = "Match_Outcome") 

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

As with most things in R, there are several ways to achieve the same outcome. You can also use the functions `ggtitle()`, `xlab()` and `ylab()` to change the plot title, X axis title and Y axis title, respectively. 


```r
gg <- ggplot(data = dat, aes(x = match_outcome, y = Home.score, 
                             colour = match_outcome)) +
  geom_boxplot() +
  ggtitle("Point distribution when home teams win, lose and draw") +
  xlab("Match Outcome") +
  ylab("Points scored when playing at home")

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

## Axes

To demonstrate how we can change the scale of the x and y axes, lets create a new plot to shows the relationship between `Marks.Inside.50` and `Goals`. 


```r
gg <- ggplot(data = dat, aes(x = Marks.Inside.50, y = Goals)) +
  geom_point()

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

Oops! Looks like there are a bunch of zeros for `Marks.Inside.50` in the dataset (most likely from a time before they collected this stat). Lets filter those out first.


```r
gg <- dat %>%
  filter(Marks.Inside.50 > 0) %>%
  ggplot(aes(x = Marks.Inside.50, y = Goals)) +
  geom_point(alpha = 0.6)

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

By default, **ggplot2** tries to scale the x and y axis to something that makes the most sense and that fits the data well. For example, `Marks.Inside.50` and Goals both have values that range from about 5-55. Therefore the x and y axis limits don't extend out beyond these by too much. 

We might want to change out x and y axis limits so the plot includes the origin (i.e. 0,0). We can adjust the axes ticks of continuous variables using `scale_y_continuous()` and `scale_x_continuous()`. 

For example:



```r
gg + 
  scale_x_continuous(limits = c(0, 60)) +
  scale_y_continuous(limits = c(0, 60))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

The `limits` argument allows us to set the minimum and maximum axes limits for our plot. You can see we have set the limits to range from 0-60 for both the x and y axes.

Note how the axes ticks and labels now appear at 0, 20, 40, 60 for both the x and y axes. We can adjust this using the `breaks` argument:


```r
gg + 
  scale_x_continuous(limits = c(0, 60), 
                     breaks = c(0, 10, 20, 30, 40, 50, 60)) +
  scale_y_continuous(limits = c(0, 60), 
                     breaks = c(0, 10, 20, 30, 40, 50, 60))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

In the example above, I wrote out the breaks manually, but we can do this more efficiently using the `seq()` function: 


```r
gg + 
  scale_x_continuous(limits = c(0, 60), 
                     breaks = seq(0, 60, by = 10)) +
  scale_y_continuous(limits = c(0, 60), 
                     breaks = seq(0, 60, by = 10))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

You can read the code `seq(0, 60, by = 10)` as "create a sequence of values from 0 to 60, and increase each value by 10". 

Take a look at the result of `seq(0, 60, by = 10)` to get what I mean:


```r
seq(0, 60, by = 10)
```

```
## [1]  0 10 20 30 40 50 60
```

Play around with a few different sequences to get the hang of this function. 

Another handy argument to the `scale_*` functions is `labels` which allows you to change the tick labels. 

For example: 


```r
gg + 
  scale_x_continuous(limits = c(0, 60), 
                     labels = c("zero", "twenty", "forty", "sixty")) +
  scale_y_continuous(limits = c(0, 60), 
                     labels = c("zero", "twenty", "forty", "sixty"))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

To see other useful arguments run `?scale_x_continuous` or `?scale_y_continuous` to bring up the R documentation. You can also present a **transformed** version of your variable using the following functions:

- `scale_*_log10()`: applies a log10 transformation
- `scale_*_sqrt()`: applies a square-root transformation
- `scale_*_reverse()`: reverses the axis (be careful not to mislead your audience by doing this though!)

## Colour 

Lets start by revising what we already know about dealing with colour in **ggplot2** (from the lesson on exploratory graphs). 

1. If we want to change the colour of a plot manually we can use the `fill` and/or `colour` arguments **outside** of the `aes()` function:


```r
ggplot(data = dat) +
  geom_histogram(aes(x = Marks), fill = "deepskyblue", colour = "deeppink")
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

Recall that you can see available colour names by running `colours()`, but you can also use [Hex codes](https://htmlcolorcodes.com). 

**Note:** `colour` **and** `color` **are both accepted in ggplot2**

2. If we want to map colour to a variable, we need to set the `fill` and/or `colour` arguments inside the `aes()` function. 


```r
gg <- dat %>%
  filter(Marks.Inside.50 > 0) %>%
  ggplot() +
  geom_point(aes(x = Marks.Inside.50, y = Goals, colour = Goals))

gg
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

Here we have mapped the `colour` argument to the variable `Goals`, so that higher values of goals will be a lighter blue, and lower values of goals will be a darker blue. When you map colour/fill to a **continuous** variable you will achieve a nice gradient like this. 

If, on the other hand, you map colour/fill to a **discrete variable**, you will get discrete colours like this:


```r
ggplot(data = dat) +
  geom_boxplot(aes(x = match_outcome, y = Home.score, colour = match_outcome))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

So how can we change the default colours when we are mapping to a **discrete variable**?

There are a few ways to do this, but one option is to use the `scale_colour_manual()` (if you have mapped the `colour` argument to a variable) and/or `scale_fill_manual()` (if you have mapped the `fill` argument to a variable). 

For example:


```r
ggplot(data = dat) +
  geom_boxplot(aes(x = match_outcome, y = Home.score, colour = match_outcome)) +
  scale_colour_manual(values = c("dodgerblue", "aquamarine3", "darkorchid"))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

Here is another example using `scale_fill_manual()` instead:


```r
ggplot(data = dat) +
  geom_boxplot(aes(x = match_outcome, y = Home.score, fill = match_outcome)) +
  scale_fill_manual(values = c("dodgerblue", "aquamarine3", "darkorchid"))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

If you have a palette that you like and want to use frequently, you could save that palette in its own object to use:


```r
pretty_colours <- c("dodgerblue", "aquamarine3", "darkorchid")

ggplot(data = dat) +
  geom_boxplot(aes(x = match_outcome, y = Home.score, fill = match_outcome)) +
  scale_fill_manual(values = pretty_colours)
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

What about mapping colour/fill to a **continuous variable**?

Again, there are a few ways to do this, but one way is to use the `scale_fill_gradient()` and/or the `scale_colour_gradient()` functions. 

For example: 


```r
dat %>%
  filter(Marks.Inside.50 > 0) %>%
  ggplot() +
  geom_point(aes(x = Marks.Inside.50, y = Goals, colour = Goals)) +
  scale_colour_gradient(low = "green", high = "red")
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-23-1.png)<!-- -->


## Annotations

Sometimes we may want to add annotations to our plots, particularly if we want to label some specific data points. One way to do this is to map data points to a variable using `geom_text()`. For example, in the plot below, the data points are mapped to the variable `Home.team`:


```r
# get the team average of Goals and Marks.Inside.50 for 2019
team_averages <- 
  dat %>%
  filter(Season == 2019) %>%
  group_by(Home.team) %>%
  summarise_at(vars(Goals, Marks.Inside.50), mean)

team_averages
```

```
## # A tibble: 18 x 3
##    Home.team              Goals Marks.Inside.50
##    <chr>                  <dbl>           <dbl>
##  1 Adelaide                21.7            19.3
##  2 Brisbane Lions          25.1            22.1
##  3 Carlton                 25.2            22.4
##  4 Collingwood             21.8            22.5
##  5 Essendon                21.7            20.6
##  6 Fremantle               22.5            20.7
##  7 Geelong                 21.9            22.3
##  8 Gold Coast              23.9            23.3
##  9 Greater Western Sydney  25.4            21.6
## 10 Hawthorn                22.2            21.2
## 11 Melbourne               23.7            25  
## 12 North Melbourne         24.5            22.6
## 13 Port Adelaide           21.1            20.9
## 14 Richmond                21.3            20.5
## 15 St Kilda                24.6            24.2
## 16 Sydney                  23.5            22.3
## 17 West Coast              23.8            22.8
## 18 Western Bulldogs        23              26.5
```


```r
ggplot(data = team_averages, aes(x = Marks.Inside.50, y = Goals)) +
  geom_point() +
  geom_text(aes(label = Home.team))
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

The plot above looks a bit messy as the geom_text labels will inherit the x and y coordinates from the aesthetics you specify in the `ggplot()` function (unless you override these in `geom_text()`). 

To tidy this up, we can **nudge** the labels up or down along the y axis, and left or right along the x axis using the `nudge_y` and `nudge_x` arguments, respectively. The value you enter for these arguments will be on the scale of the respective axis.

For example: 



```r
ggplot(data = team_averages, aes(x = Marks.Inside.50, y = Goals)) +
  geom_point() +
  geom_text(aes(label = Home.team), nudge_y = 0.25, nudge_x = 0.25)
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

Sometimes it might take a bit of playing around to get the nudging right if there are a number of clustered points!

## Themes

`ggplot2` includes eight built-in **themes** which allow you to modify the non-data aspects of your plot^[Grolemund & Wickham, R for Data Science, https://r4ds.had.co.nz/graphics-for-communication.html#themes] (e.g. background, gridlines etc):

- `theme_grey()`: this is the default theme
- `theme_bw()`: white background with gridlines
- `theme_classic()`: white background with no gridlines
- `theme_dark()`: dark background
- `theme_light()`: light axes and gridlines
- `theme_linedraw()`: black gridlines
- `theme_minimal()`: no background
- `theme_void()`: nothing but the geoms

For example, here is the `theme_classic()`:


```r
dat %>%
  filter(Marks.Inside.50 > 0) %>%
  ggplot() +
  geom_point(aes(x = Marks.Inside.50, y = Goals, colour = Goals)) +
  scale_colour_gradient(low = "green", high = "red") +
  theme_classic()
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-27-1.png)<!-- -->

And here is the `theme_dark()`:


```r
dat %>%
  filter(Marks.Inside.50 > 0) %>%
  ggplot() +
  geom_point(aes(x = Marks.Inside.50, y = Goals, colour = Goals)) +
  scale_colour_gradient(low = "green", high = "red") +
  theme_dark()
```

![](Creating-final-graphs_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

If you would like greater control of the theme of your plots you can set parameters yourself using the `theme()` function. 
We won't go through these in this lesson, but I recommend checking out the ggplot2 documentation [here](https://ggplot2.tidyverse.org/reference/theme.html). 


## For more

`ggplot2` is a comprehensive package for visualisation and we have only scraped the surface in this lesson. For more information I recommend the following resources:

- The official `ggplot2` [tidyverse site](https://ggplot2.tidyverse.org/index.html)
- [www.r-graph-gallery.com](https://www.r-graph-gallery.com/index.html)
- [Chapter 28: Graphics for communication](https://r4ds.had.co.nz/graphics-for-communication.html), R for Data Science, by Garrett Grolemund and Hadley Wickham

## References




