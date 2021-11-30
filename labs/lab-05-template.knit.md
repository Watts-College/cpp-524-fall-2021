---
title: "Lab 5"
output:
  html_document:
    theme: readable
    df_print: paged
    highlight: tango
    toc: yes
    toc_float: no
---












# Load Data


```r
library( dplyr )
library( scales )
library( stargazer )
library( pander )

# STARGAZER OUTPUT
#
# Use:
#
#   s.type="text"
#
# while running chunks interactively
# to see table output.
# This sets it to "html"
# when knitting the file.
s.type="html"
```






```r
URL <- "https://github.com/DS4PS/cpp-524-sum-2021/blob/main/labs/data/counterfactuals.csv?raw=true"
d <- read.csv( URL )

# set factor levels
d$group <- factor( d$group, 
                   levels=c("high.ses","treatment","control"))
```





<img src="lab-05-template_files/figure-html/unnamed-chunk-4-1.png" width="768" />





# Reflexive Pre-Post Estimator (T2-T1)



<img src="lab-05-template_files/figure-html/unnamed-chunk-5-1.png" width="768" />



```r
dm <- filter( d, 
              group %in% c("treatment") &
              time %in% c("time1","time2") )

dm$post.dummy <- ifelse( dm$time=="time2", 1, 0 )

m <- lm( ability ~ post.dummy, data=dm )

stargazer( m, type=s.type, 
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>-0.72<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.15)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">post.dummy</td><td>2.91<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.21)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>94</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.67</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

## Question 1a

> What is the average score for kids in the treatment group in period 1 of the study? What is the average score for the same kids in period 2?

*Note, you can check your answers against the group means in Table 2 above.*

**ANSWER:**



## Question 1b

> Explain what it means when b0 is statistically significant in the reflexive model. Explain what it means when b1 is statistically significant.

**ANSWER:**


## Question 1c

> What is the effect size according to this model?

**ANSWER:**


<br>
<hr>
<br>


Test for zero trend assumption: C1=C2

<img src="lab-05-template_files/figure-html/unnamed-chunk-7-1.png" width="672" />


```r
dm <- filter( d, 
              group %in% c("control") &
              time %in% c("time1","time2") )

dm$post.dummy <- ifelse( dm$time=="time2", 1, 0 )

m <- lm( ability ~ post.dummy, data=dm )

