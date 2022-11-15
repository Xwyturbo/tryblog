---
title: factoextra包–层次聚类图美化
author: Xwyturbo
date: '2022-11-15'
slug: factoextra包–层次聚类图美化
categories:
  - R
  - Example
tags:
  - R Markdown
output:
  prettydoc::html_pretty:
    theme: cayman
    highlight: github
vignette: >
  %\VignetteIndexEntry{Vignette Title}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
---





```r
  library(readxl)
  library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.3.6      ✔ purrr   0.3.5 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
  library(ggpubr)
  library(factoextra)
```

```
## Warning: package 'factoextra' was built under R version 4.2.2
```

```
## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa
```

```r
  sd_dataset <- read_xlsx("D:/wenyu/Rrojects/sui_de/sui_de_1107/sd_dataset.xlsx")
  
  #  层次聚类，先生成距离矩阵。
  d1 <- dist(sd_dataset[,15:28],"canberra")
  hc7 <- hclust(d1,"ward.D2")
  tree = as.dendrogram(hc7)
  
  c2 <- cutree(hc7,2)
  table(c2)
```

```
## c2
##   1   2 
## 244 421
```

```r
  plot(cut(tree, h=25)$upper, horiz=FALSE)
  rect.hclust(hc7,2)
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-1.png" width="672" />

```r
  c3 <- cutree(hc7,3)
  table(c3)
```

```
## c3
##   1   2   3 
## 244 149 272
```

```r
  plot(cut(tree, h=25)$upper, horiz=FALSE)
  rect.hclust(hc7,3)
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-2.png" width="672" />

```r
  c4 <- cutree(hc7,4)
  table(c4)
```

```
## c4
##   1   2   3   4 
## 244 149 193  79
```

```r
  plot(cut(tree, h=25)$upper, horiz=FALSE)
  rect.hclust(hc7,4)
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-3.png" width="672" />

```r
  c5 <- cutree(hc7,5)
  table(c5)
```

```
## c5
##   1   2   3   4   5 
## 108 149 136 193  79
```

```r
  plot(cut(tree, h=25)$upper, horiz=FALSE)
  rect.hclust(hc7,5)
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-4.png" width="672" />

```r
  #install.packages('ggdendro')
  #install.packages('factoextra')
  library(ggdendro)
```

```
## Warning: package 'ggdendro' was built under R version 4.2.2
```

```r
  library(ggplot2)
  library(factoextra)
  library(ggpubr)
  library(ggsci)
  
  # 查询出版配色
  pal_aaas(palette = c("default"), alpha = 1)(5)
```

```
## [1] "#3B4992FF" "#EE0000FF" "#008B45FF" "#631879FF" "#008280FF"
```

```r
  pal_npg(palette = c("nrc"), alpha = 1)(8)
```

```
## [1] "#E64B35FF" "#4DBBD5FF" "#00A087FF" "#3C5488FF" "#F39B7FFF" "#8491B4FF"
## [7] "#91D1C2FF" "#DC0000FF"
```

```r
  ggdendrogram(hc7)
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-5.png" width="672" />

```r
  c5 <- cutree(hc7,5)
  table(c5)
```

```
## c5
##   1   2   3   4   5 
## 108 149 136 193  79
```

```r
  c4 <- cutree(hc7,4)
  table(c4)
```

```
## c4
##   1   2   3   4 
## 244 149 193  79
```

```r
  c3 <- cutree(hc7,3)
  table(c3)
```

```
## c3
##   1   2   3 
## 244 149 272
```

```r
  c2 <- cutree(hc7,2)
  table(c2)
```

```
## c2
##   1   2 
## 244 421
```

```r
  p52 <- fviz_dend(hc7,k = 2,
            xlab = "",
            ylab = "Height",
            main = "",
            k_colors = c("#3B4992FF","#EE0000FF"),
            ggtheme = theme_bw())
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p52
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-6.png" width="672" />

```r
  p53 <- fviz_dend(hc7,k = 3,
                   xlab = "",
                   ylab = "Height",
                   main = "",
                   k_colors = c("#3B4992FF","#EE0000FF","#008B45FF"),
                   ggtheme = theme_bw())
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p53
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-7.png" width="672" />

```r
  p54 <- fviz_dend(hc7,k = 4,
                   xlab = "",
                   ylab = "Height",
                   main = "",
                   k_colors = c("#3B4992FF","#EE0000FF",
                                "#008B45FF","#631879FF"),
                   ggtheme = theme_bw())
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p54
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-8.png" width="672" />

```r
  p55 <- fviz_dend(hc7,k = 5,
                   xlab = "",
                   ylab = "Height",
                   main = "",
                   k_colors = c("#3B4992FF","#F39B7FFF",
                                "#EE0000FF",
                                "#008280FF","#631879FF"),
                   ggtheme = theme_bw())
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p55
```

<img src="/post/2022-11-15-factoextra/factoextra包–层次聚类图美化_files/figure-html/unnamed-chunk-1-9.png" width="672" />

```r
  # ggexport(p52,filename = "聚类谱系图2.tiff",
  #          width = 2000,height = 2000,
  #          res = 600)   ##批量注释Ctrl+shift+C 
```

