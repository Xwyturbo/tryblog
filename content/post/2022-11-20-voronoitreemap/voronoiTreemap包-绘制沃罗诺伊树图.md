---
title: voronoiTreemap包-绘制沃罗诺伊树图
author: Xwxyturbo
date: '2022-11-20'
slug: voronoiTreemap包-绘制沃罗诺伊树图
categories:
  - Example
tags:
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
<script src="/rmarkdown-libs/d3vt/d3.v4.min.js"></script>
<script src="/rmarkdown-libs/d3vt/plugins/d3-voronoi-treemap.js"></script>
<script src="/rmarkdown-libs/d3vt/plugins/d3-voronoi-map.js"></script>
<script src="/rmarkdown-libs/d3vt/plugins/d3-weighted-voronoi.js"></script>
<script src="/rmarkdown-libs/d3vt/plugins/seedrandom.min.js"></script>
<script src="/rmarkdown-libs/d3vt-binding/d3vt.js"></script>

``` r
library(voronoiTreemap)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.5 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(writexl)
library(readxl)
df <- read_xlsx("D:/wenyu/Rrojects/vTreemap/df.xlsx")
df %>% 
  knitr::kable()
```

| h1    | h2     | h3              | color    | weight | codes | province |   prod |
|:------|:-------|:----------------|:---------|-------:|:------|:---------|-------:|
| Total | East   | Anhui           | \#CED7BA |   4.35 | AH    | AH       |  95.20 |
| Total | North  | Beijing         | \#009593 |   5.05 | BJ    | BJ       | 110.30 |
| Total | South  | Chongqing       | \#E4D1B3 |   3.80 | CQ    | CQ       |  82.90 |
| Total | South  | Fujian          | \#E4D1B3 |   4.88 | FJ    | FJ       | 106.60 |
| Total | South  | Guangdong       | \#E4D1B3 |   4.66 | GD    | GD       | 101.70 |
| Total | North  | Gansu           | \#009593 |   1.72 | GS    | GS       |  37.60 |
| Total | South  | Guangxi         | \#E4D1B3 |   4.04 | GX    | GX       |  88.10 |
| Total | South  | Guizhou         | \#E4D1B3 |   3.11 | GZ    | GZ       |  67.90 |
| Total | East   | Henan           | \#CED7BA |   3.55 | HA    | HA       |  77.60 |
| Total | East   | Hubei           | \#CED7BA |   4.19 | HB    | HB       |  91.40 |
| Total | North  | Hebei           | \#009593 |   3.25 | HE    | HE       |  71.00 |
| Total | South  | Hainan          | \#E4D1B3 |   3.56 | HI    | HI       |  77.80 |
| Total | Nodata | Hongkonng       | \#D35C79 |   0.01 | HK    | HK       |   0.01 |
| Total | North  | Heilongjiang    | \#009593 |   0.52 | HL    | HL       |  11.40 |
| Total | South  | Hunan           | \#E4D1B3 |   3.75 | HN    | HN       |  81.90 |
| Total | North  | Jilin           | \#009593 |   0.65 | JL    | JL       |  14.10 |
| Total | East   | Jiangsu         | \#CED7BA |   4.47 | JS    | JS       |  97.50 |
| Total | South  | Jiangxi         | \#E4D1B3 |   4.03 | JX    | JX       |  88.00 |
| Total | North  | Liaoning        | \#009593 |   1.44 | LN    | LN       |  31.50 |
| Total | Nodata | Macao           | \#D35C79 |   0.01 | MO    | MO       |   0.01 |
| Total | North  | InnerMongoriaIM | \#009593 |   1.36 | NM    | NM       |  29.60 |
| Total | North  | Ningxia         | \#009593 |   2.49 | NX    | NX       |  54.30 |
| Total | North  | Qinghai         | \#009593 |   1.95 | QH    | QH       |  42.60 |
| Total | South  | Sichuan         | \#E4D1B3 |   3.81 | SC    | SC       |  83.10 |
| Total | East   | Shandong        | \#CED7BA |   4.13 | SD    | SD       |  90.10 |
| Total | East   | Shanghai        | \#CED7BA |   4.20 | SH    | SH       |  91.60 |
| Total | North  | Shanxi          | \#009593 |   2.60 | SN    | SX       |  29.30 |
| Total | North  | Shaanxi         | \#009593 |   1.34 | SX    | SX       |  56.80 |
| Total | North  | Tianjing        | \#009593 |   4.37 | TJ    | TJ       |  95.50 |
| Total | Nodata | Taiwan          | \#D35C79 |   0.01 | TW    | TW       |   0.01 |
| Total | North  | Xingjiang       | \#009593 |   3.76 | XJ    | XJ       |  82.10 |
| Total | South  | Xizang          | \#E4D1B3 |   0.55 | XZ    | XZ       |  11.90 |
| Total | South  | Yunnan          | \#E4D1B3 |   3.87 | YN    | YN       |  84.40 |
| Total | East   | Zhejiang        | \#CED7BA |   4.54 | ZJ    | ZJ       |  99.10 |

``` r
solar_json <- vt_export_json(vt_input_from_df(df, 
                                            scaleToPerc = FALSE,
                                            hierachyVar0 = "h1",
                                            hierachyVar1 = "h2", 
                                            hierachyVar2 = "h3", 
                                            colorVar = "color", 
                                            weightVar="prod",
                                            labelVar = "codes"))
vt_d3(solar_json,label = T, 
      color_border = "#000000",
      legend = TRUE, legend_title = "Urban Economic Region",
      seed = 2,
      size_border = "1px")
```

