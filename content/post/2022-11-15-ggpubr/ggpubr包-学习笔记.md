---
title: ggpubr包--学习笔记
author: Xwyturbo
date: '2022-11-15'
slug: ggpubr包-学习笔记
categories:
  - R
tags:
  - R Markdown
  - plot
output:
  prettydoc::html_pretty:
    theme: cayman
    highlight: github
vignette: >
  %\VignetteIndexEntry{Vignette Title}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
---

<script src="/rmarkdown-libs/htmlwidgets/htmlwidgets.js"></script>
<script src="/rmarkdown-libs/plotly-binding/plotly.js"></script>
<script src="/rmarkdown-libs/typedarray/typedarray.min.js"></script>
<script src="/rmarkdown-libs/jquery/jquery.min.js"></script>
<link href="/rmarkdown-libs/crosstalk/css/crosstalk.min.css" rel="stylesheet" />
<script src="/rmarkdown-libs/crosstalk/js/crosstalk.min.js"></script>
<link href="/rmarkdown-libs/plotly-htmlwidgets-css/plotly-htmlwidgets.css" rel="stylesheet" />
<script src="/rmarkdown-libs/plotly-main/plotly-latest.min.js"></script>

# 1.R包的安装及加载

``` r
#install.packages("ggpubr")
library(ggpubr)
```

    ## Loading required package: ggplot2

# 2.常用基本图形的绘制

## 2.1带有均值线和地毯线的密度图

``` r
library(ggpubr)
#构建数据集set.seed(1234)
df <- data.frame( sex=factor(rep(c("f", "M"), each=200)),
                 weight=c(rnorm(200, 55), rnorm(200, 58)))
# 预览数据格式
head(df)
```

    ##   sex   weight
    ## 1   f 56.91919
    ## 2   f 54.36798
    ## 3   f 55.84918
    ## 4   f 53.27499
    ## 5   f 55.45501
    ## 6   f 52.86464

``` r
# 绘制密度图
# rug参数添加地毯线，
# add参数可以添加均值mean和中位数median，
# 按性别”sex“分组标记边框线颜色和填充色，使用palette参数自定义颜色
p1 <- ggdensity(df, x="weight", add = "mean", rug = TRUE, color = "sex", 
                fill = "sex",palette = c("#00AFBB", "#E7B800")) 
p1
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-2-1.png" width="672" />

``` r
p11 <- ggdensity(df, x="weight",facet.by = "sex",
                add = "mean", rug = TRUE, color = "sex", 
                fill = "sex",palette = c("#00AFBB", "#E7B800")) 
p11
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-2-2.png" width="672" />

## 2.2带有均值线和边际地毯线的直方图

``` r
library(ggpubr)
p2 <- gghistogram(df, x="weight", 
                  add = "mean", rug = TRUE, color = "sex", 
                  fill = "sex",palette = c("#00AFBB", "#E7B800"))
```

    ## Warning: Using `bins = 30` by default. Pick better value with the argument
    ## `bins`.

``` r
p2
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-3-1.png" width="672" />

``` r
p22 <- gghistogram(df, x="weight",facet.by = "sex", 
                  add = "mean", rug = TRUE, color = "sex", 
                  fill = "sex",palette = c("#00AFBB", "#E7B800"))
```

    ## Warning: Using `bins = 30` by default. Pick better value with the argument
    ## `bins`.

``` r
p22
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-3-2.png" width="672" />

## 2.3箱线图+分组形状+统计

``` r
library(ggpubr)
library(datasets)
data(ToothGrowth)
str(ToothGrowth)
```

    ## 'data.frame':    60 obs. of  3 variables:
    ##  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
    ##  $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...

``` r
head(ToothGrowth)
```

    ##    len supp dose
    ## 1  4.2   VC  0.5
    ## 2 11.5   VC  0.5
    ## 3  7.3   VC  0.5
    ## 4  5.8   VC  0.5
    ## 5  6.4   VC  0.5
    ## 6 10.0   VC  0.5

``` r
#jitter参数添加扰动点，点的形状shape由dose变量映射
p3 <- ggboxplot(ToothGrowth, x="dose", y="len", color = "dose",
              palette = c("#00AFBB", "#E7B800", "#FC4E07"),
              add = "jitter", shape="dose") 
p3
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-4-1.png" width="672" />

``` r
p32 <- ggboxplot(ToothGrowth, x="dose", y="len",facet.by = "dose",
                 color = "dose",
              palette = c("#00AFBB", "#E7B800", "#FC4E07"),
              add = "jitter", shape="dose") 
