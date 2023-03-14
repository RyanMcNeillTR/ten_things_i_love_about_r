More Cool Things
================

# Housekeeping

``` r
library(tidyverse)
```

    ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ✔ ggplot2 3.4.1     ✔ purrr   1.0.1
    ✔ tibble  3.1.8     ✔ dplyr   1.1.0
    ✔ tidyr   1.3.0     ✔ stringr 1.5.0
    ✔ readr   2.1.4     ✔ forcats 1.0.0
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()

``` r
library(sf)
```

    Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE

``` r
library(stars)
```

    Loading required package: abind

# Quick Tips

Turn off scientific notation.

``` r
options(scipen=999)
```

# GIS stuff

## Lots of buffers

Here’s a fun way to make lots of buffer lots of distances at once.

Say you have a point.

``` r
zurich_dot <- tribble(
  ~site, ~x, ~y,
  "Zurich", 8.541111, 47.374444
  ) %>%
  st_as_sf(coords = c("x", "y"), crs = 4326)
```

And you want to make buffers for every 10km.

Step by Step. First we use the full_seq function to create a vector of
values from 10 to 100 by 10.

We turn it into a tibble.

We rename the field.

``` r
full_seq(c(10000, 100000), 10000) %>% # this function create
  enframe() %>% 
  rename(distance=value) 
```

    # A tibble: 10 × 2
        name distance
       <int>    <dbl>
     1     1    10000
     2     2    20000
     3     3    30000
     4     4    40000
     5     5    50000
     6     6    60000
     7     7    70000
     8     8    80000
     9     9    90000
    10    10   100000

So now imagine if we had a field to contain the buffer shape.

``` r
lotsa_buffers <- full_seq(c(10000, 100000), 10000) %>% 
  enframe() %>% 
  rename(distance=value) %>%
  mutate(the_buffer = st_buffer(zurich_dot$geometry, dist=distance)) %>%
  st_as_sf()

lotsa_buffers
```

    Simple feature collection with 10 features and 2 fields
    Geometry type: POLYGON
    Dimension:     XY
    Bounding box:  xmin: 7.199455 ymin: 46.4655 xmax: 9.881571 ymax: 48.28313
    Geodetic CRS:  WGS 84
    # A tibble: 10 × 3
        name distance                                                     the_buffer
       <int>    <dbl>                                                  <POLYGON [°]>
     1     1    10000 ((8.565262 47.28541, 8.565444 47.28601, 8.570637 47.28562, 8.…
     2     2    20000 ((8.806307 47.38841, 8.807057 47.39082, 8.807806 47.39323, 8.…
     3     3    30000 ((8.895287 47.25031, 8.895475 47.25091, 8.900688 47.2505, 8.9…
     4     4    40000 ((8.993648 47.56411, 8.995183 47.56894, 8.989161 47.56941, 8.…
     5     5    50000 ((9.050543 47.66495, 9.051705 47.66858, 9.04566 47.66906, 9.0…
     6     6    60000 ((9.246589 47.12339, 9.246979 47.12459, 9.257416 47.12374, 9.…
     7     7    70000 ((9.434924 47.178, 9.44131 47.19715, 9.447703 47.21631, 9.454…
     8     8    80000 ((9.452241 47.75037, 9.455479 47.76005, 9.443292 47.76106, 9.…
     9     9    90000 ((9.492854 47.87149, 9.494485 47.87634, 9.482249 47.87736, 9.…
    10    10   100000 ((9.683831 47.84049, 9.687154 47.85018, 9.674897 47.85122, 9.…

Yo that’s fancy.

OK, but now we need to put it to use.

Let’s say we have a raster of 2020 population density in Switzerland. We
can read it into a stars object like so.

``` r
swiss_raster <- read_stars("./data/che_pd_2020_1km.tif")
```

For the purposes of this example, let’s convert to vectors.

``` r
swiss_raster_sf <- swiss_raster %>%
  st_as_sf()
```

Now let’s do a spatial filter for each buffer.

``` r
lotsa_buffers_w_swiss_geoms <- lotsa_buffers %>%
  mutate(the_geoms = map(st_geometry(the_buffer), function(x) { st_filter(swiss_raster_sf, x) }))
```

Now let’s take a look.

``` r
lotsa_buffers_w_swiss_geoms %>%
  st_drop_geometry() 
```

    # A tibble: 10 × 3
        name distance the_geoms        
     * <int>    <dbl> <list>           
     1     1    10000 <sf [585 × 2]>   
     2     2    20000 <sf [2,258 × 2]> 
     3     3    30000 <sf [4,854 × 2]> 
     4     4    40000 <sf [8,294 × 2]> 
     5     5    50000 <sf [11,983 × 2]>
     6     6    60000 <sf [16,090 × 2]>
     7     7    70000 <sf [20,358 × 2]>
     8     8    80000 <sf [24,825 × 2]>
     9     9    90000 <sf [29,064 × 2]>
    10    10   100000 <sf [33,695 × 2]>

OK so what we need is in the the_geoms. It’s a list, which means we can
use unnest. Now look at what we have.

``` r
lotsa_buffers_w_swiss_geoms %>%
  st_drop_geometry() %>% 
  unnest_wider(the_geoms) %>%
  select(!geometry) %>% 
  janitor::clean_names() %>% 
  unnest_longer(che_pd_2020_1km_tif)
```

    # A tibble: 152,006 × 3
        name distance che_pd_2020_1km_tif
       <int>    <dbl>               <dbl>
     1     1    10000                583.
     2     1    10000                722.
     3     1    10000                603.
     4     1    10000                467.
     5     1    10000               1042.
     6     1    10000               1073.
     7     1    10000                390.
     8     1    10000                444.
     9     1    10000               1399.
    10     1    10000                935.
    # … with 151,996 more rows

Let’s take it even farther. Now you know the population within each
buffer.

``` r
lotsa_buffers_w_swiss_geoms %>%
  st_drop_geometry() %>% 
  unnest_wider(the_geoms) %>%
  select(!geometry) %>% 
  janitor::clean_names() %>% 
  unnest_longer(che_pd_2020_1km_tif) %>%
  group_by(distance) %>%
  summarise(tot_pop = sum(che_pd_2020_1km_tif))
```

    # A tibble: 10 × 2
       distance   tot_pop
          <dbl>     <dbl>
     1    10000  1319375.
     2    20000  2606336.
     3    30000  3794749.
     4    40000  5041164.
     5    50000  5946273.
     6    60000  6693469.
     7    70000  7374181.
     8    80000  8581312.
     9    90000  9053498.
    10   100000 10112546.
