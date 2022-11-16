---
title: factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图
author: Xwyturbo
date: '2022-11-16'
slug: factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图
categories:
  - R
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


#  1.数据的预处理

```r
  library(readxl)
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
  library(bruceR)
```

```
## 
## bruceR (version 0.8.9)
## BRoadly Useful Convenient and Efficient R functions
## 
## Packages also loaded:
## √ dplyr     	√ emmeans     	√ ggplot2
## √ tidyr     	√ effectsize  	√ ggtext
## √ stringr   	√ performance 	√ cowplot
## √ forcats   	√ lmerTest    	√ see
## √ data.table
## 
## Main functions of `bruceR`:
## cc()          	Describe() 	TTEST()
## add()         	Freq()     	MANOVA()
## .mean()       	Corr()     	EMMEANS()
## set.wd()      	Alpha()    	PROCESS()
## import()      	EFA()      	model_summary()
## print_table() 	CFA()      	lavaan_summary()
## 
## https://psychbruce.github.io/bruceR/
## 
## These R packages are dependencies of `bruceR` but not installed:
## vars, phia, BayesFactor, GPArotation
## ***** Please Install All Dependencies *****
## install.packages("bruceR", dep=TRUE)
```

```r
  library(writexl)
  library(ggpubr)
```

```
## 
## Attaching package: 'ggpubr'
## 
## The following object is masked from 'package:cowplot':
## 
##     get_legend
```

```r
  library(plotly)
```

```
## 
## Attaching package: 'plotly'
## 
## The following object is masked from 'package:bruceR':
## 
##     export
## 
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
  sui_de_initial <- read_xlsx("D:/wenyu/Rrojects/sui_de/sui_de_1107/sui_de_initial.xlsx")
  sd_data <- read_xlsx("D:/wenyu/Rrojects/sui_de/sui_de_1107/14种土地利用亚类面积.xlsx")
  
  #  计算每个村庄每种地类的占村庄全部用地的比重。
  sd1 <- sd_data/rowSums(sd_data)
  #  计算县域范围内每种地类占全部用地的比重
  sd2 <- colSums(sd_data)/sum(sd_data)
  
  #  按照14个亚类计算土地利用强度。
  sd_data  <- as_tibble(sd_data)
  sd_dataset <- sd_data %>% 
    mutate(lq1 = sd1$科教文卫用地/sd2[1],
           lq2 = sd1$交通运输用地/sd2[2],
           lq3 = sd1$林地/sd2[3],
           lq4 = sd1$园地/sd2[4],
           lq5 = sd1$水域及水利设施/sd2[5],
           lq6 = sd1$旱地/sd2[6],
           lq7 = sd1$草地/sd2[7],
           lq8 = sd1$水浇地/sd2[8],
           lq9 = sd1$设施农用地/sd2[9],
           lq10 = sd1$矿业用地/sd2[10],
           lq11 = sd1$公共服务用地/sd2[11],
           lq12 = sd1$商服用地 /sd2[12],
           lq13 = sd1$工业用地/sd2[13],
           lq14 = sd1$物流仓储用地/sd2[14])
  
  # library(writexl)
  # write_xlsx(sd_dataset,"sd_dataset.xlsx")
```


```r
  library(readxl)
  library(readxl)
  library(tidyverse)
  library(bruceR)
  library(writexl)
  library(ggpubr)
  library(plotly)

  sd_mianji <- sd_dataset[,1:14]
  sd_mianji <- sd_mianji %>% 
    mutate(村庄 = sui_de_initial$QSDWMC )
  sd_mjlong = sd_mianji %>%
    pivot_longer(-村庄, names_to = "地类", values_to = "面积/公亩")
  str(sd_mjlong)
```

```
## tibble [9,310 × 3] (S3: tbl_df/tbl/data.frame)
##  $ 村庄     : chr [1:9310] "踊跃" "踊跃" "踊跃" "踊跃" ...
##  $ 地类     : chr [1:9310] "科教文卫用地" "交通运输用地" "林地" "园地" ...
##  $ 面积/公亩: num [1:9310] 0 0 11010 21856.3 94.9 ...
```

```r
  p1 <- ggboxplot(sd_mjlong, x="地类", y="面积/公亩",fill ="地类",
                  palette = "aaas",#杂志Science的配色
                  sorting = "descending",                   
                  rotate = TRUE,                    
                  ggtheme = theme_bw())+
    theme(legend.position = "none")+
    ylim(0,20001)
  p1
```

```
## Warning: Removed 94 rows containing non-finite values (stat_boxplot).
```

