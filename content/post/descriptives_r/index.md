---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "My Top Descriptive Statistics Function in R"
subtitle: ""
summary: "Describing and summarizing data in R"
authors: [admin]
tags: [r, statistics, descriptives]
categories: []
date: 2022-03-09T20:00:05-05:00
lastmod: 2022-03-09T20:00:05-05:00
featured: true
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Here are the functions I often use to describe and summarize my data.
 
After putting this together, I decided to google about it to see what other's have been using, and found [Adam Medcalf's post](https://dabblingwithdata.wordpress.com/2018/01/02/my-favourite-r-package-for-summarising-data/) explaining the same functions. So if you want to learn how to use them, I would recommend you to check [Adam's post](https://dabblingwithdata.wordpress.com/2018/01/02/my-favourite-r-package-for-summarising-data/). 

I have here as easy access for myself. As I find more functions, I will keep updating this post.

I'm using as data example the `mtcars` dataset that is built-in in R. And also, using my best friend pipe (`%>%`)

```R
library(dplyr)

mtcars %>% summary()
mtcars %>% skimr::skim() 
mtcars %>% Hmisc::describe()
mtcars %>% psych::describe()
mtcars %>% pastecs::stat.desc(norm = T) %>% t() %>% round(2) 
mtcars %>% summarytools::dfSummary() %>% summarytools::view()
mtcars %>% summarytools::descr(transpose = T)
```

As I find more time, I will add the outputs and more examples here. This is a growing project.