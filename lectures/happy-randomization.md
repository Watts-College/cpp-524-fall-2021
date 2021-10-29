---
layout: default
title: Happy Randomization
---

<div class = "uk-container uk-container-small">
  
<br><br>

Chapter 1 contains a nice example of the importance of checking for group balance after randomization (test for "happy" randomization). 

Specifically the study groups kids into 20 different block groups, and it randomizes blocks, not kids. 

In the study they have collected academic performance scores for all of the kids and they have aggregated the scores by block groups. Instead of randomly assigning each block group to one of the four study groups (T1, T2, T3, T4) they assign them in tranches. 
  
The start with the top 4 blocks and assign them each to one study group, then work with the group of the 4 next highest performers and randomly assign them next, etc. 
  
Why not just randomly assign all of the blocks to the 4 study groups and not worry about tranches? Since we are using randomization for assignment it should work out, right? 
  
In this case since we have small sample sizes - only 5 blocks per group - there is a high likelyhood that pure randomization will fail to produce balanced groups. This can be shown through a simulation of random assignment. Note that in the actual study they DO achieve group balance using the tranches method. 

The take-away is that randomization is not a silver bullet that solves all problems. Depending upon the group structure and sample sizes it is not guaranteed that random assignment will produce group balance. The test for group equivalence can be used to ensure randomization was "happy" or that manual group construction approaches like matching have worked as expected. 
  
The lab this week walks you through the process of testing for study group equivalency. 


```r
# group academic performance prior to the program start
# (groups are NOT balanced) 
  
study.group   ave.y
group1          51
group2          47
group3          71
group4          33
```

```r
# assign 5 students to each block,
# assign 5 blocks to each of 4 study groups

# study population of 100 students with pre-study
# academic performance measured in percentiles:
y <- 1:100  
     
# 5 students in each block
blocks <- rep( LETTERS[1:20], each=5 )
 
# randomize order of blocks then
# assign 5 blocks to each group:

x <- sample( blocks, 20 )
study.group <- NULL
study.group[ blocks %in% x[1:5] ]   <- "group1"
study.group[ blocks %in% x[6:10] ]  <- "group2"
study.group[ blocks %in% x[11:15] ] <- "group3"
study.group[ blocks %in% x[16:20] ] <- "group4"

# preview data 
d <- data.frame( block, y, study.group )
head( d, 20 )
   block  y study.group
1      A  1      group4
2      A  2      group4
3      A  3      group4
4      A  4      group4
5      A  5      group4
6      B  6      group3
7      B  7      group3
8      B  8      group3
9      B  9      group3
10     B 10      group3
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
 
d %>% group_by( study.group ) %>% summarize( ave.y=mean(y) )
# study.group   ave.y
# group1         51
# group2         47
# group3         71
# group4         33

```
 


</div>
<br><br><br>