```
## Warning: This manual palette can handle a maximum of 10 values. You have
## supplied 14.
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-2-1.png" width="672" />

```r
  # ggexport(p1,filename = "pics/14种地类规模.png",
  #          width = 4000,height = 2000,
  #          res = 600)

  
  sd_quweishang <- sd_dataset[,15:28]
  names(sd_quweishang) <- c("科教文卫用地","交通运输用地","林地",
                            "园地","水域及水利设施","旱地","草地",
                            "水浇地","设施农用地","矿业用地",
                            "公共服务用地","商服用地",
                            "工业用地","物流仓储用地")
  sd_quweishang <- sd_quweishang %>% 
    mutate(村庄 = sui_de_initial$QSDWMC )
  sd_longer = sd_quweishang %>%
    pivot_longer(-村庄, names_to = "地类", values_to = "区位商")
  
  p2 <- ggboxplot(sd_longer, x="地类", y="区位商",fill ="地类",
                 palette = "aaas",
                 sorting = "descending",                   
                 rotate = TRUE,                    
                 ggtheme = theme_bw())+
    theme(legend.position = "none")+
    geom_hline(yintercept = 1,linetype = 2,
               color = "red")+
    scale_y_continuous(breaks = 1)+
    ylim(0,10)
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which will
## replace the existing scale.
```

```r
  p2
```

```
## Warning: Removed 160 rows containing non-finite values (stat_boxplot).
## This manual palette can handle a maximum of 10 values. You have supplied 14.
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-2-2.png" width="672" />

```r
  # ggexport(p,filename = "pics/绥德县14种地类区位商.png",
  #          width = 4000,height = 2000,
  #          res = 600)
```


#  2.层次聚类图美化

```r
  library(ggdendro)
```

```
## Warning: package 'ggdendro' was built under R version 4.2.2
```

```r
  library(ggplot2)
  library(factoextra)
```

```
## Warning: package 'factoextra' was built under R version 4.2.2
```

```
## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa
```

```r
  library(ggpubr)
  library(ggsci)
```

```
## 
## Attaching package: 'ggsci'
```

```
## The following objects are masked from 'package:see':
## 
##     scale_color_material, scale_colour_material, scale_fill_material
```

```r
  library(readxl)
  library(tidyverse)
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

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-1.png" width="672" />

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

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-2.png" width="672" />

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

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-3.png" width="672" />

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

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-4.png" width="672" />

```r
  #vignette("ggsci")
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
  p52 <- fviz_dend(hc7,k = 2,
                   xlab = "村庄",
                   ylab = "Height",
                   main = "",
                   rect = TRUE,
                   k_colors = c("#3B4992FF","#EE0000FF"),
                   ggtheme = theme_bw())+
    annotate("text", x = 100, y = 32,label = "村庄数:244",colour= "#3B4992FF",size=3)+
    annotate("text", x = 400, y = 48,label = "村庄数:421",colour= "#EE0000FF",size=3) 
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p52
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-5.png" width="672" />

```r
  # ggexport(p52,filename = "pics/聚类谱系图2.png",
  #          width = 3400,height = 3000,
  #          res = 600)
  
  
  p53 <- fviz_dend(hc7,k = 3,
                   xlab = "村庄",
                   ylab = "Height",
                   main = "",
                   rect = TRUE,
                   k_colors = c("#3B4992FF","#EE0000FF","#008B45FF"),
                   ggtheme = theme_bw())+
    annotate("text", x = 110, y = 32,label = "村庄数:244",colour= "#3B4992FF",
             size = 3)+
    annotate("text", x = 300, y = 32,label = "村庄数:149",colour= "#EE0000FF",
             size = 3)+
    annotate("text", x = 500, y = 32,label = "村庄数:272",colour= "#008B45FF",
             size = 3)
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p53
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-6.png" width="672" />

```r
  # ggexport(p53,filename = "pics/聚类谱系图3.png",
  #          width = 3400,height = 3000,
  #          res = 600)
  
  p54 <- fviz_dend(hc7,k = 4,
                   xlab = "村庄",
                   ylab = "Height",
                   main = "",
                   rect = TRUE,
                   k_colors = c("#3B4992FF","#EE0000FF",
                                "#008B45FF","#631879FF"),
                   ggtheme = theme_bw())+
    annotate("text", x = 110, y = 32,label = "村庄数:244",colour= "#3B4992FF",
             size = 3)+
    annotate("text", x = 310, y = 32,label = "村庄数:149",colour= "#EE0000FF",
             size = 3)+
    annotate("text", x = 430, y = 32,label = "村庄数:79",colour= "#008B45FF",
             size = 3)+
    annotate("text", x = 550, y = 28,label = "村庄数:193",colour = "#631879FF",
             size = 3)
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  table(c3)
```

```
## c3
##   1   2   3 
## 244 149 272
```

```r
  table(c5)  