p32
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-4-2.png" width="672" />

``` r
# stat_compare_means参数比较不同组之间的均值，
# 并增加不同组间比较的p-value值，可以自定义需要标注的组间比较
my_comparisons <- list(c("0.5", "1"), c("1", "2"), c("0.5", "2"))
p4 <- p3 + stat_compare_means(comparisons = my_comparisons)+
      stat_compare_means(label.y = 50)
p4
```

    ## Warning in wilcox.test.default(c(4.2, 11.5, 7.3, 5.8, 6.4, 10, 11.2, 11.2, :
    ## cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(c(4.2, 11.5, 7.3, 5.8, 6.4, 10, 11.2, 11.2, :
    ## cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(c(16.5, 16.5, 15.2, 17.3, 22.5, 17.3, 13.6, :
    ## cannot compute exact p-value with ties

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-4-3.png" width="672" />

## 2.4内有箱线图的小提琴图+星标记

``` r
library(ggpubr)
library(datasets)
data(ToothGrowth)
str(ToothGrowth)
```

    ## 'data.frame':    60 obs. of  3 variables:
    ##  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
    ##  $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...

``` r
head(ToothGrowth)
```

    ##    len supp dose
    ## 1  4.2   VC  0.5
    ## 2 11.5   VC  0.5
    ## 3  7.3   VC  0.5
    ## 4  5.8   VC  0.5
    ## 5  6.4   VC  0.5
    ## 6 10.0   VC  0.5

``` r
# add = “boxplot”添加箱线图
# stat_compare_means中设置lable=”p.signif”，
# 即可添加星添加组间比较连线和统计P值按星分类
# add添加箱线图，label标注选择显著性标记（星号）
p5 <- ggviolin(ToothGrowth, x="dose", y="len", fill = "dose",
        palette = c("#00AFBB", "#E7B800", "#FC4E07"),
        add = "boxplot", add.params = list(fill="white"))+        
        stat_compare_means(comparisons = my_comparisons, label = "p.signif") +         
        stat_compare_means(label.y = 50)
 p5
```

    ## Warning in wilcox.test.default(c(4.2, 11.5, 7.3, 5.8, 6.4, 10, 11.2, 11.2, :
    ## cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(c(4.2, 11.5, 7.3, 5.8, 6.4, 10, 11.2, 11.2, :
    ## cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(c(16.5, 16.5, 15.2, 17.3, 22.5, 17.3, 13.6, :
    ## cannot compute exact p-value with ties

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-5-1.png" width="672" />

## 2.5条形/柱状图绘制(barplot)

``` r
library(ggpubr)
# 加载数据集
data("mtcars")
df2 <- mtcars
# 设置因子变量
df2$cyl <- factor(df2$cyl)
df2$name <- rownames(df2) #添加一新列name
head(df2[, c("name", "wt", "mpg", "cyl")])
```

    ##                                name    wt  mpg cyl
    ## Mazda RX4                 Mazda RX4 2.620 21.0   6
    ## Mazda RX4 Wag         Mazda RX4 Wag 2.875 21.0   6
    ## Datsun 710               Datsun 710 2.320 22.8   4
    ## Hornet 4 Drive       Hornet 4 Drive 3.215 21.4   6
    ## Hornet Sportabout Hornet Sportabout 3.440 18.7   8
    ## Valiant                     Valiant 3.460 18.1   6

``` r
# 颜色按nature配色方法(支持 ggsci包中的本色方案 ，如: “npg”, “aaas”, “lancet”, “jco”, “ucscgb”, “uchicago”, “simpsons” and “rickandmorty”)
  p6 <- ggbarplot(df2, x="name", y="mpg", fill = "cyl", color = "white",
           palette = "npg", #杂志nature的配色         
           sort.val = "desc", #降序排序         
           sort.by.groups=FALSE, #不按组排序         
           x.text.angle=60)
 p6
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-6-1.png" width="672" />

``` r
 # 按组进行排序
 p7 <- ggbarplot(df2, x="name", y="mpg", fill = "cyl", color = "white",
           palette = "aaas", #杂志Science的配色
           sort.val = "asc", #升序排序,区别于desc         
           sort.by.groups=TRUE,x.text.angle=60)
 #按组排序x.text.angl设置x轴标签旋转角度
 p7
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-6-2.png" width="672" />

