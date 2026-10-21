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

- use rvest package for html data

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

- use chromote package to do this!
- html_element: extracts html, selects first element automatically
- html_text: converts html to text

## get some water data

this is coming from an API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/api/v3/views/ia2d-e54m/query.csv") |> 
  content("parsed")
```

    ## Rows: 65 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): :id, :version
    ## dbl  (4): year, new_york_city_population, nyc_consumption_million_gallons_pe...
    ## dttm (2): :created_at, :updated_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

- GET: from httr package
- content(“parsed”): goes from html to tibble

## BRPSS dataset

same process, different data

``` r
brfss_2010 = 
  GET("https://chronicdata.cdc.gov/api/v3/views/acme-vg9e/query.csv") |> 
  content("parsed")
```

    ## Rows: 134203 Columns: 27
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (18): :id, :version, locationabbr, locationdesc, class, topic, question...
    ## dbl   (6): year, sample_size, data_value, confidence_limit_low, confidence_l...
    ## lgl   (1): locationid
    ## dttm  (2): :created_at, :updated_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## some data aren’t nice…complicated

pokemon

``` r
pokemon = 
  GET("http://pokeapi.co/api/v2/pokemon/1") |>
  content()

pokemon[["name"]]
```

    ## [1] "bulbasaur"

``` r
pokemon[["height"]]
```

    ## [1] 7

``` r
pokemon[["abilities"]]
```

    ## [[1]]
    ## [[1]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[1]]$slot
    ## [1] 1
    ## 
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "overgrow"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/65/"
    ## 
    ## 
    ## 
    ## [[2]]
    ## [[2]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[2]]$slot
    ## [1] 3
    ## 
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "chlorophyll"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/34/"