<div id="htmlwidget-1" style="width:672px;height:480px;" class="d3vt html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"data":"{\"name\":\"Total\",\"children\":[{\"name\":\"East\",\"color\":\"#CED7BA\",\"children\":[{\"name\":\"Anhui\",\"weight\":95.2,\"color\":\"#CED7BA\",\"code\":\"AH\"},{\"name\":\"Henan\",\"weight\":77.6,\"color\":\"#CED7BA\",\"code\":\"HA\"},{\"name\":\"Hubei\",\"weight\":91.4,\"color\":\"#CED7BA\",\"code\":\"HB\"},{\"name\":\"Jiangsu\",\"weight\":97.5,\"color\":\"#CED7BA\",\"code\":\"JS\"},{\"name\":\"Shandong\",\"weight\":90.1,\"color\":\"#CED7BA\",\"code\":\"SD\"},{\"name\":\"Shanghai\",\"weight\":91.6,\"color\":\"#CED7BA\",\"code\":\"SH\"},{\"name\":\"Zhejiang\",\"weight\":99.1,\"color\":\"#CED7BA\",\"code\":\"ZJ\"}]},{\"name\":\"North\",\"color\":\"#009593\",\"children\":[{\"name\":\"Beijing\",\"weight\":110.3,\"color\":\"#009593\",\"code\":\"BJ\"},{\"name\":\"Gansu\",\"weight\":37.6,\"color\":\"#009593\",\"code\":\"GS\"},{\"name\":\"Hebei\",\"weight\":71,\"color\":\"#009593\",\"code\":\"HE\"},{\"name\":\"Heilongjiang\",\"weight\":11.4,\"color\":\"#009593\",\"code\":\"HL\"},{\"name\":\"Jilin\",\"weight\":14.1,\"color\":\"#009593\",\"code\":\"JL\"},{\"name\":\"Liaoning\",\"weight\":31.5,\"color\":\"#009593\",\"code\":\"LN\"},{\"name\":\"InnerMongoriaIM\",\"weight\":29.6,\"color\":\"#009593\",\"code\":\"NM\"},{\"name\":\"Ningxia\",\"weight\":54.3,\"color\":\"#009593\",\"code\":\"NX\"},{\"name\":\"Qinghai\",\"weight\":42.6,\"color\":\"#009593\",\"code\":\"QH\"},{\"name\":\"Shanxi\",\"weight\":29.3,\"color\":\"#009593\",\"code\":\"SN\"},{\"name\":\"Shaanxi\",\"weight\":56.8,\"color\":\"#009593\",\"code\":\"SX\"},{\"name\":\"Tianjing\",\"weight\":95.5,\"color\":\"#009593\",\"code\":\"TJ\"},{\"name\":\"Xingjiang\",\"weight\":82.1,\"color\":\"#009593\",\"code\":\"XJ\"}]},{\"name\":\"South\",\"color\":\"#E4D1B3\",\"children\":[{\"name\":\"Chongqing\",\"weight\":82.9,\"color\":\"#E4D1B3\",\"code\":\"CQ\"},{\"name\":\"Fujian\",\"weight\":106.6,\"color\":\"#E4D1B3\",\"code\":\"FJ\"},{\"name\":\"Guangdong\",\"weight\":101.7,\"color\":\"#E4D1B3\",\"code\":\"GD\"},{\"name\":\"Guangxi\",\"weight\":88.1,\"color\":\"#E4D1B3\",\"code\":\"GX\"},{\"name\":\"Guizhou\",\"weight\":67.9,\"color\":\"#E4D1B3\",\"code\":\"GZ\"},{\"name\":\"Hainan\",\"weight\":77.8,\"color\":\"#E4D1B3\",\"code\":\"HI\"},{\"name\":\"Hunan\",\"weight\":81.9,\"color\":\"#E4D1B3\",\"code\":\"HN\"},{\"name\":\"Jiangxi\",\"weight\":88,\"color\":\"#E4D1B3\",\"code\":\"JX\"},{\"name\":\"Sichuan\",\"weight\":83.1,\"color\":\"#E4D1B3\",\"code\":\"SC\"},{\"name\":\"Xizang\",\"weight\":11.9,\"color\":\"#E4D1B3\",\"code\":\"XZ\"},{\"name\":\"Yunnan\",\"weight\":84.4,\"color\":\"#E4D1B3\",\"code\":\"YN\"}]},{\"name\":\"Nodata\",\"color\":\"#D35C79\",\"children\":[{\"name\":\"Hongkonng\",\"weight\":0.01,\"color\":\"#D35C79\",\"code\":\"HK\"},{\"name\":\"Macao\",\"weight\":0.01,\"color\":\"#D35C79\",\"code\":\"MO\"},{\"name\":\"Taiwan\",\"weight\":0.01,\"color\":\"#D35C79\",\"code\":\"TW\"}]}]}","options":{"legend":true,"title":null,"legend_title":"Urban Economic Region","seed":2,"footer":null,"label":true},"colors":{"circle":"#aaaaaa","border":"#000000","label":"#000000"},"size":{"border":"1px","border_hover":"3px","circle":"2px"}},"evals":[],"jsHooks":[]}</script>