``` r
# 偏差图绘制(Deviation graphs),偏差图展示了与参考值之间的偏差。
df2$mpg_z <- (df2$mpg-mean(df2$mpg))/sd(df2$mpg)    
# 相当于Zscore标准化，减均值，除标准差
df2$mpg_grp <- factor(ifelse(df2$mpg_z<0, "low", "high"), 
                      levels = c("low", "high"))
#设置分组因子
head(df2[, c("name", "wt", "mpg", "mpg_grp", "cyl")])
```

    ##                                name    wt  mpg mpg_grp cyl
    ## Mazda RX4                 Mazda RX4 2.620 21.0    high   6
    ## Mazda RX4 Wag         Mazda RX4 Wag 2.875 21.0    high   6
    ## Datsun 710               Datsun 710 2.320 22.8    high   4
    ## Hornet 4 Drive       Hornet 4 Drive 3.215 21.4    high   6
    ## Hornet Sportabout Hornet Sportabout 3.440 18.7     low   8
    ## Valiant                     Valiant 3.460 18.1     low   6

``` r
p8 <- ggbarplot(df2, x="name", y="mpg_z", fill = "mpg_grp", color = "white",
        palette = "jco", sort.val = "asc", sort.by.groups = FALSE,                  
        x.text.angle=60, ylab = "MPG z-score", xlab = FALSE)
p8
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-6-3.png" width="672" />

``` r
# rotate设置x/y轴对换
p9 <- ggbarplot(df2, x="name", y="mpg_z", fill = "mpg_grp", color = "white",
         palette = "jco", sort.val = "desc", sort.by.groups = FALSE,
         x.text.angle=90, ylab = "MPG z-score", xlab = FALSE, 
         rotate=TRUE, ggtheme = theme_minimal())   
p9
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-6-4.png" width="672" />

## 2.6棒棒糖图绘制(Lollipop chart),棒棒图可以代替条形图展示数据

``` r
library(ggpubr)
library(plotly)
```

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

``` r
# 加载数据集
data("mtcars")
df2 <- mtcars
df2$cyl <- factor(df2$cyl)
df2$name <- rownames(df2) #添加一新列name
p10 <- ggdotchart(df2, x="name", y="mpg", color = "cyl",
          palette = c("#00AFBB", "#E7B800", "#FC4E07"),          
          sorting = "ascending",          
          add = "segments", ggtheme = theme_pubr())
p10
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-7-1.png" width="672" />

``` r
# 设置其他参数, dot.size = 6调整糖的大小，添加label标签，设置字体样式和方向
p11 <- ggdotchart(df2, x="name", y="mpg", color = "cyl", 
         palette = c("#00AFBB", "#E7B800", "#FC4E07"),          
         sorting = "descending", add = "segments", rotate = TRUE,
         group = "cyl", dot.size = 6,          
         label = round(df2$mpg), font.label = list(color="white",
         size=9, vjust=0.5), ggtheme = theme_pubr())
p11
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-7-2.png" width="672" />

``` r
# 偏差图绘制(Deviation graphs),偏差图展示了与参考值之间的偏差。
df2$mpg_z <- (df2$mpg-mean(df2$mpg))/sd(df2$mpg)    
# 相当于Zscore标准化，减均值，除标准差
df2$mpg_grp <- factor(ifelse(df2$mpg_z<0, "low", "high"), 
                      levels = c("low", "high"))
#棒棒糖偏差图
p12 <- ggdotchart(df2, x = "name", y = "mpg_z",
                    color = "cyl", # Color by groups                    
                    palette = c("#00AFBB", "#E7B800", "#FC4E07"), 
                  # Custom color palette   
                    sorting = "descending", # Sort value in descending order               
                    add = "segments", # Add segments from y = 0 to dots                    
                    add.params = list(color = "lightgray", size = 2),
                  # Change segment color and size                    
                    group = "cyl", # Order by groups                   
                    dot.size = 6, # Large dot size                    
                    label = round(df2$mpg_z,1), 
                  # Add mpg values as dot labels，设置一位小数                    
                    font.label = list(color = "white", size = 9, vjust = 0.5),
                  # Adjust label parameters                    
                    ggtheme = theme_pubr()) +                    
                    geom_hline(yintercept = 0, linetype = 2,
                               color = "lightgray")
ggplotly(p12)
```

    ## Warning: `gather_()` was deprecated in tidyr 1.2.0.
    ## ℹ Please use `gather()` instead.
    ## ℹ The deprecated feature was likely used in the plotly package.
    ##   Please report the issue at <]8;;https://github.com/plotly/plotly.R/issueshttps://github.com/plotly/plotly.R/issues]8;;>.

