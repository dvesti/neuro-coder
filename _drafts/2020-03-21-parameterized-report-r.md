---
layout: single
title:  "Parametrized reports in R"
date: 2020-03-20 16:16:16 +0100
categories: [Data science]
tags: R knitr
---

Given we usually don't have the resources to pay our participants, I consider it the least I can do for them is to provide a nice summary of the time they spent with us. So I prepare and send individualized reports generated in R and knitr to each of them. This requires generating and *knitting* R notebook several times with changing only few variables (e.g. participant's specific file to load). Doing it by hand for 50+ people can get quite out of hand. The optimal solution would be if I could prepare one `.Rmd` file and then `knit` it with some argument like a function does.

Enter parameterized reports.

## Parameters
Parameters in R notebooks can be set and provided to the rendered document in the `params` header:

```
---
output: html_document
parames:
  id: "NEO"
  source: "path/to/file"
---
```

And if declared, we can then use this in the rest of the r code:
```r
df_navigation <- read.table(params$source)

if(params$id == "NEO"){
  print("He is the one")
}
```

Passing parameters is then done with the `render` function. Typically, knit button calls `rmarkdown::render()`, which accepts your `.Rmd` file as a parameter. But it also takes `params` parameter, so we can pass it our desired values.
```r
rmarkdown::render("MyDocument.Rmd", params = list(
  id = "Morpheus",
  source = "Morpheus.csv"
)
```

`render` also takes `out` parameter, so in my case I can generate all participant reports with the following loop (given my filenames are consistent)
```r
participants <- c("NEO", "Morpheus")
for(participant in participants){
  f <- paste0(participant, ".csv")
  p <- list(id = participant, source = f)
  message('Knitting for ', participant)
  rmarkdown::render("report.Rmd", params = p, output_dir = "participants", 
      output_file = participant)
}
```

This should create `participants/NEO.html` and `participants/Morpheus.html` files in my working directory.

## Parameterized chunk options

You can use the params just as other regular chunk options and e.g. only include specific chunks if you need them. E.g. some might take too long, sometimes you want to change how you preprocess data etc.
```r
---
output: html_document
parames:
  id: "NEO"
  include_graphs: FALSE
  remove_outliers: TRUE
---
This is the the report for `r params$id`.

```{r, eval = params$remove_outliers}
df_navigation <- df_navigation[df_navigation$got_lost == FALSE, ]
`` `

```{r, include = params$include_graphs}
  # do this only if I you really need this graph. Maybe it takes too long to plot.
  hist(df_navigation$duration)
`` `
```

Yihui [warns](https://bookdown.org/yihui/rmarkdown/params-use.html), that "Parameters will not be available immediately after loading the file, but require any line of the report to be executed first". So in case things are failing, just keep this in mind.

## Parameterized title

Another neat trick stems form the fact that the YAML header block also evaluates R code, so we can set the title of our document from the params.

```
---
output: html_document
params: 
    id: "NEO"
title: "`r paste0("Output for participant ", params$id)`"
---
```

And yet another other option to create your header is actually even neater. There isn't a rule that there can be only a single YAML block in knitr, so we can actually write the document as this
```
---
output: html_document
params: 
    id: "NEO"
---
```{r, echo=FALSE, output='hide'}
id <- tolower(params$id)
created_title <- paste0("Output for participant ", id)
`` `
---
title: `r created_title`
---
```

## Source
Most of the information comes from Yihui's [great book](https://bookdown.org/yihui/rmarkdown/) on R markdown.