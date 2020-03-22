---
layout: single
title:  "Geniality of RLE function in R"
date:   2020-03-10 16:16:16 +0100
categories: [Data Science]
tags: R tricks
comments: true
---
I have been a happy user of the `rle` R function for so long that I decided to share its briliance. `rle` is a simple function which calculates how many times in a row the the vector didn't change its value. It returns two values: `values` nad `length`. To illustrate, why that might be interesting, let's start with a problem this function helped me solve.

## The Movement Onset Problem

In my research, I often want to determine when a person in a virtual environemnt starts moving. So if we consider to have a simple vector `speed` we can easilly check if the vector `speed` is above a certain speed threshold with the following code

```r
threshold <- 5
is_walking <- speed > threshold
is_walking[1:6]
> FALSE FALSE FALSE TRUE TRUE FALSE 
```
But here comes the problem. Because of many inaccuracies of speed measuring, people sometimes just "float" around the threshold, or they just move a bit but don't move consistently. So you might end up with a vector of movement like `0,0,0,1,0,0,0`. With recording frequency of 20 hz (50ms per speed recording), I wouldn't necessarily consider that single above speed incident a movement start. In general, I want to only label situations where the person keeps continuously moving above a certain threshold for at least X seconds. We have a vector `time`, but what we need now is to `sum` the length of each consecutive sections above speed threshold.

## The Solution
Let's first see see what `rle` outputs.

```r
vec <- c(1,1,1,2,3,3,2,2,1)
rle(vec)
Run Length Encoding
  lengths: int [1:5] 3 1 2 2 1
  values : num [1:5] 1 2 3 2 1
```
This shows us that 1 occurs three times, then 2 occured two times, 3 occured two times, 2 two times and finally 1 occured oncce. From this I can easilly see how long a certain value (say speed or position) didn't change.

Assuming we have a `speed` vector and `time` vector, we can create and separate `is_walking` into groups and calculate the sum of time underneeth. I firstly generate grouping variable with the `rep` function and then aggregate over it.
```r
is_walking <- speed > 5
rl <- rle(is_walking)
is_walking_group <- rep(1:length(rl$lengths), rl$lengths)
df_moving <- aggregate(time, by=list(is_moving=is_walking, group=is_walking_group), FUN=sum)
head(df_moving)
  is_walking group       x
1     FALSE     1 14.2888
2      TRUE     2  4.3362
3     FALSE     3  1.7191
4      TRUE     4  0.8671
5     FALSE     5  0.6670
6      TRUE     6  0.9681
```
This basically converts each section of consecutive `TRUE` or `FALSE` statements into a group with a number designation (for aggregation purposes only) and then we sum `time` per group (`is_walking` is redundant, but makes the table more readable).

So there you have it. In case you need to sum or do some stats on consecutive numbers, `rle` is here for you. You can check my final code in my package [navr](https://github.com/hejtmy/navr) in the function to search for [movement onsets](https://github.com/hejtmy/navr/blob/master/R/navr-onsets.R).