<div id="htmlwidget-1" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"data":[{"x":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32],"y":[2.29127161552575,2.04238943053993,1.7105465172255,1.7105465172255,1.19619000158812,0.980492107933741,0.715017777282194,0.449543446630647,0.449543446630647,0.233845552976265,0.217253407310543,0.217253407310543,0.150884824647657,0.150884824647657,-0.0648130690067253,-0.147773797335334,-0.330287399658272,-0.380063836655437,-0.147773797335334,-0.230734525663942,-0.463024564984046,-0.612353875975541,-0.711906749969871,-0.761683186967036,-0.811459623964201,-0.811459623964201,-0.844643915295645,-0.89442035229281,-0.960788934955696,-1.12671039161291,-1.60788261591884,-1.60788261591884],"text":["name: Toyota Corolla<br />mpg_z:  2.29127162<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  2.29127162<br />cyl: 4","name: Fiat 128<br />mpg_z:  2.04238943<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  2.04238943<br />cyl: 4","name: Honda Civic<br />mpg_z:  1.71054652<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  1.71054652<br />cyl: 4","name: Lotus Europa<br />mpg_z:  1.71054652<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  1.71054652<br />cyl: 4","name: Fiat X1-9<br />mpg_z:  1.19619000<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  1.19619000<br />cyl: 4","name: Porsche 914-2<br />mpg_z:  0.98049211<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.98049211<br />cyl: 4","name: Merc 240D<br />mpg_z:  0.71501778<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.71501778<br />cyl: 4","name: Datsun 710<br />mpg_z:  0.44954345<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.44954345<br />cyl: 4","name: Merc 230<br />mpg_z:  0.44954345<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.44954345<br />cyl: 4","name: Toyota Corona<br />mpg_z:  0.23384555<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.23384555<br />cyl: 4","name: Volvo 142E<br />mpg_z:  0.21725341<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.21725341<br />cyl: 4","name: Hornet 4 Drive<br />mpg_z:  0.21725341<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.21725341<br />cyl: 6","name: Mazda RX4<br />mpg_z:  0.15088482<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.15088482<br />cyl: 6","name: Mazda RX4 Wag<br />mpg_z:  0.15088482<br />cyl: lightgray<br />ymin: 0<br />mpg_z:  0.15088482<br />cyl: 6","name: Ferrari Dino<br />mpg_z: -0.06481307<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.06481307<br />cyl: 6","name: Merc 280<br />mpg_z: -0.14777380<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.14777380<br />cyl: 6","name: Valiant<br />mpg_z: -0.33028740<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.33028740<br />cyl: 6","name: Merc 280C<br />mpg_z: -0.38006384<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.38006384<br />cyl: 6","name: Pontiac Firebird<br />mpg_z: -0.14777380<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.14777380<br />cyl: 8","name: Hornet Sportabout<br />mpg_z: -0.23073453<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.23073453<br />cyl: 8","name: Merc 450SL<br />mpg_z: -0.46302456<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.46302456<br />cyl: 8","name: Merc 450SE<br />mpg_z: -0.61235388<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.61235388<br />cyl: 8","name: Ford Pantera L<br />mpg_z: -0.71190675<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.71190675<br />cyl: 8","name: Dodge Challenger<br />mpg_z: -0.76168319<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.76168319<br />cyl: 8","name: Merc 450SLC<br />mpg_z: -0.81145962<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.81145962<br />cyl: 8","name: AMC Javelin<br />mpg_z: -0.81145962<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.81145962<br />cyl: 8","name: Maserati Bora<br />mpg_z: -0.84464392<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.84464392<br />cyl: 8","name: Chrysler Imperial<br />mpg_z: -0.89442035<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.89442035<br />cyl: 8","name: Duster 360<br />mpg_z: -0.96078893<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -0.96078893<br />cyl: 8","name: Camaro Z28<br />mpg_z: -1.12671039<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -1.12671039<br />cyl: 8","name: Cadillac Fleetwood<br />mpg_z: -1.60788262<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -1.60788262<br />cyl: 8","name: Lincoln Continental<br />mpg_z: -1.60788262<br />cyl: lightgray<br />ymin: 0<br />mpg_z: -1.60788262<br />cyl: 8"],"type":"scatter","mode":"lines","opacity":1,"line":{"color":"transparent"},"error_y":{"array":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"arrayminus":[2.29127161552575,2.04238943053993,1.7105465172255,1.7105465172255,1.19619000158812,0.980492107933741,0.715017777282194,0.449543446630647,0.449543446630647,0.233845552976265,0.217253407310543,0.217253407310543,0.150884824647657,0.150884824647657,-0.0648130690067253,-0.147773797335334,-0.330287399658272,-0.380063836655437,-0.147773797335334,-0.230734525663942,-0.463024564984046,-0.612353875975541,-0.711906749969871,-0.761683186967036,-0.811459623964201,-0.811459623964201,-0.844643915295645,-0.89442035229281,-0.960788934955696,-1.12671039161291,-1.60788261591884,-1.60788261591884],"type":"data","width":0,"symmetric":false,"color":"rgba(211,211,211,1)"},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1,2,3,4,5,6,7,8,9,10,11],"y":[2.29127161552575,2.04238943053993,1.7105465172255,1.7105465172255,1.19619000158812,0.980492107933741,0.715017777282194,0.449543446630647,0.449543446630647,0.233845552976265,0.217253407310543],"text":["name: Toyota Corolla<br />mpg_z:  2.29127162<br />cyl: 4","name: Fiat 128<br />mpg_z:  2.04238943<br />cyl: 4","name: Honda Civic<br />mpg_z:  1.71054652<br />cyl: 4","name: Lotus Europa<br />mpg_z:  1.71054652<br />cyl: 4","name: Fiat X1-9<br />mpg_z:  1.19619000<br />cyl: 4","name: Porsche 914-2<br />mpg_z:  0.98049211<br />cyl: 4","name: Merc 240D<br />mpg_z:  0.71501778<br />cyl: 4","name: Datsun 710<br />mpg_z:  0.44954345<br />cyl: 4","name: Merc 230<br />mpg_z:  0.44954345<br />cyl: 4","name: Toyota Corona<br />mpg_z:  0.23384555<br />cyl: 4","name: Volvo 142E<br />mpg_z:  0.21725341<br />cyl: 4"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,175,187,1)","opacity":1,"size":22.6771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(0,175,187,1)"}},"hoveron":"points","name":"(4,1)","legendgroup":"(4,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[12,13,14,15,16,17,18],"y":[0.217253407310543,0.150884824647657,0.150884824647657,-0.0648130690067253,-0.147773797335334,-0.330287399658272,-0.380063836655437],"text":["name: Hornet 4 Drive<br />mpg_z:  0.21725341<br />cyl: 6","name: Mazda RX4<br />mpg_z:  0.15088482<br />cyl: 6","name: Mazda RX4 Wag<br />mpg_z:  0.15088482<br />cyl: 6","name: Ferrari Dino<br />mpg_z: -0.06481307<br />cyl: 6","name: Merc 280<br />mpg_z: -0.14777380<br />cyl: 6","name: Valiant<br />mpg_z: -0.33028740<br />cyl: 6","name: Merc 280C<br />mpg_z: -0.38006384<br />cyl: 6"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(231,184,0,1)","opacity":1,"size":22.6771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(231,184,0,1)"}},"hoveron":"points","name":"(6,1)","legendgroup":"(6,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[19,20,21,22,23,24,25,26,27,28,29,30,31,32],"y":[-0.147773797335334,-0.230734525663942,-0.463024564984046,-0.612353875975541,-0.711906749969871,-0.761683186967036,-0.811459623964201,-0.811459623964201,-0.844643915295645,-0.89442035229281,-0.960788934955696,-1.12671039161291,-1.60788261591884,-1.60788261591884],"text":["name: Pontiac Firebird<br />mpg_z: -0.14777380<br />cyl: 8","name: Hornet Sportabout<br />mpg_z: -0.23073453<br />cyl: 8","name: Merc 450SL<br />mpg_z: -0.46302456<br />cyl: 8","name: Merc 450SE<br />mpg_z: -0.61235388<br />cyl: 8","name: Ford Pantera L<br />mpg_z: -0.71190675<br />cyl: 8","name: Dodge Challenger<br />mpg_z: -0.76168319<br />cyl: 8","name: Merc 450SLC<br />mpg_z: -0.81145962<br />cyl: 8","name: AMC Javelin<br />mpg_z: -0.81145962<br />cyl: 8","name: Maserati Bora<br />mpg_z: -0.84464392<br />cyl: 8","name: Chrysler Imperial<br />mpg_z: -0.89442035<br />cyl: 8","name: Duster 360<br />mpg_z: -0.96078893<br />cyl: 8","name: Camaro Z28<br />mpg_z: -1.12671039<br />cyl: 8","name: Cadillac Fleetwood<br />mpg_z: -1.60788262<br />cyl: 8","name: Lincoln Continental<br />mpg_z: -1.60788262<br />cyl: 8"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(252,78,7,1)","opacity":1,"size":22.6771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(252,78,7,1)"}},"hoveron":"points","name":"(8,1)","legendgroup":"(8,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[13,14,8,12,20,17,29,7,9,16,18,22,21,25,31,32,28,2,3,1,10,24,26,30,19,5,6,4,23,15,27,11],"y":[0.150884824647657,0.150884824647657,0.449543446630647,0.217253407310543,-0.230734525663942,-0.330287399658272,-0.960788934955696,0.715017777282194,0.449543446630647,-0.147773797335334,-0.380063836655437,-0.612353875975541,-0.463024564984046,-0.811459623964201,-1.60788261591884,-1.60788261591884,-0.89442035229281,2.04238943053993,1.7105465172255,2.29127161552575,0.233845552976265,-0.761683186967036,-0.811459623964201,-1.12671039161291,-0.147773797335334,1.19619000158812,0.980492107933741,1.7105465172255,-0.711906749969871,-0.0648130690067253,-0.844643915295645,0.217253407310543],"text":[0.2,0.2,0.4,0.2,-0.2,-0.3,-1,0.7,0.4,-0.1,-0.4,-0.6,-0.5,-0.8,-1.6,-1.6,-0.9,2,1.7,2.3,0.2,-0.8,-0.8,-1.1,-0.1,1.2,1,1.7,-0.7,-0.1,-0.8,0.2],"hovertext":["name: Mazda RX4<br />mpg_z:  0.15088482<br />label.xx:  0.2","name: Mazda RX4 Wag<br />mpg_z:  0.15088482<br />label.xx:  0.2","name: Datsun 710<br />mpg_z:  0.44954345<br />label.xx:  0.4","name: Hornet 4 Drive<br />mpg_z:  0.21725341<br />label.xx:  0.2","name: Hornet Sportabout<br />mpg_z: -0.23073453<br />label.xx: -0.2","name: Valiant<br />mpg_z: -0.33028740<br />label.xx: -0.3","name: Duster 360<br />mpg_z: -0.96078893<br />label.xx: -1.0","name: Merc 240D<br />mpg_z:  0.71501778<br />label.xx:  0.7","name: Merc 230<br />mpg_z:  0.44954345<br />label.xx:  0.4","name: Merc 280<br />mpg_z: -0.14777380<br />label.xx: -0.1","name: Merc 280C<br />mpg_z: -0.38006384<br />label.xx: -0.4","name: Merc 450SE<br />mpg_z: -0.61235388<br />label.xx: -0.6","name: Merc 450SL<br />mpg_z: -0.46302456<br />label.xx: -0.5","name: Merc 450SLC<br />mpg_z: -0.81145962<br />label.xx: -0.8","name: Cadillac Fleetwood<br />mpg_z: -1.60788262<br />label.xx: -1.6","name: Lincoln Continental<br />mpg_z: -1.60788262<br />label.xx: -1.6","name: Chrysler Imperial<br />mpg_z: -0.89442035<br />label.xx: -0.9","name: Fiat 128<br />mpg_z:  2.04238943<br />label.xx:  2.0","name: Honda Civic<br />mpg_z:  1.71054652<br />label.xx:  1.7","name: Toyota Corolla<br />mpg_z:  2.29127162<br />label.xx:  2.3","name: Toyota Corona<br />mpg_z:  0.23384555<br />label.xx:  0.2","name: Dodge Challenger<br />mpg_z: -0.76168319<br />label.xx: -0.8","name: AMC Javelin<br />mpg_z: -0.81145962<br />label.xx: -0.8","name: Camaro Z28<br />mpg_z: -1.12671039<br />label.xx: -1.1","name: Pontiac Firebird<br />mpg_z: -0.14777380<br />label.xx: -0.1","name: Fiat X1-9<br />mpg_z:  1.19619000<br />label.xx:  1.2","name: Porsche 914-2<br />mpg_z:  0.98049211<br />label.xx:  1.0","name: Lotus Europa<br />mpg_z:  1.71054652<br />label.xx:  1.7","name: Ford Pantera L<br />mpg_z: -0.71190675<br />label.xx: -0.7","name: Ferrari Dino<br />mpg_z: -0.06481307<br />label.xx: -0.1","name: Maserati Bora<br />mpg_z: -0.84464392<br />label.xx: -0.8","name: Volvo 142E<br />mpg_z:  0.21725341<br />label.xx:  0.2"],"textfont":{"size":11.3385826771654,"color":"rgba(255,255,255,1)"},"type":"scatter","mode":"text","hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[0.4,32.6],"y":[0,0],"text":"yintercept: 0","type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(211,211,211,1)","dash":"dash"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":27.1581569115816,"r":7.97011207970112,"b":182.515566625156,"l":43.8356164383562},"plot_bgcolor":"rgba(255,255,255,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,32.6],"tickmode":"array","ticktext":["Toyota Corolla","Fiat 128","Honda Civic","Lotus Europa","Fiat X1-9","Porsche 914-2","Merc 240D","Datsun 710","Merc 230","Toyota Corona","Volvo 142E","Hornet 4 Drive","Mazda RX4","Mazda RX4 Wag","Ferrari Dino","Merc 280","Valiant","Merc 280C","Pontiac Firebird","Hornet Sportabout","Merc 450SL","Merc 450SE","Ford Pantera L","Dodge Challenger","Merc 450SLC","AMC Javelin","Maserati Bora","Chrysler Imperial","Duster 360","Camaro Z28","Cadillac Fleetwood","Lincoln Continental"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32],"categoryorder":"array","categoryarray":["Toyota Corolla","Fiat 128","Honda Civic","Lotus Europa","Fiat X1-9","Porsche 914-2","Merc 240D","Datsun 710","Merc 230","Toyota Corona","Volvo 142E","Hornet 4 Drive","Mazda RX4","Mazda RX4 Wag","Ferrari Dino","Merc 280","Valiant","Merc 280C","Pontiac Firebird","Hornet Sportabout","Merc 450SL","Merc 450SE","Ford Pantera L","Dodge Challenger","Merc 450SLC","AMC Javelin","Maserati Bora","Chrysler Imperial","Duster 360","Camaro Z28","Cadillac Fleetwood","Lincoln Continental"],"nticks":null,"ticks":"outside","tickcolor":"rgba(0,0,0,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022},"tickangle":-90,"showline":true,"linecolor":"rgba(0,0,0,1)","linewidth":0.66417600664176,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"name","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1.80284032749107,2.48622932709798],"tickmode":"array","ticktext":["-1","0","1","2"],"tickvals":[-1,0,1,2],"categoryorder":"array","categoryarray":["-1","0","1","2"],"nticks":null,"ticks":"outside","tickcolor":"rgba(0,0,0,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022},"tickangle":-0,"showline":true,"linecolor":"rgba(0,0,0,1)","linewidth":0.66417600664176,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"mpg_z","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":2.06156048675734,"font":{"color":"rgba(0,0,0,1)","family":"","size":12.7521793275218},"title":{"text":"cyl","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022}}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"363c5f4724e7":{"x":{},"y":{},"colour":{},"ymin":{},"ymax":{},"type":"scatter"},"363c30735d01":{"x":{},"y":{},"colour":{}},"363c2ef016af":{"x":{},"y":{},"label":{}},"363c15597f3d":{"yintercept":{}}},"cur_data":"363c5f4724e7","visdat":{"363c5f4724e7":["function (y) ","x"],"363c30735d01":["function (y) ","x"],"363c2ef016af":["function (y) ","x"],"363c15597f3d":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

## 2.7Cleveland点图绘制

``` r
library(ggpubr)
library(ggpubr)
# 加载数据集
data("mtcars")
df2 <- mtcars
df2$cyl <- factor(df2$cyl)
df2$name <- rownames(df2) #添加一新列name
# 偏差图绘制(Deviation graphs),偏差图展示了与参考值之间的偏差。
df2$mpg_z <- (df2$mpg-mean(df2$mpg))/sd(df2$mpg)    
# 相当于Zscore标准化，减均值，除标准差
df2$mpg_grp <- factor(ifelse(df2$mpg_z<0, "low", "high"), 
                      levels = c("low", "high"))
# theme_cleveland()主题可设置为Cleveland点图样式
p13 <- ggdotchart(df2, x = "name", y = "mpg",
                    color = "cyl", # Color by groups                    
                    palette = c("#00AFBB", "#E7B800", "#FC4E07"), # Custom color palette                    
                    sorting = "descending", # Sort value in descending order                    
                    rotate = TRUE, # Rotate vertically                    
                    dot.size = 2, # Large dot size                    
                    y.text.col = TRUE, # Color y text by groups                    
                    ggtheme = theme_pubr() # ggplot2 theme
                    ) +                    
                    theme_cleveland() # Add dashed grids
```

    ## Warning: Vectorized input to `element_text()` is not officially supported.
    ## Results may be unexpected or may change in future versions of ggplot2.

``` r
p13
```

<img src="/post/2022-11-15-ggpubr/ggpubr包-学习笔记_files/figure-html/unnamed-chunk-8-1.png" width="672" />

# 3.导出图片

``` r
library(ggpubr)
library(datasets)
# Load data
data(ToothGrowth)
df <- ToothGrowth
df$dose <- as.factor(df$dose)

# Box plot
bxp <- ggboxplot(df, x = "dose", y = "len",
    color = "dose", palette = "jco")
# Dot plot
dp <- ggdotplot(df, x = "dose", y = "len",
    color = "dose", palette = "jco")
# Density plot
dens <- ggdensity(df, x = "len", fill = "dose", palette = "jco")

# Export to pdf
#ggarrange(bxp, dp, dens, ncol = 2) %>%
 # ggexport(filename = "test.pdf")

# Export to png
#ggarrange(bxp, dp, dens, ncol = 2) %>%
  #ggexport(filename = "test.png")
```

# 4.常用基本绘图函数及参数

## 4.1基本绘图函数

``` r
#gghistogram        Histogram plot #绘制直方图
#ggdensity        Density plot #绘制密度图
#ggdotplot        Dot plot #绘制点图
#ggdotchart        Cleveland's Dot Plots #绘制Cleveland点图
#ggline        Line plot #绘制线图
#ggbarplot        Bar plot #绘制条形/柱状图
#ggerrorplot        Visualizing Error #绘制误差棒图
#ggstripchart        Stripcharts #绘制线带图
#ggboxplot        Box plot #绘制箱线图
#ggviolin        Violin plot #绘制小提琴图
#ggpie        Pie chart #绘制饼图
#ggqqplot        QQ Plots #绘制QQ图
#ggscatter        Scatter plot #绘制散点图
#ggmaplot        MA-plot from means and log fold changes #绘制M-A图
#ggpaired        Plot Paired Data #绘制配对数据
#ggecdf          Empirical cumulative density function  #绘制经验累积密度分布图
```

## 4.2基本参数

``` r
# ggtext        Text #添加文本
# border        Set ggplot Panel Border Line #设置画布边框线
# grids        Add Grids to a ggplot #添加网格线
# font        Change the Appearance of Titles and Axis Labels #设置字体类型
# bgcolor        Change ggplot Panel Background Color #更改画布背景颜色
# background_image        Add Background Image to ggplot2 #添加背景图片
# facet        Facet a ggplot into Multiple Panels #设置分面
# ggpar        Graphical parameters #添加画图参数
# ggparagraph        Draw a Paragraph of Text #添加文本段落
# ggtexttable        Draw a Textual Table #添加文本表格
# ggadd        Add Summary Statistics or a Geom onto a ggplot #添加基本统计结果或其他几何图形
# ggarrange        Arrange Multiple ggplots #排版多个图形
# annotate_figure          Annotate Arranged Figure #添加注释信息
# gradient_color        Set Gradient Color #设置连续型颜色
# xscale        Change Axis Scale: log2, log10 and more #更改坐标轴的标度
# add_summary        Add Summary Statistics onto a ggplot #添加基本统计结果
# set_palette        Set Color Palette #设置画板颜色
# rotate        Rotate a ggplot Horizontally #设置图形旋转
# rotate_axis_text        Rotate Axes Text #旋转坐标轴文本
# stat_stars        Add Stars to a Scatter Plot #添加散点图星标
# stat_cor        Add Correlation Coefficients with P-values to a Scatter Plot #添加相关系数
# stat_compare_means        Add Mean Comparison P-values to a ggplot #添加平均值比较的P值
# diff_express      Differential gene expression analysis results #内置差异分析结果数据集
# ggexport    Export ggplots # 导出图片
# theme_transparent        Create a ggplot with Transparent Background #设置透明背景
# theme_pubr        Publication ready theme #设置出版物主题
```

# 5.参考来源

### \[1\]: <https://www.rdocumentation.org/packages/ggpubr/versions/0.1.4>

### \[2\]: <https://mp.weixin.qq.com/s/ZKxzKZ4NBTcsJ6vFimxoGA>

### \[3\]: <http://blog.sciencenet.cn/blog-3334560-1091714.html>

### \[4\]: <https://mp.weixin.qq.com/s/ZR2sfhVnqxHwDydz7iCGRw>
