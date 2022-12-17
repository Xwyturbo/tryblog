---
title: gganimate包-GIF绘制导出
author: Xwyturbo
date: '2022-12-17'
slug: gganimate包-GIF绘制导出
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


#  1.1 加载数据集
##  原始数据：使用gapminder这个数据集合。
##  该数据一共有6列，依次为
##  country（国家）continent（洲）
##  year（年份）lifeExp（生活指数）
##  pop（人口） gdpPercap（国内生产总值）

```r
library(gapminder)
```

```
## Warning: package 'gapminder' was built under R version 4.2.2
```

```r
library(ggdark)
```

```
## Warning: package 'ggdark' was built under R version 4.2.2
```

```r
library(ggplot2)
library(gganimate)
```

```
## Warning: package 'gganimate' was built under R version 4.2.2
```

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ✔ purrr   0.3.5      
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
head(gapminder)
```

```
## # A tibble: 6 × 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

```r
mydata <- gapminder %>% 
  filter(country == 'France'|
           country == 'Italy'|
           country == 'China'|
           country == 'Japan'|
           country == 'Austria'|
           country == 'Brazil'|
           country == 'Colombia'|
           country == 'Cuba'|
           country == 'Germany'|
           country == 'India')
mydata
```

```
## # A tibble: 120 × 6
##    country continent  year lifeExp     pop gdpPercap
##    <fct>   <fct>     <int>   <dbl>   <int>     <dbl>
##  1 Austria Europe     1952    66.8 6927772     6137.
##  2 Austria Europe     1957    67.5 6965860     8843.
##  3 Austria Europe     1962    69.5 7129864    10751.
##  4 Austria Europe     1967    70.1 7376998    12835.
##  5 Austria Europe     1972    70.6 7544201    16662.
##  6 Austria Europe     1977    72.2 7568430    19749.
##  7 Austria Europe     1982    73.2 7574613    21597.
##  8 Austria Europe     1987    74.9 7578903    23688.
##  9 Austria Europe     1992    76.0 7914969    27042.
## 10 Austria Europe     1997    77.5 8069876    29096.
## # … with 110 more rows
```

#  1.2 绘制动态条形图
##  使用gganimate这个包绘制动态图：
##  使用函数transition_time()添加动态，
##  并指定动态依据哪个变量变化，这里动态变量是year。

```r
ps1 = ggplot(mydata, aes(x=reorder(country, lifeExp),
                        y=lifeExp, fill=country,frame=year)) +
  geom_bar(stat= 'identity', position = 'dodge',show.legend = FALSE) +
  geom_text(aes(label=paste0(lifeExp)),
            col="black",hjust=-0.2)+
  theme(axis.text.x = element_text(size = 12,angle = 90,
                                   hjust = 0.2, vjust = 0.2),
        legend.position="none") +
  theme(panel.background=element_rect(fill='transparent'))+
  theme(axis.text.y=element_text(angle=0,colour="black",
                                 size=12,hjust=1))+
  theme(axis.text.x=element_text(angle=0,colour="white",
                                 size=2,hjust=1))+
  theme(panel.grid =element_blank()) + ## 删去网格线
  theme(axis.text = element_blank()) + ## 删去所有刻度标签
  theme(axis.ticks = element_blank()) + ## 删去所有刻度线
  coord_flip()+ #横纵坐标位置转换
  transition_time(year) + #设置动态
  labs(title = paste('Year:', '{frame_time}'),x = '',
       y ='各国生活指数')+
  ease_aes('linear')
ps1
```

![](gganimate包-GIF绘制导出_files/figure-html/unnamed-chunk-2-1.gif)<!-- -->

#  1.3 绘制动态点图
##  使用gganimate这个包绘制动态图：
##  使用函数transition_time()添加动态，
##  并指定动态依据哪个变量变化，这里动态变量是year。

```r
ps2 = ggplot(mydata, aes(x=year,y=lifeExp)) +
  geom_point(aes(color = country)) +
  theme_bw()+
  transition_time(year) + #设置动态
  labs(title = paste('Year:', '{frame_time}'),x = '',
       y ='各国生活指数')+
  ease_aes('linear')
ps2
```

![](gganimate包-GIF绘制导出_files/figure-html/unnamed-chunk-3-1.gif)<!-- -->

#  1.4 绘制动态折线图
##  使用gganimate这个包绘制动态图：
##  使用函数transition_reveal()添加动态，
##  并指定动态依据哪个变量变化，这里动态变量是year。

```r
ps3 = ggplot(mydata, aes(x=year,y=lifeExp)) +
  geom_line(aes(color = country)) +
  geom_point(aes(color = country)) +
  theme_bw()+
  transition_reveal(year) +
  labs(x = 'Year', 
       y ='各国生活指数')+
  ease_aes('linear')
ps3
```

```
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
```

![](gganimate包-GIF绘制导出_files/figure-html/unnamed-chunk-4-1.gif)<!-- -->

#  1.5读取excel表数据，绘制动态折线图

```r
library(tidyverse)
library(readxl)
library(ggplot2)
library(ggpubr)
library(patchwork)
library(gganimate)
x3 <- read_xlsx("D:/wenyu/BruceR/不同批次下训练的损失值.xlsx",sheet = 3)
x3
```

```
## # A tibble: 60 × 5
##    epoch train_loss train_acc val_loss val_acc
##    <dbl>      <dbl>     <dbl>    <dbl>   <dbl>
##  1     1      0.686     0.577    0.662   0.649
##  2     2      0.678     0.579    0.66    0.622
##  3     3      0.666     0.582    0.618   0.648
##  4     4      0.559     0.613    0.411   0.89 
##  5     5      0.343     0.911    0.264   0.922
##  6     6      0.274     0.912    0.26    0.931
##  7     7      0.265     0.904    0.19    0.944
##  8     8      0.184     0.934    0.156   0.944
##  9     9      0.238     0.909    0.204   0.934
## 10    10      0.187     0.929    0.142   0.959
## # … with 50 more rows
```

```r
x31 <- x3[,c(1,2,4)] %>% 
  pivot_longer(-epoch,names_to = "type", 
               values_to = "loss") 
x31
```

```
## # A tibble: 120 × 3
##    epoch type        loss
##    <dbl> <chr>      <dbl>
##  1     1 train_loss 0.686
##  2     1 val_loss   0.662
##  3     2 train_loss 0.678
##  4     2 val_loss   0.66 
##  5     3 train_loss 0.666
##  6     3 val_loss   0.618
##  7     4 train_loss 0.559
##  8     4 val_loss   0.411
##  9     5 train_loss 0.343
## 10     5 val_loss   0.264
## # … with 110 more rows
```

```r
ps31 = ggplot(x31, aes(x=epoch,y=loss)) +
  geom_line(aes(color = type)) +
  geom_point(aes(color = type)) +
  theme_bw()+
  transition_reveal(epoch) +
  labs(x = 'epoch', 
       y ='loss')+
  ease_aes('linear')
ps31
```

```
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
```

![](gganimate包-GIF绘制导出_files/figure-html/unnamed-chunk-5-1.gif)<!-- -->

```r
# ps31 <- animate(ps,fps = 24,duration = 15)
# anim_save("ps031.gif")
```

