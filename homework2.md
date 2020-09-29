---
title: "p8105_hw2_vd2392"
author: "Vihar Desu"
output: html_document
---
### Setup

```r
library(tidyverse)
library(ggplot2)
```

### Problem 1: Constructing Data Frame

```r
df <- tibble(
  samp = rnorm(10),
  samp_gt_0 = samp > 0,
  character_vec = c("a", "b", "c", "d", "e", "f", "g", "h", "i", "j"),
  factor_vec = c("low", "low", "low", "med", "med", "med", "high", "high", "high", "high")
)
```