```

```
## c5
##   1   2   3   4   5 
## 108 149 136 193  79
```

```r
  p54
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-7.png" width="672" />

```r
  # ggexport(p54,filename = "pics/聚类谱系图4.png",
  #          width = 3400,height = 3000,
  #          res = 600)
  
  p55 <- fviz_dend(hc7,k = 5,
                   xlab = "村庄",
                   ylab = "Height",
                   main = "",
                   rect = TRUE,
                   k_colors = c("#3B4992FF","#F39B7FFF",
                                "#EE0000FF",
                                "#008B45FF","#631879FF"),
                   ggtheme = theme_bw())+
    annotate("text", x = 60, y = 32,label = "村庄数:136",colour= "#3B4992FF",
             size = 3)+
    annotate("text", x = 190, y = 28,label = "村庄数:108",colour= "#F39B7FFF",
             size = 3)+
    annotate("text", x = 310, y = 32,label = "村庄数:149",colour= "#EE0000FF",
             size = 3)+
    annotate("text", x = 430, y = 32,label = "村庄数:79",colour= "#008B45FF",
             size = 3)+
    annotate("text", x = 550, y = 28,label = "村庄数:193",colour = "#631879FF",
             size = 3)
```

```
## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
## "none")` instead.
```

```r
  p55
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-3-8.png" width="672" />

```r
  # ggexport(p55,filename = "pics/聚类谱系图5.png",
  #          width = 3400,height = 3000,
  #          res = 600)
```

#  3.箱线图

```r
  library(ggdendro)
  library(ggplot2)
  library(factoextra)
  library(ggpubr)
  library(ggsci)
  library(readxl)
  library(tidyverse)
  sd_dataset_hclust <- read_xlsx("D:/wenyu/Rrojects/sui_de/sui_de_1107/sd_hclust.xlsx")
  table(sd_dataset_hclust$结果_5)
```

```
## 
##   1   2   3   4   5 
## 108 149 136 193  79
```

```r
  sdhc1 <-  sd_dataset_hclust %>% 
    filter(结果_5 == 3) 
  sdhc1_mianji <- sdhc1[,c(1:14,35)]
  sdhc1_quweishang <- sdhc1[,c(15:28,35)]
  str(sdhc1_quweishang)
```

```
## tibble [136 × 15] (S3: tbl_df/tbl/data.frame)
##  $ lq1 : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ lq2 : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ lq3 : num [1:136] 0.868 1.162 1.039 1.06 1.481 ...
##  $ lq4 : num [1:136] 0.503 0.842 0.539 0.421 0.502 ...
##  $ lq5 : num [1:136] 0.3743 0.0823 1.2515 0 0.3604 ...
##  $ lq6 : num [1:136] 0.945 1.382 0.953 1.031 0.883 ...
##  $ lq7 : num [1:136] 1.399 0.906 1.284 1.338 1.205 ...
##  $ lq8 : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ lq9 : num [1:136] 0 0 0 0 0 ...
##  $ lq10: num [1:136] 0 0 0 0 0 ...
##  $ lq11: num [1:136] 0 0 0 0 0.543 ...
##  $ lq12: num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ lq13: num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ lq14: num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 村庄: chr [1:136] "张家塔" "周家沟" "东沟" "红旗沟" ...
```

```r
  names(sdhc1_quweishang) <- c("科教文卫用地","交通运输用地","林地",
                               "园地","水域及水利设施","旱地","草地",
                               "水浇地","设施农用地","矿业用地",
                               "公共服务用地","商服用地",
                               "工业用地","物流仓储用地","村庄")
  str(sdhc1_quweishang)
```

```
## tibble [136 × 15] (S3: tbl_df/tbl/data.frame)
##  $ 科教文卫用地  : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 交通运输用地  : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 林地          : num [1:136] 0.868 1.162 1.039 1.06 1.481 ...
##  $ 园地          : num [1:136] 0.503 0.842 0.539 0.421 0.502 ...
##  $ 水域及水利设施: num [1:136] 0.3743 0.0823 1.2515 0 0.3604 ...
##  $ 旱地          : num [1:136] 0.945 1.382 0.953 1.031 0.883 ...
##  $ 草地          : num [1:136] 1.399 0.906 1.284 1.338 1.205 ...
##  $ 水浇地        : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 设施农用地    : num [1:136] 0 0 0 0 0 ...
##  $ 矿业用地      : num [1:136] 0 0 0 0 0 ...
##  $ 公共服务用地  : num [1:136] 0 0 0 0 0.543 ...
##  $ 商服用地      : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 工业用地      : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 物流仓储用地  : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 村庄          : chr [1:136] "张家塔" "周家沟" "东沟" "红旗沟" ...
```