stargazer( m, type=s.type, 
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>-0.65<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">post.dummy</td><td>1.28<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.17)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>180</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.24</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>


## Question 2a

> What is the average score for kids in the control group in period 1 of the study? What is the average score for the same kids in period 2?

**ANSWER:**




## Question 2b

> Which coefficient represents the test for whether we observe a secular trend in student achievement gains independent of the treatment? What is the decision rule?

**ANSWER:**



## Question 2c

> What does this model tell us about the appropriateness of the reflexive model?

**ANSWER:**


<br>
<hr>
<br>



# Post-Test Only Estimator (T2-C2)


<img src="lab-05-template_files/figure-html/unnamed-chunk-9-1.png" width="768" />




```r
dm <- filter( d, 
              group %in% c("treatment","control") &
              time=="time2" )

dm$treat.dummy <- ifelse( dm$group=="treatment", 1, 0 )

m <- lm( ability ~ treat.dummy, data=dm )

stargazer( m, type=s.type, 
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>0.64<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.dummy</td><td>1.55<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.20)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>137</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.31</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>


## Question 3a

> What is the average score for kids in the control group in the study? What is the average score for the kids in the treatment group? 

*Note, you can check your answers against the group means in Table 2 above.*

**ANSWER:**


## Question 3b

> What is the effect size identified by the model?


## Question 3c

> What is the identifying assumption of this model? Or stated differently, what must be true in order for the post-test only estimator to be appropriate?

**ANSWER:**

## Question 3d

> According to the model below is the assumption met? How can you tell?

**ANSWER:**



```r
dm <- filter( d, 
              group %in% c("treatment","control") &
              time=="time1" )

dm$treat.dummy <- ifelse( dm$group=="treatment", 1, 0 )

m <- lm( ability ~ treat.dummy, data=dm )

stargazer( m, type=s.type, 
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>-0.65<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.dummy</td><td>-0.07</td></tr>
<tr><td style="text-align:left"></td><td>(0.20)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>137</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.001</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

<br>
<hr>
<br>



# Diff-in-Diff Estimator [ gains - trend ]

Total Gains: T2-T1  
Trend: C2-C1  
DID Estimator: [ gains - trend ] = [ (T2-T1) - (C2-C1) ]   


<img src="lab-05-template_files/figure-html/unnamed-chunk-12-1.png" width="768" />




```r
dm <- filter( d, group %in% c("treatment","control") &
                time %in% c("time1","time2") )

dm$treat.dummy <- ifelse( dm$group=="treatment", 1, 0 )
dm$post.dummy  <- ifelse( dm$time=="time2", 1, 0 )

dm$treat.post.dummy <- dm$treat.dummy * dm$post.dummy

m <- lm( ability ~ treat.dummy + post.dummy + treat.post.dummy,
         data=dm)

stargazer( m, type=s.type, 
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>-0.65<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.dummy</td><td>-0.07</td></tr>
<tr><td style="text-align:left"></td><td>(0.20)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">post.dummy</td><td>1.28<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.16)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.post.dummy</td><td>1.63<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.28)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>274</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.48</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>



```r
b0 <- m$coefficients[1] %>% as.numeric() %>% round(2)
b1 <- m$coefficients[2] %>% as.numeric() %>% round(2)
b2 <- m$coefficients[3] %>% as.numeric() %>% round(2)
b3 <- m$coefficients[4] %>% as.numeric() %>% round(2)

b0                 # C1
```

```
## [1] -0.65
```

```r
b0 + b1            # T1
```

```
## [1] -0.72
```

```r
b0 + b2            # C2
```

```
## [1] 0.63
```

```r
b0 + b1 + b2 + b3  # T2
```

```
## [1] 2.19
```

```r
b0 + b1 + b2       # CF
```

```
## [1] 0.56
```

```r
CF <- b0 + b1 + b2
T2 <- b0 + b1 + b2 + b3   
T2-CF
```

```
## [1] 1.63
```


## Question 4a

> Are the treatment and control groups equivalent prior to the intervention? How do you know?

**ANSWER:**


## Question 4b

> Do we observe secular trends (gains independent of the treatment)? How do you know?

**ANSWER:**


## Question 4c

> What is the effect size in this model (gains from the treatment)?

**ANSWER:**


## Question 4d

> What does statistical significance of b3 represent? In other words, which contrast is being tested?

**ANSWER:**



<br>
<hr>
<br>


## Question 5a

> Do the reflexive and diff-in-diff models generate the same results (approximately)?  Why?

**ANSWER:**


## Question 5b

> Do the post-test only and diff-in-diff models generate the same results (approximately)? Why?

**ANSWER:**



<br>
<hr>
<br>



# Alternative Diff-in-Diff Estimator 

<img src="lab-05-template_files/figure-html/unnamed-chunk-15-1.png" width="768" />


```r
dm <- filter( d, group %in% c("treatment","high.ses") &
                time %in% c("time1","time2") )

dm$treat.dummy <- ifelse( dm$group=="treatment", 1, 0 )
dm$pre.dummy   <- ifelse( dm$time=="time1", 1, 0 )
dm$post.dummy  <- ifelse( dm$time=="time2", 1, 0 )

dm$treat.post.dummy <- dm$treat.dummy * dm$post.dummy 

m <- lm( ability ~ treat.dummy + post.dummy + treat.post.dummy,
         data=dm)

stargazer( m, type=s.type,
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>1.44<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.19)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.dummy</td><td>-2.16<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.24)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">post.dummy</td><td>1.71<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.27)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.post.dummy</td><td>1.20<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.34)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>150</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.68</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>


## Question 6a

> Are the pre-treatment differences (C1=T1?) different in this model versus the previous diff-in-diff? Why or why not?

**ANSWER:**



## Question 6b

> Does the diff-in-diff model require that study groups are equivalent prior to treatment to generate valid results?

**ANSWER:**


## Question 6c

> Is the secular trend identified by this model different from the previous diff-in-diff (approximately)? Why or why not?

**ANSWER:**



## Question 6d

> The treatment effects from this model are approximately the same as the previous diff-in-diff model, even though they use very different comparison groups. Why does this model still work using the high SES group?

**ANSWER:**



## Question 6e

> What is the identification assumption of the diff-in-diff model?

**ANSWER:**



<br>
<hr>
<br>

# Parallel Lines Test

Does the high SES group model secular trend appropriately?

Test: are the study group trend lines parallel prior to the intervention?

<img src="lab-05-template_files/figure-html/unnamed-chunk-17-1.png" width="768" />


```r
dm <- filter( d, group %in% c("treatment","high.ses") &
                time %in% c("time0","time1") )

dm$treat.dummy <- ifelse( dm$group=="treatment", 1, 0 )
dm$pre.dummy   <- ifelse( dm$time=="time0", 1, 0 )
dm$post.dummy  <- ifelse( dm$time=="time1", 1, 0 )

dm$treat.post <- dm$treat.dummy * dm$post.dummy 

m <- lm( ability ~ treat.dummy + post.dummy + treat.post,
         data=dm)

stargazer( m, type=s.type,
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>-0.02</td></tr>
<tr><td style="text-align:left"></td><td>(0.21)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.dummy</td><td>-1.73<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.27)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">post.dummy</td><td>1.46<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.30)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.post</td><td>-0.43</td></tr>
<tr><td style="text-align:left"></td><td>(0.38)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>150</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.51</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

Does the control group model secular trend appropriately?

Test: are the study group trend lines parallel prior to the intervention? 

<img src="lab-05-template_files/figure-html/unnamed-chunk-19-1.png" width="768" />


```r
dm <- filter( d, group %in% c("treatment","control") &
                time %in% c("time0","time1") )

dm$treat.dummy <- ifelse( dm$group=="treatment", 1, 0 )
dm$post.dummy  <- ifelse( dm$time=="time1", 1, 0 )

dm$treat.post <- dm$treat.dummy * dm$post.dummy 

m <- lm( ability ~ treat.dummy + post.dummy + treat.post,
         data=dm)

stargazer( m, type=s.type,
           omit.stat=c("f","ser","adj.rsq"),
           intercept.top=TRUE, intercept.bottom=FALSE,
           digits=2 )
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>ability</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>-1.76<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.dummy</td><td>0.001</td></tr>
<tr><td style="text-align:left"></td><td>(0.21)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">post.dummy</td><td>1.11<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.17)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">treat.post</td><td>-0.08</td></tr>
<tr><td style="text-align:left"></td><td>(0.29)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>274</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.18</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

## Question 7

> Which coefficient captures the parallel lines test? Do we want it to be significant or not?

**ANSWER:**


























<br>

-----

<br>



<style type="text/css">
body{
     font-family:system-ui,-apple-system,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
     font-size:calc(1.5em + 0.25vw);
     font-weight:300;line-height:1.65;
     -webkit-font-smoothing:antialiased;
     -moz-osx-font-smoothing:grayscale;
     margin-left:20%;
     margin-right:20%} 
     

h1, h2, h3, h4 { color: #995c00; }

h2 { margin-top:80px; }



.footer {
  background-color:#726e6e;
  height:340px;
  color:white;
  padding: 20px 3px 20px 3px;
  margin:0px;
  line-height: normal;
}

.footer a{ color:orange; text-decoration:bold !important; } 
 
 
 
 table{
   border-spacing:1px;
   margin-top:80px;
   margin-bottom:100px !important;
   margin-left: auto;
   margin-right: auto;
   align:center} 


 
td{ padding: 6px 10px 6px 10px } 

th{ text-align: left; } 

blockquote {
    margin-top:80px;
    border-left: 5px solid #995c00;
    color: #995c00;
}
</style>




