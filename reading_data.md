Reading Data From the Web
================
2026-10-21

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
library(chromote)
```

## scrape table

i want the first table from [this
page](https://www.samhsa.gov/data/sites/default/files/NSDUHsaeShortTermCHG2015/NSDUHsaeShortTermCHG2015.htm)

read in the html

``` r
url = "https://www.samhsa.gov/data/sites/default/files/NSDUHsaeShortTermCHG2015/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

extract table(s); focus on first one

``` r
table_mj =
  drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

- first: selects first element
- html_table: prints html into table
- slice: removes row

## Star Wars movie info

I want the data from [here](https://www.imdb.com/list/ls070150896/)

``` r
sw_html = 
  read_html_live("https://www.imdb.com/list/ls070150896/")
```

- read_html_live helps with non-html files

grab elements i want

``` r
title_vec =
  sw_html |> 
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |> 
  html_text()
  
metascore_vec = 
  sw_html |>
  html_elements(".metacritic-score-box") |>
  html_text()

runtime_vec = 
  sw_html |>
  html_elements(".base .ipc-inline-list__item:nth-child(2)") |>
  html_text()

sw_df = 
  tibble(
    title = title_vec,
    score = metascore_vec,
    runtime = runtime_vec)
```

- html_element: extract html with css tag, selects first element
  automatically
- html_text: converts html to text
