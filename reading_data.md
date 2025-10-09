Reading data from the web
================
2025-10-09

## Scrape a table

I want the first table from [this
page](https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm)

read in the html

``` r
url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

extract the table(s); focus on the first one

``` r
tabl_marj = 
  drug_use_html %>%
  html_nodes(css = "table") %>%
  .[[1]] %>%
  html_table() %>%
  slice(-1) %>%
  as_tibble()
```

## Star War Movie info

I want the data from [here](https://www.imdb.com/list/ls070150896/).

``` r
url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(url)
```

Grab elements that I want

``` r
title_vec = 
  swm_html %>%
  html_nodes(css = ".ipc-title-link-wrapper .ipc-title__text--reduced") %>%
  html_text()

runtime_vec = 
  swm_html %>%
  html_nodes(css = ".dli-title-metadata-item:nth-child(2)") %>%
  html_text()


swm_df = 
  tibble(
    title = title_vec,
    runtime = runtime_vec)
```

## Get some water data

This is coming from an API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>%
  content("parsed")
```

    ## Rows: 65 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>%
  content("text") %>%
  jsonlite::fromJSON() %>%
  as_tibble()
```