```r
  str(sdhc1_mianji)
```

```
## tibble [136 × 15] (S3: tbl_df/tbl/data.frame)
##  $ 科教文卫用地  : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 交通运输用地  : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 林地          : num [1:136] 3836 3502 2934 2967 6654 ...
##  $ 园地          : num [1:136] 3043 3474 2087 1613 3093 ...
##  $ 水域及水利设施: num [1:136] 152 22.8 325 0 148.9 ...
##  $ 旱地          : num [1:136] 5815 5803 3750 4021 5529 ...
##  $ 草地          : num [1:136] 16783 7410 9850 10168 14706 ...
##  $ 水浇地        : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 设施农用地    : num [1:136] 0 0 0 0 0 ...
##  $ 矿业用地      : num [1:136] 0 0 0 0 0 ...
##  $ 公共服务用地  : num [1:136] 0 0 0 0 8.68 ...
##  $ 商服用地      : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 工业用地      : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 物流仓储用地  : num [1:136] 0 0 0 0 0 0 0 0 0 0 ...
##  $ 村庄          : chr [1:136] "张家塔" "周家沟" "东沟" "红旗沟" ...
```

```r
  ## 先把各地类面积的宽表转成长表
  sdhc1_mjlong = sdhc1_mianji %>%
    pivot_longer(-村庄, names_to = "地类", values_to = "面积/公亩")
  str(sdhc1_mjlong)
```

```
## tibble [1,904 × 3] (S3: tbl_df/tbl/data.frame)
##  $ 村庄     : chr [1:1904] "张家塔" "张家塔" "张家塔" "张家塔" ...
##  $ 地类     : chr [1:1904] "科教文卫用地" "交通运输用地" "林地" "园地" ...
##  $ 面积/公亩: num [1:1904] 0 0 3836 3043 152 ...
```

```r
  ## 基于各地类面积长表，绘制箱线图
  p1 <- ggboxplot(sdhc1_mjlong, x="地类", y="面积/公亩",fill ="地类",
                  palette = "aaas",#杂志Science的配色
                  sorting = "descending",                   
                  rotate = TRUE,                    
                  ggtheme = theme_bw())+
    theme(legend.position = "none")+
    ylim(0,20001)
  p1
```

```
## Warning: Removed 8 rows containing non-finite values (stat_boxplot).
```

```
## Warning: This manual palette can handle a maximum of 10 values. You have
## supplied 14.
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
  # ggexport(p1,filename = "pics/聚类5对应的136个村庄地类面积.png",
  #          width = 4000,height = 2000,
  #          res = 600)
  
  
  ## 先把各地类区位商的宽表转成长表
  sdhc1_qwsl= sdhc1_quweishang %>%
    pivot_longer(-村庄, names_to = "地类", values_to = "区位商")
  str(sdhc1_qwsl)
```

```
## tibble [1,904 × 3] (S3: tbl_df/tbl/data.frame)
##  $ 村庄  : chr [1:1904] "张家塔" "张家塔" "张家塔" "张家塔" ...
##  $ 地类  : chr [1:1904] "科教文卫用地" "交通运输用地" "林地" "园地" ...
##  $ 区位商: num [1:1904] 0 0 0.868 0.503 0.374 ...
```

```r
  ## 基于各地类面积长表，绘制箱线图
  p1 <- ggboxplot(sdhc1_qwsl, x="地类", y="区位商",fill ="地类",
                  palette = "aaas",
                  sorting = "descending",                   
                  rotate = TRUE,                    
                  ggtheme = theme_bw())+
    theme(legend.position = "none")+
    geom_hline(yintercept = 1,linetype = 2,
               color = "red")+
    scale_y_continuous(breaks = 1)+
    ylim(0,5)
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which will
## replace the existing scale.
```

```r
  p1
```

```
## Warning: Removed 7 rows containing non-finite values (stat_boxplot).
## This manual palette can handle a maximum of 10 values. You have supplied 14.
```

<img src="factoextra+ggpubr+ggplot2-层次聚类图添加标签及箱线图_files/figure-html/unnamed-chunk-4-2.png" width="672" />

```r
  # ggexport(p1,filename = "pics/聚类5对应的136个村庄地类区位商.png",
  #          width = 4000,height = 2000,
  #          res = 600)
```


