---
layout: default
title: Happy Randomization
---

<div class = "uk-container uk-container-small">
  
<br><br>

Chapter 1 contains a nice example of the importance of checking for group balance after randomization (test for "happy" randomization). 

Specifically the study groups kids into 20 different block groups, and it randomizes blocks, not kids. 

In the study they have collected academic performance scores for all of the kids and they have aggregated the scores by block groups. Instead of randomly assigning each block group to one of the four study groups (T1, T2, T3, T4) they... 


```r
# group traits prior to the study (they should all be equivalent)
study.group   ave.y
group1          51
group2          47
group3          71
group4          33
```

```r
> y <- 1:100
> block <- rep( LETTERS[1:20], each=5 )
> 
> # randomize order of blocks
> x <- sample( LETTERS[1:20], 20 )
> 
> study.group <- NULL
> study.group[ block %in% x[1:5] ]   <- "group1"
> study.group[ block %in% x[6:10] ]  <- "group2"
> study.group[ block %in% x[11:15] ] <- "group3"
> study.group[ block %in% x[16:20] ] <- "group4"
> 
> d <- data.frame( block, y, study.group )
> head( d, 20 )
   block  y study.group
1      A  1      group4
2      A  2      group4
3      A  3      group4
4      A  4      group4
5      A  5      group4
6      B  6      group4
7      B  7      group4
8      B  8      group4
9      B  9      group4
10     B 10      group4
11     C 11      group4
12     C 12      group4
13     C 13      group4
14     C 14      group4
15     C 15      group4
16     D 16      group2
17     D 17      group2
18     D 18      group2
19     D 19      group2
20     D 20      group2
> 
> d %>% group_by( study.group ) %>% summarize( ave=mean(y) )
# A tibble: 4 x 2
  study.group   ave
  <chr>       <dbl>
1 group1         51
2 group2         47
3 group3         71
4 group4         33

```
 


</div>
<br><br><br>
