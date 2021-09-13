# Lecture Slide Format

xaringan package for remark.js slides + R

https://arm.rbind.io/slides/xaringan.html#1



```r
devtools::install_github('yihui/xaringan')
devtools::install_github("gadenbuie/xaringanthemer")
devtools::install_github("hadley/emo")
devtools::install_github("ropenscilabs/icon")
```

See examples at 

* https://github.com/jenniferthompson/RMedicine2018
* https://github.com/mkearney/rtweet-workshop/blob/master/index.Rmd

#### Setting themes

````
```{r xaringan-themer, include = FALSE}
library(xaringanthemer)
mono_light(
  base_color = "#125972", ## picked to coordinate with rOpenSci logo
  code_highlight_color = "#c0e8f5",
  link_color = "#197b9e",
  header_font_google = google_font("Josefin Sans"),
  text_font_google   = google_font("Montserrat", "300", "300i"),
  code_font_google   = google_font("Droid Mono")
)
```
````


#### Example title slide

````
---
title: "The Life & Times of a Reproducible Clinical Project in R"
author: "Jennifer L Thompson, MPH<br><span style = 'font-size: 50%;'>Vanderbilt University Medical Center<br>Department of Biostatistics + Center for Critical Illness, Brain Dysfunction & Survivorship</span>"
date: 'R/Medicine 2018<br><br>`r icon::fa("link")` [bit.ly/jlt-rmed2018](https://bit.ly/jlt-rmed2018)<br>`r icon::fa("twitter")` [jent103](https://twitter.com/jent103)'
output:
  xaringan::moon_reader:
    chakra: libs/remark-latest.min.js
    css: [xaringan-themer.css, my-theme.css]
    nature:
      countIncrementalSlides: false
      titleSlideClass: ["left", "middle", "inverse"]
      highlightLines: true
      highlightStyle: solarized-dark
    includes:
      in_header: header.html
---
````
