Ten Things I Love About R
================

# The tidyverse

When I first started using R, there was no
[tidyverse](https://www.tidyverse.org). There was just base R, which
caused me heartburn.

But the tidyverse changed all that. It makes using R a breeze.

The tidyverse has packages for [reading
data](https://readr.tidyverse.org), [manipulating
data](https://dplyr.tidyverse.org), [cleaning
data](https://tidyr.tidyverse.org) and many other things.

You can install them with just a simple command.

``` r
#install.packages(tidyverse)
```

Then you can load all the packages at once:

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

The people behind the tidyverse believe in what they call tidy data.
It’s a concept we use every single day.

Each column is a variable. Each record is an observation. Every cell is
a single value.

Those concepts flow through other packages not in the tidyverse, but
adhere to tidy data principles.

For example, [sf](https://r-spatial.github.io/sf/) is a GIS package that
adheres to tidy principles, which you’ll see below. For rasters, there’s
[stars](https://r-spatial.github.io/stars/) package that’s meant for
spatio-temporal arrays.

For machine learning/modelling, there’s
[tidymodels](https://www.tidymodels.org).

One other thing. You’ll notice a number of R packages, particularly in
the tidyverse, have these

# Import Data

Importing data is a pain in the butt. I think R makes some of those
tasks easier.

You can deal with lots of

For example, if I need to get data into Postgres or SQL Server, I’ll use
R as an intermediary.

I can use the tidyverse’s [readr](https://readr.tidyverse.org).

``` r
single_dataset <- read_csv("./data/importing/abw.csv")
```

    Rows: 4 Columns: 19
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (1): indicator
    dbl (18): 2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, ...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
single_dataset
```

    # A tibble: 4 × 19
      indic…¹ `2000`  `2001`  `2002`  `2003`  `2004`  `2005`  `2006`  `2007`  `2008`
      <chr>    <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    1 SP.URB… 4.16e4 4.20e+4 4.22e+4 4.23e+4 4.23e+4 4.24e+4 4.26e+4 4.27e+4 4.29e+4
    2 SP.URB… 1.66e0 9.56e-1 4.01e-1 1.97e-1 9.46e-2 1.94e-1 3.67e-1 4.08e-1 4.13e-1
    3 SP.POP… 8.91e4 9.07e+4 9.18e+4 9.27e+4 9.35e+4 9.45e+4 9.56e+4 9.68e+4 9.80e+4
    4 SP.POP… 2.54e0 1.77e+0 1.19e+0 9.97e-1 9.01e-1 1.00e+0 1.18e+0 1.23e+0 1.24e+0
    # … with 9 more variables: `2009` <dbl>, `2010` <dbl>, `2011` <dbl>,
    #   `2012` <dbl>, `2013` <dbl>, `2014` <dbl>, `2015` <dbl>, `2016` <dbl>,
    #   `2017` <dbl>, and abbreviated variable name ¹​indicator

You’ll see there are other [readr
functions](https://readr.tidyverse.org/reference/index.html) to help you
import data.

Sidenote: Have a SAS, SPSS or Stata dataset? You can import it with
[haven](https://haven.tidyverse.org).

One thing I like to do is import everything as character, then later
convert to appropriate data types. I can do that easily with
[read_csv](https://readr.tidyverse.org/reference/read_delim.html).

``` r
single_dataset <- read_csv("./data/importing/abw.csv", col_types = cols(.default = "c"))
```

# FS

OK. Say you have a directory of files. You want to import them all into
a single dataset.

There’s a tidy-friendly package called [fs](https://fs.r-lib.org) for
working with file systems.

You can install it like this:

``` r
#install.packages("fs")
```

Ok back to the topic at hand.

Let’s initialize fs.

``` r
library(fs)
```

What’s in our directory? Annoyingly, there’s a different file for every
country. What to do?

``` r
dir_ls(path="./data/importing")
```

    ./data/importing/ABW.csv ./data/importing/AFE.csv ./data/importing/AFG.csv 
    ./data/importing/AFW.csv ./data/importing/AGO.csv ./data/importing/ALB.csv 
    ./data/importing/AND.csv ./data/importing/ARB.csv ./data/importing/ARE.csv 
    ./data/importing/ARG.csv ./data/importing/ARM.csv ./data/importing/ASM.csv 
    ./data/importing/ATG.csv ./data/importing/AUS.csv ./data/importing/AUT.csv 
    ./data/importing/AZE.csv ./data/importing/BDI.csv ./data/importing/BEL.csv 
    ./data/importing/BEN.csv ./data/importing/BFA.csv ./data/importing/BGD.csv 
    ./data/importing/BGR.csv ./data/importing/BHR.csv ./data/importing/BHS.csv 
    ./data/importing/BIH.csv ./data/importing/BLR.csv ./data/importing/BLZ.csv 
    ./data/importing/BMU.csv ./data/importing/BOL.csv ./data/importing/BRA.csv 
    ./data/importing/BRB.csv ./data/importing/BRN.csv ./data/importing/BTN.csv 
    ./data/importing/BWA.csv ./data/importing/CAF.csv ./data/importing/CAN.csv 
    ./data/importing/CEB.csv ./data/importing/CHE.csv ./data/importing/CHI.csv 
    ./data/importing/CHL.csv ./data/importing/CHN.csv ./data/importing/CIV.csv 
    ./data/importing/CMR.csv ./data/importing/COD.csv ./data/importing/COG.csv 
    ./data/importing/COL.csv ./data/importing/COM.csv ./data/importing/CPV.csv 
    ./data/importing/CRI.csv ./data/importing/CSS.csv ./data/importing/CUB.csv 
    ./data/importing/CUW.csv ./data/importing/CYM.csv ./data/importing/CYP.csv 
    ./data/importing/CZE.csv ./data/importing/DEU.csv ./data/importing/DJI.csv 
    ./data/importing/DMA.csv ./data/importing/DNK.csv ./data/importing/DOM.csv 
    ./data/importing/DZA.csv ./data/importing/EAP.csv ./data/importing/EAR.csv 
    ./data/importing/EAS.csv ./data/importing/ECA.csv ./data/importing/ECS.csv 
    ./data/importing/ECU.csv ./data/importing/EGY.csv ./data/importing/EMU.csv 
    ./data/importing/ERI.csv ./data/importing/ESP.csv ./data/importing/EST.csv 
    ./data/importing/ETH.csv ./data/importing/EUU.csv ./data/importing/FCS.csv 
    ./data/importing/FIN.csv ./data/importing/FJI.csv ./data/importing/FRA.csv 
    ./data/importing/FRO.csv ./data/importing/FSM.csv ./data/importing/GAB.csv 
    ./data/importing/GBR.csv ./data/importing/GEO.csv ./data/importing/GHA.csv 
    ./data/importing/GIB.csv ./data/importing/GIN.csv ./data/importing/GMB.csv 
    ./data/importing/GNB.csv ./data/importing/GNQ.csv ./data/importing/GRC.csv 
    ./data/importing/GRD.csv ./data/importing/GRL.csv ./data/importing/GTM.csv 
    ./data/importing/GUM.csv ./data/importing/GUY.csv ./data/importing/HIC.csv 
    ./data/importing/HKG.csv ./data/importing/HND.csv ./data/importing/HPC.csv 
    ./data/importing/HRV.csv ./data/importing/HTI.csv ./data/importing/HUN.csv 
    ./data/importing/IBD.csv ./data/importing/IBT.csv ./data/importing/IDA.csv 
    ./data/importing/IDB.csv ./data/importing/IDN.csv ./data/importing/IDX.csv 
    ./data/importing/IMN.csv ./data/importing/IND.csv ./data/importing/INX.csv 
    ./data/importing/IRL.csv ./data/importing/IRN.csv ./data/importing/IRQ.csv 
    ./data/importing/ISL.csv ./data/importing/ISR.csv ./data/importing/ITA.csv 
    ./data/importing/JAM.csv ./data/importing/JOR.csv ./data/importing/JPN.csv 
    ./data/importing/KAZ.csv ./data/importing/KEN.csv ./data/importing/KGZ.csv 
    ./data/importing/KHM.csv ./data/importing/KIR.csv ./data/importing/KNA.csv 
    ./data/importing/KOR.csv ./data/importing/KWT.csv ./data/importing/LAC.csv 
    ./data/importing/LAO.csv ./data/importing/LBN.csv ./data/importing/LBR.csv 
    ./data/importing/LBY.csv ./data/importing/LCA.csv ./data/importing/LCN.csv 
    ./data/importing/LDC.csv ./data/importing/LIC.csv ./data/importing/LIE.csv 
    ./data/importing/LKA.csv ./data/importing/LMC.csv ./data/importing/LMY.csv 
    ./data/importing/LSO.csv ./data/importing/LTE.csv ./data/importing/LTU.csv 
    ./data/importing/LUX.csv ./data/importing/LVA.csv ./data/importing/MAC.csv 
    ./data/importing/MAF.csv ./data/importing/MAR.csv ./data/importing/MCO.csv 
    ./data/importing/MDA.csv ./data/importing/MDG.csv ./data/importing/MDV.csv 
    ./data/importing/MEA.csv ./data/importing/MEX.csv ./data/importing/MHL.csv 
    ./data/importing/MIC.csv ./data/importing/MKD.csv ./data/importing/MLI.csv 
    ./data/importing/MLT.csv ./data/importing/MMR.csv ./data/importing/MNA.csv 
    ./data/importing/MNE.csv ./data/importing/MNG.csv ./data/importing/MNP.csv 
    ./data/importing/MOZ.csv ./data/importing/MRT.csv ./data/importing/MUS.csv 
    ./data/importing/MWI.csv ./data/importing/MYS.csv ./data/importing/NAC.csv 
    ./data/importing/NAM.csv ./data/importing/NCL.csv ./data/importing/NER.csv 
    ./data/importing/NGA.csv ./data/importing/NIC.csv ./data/importing/NLD.csv 
    ./data/importing/NOR.csv ./data/importing/NPL.csv ./data/importing/NRU.csv 
    ./data/importing/NZL.csv ./data/importing/OED.csv ./data/importing/OMN.csv 
    ./data/importing/OSS.csv ./data/importing/PAK.csv ./data/importing/PAN.csv 
    ./data/importing/PER.csv ./data/importing/PHL.csv ./data/importing/PLW.csv 
    ./data/importing/PNG.csv ./data/importing/POL.csv ./data/importing/PRE.csv 
    ./data/importing/PRI.csv ./data/importing/PRK.csv ./data/importing/PRT.csv 
    ./data/importing/PRY.csv ./data/importing/PSE.csv ./data/importing/PSS.csv 
    ./data/importing/PST.csv ./data/importing/PYF.csv ./data/importing/QAT.csv 
    ./data/importing/ROU.csv ./data/importing/RUS.csv ./data/importing/RWA.csv 
    ./data/importing/SAS.csv ./data/importing/SAU.csv ./data/importing/SDN.csv 
    ./data/importing/SEN.csv ./data/importing/SGP.csv ./data/importing/SLB.csv 
    ./data/importing/SLE.csv ./data/importing/SLV.csv ./data/importing/SMR.csv 
    ./data/importing/SOM.csv ./data/importing/SRB.csv ./data/importing/SSA.csv 
    ./data/importing/SSD.csv ./data/importing/SSF.csv ./data/importing/SST.csv 
    ./data/importing/STP.csv ./data/importing/SUR.csv ./data/importing/SVK.csv 
    ./data/importing/SVN.csv ./data/importing/SWE.csv ./data/importing/SWZ.csv 
    ./data/importing/SXM.csv ./data/importing/SYC.csv ./data/importing/SYR.csv 
    ./data/importing/TCA.csv ./data/importing/TCD.csv ./data/importing/TEA.csv 
    ./data/importing/TEC.csv ./data/importing/TGO.csv ./data/importing/THA.csv 
    ./data/importing/TJK.csv ./data/importing/TKM.csv ./data/importing/TLA.csv 
    ./data/importing/TLS.csv ./data/importing/TMN.csv ./data/importing/TON.csv 
    ./data/importing/TSA.csv ./data/importing/TSS.csv ./data/importing/TTO.csv 
    ./data/importing/TUN.csv ./data/importing/TUR.csv ./data/importing/TUV.csv 
    ./data/importing/TZA.csv ./data/importing/UGA.csv ./data/importing/UKR.csv 
    ./data/importing/UMC.csv ./data/importing/URY.csv ./data/importing/USA.csv 
    ./data/importing/UZB.csv ./data/importing/VCT.csv ./data/importing/VEN.csv 
    ./data/importing/VGB.csv ./data/importing/VIR.csv ./data/importing/VNM.csv 
    ./data/importing/VUT.csv ./data/importing/WLD.csv ./data/importing/WSM.csv 
    ./data/importing/XKX.csv ./data/importing/YEM.csv ./data/importing/ZAF.csv 
    ./data/importing/ZMB.csv ./data/importing/ZWE.csv 

fs makes it easy.

``` r
imported_data_collection <- dir_ls(path="./data/importing") %>%
  map_df(read_csv, .id = "file", col_types = cols(.default = "c")) # map applies a function to each record...so if each filename from dir_ls is a record, this applies read_csv to each record. 

imported_data_collection
```

    # A tibble: 1,064 × 20
       file   indic…¹ `2000` `2001` `2002` `2003` `2004` `2005` `2006` `2007` `2008`
       <chr>  <chr>   <chr>  <chr>  <chr>  <chr>  <chr>  <chr>  <chr>  <chr>  <chr> 
     1 ./dat… SP.URB… 41625  42025  42194  42277  42317  42399  42555  42729  42906 
     2 ./dat… SP.URB… 1.664… 0.956… 0.401… 0.196… 0.094… 0.193… 0.367… 0.408… 0.413…
     3 ./dat… SP.POP… 89101  90691  91781  92701  93540  94483  95606  96787  97996 
     4 ./dat… SP.POP… 2.539… 1.768… 1.194… 0.997… 0.900… 1.003… 1.181… 1.227… 1.241…
     5 ./dat… SP.URB… 11555… 11977… 12422… 12883… 13364… 13874… 14402… 14923… 15538…
     6 ./dat… SP.URB… 3.602… 3.655… 3.716… 3.708… 3.736… 3.814… 3.806… 3.613… 4.122…
     7 ./dat… SP.POP… 40160… 41200… 42274… 43380… 44528… 45715… 46950… 48240… 49574…
     8 ./dat… SP.POP… 2.583… 2.589… 2.606… 2.617… 2.644… 2.666… 2.702… 2.747… 2.765…
     9 ./dat… SP.URB… 43147… 43647… 46748… 50618… 52995… 55420… 58282… 59870… 61628…
    10 ./dat… SP.URB… 1.861… 1.153… 6.863… 7.953… 4.588… 4.474… 5.034… 2.688… 2.893…
    # … with 1,054 more rows, 9 more variables: `2009` <chr>, `2010` <chr>,
    #   `2011` <chr>, `2012` <chr>, `2013` <chr>, `2014` <chr>, `2015` <chr>,
    #   `2016` <chr>, `2017` <chr>, and abbreviated variable name ¹​indicator

And, take note how it also includes the filename from which that data
originated. Useful.

# Clean column names

I use a single function from the [janitor
package](https://sfirke.github.io/janitor/index.html). More precisely: I
use
[clean_names](https://sfirke.github.io/janitor/reference/clean_names.html)
constantly.

The data we just imported gives us a perfect use case. Here are our ugly
column names. Column names starting with numbers? Unacceptable.

``` r
colnames(imported_data_collection)
```

     [1] "file"      "indicator" "2000"      "2001"      "2002"      "2003"     
     [7] "2004"      "2005"      "2006"      "2007"      "2008"      "2009"     
    [13] "2010"      "2011"      "2012"      "2013"      "2014"      "2015"     
    [19] "2016"      "2017"     

Yuck. But we can use clean_names:

``` r
imported_data_collection_fixed <- imported_data_collection %>%
  janitor::clean_names() # Notice here that I used the library name and function name. You can do this if you don't want to load the entire package into memory. 

imported_data_collection_fixed
```

    # A tibble: 1,064 × 20
       file      indic…¹ x2000 x2001 x2002 x2003 x2004 x2005 x2006 x2007 x2008 x2009
       <chr>     <chr>   <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr>
     1 ./data/i… SP.URB… 41625 42025 42194 42277 42317 42399 42555 42729 42906 43079
     2 ./data/i… SP.URB… 1.66… 0.95… 0.40… 0.19… 0.09… 0.19… 0.36… 0.40… 0.41… 0.40…
     3 ./data/i… SP.POP… 89101 90691 91781 92701 93540 94483 95606 96787 97996 99212
     4 ./data/i… SP.POP… 2.53… 1.76… 1.19… 0.99… 0.90… 1.00… 1.18… 1.22… 1.24… 1.23…
     5 ./data/i… SP.URB… 1155… 1197… 1242… 1288… 1336… 1387… 1440… 1492… 1553… 1617…
     6 ./data/i… SP.URB… 3.60… 3.65… 3.71… 3.70… 3.73… 3.81… 3.80… 3.61… 4.12… 4.11…
     7 ./data/i… SP.POP… 4016… 4120… 4227… 4338… 4452… 4571… 4695… 4824… 4957… 5094…
     8 ./data/i… SP.POP… 2.58… 2.58… 2.60… 2.61… 2.64… 2.66… 2.70… 2.74… 2.76… 2.75…
     9 ./data/i… SP.URB… 4314… 4364… 4674… 5061… 5299… 5542… 5828… 5987… 6162… 6443…
    10 ./data/i… SP.URB… 1.86… 1.15… 6.86… 7.95… 4.58… 4.47… 5.03… 2.68… 2.89… 4.44…
    # … with 1,054 more rows, 8 more variables: x2010 <chr>, x2011 <chr>,
    #   x2012 <chr>, x2013 <chr>, x2014 <chr>, x2015 <chr>, x2016 <chr>,
    #   x2017 <chr>, and abbreviated variable name ¹​indicator

# Work with dates

``` r
library(nycflights13)
library(lubridate)
```


    Attaching package: 'lubridate'

    The following objects are masked from 'package:base':

        date, intersect, setdiff, union

We can use lubridate to do some nice things.

``` r
flights %>% 
  select(year, month, day, hour, minute) %>% 
  mutate(departure = make_datetime(year, month, day, hour, minute))
```

    # A tibble: 336,776 × 6
        year month   day  hour minute departure          
       <int> <int> <int> <dbl>  <dbl> <dttm>             
     1  2013     1     1     5     15 2013-01-01 05:15:00
     2  2013     1     1     5     29 2013-01-01 05:29:00
     3  2013     1     1     5     40 2013-01-01 05:40:00
     4  2013     1     1     5     45 2013-01-01 05:45:00
     5  2013     1     1     6      0 2013-01-01 06:00:00
     6  2013     1     1     5     58 2013-01-01 05:58:00
     7  2013     1     1     6      0 2013-01-01 06:00:00
     8  2013     1     1     6      0 2013-01-01 06:00:00
     9  2013     1     1     6      0 2013-01-01 06:00:00
    10  2013     1     1     6      0 2013-01-01 06:00:00
    # … with 336,766 more rows

We can easily extract elements of the date.

``` r
flights %>% 
  select(year, month, day, hour, minute) %>% 
  mutate(departure = make_datetime(year, month, day, hour, minute)) %>%
  select(departure) %>%
  mutate(the_year = year(departure),
         the_day = day(departure))
```

    # A tibble: 336,776 × 3
       departure           the_year the_day
       <dttm>                 <dbl>   <int>
     1 2013-01-01 05:15:00     2013       1
     2 2013-01-01 05:29:00     2013       1
     3 2013-01-01 05:40:00     2013       1
     4 2013-01-01 05:45:00     2013       1
     5 2013-01-01 06:00:00     2013       1
     6 2013-01-01 05:58:00     2013       1
     7 2013-01-01 06:00:00     2013       1
     8 2013-01-01 06:00:00     2013       1
     9 2013-01-01 06:00:00     2013       1
    10 2013-01-01 06:00:00     2013       1
    # … with 336,766 more rows

And then we can do all kinds of things with differences in times.

``` r
make_datetime_100 <- function(year, month, day, time) {
  make_datetime(year, month, day, time %/% 100, time %% 100)
}

flights_dt <- flights %>% 
  filter(!is.na(dep_time), !is.na(arr_time)) %>% 
  mutate(
    dep_time = make_datetime_100(year, month, day, dep_time),
    arr_time = make_datetime_100(year, month, day, arr_time),
    sched_dep_time = make_datetime_100(year, month, day, sched_dep_time),
    sched_arr_time = make_datetime_100(year, month, day, sched_arr_time)
  ) %>% 
  select(origin, dest, ends_with("delay"), ends_with("time"))

flights_dt
```

    # A tibble: 328,063 × 9
       origin dest  dep_delay arr_delay dep_time            sched_dep_time     
       <chr>  <chr>     <dbl>     <dbl> <dttm>              <dttm>             
     1 EWR    IAH           2        11 2013-01-01 05:17:00 2013-01-01 05:15:00
     2 LGA    IAH           4        20 2013-01-01 05:33:00 2013-01-01 05:29:00
     3 JFK    MIA           2        33 2013-01-01 05:42:00 2013-01-01 05:40:00
     4 JFK    BQN          -1       -18 2013-01-01 05:44:00 2013-01-01 05:45:00
     5 LGA    ATL          -6       -25 2013-01-01 05:54:00 2013-01-01 06:00:00
     6 EWR    ORD          -4        12 2013-01-01 05:54:00 2013-01-01 05:58:00
     7 EWR    FLL          -5        19 2013-01-01 05:55:00 2013-01-01 06:00:00
     8 LGA    IAD          -3       -14 2013-01-01 05:57:00 2013-01-01 06:00:00
     9 JFK    MCO          -3        -8 2013-01-01 05:57:00 2013-01-01 06:00:00
    10 LGA    ORD          -2         8 2013-01-01 05:58:00 2013-01-01 06:00:00
    # … with 328,053 more rows, and 3 more variables: arr_time <dttm>,
    #   sched_arr_time <dttm>, air_time <dbl>

You can do simple date comparisons.

``` r
flights_dt %>% 
  mutate(late_departure = difftime(ymd_hms(sched_dep_time), ymd_hms(dep_time), units="secs"))
```

    # A tibble: 328,063 × 10
       origin dest  dep_delay arr_delay dep_time            sched_dep_time     
       <chr>  <chr>     <dbl>     <dbl> <dttm>              <dttm>             
     1 EWR    IAH           2        11 2013-01-01 05:17:00 2013-01-01 05:15:00
     2 LGA    IAH           4        20 2013-01-01 05:33:00 2013-01-01 05:29:00
     3 JFK    MIA           2        33 2013-01-01 05:42:00 2013-01-01 05:40:00
     4 JFK    BQN          -1       -18 2013-01-01 05:44:00 2013-01-01 05:45:00
     5 LGA    ATL          -6       -25 2013-01-01 05:54:00 2013-01-01 06:00:00
     6 EWR    ORD          -4        12 2013-01-01 05:54:00 2013-01-01 05:58:00
     7 EWR    FLL          -5        19 2013-01-01 05:55:00 2013-01-01 06:00:00
     8 LGA    IAD          -3       -14 2013-01-01 05:57:00 2013-01-01 06:00:00
     9 JFK    MCO          -3        -8 2013-01-01 05:57:00 2013-01-01 06:00:00
    10 LGA    ORD          -2         8 2013-01-01 05:58:00 2013-01-01 06:00:00
    # … with 328,053 more rows, and 4 more variables: arr_time <dttm>,
    #   sched_arr_time <dttm>, air_time <dbl>, late_departure <drtn>

# Interact with databases

OK. I’ve imported my data and I want to send it to Postgres.

[RPostgres](https://rpostgres.r-dbi.org) makes that easy.

First, if needed, install via:

``` r
#install.packages("RPostgres")
```

To connect with the DB, you first need to set up a little connection
string to the DB. If you’ve ever used ODBC, it’s kinda similar to that
process.

Here’s one for a local Postgres install. Notice I used the
askForPassword function. That’s so I don’t encode my username and
password when I plan to upload this to Github.

``` r
library(RPostgres)

con <- dbConnect(RPostgres::Postgres(),
                 dbname = 'test2', 
                 host = "localhost", 
                 port = 5432, # or any other port specified by your DBA
                 user = "rstudio", 
                 password = "baseball1"
)
```

From here, you can do [all kinds of
things](https://rpostgres.r-dbi.org/reference/index.html).

Want to send our dataset to the DB?

``` r
dbWriteTable(con, "my_table_name", imported_data_collection_fixed, overwrite=TRUE)
```

Want to read it back?

``` r
showing_off <- dbReadTable(con, "my_table_name")

glimpse(showing_off)
```

    Rows: 1,064
    Columns: 20
    $ file      <chr> "./data/importing/ABW.csv", "./data/importing/ABW.csv", "./d…
    $ indicator <chr> "SP.URB.TOTL", "SP.URB.GROW", "SP.POP.TOTL", "SP.POP.GROW", …
    $ x2000     <chr> "41625", "1.66422212392663", "89101", "2.53923444445246", "1…
    $ x2001     <chr> "42025", "0.956373099407145", "90691", "1.76875662143885", "…
    $ x2002     <chr> "42194", "0.401335154395215", "91781", "1.19471805545637", "…
    $ x2003     <chr> "42277", "0.196517211141057", "92701", "0.997395547284924", …
    $ x2004     <chr> "42317", "0.0945693618486456", "93540", "0.900989229759957",…
    $ x2005     <chr> "42399", "0.193588048559493", "94483", "1.00307718391638", "…
    $ x2006     <chr> "42555", "0.367257987480091", "95606", "1.18156554573069", "…
    $ x2007     <chr> "42729", "0.408048969163451", "96787", "1.22771081489399", "…
    $ x2008     <chr> "42906", "0.413382967375507", "97996", "1.24139737678817", "…
    $ x2009     <chr> "43079", "0.402396309678717", "99212", "1.23323132057229", "…
    $ x2010     <chr> "43206", "0.294373510368369", "100341", "1.13154104048985", …
    $ x2011     <chr> "43493", "0.662063111068539", "101288", "0.939355909629835",…
    $ x2012     <chr> "43864", "0.849393249627712", "102112", "0.810230587788097",…
    $ x2013     <chr> "44228", "0.826413457840807", "102880", "0.749301039347625",…
    $ x2014     <chr> "44588", "0.810669184722173", "103594", "0.691615260101675",…
    $ x2015     <chr> "44943", "0.793025567597756", "104257", "0.63795916173479", …
    $ x2016     <chr> "45297", "0.784578492708788", "104874", "0.590062487333077",…
    $ x2017     <chr> "45648", "0.771898934070187", "105439", "0.537295706145068",…

What if you want to write your own SQL?

You can do it a couple ways.

``` r
# first way
showing_off <- dbGetQuery(con, "select indicator, x2000 from my_table_name")

showing_off
```

           indicator               x2000
    1    SP.URB.TOTL               41625
    2    SP.URB.GROW    1.66422212392663
    3    SP.POP.TOTL               89101
    4    SP.POP.GROW    2.53923444445246
    5    SP.URB.TOTL           115551653
    6    SP.URB.GROW    3.60226233978761
    7    SP.POP.TOTL           401600588
    8    SP.POP.GROW     2.5835792421522
    9    SP.URB.TOTL             4314700
    10   SP.URB.GROW    1.86137729962901
    11   SP.POP.TOTL            19542982
    12   SP.POP.GROW     1.4438030241194
    13   SP.URB.TOTL            95272890
    14   SP.URB.GROW    4.14518949083499
    15   SP.POP.TOTL           269611898
    16   SP.POP.GROW    2.74959971917366
    17   SP.URB.TOTL             8211294
    18   SP.URB.GROW    5.64866955326432
    19   SP.POP.TOTL            16394062
    20   SP.POP.GROW    3.24412146672851
    21   SP.URB.TOTL             1289391
    22   SP.URB.GROW   0.742478629285177
    23   SP.POP.TOTL             3089027
    24   SP.POP.GROW  -0.637356833943492
    25   SP.URB.TOTL               61070
    26   SP.URB.GROW   0.377327984395132
    27   SP.POP.TOTL               66097
    28   SP.POP.GROW   0.670960073758389
    29   SP.URB.TOTL           152305719
    30   SP.URB.GROW    2.76137514575139
    31   SP.POP.TOTL           287065982
    32   SP.POP.GROW    2.28593431739384
    33   SP.URB.TOTL             2627996
    34   SP.URB.GROW    6.11272936104507
    35   SP.POP.TOTL             3275333
    36   SP.POP.GROW    5.58038700101525
    37   SP.URB.TOTL            33045629
    38   SP.URB.GROW    1.34664689815167
    39   SP.POP.TOTL            37070774
    40   SP.POP.GROW    1.13327702210541
    41   SP.URB.TOTL             2048957
    42   SP.URB.GROW   -1.61037490478258
    43   SP.POP.TOTL             3168523
    44   SP.POP.GROW   -1.17678628879226
    45   SP.URB.TOTL               51584
    46   SP.URB.GROW     1.6142305917451
    47   SP.POP.TOTL               58230
    48   SP.POP.GROW    1.09822902395737
    49   SP.URB.TOTL               24113
    50   SP.URB.GROW    0.53641727893261
    51   SP.POP.TOTL               75055
    52   SP.POP.GROW    1.65779341376666
    53   SP.URB.TOTL            16028911
    54   SP.URB.GROW   0.984333804176905
    55   SP.POP.TOTL            19028802
    56   SP.POP.GROW    1.14447285147721
    57   SP.URB.TOTL             4824004
    58   SP.URB.GROW  -0.220175991539749
    59   SP.POP.TOTL             8011566
    60   SP.POP.GROW   0.240466652446524
    61   SP.URB.TOTL             4135854
    62   SP.URB.GROW    1.21344378120309
    63   SP.POP.TOTL             8048600
    64   SP.POP.GROW   0.821519963674211
    65   SP.URB.TOTL              520130
    66   SP.URB.GROW    4.62153763088361
    67   SP.POP.TOTL             6307659
    68   SP.POP.GROW     2.0417212041938
    69   SP.URB.TOTL             9956937
    70   SP.URB.GROW   0.308431312307231
    71   SP.POP.TOTL            10251250
    72   SP.POP.GROW   0.242517956221334
    73   SP.URB.TOTL             2682552
    74   SP.URB.GROW    3.87148751501267
    75   SP.POP.TOTL             6998023
    76   SP.POP.GROW    3.03845662138601
    77   SP.URB.TOTL             2120383
    78   SP.URB.GROW    6.85756484832227
    79   SP.POP.TOTL            11882888
    80   SP.POP.GROW    2.98388558736392
    81   SP.URB.TOTL            30476706
    82   SP.URB.GROW    3.56396663214103
    83   SP.POP.TOTL           129193327
    84   SP.POP.GROW    1.90552404891968
    85   SP.URB.TOTL             5629167
    86   SP.URB.GROW  -0.171157650642926
    87   SP.POP.TOTL             8170172
    88   SP.POP.GROW  -0.493896416633318
    89   SP.URB.TOTL              628716
    90   SP.URB.GROW    2.74720093000752
    91   SP.POP.TOTL              711442
    92   SP.POP.GROW     2.7515762605179
    93   SP.URB.TOTL              266534
    94   SP.URB.GROW    1.69321459564921
    95   SP.POP.TOTL              325014
    96   SP.POP.GROW    1.46976235766802
    97   SP.URB.TOTL             1771376
    98   SP.URB.GROW    1.37816316655527
    99   SP.POP.TOTL             4179350
    100  SP.POP.GROW   0.632139635256837
    101  SP.URB.TOTL             6983033
    102  SP.URB.GROW   0.233049857909096
    103  SP.POP.TOTL             9979610
    104  SP.POP.GROW  -0.471131334643547
    105  SP.URB.TOTL              109140
    106  SP.URB.GROW    2.84002286407907
    107  SP.POP.TOTL              240406
    108  SP.POP.GROW    3.24372870048271
    109  SP.URB.TOTL               61833
    110  SP.URB.GROW   0.890208773614165
    111  SP.POP.TOTL               61833
    112  SP.POP.GROW   0.890208773614165
    113  SP.URB.TOTL             5309144
    114  SP.URB.GROW    2.65383928729288
    115  SP.POP.TOTL             8592656
    116  SP.POP.GROW    1.80379049207336
    117  SP.URB.TOTL           142795391
    118  SP.URB.GROW    2.22769397356534
    119  SP.POP.TOTL           175873720
    120  SP.POP.GROW    1.36677132973907
    121  SP.URB.TOTL               89526
    122  SP.URB.GROW  -0.771096201563446
    123  SP.POP.TOTL              264657
    124  SP.POP.GROW    0.18418126822941
    125  SP.URB.TOTL              237635
    126  SP.URB.GROW    2.78031536453181
    127  SP.POP.TOTL              333926
    128  SP.POP.GROW    2.08216354349989
    129  SP.URB.TOTL              149256
    130  SP.URB.GROW     6.9578805637825
    131  SP.POP.TOTL              587207
    132  SP.POP.GROW    2.80057018423756
    133  SP.URB.TOTL              919084
    134  SP.URB.GROW    3.67700580793188
    135  SP.POP.TOTL             1726985
    136  SP.POP.GROW    2.07271158015976
    137  SP.URB.TOTL             1414914
    138  SP.URB.GROW    3.01842513430412
    139  SP.POP.TOTL             3759170
    140  SP.POP.GROW    2.80036047898137
    141  SP.URB.TOTL            24388404
    142  SP.URB.GROW    1.40422595439655
    143  SP.POP.TOTL            30685730
    144  SP.POP.GROW   0.931281553452429
    145  SP.URB.TOTL            66669587
    146  SP.URB.GROW  -0.608882078841546
    147  SP.POP.TOTL           108447824
    148  SP.POP.GROW  -0.562187468913606
    149  SP.URB.TOTL             5272018
    150  SP.URB.GROW   0.487032192288853
    151  SP.POP.TOTL             7184250
    152  SP.POP.GROW   0.561954617400164
    153  SP.URB.TOTL               44267
    154  SP.URB.GROW   0.391575969156542
    155  SP.POP.TOTL              145306
    156  SP.POP.GROW   0.341241968130063
    157  SP.URB.TOTL            13213754
    158  SP.URB.GROW    1.49236285157488
    159  SP.POP.TOTL            15351799
    160  SP.POP.GROW    1.14904172536735
    161  SP.URB.TOTL           452999147
    162  SP.URB.GROW    3.64925271115854
    163  SP.POP.TOTL          1262645000
    164  SP.POP.GROW   0.787956592953992
    165  SP.URB.TOTL             7249898
    166  SP.URB.GROW    3.70221408199557
    167  SP.POP.TOTL            16799670
    168  SP.POP.GROW    2.73825090531507
    169  SP.URB.TOTL             6873014
    170  SP.URB.GROW    3.95780816719243
    171  SP.POP.TOTL            15091594
    172  SP.POP.GROW    2.63602726719356
    173  SP.URB.TOTL            17075023
    174  SP.URB.GROW    4.22587294178986
    175  SP.POP.TOTL            48616317
    176  SP.POP.GROW    2.89884128806625
    177  SP.URB.TOTL             1839519
    178  SP.URB.GROW    4.61063543293432
    179  SP.POP.TOTL             3134030
    180  SP.POP.GROW    3.81697887153797
    181  SP.URB.TOTL            29002337
    182  SP.URB.GROW    2.19886492761409
    183  SP.POP.TOTL            39215135
    184  SP.POP.GROW    1.61983140551157
    185  SP.URB.TOTL              150722
    186  SP.URB.GROW    1.78397456253272
    187  SP.POP.TOTL              536758
    188  SP.POP.GROW    1.94042775607481
    189  SP.URB.TOTL              244866
    190  SP.URB.GROW    3.65530991464629
    191  SP.POP.TOTL              458251
    192  SP.POP.GROW    1.89987187752495
    193  SP.URB.TOTL             2349793
    194  SP.URB.GROW    3.50078660942677
    195  SP.POP.TOTL             3979193
    196  SP.POP.GROW    1.97358823214158
    197  SP.URB.TOTL             3317222
    198  SP.URB.GROW   0.991089510416259
    199  SP.POP.TOTL             6582141
    200  SP.POP.GROW   0.715212644247117
    201  SP.URB.TOTL             8365215
    202  SP.URB.GROW   0.633089365845533
    203  SP.POP.TOTL            11105791
    204  SP.POP.GROW    0.32194458830191
    205  SP.URB.TOTL              121481
    206  SP.URB.GROW   -3.46254083518543
    207  SP.POP.TOTL              133860
    208  SP.POP.GROW   -4.07538613162288
    209  SP.URB.TOTL               39658
    210  SP.URB.GROW    3.49733538150567
    211  SP.POP.TOTL               39658
    212  SP.POP.GROW    3.49733538150567
    213  SP.URB.TOTL              650946
    214  SP.URB.GROW    1.94647761607177
    215  SP.POP.TOTL              948237
    216  SP.POP.GROW    1.77009330000432
    217  SP.URB.TOTL             7587516
    218  SP.URB.GROW  -0.458662204702604
    219  SP.POP.TOTL            10255063
    220  SP.POP.GROW  -0.280414108078884
    221  SP.URB.TOTL            61629857
    222  SP.URB.GROW   0.411941893833571
    223  SP.POP.TOTL            82211508
    224  SP.POP.GROW   0.135431600393084
    225  SP.URB.TOTL              567893
    226  SP.URB.GROW    3.16311796731286
    227  SP.POP.TOTL              742033
    228  SP.POP.GROW     3.1016497493543
    229  SP.URB.TOTL               44606
    230  SP.URB.GROW  -0.382623560679492
    231  SP.POP.TOTL               68346
    232  SP.POP.GROW  -0.513704757719484
    233  SP.URB.TOTL             4544013
    234  SP.URB.GROW   0.350679180938332
    235  SP.POP.TOTL             5339616
    236  SP.POP.GROW   0.334233618829261
    237  SP.URB.TOTL             5274195
    238  SP.URB.GROW    2.86364158534241
    239  SP.POP.TOTL             8540791
    240  SP.POP.GROW    1.52687888895349
    241  SP.URB.TOTL            18439845
    242  SP.URB.GROW    2.74811230466512
    243  SP.POP.TOTL            30774621
    244  SP.POP.GROW    1.40229085828763
    245  SP.URB.TOTL           664869369
    246  SP.URB.GROW    3.59317184748342
    247  SP.POP.TOTL          1817064785
    248  SP.POP.GROW    1.00015598826199
    249  SP.URB.TOTL           957507719
    250  SP.URB.GROW    2.77926436781644
    251  SP.POP.TOTL          2499206970
    252  SP.POP.GROW    1.88565238059226
    253  SP.URB.TOTL           848763919
    254  SP.URB.GROW    2.94994040900316
    255  SP.POP.TOTL          2048148696
    256  SP.POP.GROW    0.94409698205007
    257  SP.URB.TOTL           238079874
    258  SP.URB.GROW   0.203129460542058
    259  SP.POP.TOTL           370646199
    260  SP.POP.GROW  0.0268381789157814
    261  SP.URB.TOTL           591738229
    262  SP.URB.GROW   0.304399863956235
    263  SP.POP.TOTL           862786211
    264  SP.POP.GROW   0.108791382445531
    265  SP.URB.TOTL             7613657
    266  SP.URB.GROW    2.55075761062712
    267  SP.POP.TOTL            12626507
    268  SP.POP.GROW    1.71308817043169
    269  SP.URB.TOTL            30544806
    270  SP.URB.GROW    2.18171596199955
    271  SP.POP.TOTL            71371371
    272  SP.POP.GROW    2.07183470577985
    273  SP.URB.TOTL           234381189
    274  SP.URB.GROW   0.588520975014291
    275  SP.POP.TOTL           321324622
    276  SP.POP.GROW   0.328616767600721
    277  SP.URB.TOTL              636195
    278  SP.URB.GROW    4.79009738761001
    279  SP.POP.TOTL             2392880
    280  SP.POP.GROW    1.53299543125693
    281  SP.URB.TOTL            30937864
    282  SP.URB.GROW   0.553405611176717
    283  SP.POP.TOTL            40567864
    284  SP.POP.GROW   0.447137014535835
    285  SP.URB.TOTL              969061
    286  SP.URB.GROW    0.23183289495095
    287  SP.POP.TOTL             1396985
    288  SP.POP.GROW   0.483707161729931
    289  SP.URB.TOTL             9880497
    290  SP.URB.GROW    4.23557684937443
    291  SP.POP.TOTL            67031867
    292  SP.POP.GROW    2.95880518919236
    293  SP.URB.TOTL           304085189
    294  SP.URB.GROW   0.340804127803992
    295  SP.POP.TOTL           429342455
    296  SP.POP.GROW   0.119792808014381
    297  SP.URB.TOTL           227304933
    298  SP.URB.GROW    2.84312307960302
    299  SP.POP.TOTL           612395529
    300  SP.POP.GROW    2.25004229908427
    301  SP.URB.TOTL             4253964
    302  SP.URB.GROW   0.496415301116337
    303  SP.POP.TOTL             5176209
    304  SP.POP.GROW     0.2076065154133
    305  SP.URB.TOTL              398838
    306  SP.URB.GROW    1.92111334652935
    307  SP.POP.TOTL              832509
    308  SP.POP.GROW    1.09752051437681
    309  SP.URB.TOTL            46221663
    310  SP.URB.GROW    1.02609018706937
    311  SP.POP.TOTL            60921384
    312  SP.POP.GROW   0.686782586857032
    313  SP.URB.TOTL               16591
    314  SP.URB.GROW    2.32320439136337
    315  SP.POP.TOTL               45660
    316  SP.POP.GROW    1.00591847845505
    317  SP.URB.TOTL               24945
    318  SP.URB.GROW   -1.70516264054059
    319  SP.POP.TOTL              111709
    320  SP.POP.GROW   0.152297026498527
    321  SP.URB.TOTL             1004078
    322  SP.URB.GROW    3.54974250712575
    323  SP.POP.TOTL             1272935
    324  SP.POP.GROW    2.55979388649606
    325  SP.URB.TOTL            46319551
    326  SP.URB.GROW   0.433615661051976
    327  SP.POP.TOTL            58892514
    328  SP.POP.GROW   0.357300887421605
    329  SP.URB.TOTL             2146120
    330  SP.URB.GROW   -2.40145855729194
    331  SP.POP.TOTL             4077131
    332  SP.POP.GROW    -1.9446291567975
    333  SP.URB.TOTL             8638858
    334  SP.URB.GROW    4.22698334801559
    335  SP.POP.TOTL            19665502
    336  SP.POP.GROW    2.51651873851715
    337  SP.URB.TOTL               27741
    338  SP.URB.GROW    0.16595717785441
    339  SP.POP.TOTL               27741
    340  SP.POP.GROW    0.16595717785441
    341  SP.URB.TOTL             2573538
    342  SP.URB.GROW    2.85469725493748
    343  SP.POP.TOTL             8336967
    344  SP.POP.GROW    1.96313638467112
    345  SP.URB.TOTL              688121
    346  SP.URB.GROW     4.8093581717006
    347  SP.POP.TOTL             1437539
    348  SP.POP.GROW    2.89642334291935
    349  SP.URB.TOTL              446097
    350  SP.URB.GROW    3.04871478018358
    351  SP.POP.TOTL             1230849
    352  SP.POP.GROW    2.00021192650957
    353  SP.URB.TOTL              336269
    354  SP.URB.GROW    8.08360381858775
    355  SP.POP.TOTL              684977
    356  SP.POP.GROW    4.47057515918696
    357  SP.URB.TOTL             7857551
    358  SP.URB.GROW   0.572820545681015
    359  SP.POP.TOTL            10805808
    360  SP.POP.GROW   0.409041838238246
    361  SP.URB.TOTL               38351
    362  SP.URB.GROW    1.26477676537637
    363  SP.POP.TOTL              107432
    364  SP.POP.GROW   0.568483013326884
    365  SP.URB.TOTL               45859
    366  SP.URB.GROW   0.329813441256315
    367  SP.POP.TOTL               56200
    368  SP.POP.GROW   0.178094437099469
    369  SP.URB.TOTL             5253870
    370  SP.URB.GROW    3.18010727540115
    371  SP.POP.TOTL            11589761
    372  SP.POP.GROW    2.43394364463754
    373  SP.URB.TOTL              149181
    374  SP.URB.GROW    1.32186821669843
    375  SP.POP.TOTL              160188
    376  SP.POP.GROW    1.12562122360417
    377  SP.URB.TOTL              217802
    378  SP.URB.GROW   -0.16973493319816
    379  SP.POP.TOTL              759051
    380  SP.POP.GROW   0.136711368532353
    381  SP.URB.TOTL           839091772
    382  SP.URB.GROW   0.899429047689139
    383  SP.POP.TOTL          1101322358
    384  SP.POP.GROW   0.612160081391139
    385  SP.URB.TOTL             6665000
    386  SP.URB.GROW   0.881594075853745
    387  SP.POP.TOTL             6665000
    388  SP.POP.GROW   0.881594075853745
    389  SP.URB.TOTL             3026014
    390  SP.URB.GROW    3.85074216674986
    391  SP.POP.TOTL             6656725
    392  SP.POP.GROW    2.73138300029784
    393  SP.URB.TOTL           136443921
    394  SP.URB.GROW    3.85451058826591
    395  SP.POP.TOTL           475127894
    396  SP.POP.GROW     2.7230069331732
    397  SP.URB.TOTL             2387324
    398  SP.URB.GROW  -0.555021490838623
    399  SP.POP.TOTL             4468302
    400  SP.POP.GROW  -0.986434858645568
    401  SP.URB.TOTL             2976240
    402  SP.URB.GROW     3.5487427661698
    403  SP.POP.TOTL             8360225
    404  SP.POP.GROW    1.82614145032597
    405  SP.URB.TOTL             6593735
    406  SP.URB.GROW  -0.456240196646938
    407  SP.POP.TOTL            10210971
    408  SP.POP.GROW  -0.259764908288633
    409  SP.URB.TOTL          1742482689
    410  SP.URB.GROW    2.48383423347501
    411  SP.POP.TOTL          3999140171
    412  SP.POP.GROW    1.21533207508904
    413  SP.URB.TOTL          2059347840
    414  SP.URB.GROW    2.67425581888314
    415  SP.POP.TOTL          5093906989
    416  SP.POP.GROW    1.49126530696937
    417  SP.URB.TOTL           316865151
    418  SP.URB.GROW    3.73418464477022
    419  SP.POP.TOTL          1094766818
    420  SP.POP.GROW    2.51215321548015
    421  SP.URB.TOTL           125693668
    422  SP.URB.GROW    3.80699666984914
    423  SP.POP.TOTL           370910722
    424  SP.POP.GROW    2.74314664989879
    425  SP.URB.TOTL            89914698
    426  SP.URB.GROW     4.3702128806786
    427  SP.POP.TOTL           214072421
    428  SP.POP.GROW    1.44708848408903
    429  SP.URB.TOTL           191171483
    430  SP.URB.GROW    3.68636697238043
    431  SP.POP.TOTL           723856096
    432  SP.POP.GROW    2.39419199741633
    433  SP.URB.TOTL               39158
    434  SP.URB.GROW    1.20494219286421
    435  SP.POP.TOTL               75562
    436  SP.POP.GROW    1.19018911438933
    437  SP.URB.TOTL           293168849
    438  SP.URB.GROW    2.59867555250261
    439  SP.POP.TOTL          1059633675
    440  SP.POP.GROW    1.82218400218023
    441  SP.URB.TOTL                <NA>
    442  SP.URB.GROW                <NA>
    443  SP.POP.TOTL                <NA>
    444  SP.POP.GROW                <NA>
    445  SP.URB.TOTL             2250951
    446  SP.URB.GROW     1.7481026700676
    447  SP.POP.TOTL             3805174
    448  SP.POP.GROW    1.33304266586712
    449  SP.URB.TOTL            41975934
    450  SP.URB.GROW    2.78233953467196
    451  SP.POP.TOTL            65544383
    452  SP.POP.GROW    1.64539194870845
    453  SP.URB.TOTL            16869783
    454  SP.URB.GROW    3.41511557664929
    455  SP.POP.TOTL            24628858
    456  SP.POP.GROW    3.33624669433987
    457  SP.URB.TOTL              259836
    458  SP.URB.GROW    1.51427858666661
    459  SP.POP.TOTL              281205
    460  SP.POP.GROW    1.36919283329812
    461  SP.URB.TOTL             5735757
    462  SP.URB.GROW    2.71253095110511
    463  SP.POP.TOTL             6289000
    464  SP.POP.GROW    2.64233191305756
    465  SP.URB.TOTL            38277624
    466  SP.URB.GROW   0.134599938879629
    467  SP.POP.TOTL            56942108
    468  SP.POP.GROW   0.045303631138613
    469  SP.URB.TOTL             1353488
    470  SP.URB.GROW    1.07611505949566
    471  SP.POP.TOTL             2612205
    472  SP.POP.GROW   0.611850749006637
    473  SP.URB.TOTL             3957467
    474  SP.URB.GROW    2.12192984544585
    475  SP.POP.TOTL             5056174
    476  SP.POP.GROW    2.10659408273097
    477  SP.URB.TOTL            99760751
    478  SP.URB.GROW   0.327609574744949
    479  SP.POP.TOTL           126843000
    480  SP.POP.GROW   0.167275578113187
    481  SP.URB.TOTL             8349417
    482  SP.URB.GROW  -0.169987079196277
    483  SP.POP.TOTL            14883626
    484  SP.POP.GROW  -0.300201486690526
    485  SP.URB.TOTL             6137001
    486  SP.URB.GROW     4.6496699104578
    487  SP.POP.TOTL            30851606
    488  SP.POP.GROW    2.91544684198394
    489  SP.URB.TOTL             1729037
    490  SP.URB.GROW    1.18545832829557
    491  SP.POP.TOTL             4898400
    492  SP.POP.GROW    1.19112592398474
    493  SP.URB.TOTL             2252408
    494  SP.URB.GROW    2.45675764547543
    495  SP.POP.TOTL            12118841
    496  SP.POP.GROW    1.83064817889597
    497  SP.URB.TOTL               38158
    498  SP.URB.GROW    5.10202289784141
    499  SP.POP.TOTL               88826
    500  SP.POP.GROW    1.74549473198221
    501  SP.URB.TOTL               14901
    502  SP.URB.GROW   0.835640687182618
    503  SP.POP.TOTL               45461
    504  SP.POP.GROW     1.4088796051579
    505  SP.URB.TOTL            37428328
    506  SP.URB.GROW    1.13428440765118
    507  SP.POP.TOTL            47008111
    508  SP.POP.GROW   0.836180864297766
    509  SP.URB.TOTL             1915552
    510  SP.URB.GROW    3.15487927706308
    511  SP.POP.TOTL             1934901
    512  SP.POP.GROW    3.01539399732843
    513  SP.URB.TOTL           348963364
    514  SP.URB.GROW    2.12103007005746
    515  SP.POP.TOTL           468884433
    516  SP.POP.GROW     1.4796413383072
    517  SP.URB.TOTL             1193539
    518  SP.URB.GROW    6.27632532532244
    519  SP.POP.TOTL             5430853
    520  SP.POP.GROW    1.68600720393729
    521  SP.URB.TOTL             3715752
    522  SP.URB.GROW    1.78182968975603
    523  SP.POP.TOTL             4320642
    524  SP.POP.GROW     1.6480313517987
    525  SP.URB.TOTL             1283482
    526  SP.URB.GROW    4.48806596410507
    527  SP.POP.TOTL             2895224
    528  SP.POP.GROW    3.71130059206802
    529  SP.URB.TOTL             3937589
    530  SP.URB.GROW    2.03048728445198
    531  SP.POP.TOTL             5154790
    532  SP.POP.GROW    1.89556266562551
    533  SP.URB.TOTL               44300
    534  SP.URB.GROW   0.076778903123659
    535  SP.POP.TOTL              159500
    536  SP.POP.GROW   0.782994722741428
    537  SP.URB.TOTL           393557252
    538  SP.URB.GROW    2.07993257427606
    539  SP.POP.TOTL           521281149
    540  SP.POP.GROW    1.47294661249624
    541  SP.URB.TOTL           165527209
    542  SP.URB.GROW    3.87123702795616
    543  SP.POP.TOTL           662018171
    544  SP.POP.GROW    2.43963206303424
    545  SP.URB.TOTL           110204801
    546  SP.URB.GROW    3.55150788904555
    547  SP.POP.TOTL           400821583
    548  SP.POP.GROW    2.67574122770131
    549  SP.URB.TOTL                4997
    550  SP.URB.GROW  -0.598565234837027
    551  SP.POP.TOTL               33026
    552  SP.POP.GROW    1.25228247927372
    553  SP.URB.TOTL             3451324
    554  SP.URB.GROW   0.523631902158173
    555  SP.POP.TOTL            18777606
    556  SP.POP.GROW   0.610633602585677
    557  SP.URB.TOTL           831329755
    558  SP.URB.GROW    2.93139596726328
    559  SP.POP.TOTL          2459549814
    560  SP.POP.GROW    1.87795753419972
    561  SP.URB.TOTL          2005521539
    562  SP.URB.GROW     2.7356904534672
    563  SP.POP.TOTL          5018572610
    564  SP.POP.GROW    1.51464413776077
    565  SP.URB.TOTL              390692
    566  SP.URB.GROW    2.86328747333908
    567  SP.POP.TOTL             1998630
    568  SP.POP.GROW   0.219741612882212
    569  SP.URB.TOTL           933800752
    570  SP.URB.GROW    2.44315831076814
    571  SP.POP.TOTL          2046027286
    572  SP.POP.GROW   0.759458776514506
    573  SP.URB.TOTL             2344199
    574  SP.URB.GROW  -0.792940722309791
    575  SP.POP.TOTL             3499536
    576  SP.POP.GROW  -0.703385440488978
    577  SP.URB.TOTL              367434
    578  SP.URB.GROW    2.26003451290749
    579  SP.POP.TOTL              436300
    580  SP.POP.GROW    1.34408299573143
    581  SP.URB.TOTL             1611520
    582  SP.URB.GROW   -1.18848873897458
    583  SP.POP.TOTL             2367550
    584  SP.POP.GROW  -0.963935407092365
    585  SP.URB.TOTL              431896
    586  SP.URB.GROW     1.4877568229253
    587  SP.POP.TOTL              431896
    588  SP.POP.GROW     1.4877568229253
    589  SP.URB.TOTL                <NA>
    590  SP.URB.GROW                <NA>
    591  SP.POP.TOTL               29610
    592  SP.POP.GROW    1.74083327876863
    593  SP.URB.TOTL            15229497
    594  SP.URB.GROW    1.94743958694502
    595  SP.POP.TOTL            28554415
    596  SP.POP.GROW    1.33056292746569
    597  SP.URB.TOTL               32465
    598  SP.URB.GROW   0.218936549624505
    599  SP.POP.TOTL               32465
    600  SP.POP.GROW   0.218936549624527
    601  SP.URB.TOTL             1304080
    602  SP.URB.GROW   -1.10310446057351
    603  SP.POP.TOTL             2924668
    604  SP.POP.GROW  -0.203371722054682
    605  SP.URB.TOTL             4398058
    606  SP.URB.GROW    4.03296351284223
    607  SP.POP.TOTL            16216431
    608  SP.POP.GROW    3.03990100767654
    609  SP.URB.TOTL               78271
    610  SP.URB.GROW    3.65346941142273
    611  SP.POP.TOTL              282507
    612  SP.POP.GROW    1.56830144656694
    613  SP.URB.TOTL           187755039
    614  SP.URB.GROW    2.73654015765081
    615  SP.POP.TOTL           321037453
    616  SP.POP.GROW    2.08773003994465
    617  SP.URB.TOTL            73132993
    618  SP.URB.GROW    1.96131969556176
    619  SP.POP.TOTL            97873442
    620  SP.POP.GROW    1.58455078746177
    621  SP.URB.TOTL               37187
    622  SP.URB.GROW    1.49278742965663
    623  SP.POP.TOTL               54224
    624  SP.POP.GROW   0.721837704584486
    625  SP.URB.TOTL          1895316738
    626  SP.URB.GROW    2.68864932692271
    627  SP.POP.TOTL          4617751027
    628  SP.POP.GROW    1.41509812019633
    629  SP.URB.TOTL             1186387
    630  SP.URB.GROW  0.0990892723551782
    631  SP.POP.TOTL             2026350
    632  SP.POP.GROW   0.455448702114895
    633  SP.URB.TOTL             3186959
    634  SP.URB.GROW    5.44357002629568
    635  SP.POP.TOTL            11239101
    636  SP.POP.GROW    2.90782929642605
    637  SP.URB.TOTL              360316
    638  SP.URB.GROW   0.952299180882739
    639  SP.POP.TOTL              390087
    640  SP.POP.GROW   0.645267230900854
    641  SP.URB.TOTL            12306734
    642  SP.URB.GROW    1.77244215299467
    643  SP.POP.TOTL            45538332
    644  SP.POP.GROW    1.09671263838614
    645  SP.URB.TOTL           156981676
    646  SP.URB.GROW    2.66762541081491
    647  SP.POP.TOTL           283899110
    648  SP.POP.GROW    1.99105888034856
    649  SP.URB.TOTL              354162
    650  SP.URB.GROW    1.59440537638767
    651  SP.POP.TOTL              604950
    652  SP.POP.GROW -0.0279740186992493
    653  SP.URB.TOTL             1400318
    654  SP.URB.GROW    1.81852607522354
    655  SP.POP.TOTL             2450979
    656  SP.POP.GROW   0.921869510816351
    657  SP.URB.TOTL               72426
    658  SP.URB.GROW    5.36607586626903
    659  SP.POP.TOTL               80338
    660  SP.POP.GROW    5.23958252727563
    661  SP.URB.TOTL             5170280
    662  SP.URB.GROW     3.0669112660627
    663  SP.POP.TOTL            17768505
    664  SP.POP.GROW    2.45330550582502
    665  SP.URB.TOTL             1026554
    666  SP.URB.GROW    2.47685978414761
    667  SP.POP.TOTL             2695003
    668  SP.POP.GROW    2.79918208413242
    669  SP.URB.TOTL              506439
    670  SP.URB.GROW   0.694890998843174
    671  SP.POP.TOTL             1186873
    672  SP.POP.GROW   0.982676166064297
    673  SP.URB.TOTL             1640613
    674  SP.URB.GROW    2.90504995399615
    675  SP.POP.TOTL            11229387
    676  SP.POP.GROW    2.30093521781424
    677  SP.URB.TOTL            14220716
    678  SP.URB.GROW    4.55739567987587
    679  SP.POP.TOTL            22945150
    680  SP.POP.GROW    2.54459366705921
    681  SP.URB.TOTL           247519374
    682  SP.URB.GROW    1.51255525280388
    683  SP.POP.TOTL           312909974
    684  SP.POP.GROW     1.1009288191018
    685  SP.URB.TOTL              588911
    686  SP.URB.GROW    3.89762918276995
    687  SP.POP.TOTL             1819141
    688  SP.POP.GROW    2.27194936669291
    689  SP.URB.TOTL              132030
    690  SP.URB.GROW    2.48459985865308
    691  SP.POP.TOTL              213230
    692  SP.POP.GROW    1.90137437804069
    693  SP.URB.TOTL             1881245
    694  SP.URB.GROW    3.93790676397728
    695  SP.POP.TOTL            11622665
    696  SP.POP.GROW    3.42375005583503
    697  SP.URB.TOTL            42801631
    698  SP.URB.GROW    4.15328596481643
    699  SP.POP.TOTL           122851984
    700  SP.POP.GROW    2.60286876961431
    701  SP.URB.TOTL             2827250
    702  SP.URB.GROW    1.71230408284756
    703  SP.POP.TOTL             5123222
    704  SP.POP.GROW    1.44194534225615
    705  SP.URB.TOTL            12229998
    706  SP.URB.GROW    1.71592210438949
    707  SP.POP.TOTL            15925513
    708  SP.POP.GROW   0.714770362784392
    709  SP.URB.TOTL             3414033
    710  SP.URB.GROW    1.24407427476763
    711  SP.POP.TOTL             4490967
    712  SP.POP.GROW   0.649044821192665
    713  SP.URB.TOTL             3290236
    714  SP.URB.GROW    5.82399866046924
    715  SP.POP.TOTL            24559500
    716  SP.POP.GROW    1.70977588565454
    717  SP.URB.TOTL               10377
    718  SP.URB.GROW -0.0578034698175499
    719  SP.POP.TOTL               10377
    720  SP.POP.GROW -0.0578034698175388
    721  SP.URB.TOTL             3318432
    722  SP.URB.GROW   0.680613208426653
    723  SP.POP.TOTL             3857700
    724  SP.POP.GROW   0.587564086381346
    725  SP.URB.TOTL           907363550
    726  SP.URB.GROW    1.04531648004067
    727  SP.POP.TOTL          1200179492
    728  SP.POP.GROW   0.726363968929135
    729  SP.URB.TOTL             1677758
    730  SP.URB.GROW      1.331482999273
    731  SP.POP.TOTL             2344253
    732  SP.POP.GROW    1.35943683310458
    733  SP.URB.TOTL            10743131
    734  SP.URB.GROW     3.0077552589358
    735  SP.POP.TOTL            21437978
    736  SP.POP.GROW    1.88388912656646
    737  SP.URB.TOTL            50914288
    738  SP.URB.GROW    3.68074020517277
    739  SP.POP.TOTL           154369924
    740  SP.POP.GROW    3.07555291166784
    741  SP.URB.TOTL             1867017
    742  SP.URB.GROW    3.18016218103103
    743  SP.POP.TOTL             3001731
    744  SP.POP.GROW    1.97188791278722
    745  SP.URB.TOTL            19468935
    746  SP.URB.GROW    2.08471804520449
    747  SP.POP.TOTL            26654439
    748  SP.POP.GROW    1.52044227070086
    749  SP.URB.TOTL            35966026
    750  SP.URB.GROW    2.03272225924168
    751  SP.POP.TOTL            77958223
    752  SP.POP.GROW    2.21679406368577
    753  SP.URB.TOTL               13875
    754  SP.URB.GROW     1.4226849525191
    755  SP.POP.TOTL               19726
    756  SP.POP.GROW    1.76959560691815
    757  SP.URB.TOTL              727316
    758  SP.URB.GROW    2.16539899567433
    759  SP.POP.TOTL             5508297
    760  SP.POP.GROW     3.4521329401203
    761  SP.URB.TOTL            23611695
    762  SP.URB.GROW  -0.973016273174752
    763  SP.POP.TOTL            38258629
    764  SP.POP.GROW   -1.04433539837844
    765  SP.URB.TOTL           177859062
    766  SP.URB.GROW    4.05197468592706
    767  SP.POP.TOTL           553089230
    768  SP.POP.GROW    2.82034676005263
    769  SP.URB.TOTL             3596716
    770  SP.URB.GROW   0.370913652828193
    771  SP.POP.TOTL             3810605
    772  SP.POP.GROW   0.276558688867414
    773  SP.URB.TOTL            13882837
    774  SP.URB.GROW   0.829486690742089
    775  SP.POP.TOTL            23367059
    776  SP.POP.GROW   0.698115634060685
    777  SP.URB.TOTL             5597602
    778  SP.URB.GROW    1.91610690404258
    779  SP.POP.TOTL            10289898
    780  SP.POP.GROW   0.702859953319024
    781  SP.URB.TOTL             2835060
    782  SP.URB.GROW      3.086699849536
    783  SP.POP.TOTL             5123819
    784  SP.POP.GROW    1.92694524157704
    785  SP.URB.TOTL             2103044
    786  SP.URB.GROW    2.86415437260134
    787  SP.POP.TOTL             2922153
    788  SP.POP.GROW    2.55523569844846
    789  SP.URB.TOTL              701485
    790  SP.URB.GROW    2.20693011315124
    791  SP.POP.TOTL             2035672
    792  SP.POP.GROW    1.46191499849726
    793  SP.URB.TOTL           780442277
    794  SP.URB.GROW   0.785471978025228
    795  SP.POP.TOTL          1020793420
    796  SP.POP.GROW    0.50042197994982
    797  SP.URB.TOTL              140637
    798  SP.URB.GROW    1.50879753546036
    799  SP.POP.TOTL              250927
    800  SP.POP.GROW     1.6498682624854
    801  SP.URB.TOTL              622108
    802  SP.URB.GROW    5.39848881165345
    803  SP.POP.TOTL              645937
    804  SP.POP.GROW    5.18445021336724
    805  SP.URB.TOTL            11895672
    806  SP.URB.GROW  -0.419565624342385
    807  SP.POP.TOTL            22442971
    808  SP.POP.GROW  -0.129440039806255
    809  SP.URB.TOTL           107528803
    810  SP.URB.GROW  -0.426068727524834
    811  SP.POP.TOTL           146596869
    812  SP.POP.GROW  -0.420614990249978
    813  SP.URB.TOTL             1210497
    814  SP.URB.GROW    7.19439451604905
    815  SP.POP.TOTL             8109989
    816  SP.POP.GROW    1.24573125745531
    817  SP.URB.TOTL           385843630
    818  SP.URB.GROW    2.85881008890094
    819  SP.POP.TOTL          1406946728
    820  SP.POP.GROW    1.96243373580489
    821  SP.URB.TOTL            17205160
    822  SP.URB.GROW     2.8182109540427
    823  SP.POP.TOTL            21547390
    824  SP.POP.GROW    2.52723635699494
    825  SP.URB.TOTL             8545786
    826  SP.URB.GROW    2.72286675787171
    827  SP.POP.TOTL            26298773
    828  SP.POP.GROW    2.55963690837227
    829  SP.URB.TOTL             3912769
    830  SP.URB.GROW    2.70878831646509
    831  SP.POP.TOTL             9704287
    832  SP.POP.GROW    2.35349186373024
    833  SP.URB.TOTL             4027887
    834  SP.URB.GROW    1.73204223254257
    835  SP.POP.TOTL             4027887
    836  SP.POP.GROW    1.73204223254257
    837  SP.URB.TOTL               67992
    838  SP.URB.GROW    4.55567808839687
    839  SP.POP.TOTL              429978
    840  SP.POP.GROW    2.53167345846883
    841  SP.URB.TOTL             1633120
    842  SP.URB.GROW    3.08641814084455
    843  SP.POP.TOTL             4584067
    844  SP.POP.GROW    2.40478419249569
    845  SP.URB.TOTL             3510261
    846  SP.URB.GROW    1.52944395884384
    847  SP.POP.TOTL             5958482
    848  SP.POP.GROW   0.582883767568532
    849  SP.URB.TOTL               25063
    850  SP.URB.GROW    1.92154797473259
    851  SP.POP.TOTL               26823
    852  SP.POP.GROW    1.57442145947664
    853  SP.URB.TOTL             2899625
    854  SP.URB.GROW    5.05656066455098
    855  SP.POP.TOTL             8721465
    856  SP.POP.GROW    3.94049698165867
    857  SP.URB.TOTL             3966301
    858  SP.URB.GROW   0.105872502528175
    859  SP.POP.TOTL             7516346
    860  SP.POP.GROW  -0.319524801286905
    861  SP.URB.TOTL           210783626
    862  SP.URB.GROW    3.84742064131463
    863  SP.POP.TOTL           671131355
    864  SP.POP.GROW    2.65041731976174
    865  SP.URB.TOTL             1009127
    866  SP.URB.GROW     5.1972225048576
    867  SP.POP.TOTL             6114440
    868  SP.POP.GROW     4.4186739597559
    869  SP.URB.TOTL           210824543
    870  SP.URB.GROW    3.84691171077253
    871  SP.POP.TOTL           671212486
    872  SP.POP.GROW    2.65020165426746
    873  SP.URB.TOTL            14761838
    874  SP.URB.GROW    2.50959605762182
    875  SP.POP.TOTL            30055791
    876  SP.POP.GROW    1.59709265877478
    877  SP.URB.TOTL               76778
    878  SP.URB.GROW    3.14353175850983
    879  SP.POP.TOTL              143714
    880  SP.POP.GROW    1.33511835347824
    881  SP.URB.TOTL              318265
    882  SP.URB.GROW    1.90866252229937
    883  SP.POP.TOTL              478998
    884  SP.POP.GROW    1.79897337334452
    885  SP.URB.TOTL             3030239
    886  SP.URB.GROW   -0.24377578251167
    887  SP.POP.TOTL             5388720
    888  SP.POP.GROW  -0.135376487794425
    889  SP.URB.TOTL             1009459
    890  SP.URB.GROW   0.347322840072504
    891  SP.POP.TOTL             1988925
    892  SP.POP.GROW    0.29607496005045
    893  SP.URB.TOTL             7454878
    894  SP.URB.GROW   0.193899954777834
    895  SP.POP.TOTL             8872109
    896  SP.POP.GROW   0.160575484575313
    897  SP.URB.TOTL              233778
    898  SP.URB.GROW   0.607975030947652
    899  SP.POP.TOTL             1030496
    900  SP.POP.GROW    1.18369298761011
    901  SP.URB.TOTL               30519
    902  SP.URB.GROW   -1.83437768672748
    903  SP.POP.TOTL               30519
    904  SP.POP.GROW   -1.83437768672749
    905  SP.URB.TOTL               40917
    906  SP.URB.GROW    1.28148544485373
    907  SP.POP.TOTL               81131
    908  SP.POP.GROW 0.89265856676617406
    909  SP.URB.TOTL             8471337
    910  SP.URB.GROW    3.23687225720868
    911  SP.POP.TOTL            16307654
    912  SP.POP.GROW    2.52399271863257
    913  SP.URB.TOTL               15848
    914  SP.URB.GROW     5.1466002517883
    915  SP.POP.TOTL               18744
    916  SP.POP.GROW     4.1391227494214
    917  SP.URB.TOTL             1787029
    918  SP.URB.GROW    3.56712410956491
    919  SP.POP.TOTL             8259137
    920  SP.POP.GROW    3.41450024149477
    921  SP.URB.TOTL           650945325
    922  SP.URB.GROW    3.65378531016158
    923  SP.POP.TOTL          1793649873
    924  SP.POP.GROW    1.00406124764849
    925  SP.URB.TOTL           275974565
    926  SP.URB.GROW  0.0683359303701394
    927  SP.POP.TOTL           435816101
    928  SP.POP.GROW -0.0860993080108301
    929  SP.URB.TOTL             1647994
    930  SP.URB.GROW     4.2049137134671
    931  SP.POP.TOTL             5008035
    932  SP.POP.GROW     2.8372567506566
    933  SP.URB.TOTL            19794084
    934  SP.URB.GROW     2.3318014212256
    935  SP.POP.TOTL            63066603
    936  SP.POP.GROW   0.994280693095973
    937  SP.URB.TOTL             1662407
    938  SP.URB.GROW   0.366344815044205
    939  SP.POP.TOTL             6272998
    940  SP.POP.GROW    1.33895806972214
    941  SP.URB.TOTL             2097826
    942  SP.URB.GROW     1.9897034243335
    943  SP.POP.TOTL             4569132
    944  SP.POP.GROW    1.50061066998589
    945  SP.URB.TOTL           380881145
    946  SP.URB.GROW    2.13185638871187
    947  SP.POP.TOTL           505304844
    948  SP.POP.GROW    1.50987572037849
    949  SP.URB.TOTL              213116
    950  SP.URB.GROW    2.80748909258887
    951  SP.POP.TOTL              878360
    952  SP.POP.GROW    1.34224818594807
    953  SP.URB.TOTL           154878632
    954  SP.URB.GROW    2.66440207287941
    955  SP.POP.TOTL           280976957
    956  SP.POP.GROW    1.98488554415559
    957  SP.URB.TOTL               23611
    958  SP.URB.GROW   0.726874042601457
    959  SP.POP.TOTL              102603
    960  SP.POP.GROW   0.607084495200795
    961  SP.URB.TOTL           385843630
    962  SP.URB.GROW    2.85881008890094
    963  SP.POP.TOTL          1406946728
    964  SP.POP.GROW    1.96243373580489
    965  SP.URB.TOTL           210824543
    966  SP.URB.GROW    3.84691171077253
    967  SP.POP.TOTL           671212486
    968  SP.POP.GROW    2.65020165426746
    969  SP.URB.TOTL              744768
    970  SP.URB.GROW   0.626582981569608
    971  SP.POP.TOTL             1332203
    972  SP.POP.GROW   0.386573317672307
    973  SP.URB.TOTL             6275528
    974  SP.URB.GROW    1.68467997407744
    975  SP.POP.TOTL             9893316
    976  SP.POP.GROW    1.06953869345051
    977  SP.URB.TOTL            41507751
    978  SP.URB.GROW    2.26122886011657
    979  SP.POP.TOTL            64113547
    980  SP.POP.GROW    1.45790187647314
    981  SP.URB.TOTL                4435
    982  SP.URB.GROW   0.860512556286972
    983  SP.POP.TOTL                9638
    984  SP.POP.GROW -0.0207490404313245
    985  SP.URB.TOTL             7688508
    986  SP.URB.GROW    4.47278383914827
    987  SP.POP.TOTL            34463704
    988  SP.POP.GROW    2.83680794271469
    989  SP.URB.TOTL             3551700
    990  SP.URB.GROW    5.92654401983794
    991  SP.POP.TOTL            24020697
    992  SP.POP.GROW    3.13535567369772
    993  SP.URB.TOTL            33019561
    994  SP.URB.GROW  -0.948477352408885
    995  SP.POP.TOTL            49176500
    996  SP.POP.GROW   -1.00657902702927
    997  SP.URB.TOTL          1063986983
    998  SP.URB.GROW    2.49977831743374
    999  SP.POP.TOTL          2158201213
    1000 SP.POP.GROW    0.89271112761358
    1001 SP.URB.TOTL             3029768
    1002 SP.URB.GROW   0.713783964461946
    1003 SP.POP.TOTL             3292224
    1004 SP.POP.GROW   0.403611037153845
    1005 SP.URB.TOTL           223069137
    1006 SP.URB.GROW    1.51201139138713
    1007 SP.POP.TOTL           282162411
    1008 SP.POP.GROW    1.11276899679534
    1009 SP.URB.TOTL            11370244
    1010 SP.URB.GROW    2.43420878873601
    1011 SP.POP.TOTL            24650400
    1012 SP.POP.GROW    1.38374682096563
    1013 SP.URB.TOTL               51428
    1014 SP.URB.GROW   0.647655496271435
    1015 SP.POP.TOTL              113813
    1016 SP.POP.GROW  -0.159783711477875
    1017 SP.URB.TOTL            21388675
    1018 SP.URB.GROW    2.24404422397847
    1019 SP.POP.TOTL            24427729
    1020 SP.POP.GROW     1.9042706267462
    1021 SP.URB.TOTL                8398
    1022 SP.URB.GROW    3.26786914258755
    1023 SP.POP.TOTL               20104
    1024 SP.POP.GROW    2.61037749426884
    1025 SP.URB.TOTL              100587
    1026 SP.URB.GROW   0.426410138693815
    1027 SP.POP.TOTL              108642
    1028 SP.POP.GROW  0.0395873712251061
    1029 SP.URB.TOTL            19255738
    1030 SP.URB.GROW    3.42860197272881
    1031 SP.POP.TOTL            79001142
    1032 SP.POP.GROW    1.11686737378671
    1033 SP.URB.TOTL               41628
    1034 SP.URB.GROW    3.79704140998283
    1035 SP.POP.TOTL              192074
    1036 SP.POP.GROW    2.44646014761845
    1037 SP.URB.TOTL          2866001986
    1038 SP.URB.GROW    2.18773892428798
    1039 SP.POP.TOTL          6144322697
    1040 SP.POP.GROW    1.35330175380903
    1041 SP.URB.TOTL               40439
    1042 SP.URB.GROW      1.394479630087
    1043 SP.POP.TOTL              184008
    1044 SP.POP.GROW   0.981387870498847
    1045 SP.URB.TOTL                <NA>
    1046 SP.URB.GROW                <NA>
    1047 SP.POP.TOTL             1700000
    1048 SP.POP.GROW   -3.58212764518173
    1049 SP.URB.TOTL             4893201
    1050 SP.URB.GROW    4.77890817937106
    1051 SP.POP.TOTL            18628700
    1052 SP.POP.GROW    2.79878090986914
    1053 SP.URB.TOTL            26632535
    1054 SP.URB.GROW    1.81016232925667
    1055 SP.POP.TOTL            46813266
    1056 SP.POP.GROW   0.962864025570883
    1057 SP.URB.TOTL             3442313
    1058 SP.URB.GROW    1.46484423776558
    1059 SP.POP.TOTL             9891136
    1060 SP.POP.GROW    2.76660559103692
    1061 SP.URB.TOTL             3995150
    1062 SP.URB.GROW    2.22892978426192
    1063 SP.POP.TOTL            11834676
    1064 SP.POP.GROW     1.0039687523875

Second way.

``` r
results <- dbSendQuery(con, "select indicator, x2000 from my_table_name")
dbFetch(results, n=100) 
```

          indicator              x2000
    1   SP.URB.TOTL              41625
    2   SP.URB.GROW   1.66422212392663
    3   SP.POP.TOTL              89101
    4   SP.POP.GROW   2.53923444445246
    5   SP.URB.TOTL          115551653
    6   SP.URB.GROW   3.60226233978761
    7   SP.POP.TOTL          401600588
    8   SP.POP.GROW    2.5835792421522
    9   SP.URB.TOTL            4314700
    10  SP.URB.GROW   1.86137729962901
    11  SP.POP.TOTL           19542982
    12  SP.POP.GROW    1.4438030241194
    13  SP.URB.TOTL           95272890
    14  SP.URB.GROW   4.14518949083499
    15  SP.POP.TOTL          269611898
    16  SP.POP.GROW   2.74959971917366
    17  SP.URB.TOTL            8211294
    18  SP.URB.GROW   5.64866955326432
    19  SP.POP.TOTL           16394062
    20  SP.POP.GROW   3.24412146672851
    21  SP.URB.TOTL            1289391
    22  SP.URB.GROW  0.742478629285177
    23  SP.POP.TOTL            3089027
    24  SP.POP.GROW -0.637356833943492
    25  SP.URB.TOTL              61070
    26  SP.URB.GROW  0.377327984395132
    27  SP.POP.TOTL              66097
    28  SP.POP.GROW  0.670960073758389
    29  SP.URB.TOTL          152305719
    30  SP.URB.GROW   2.76137514575139
    31  SP.POP.TOTL          287065982
    32  SP.POP.GROW   2.28593431739384
    33  SP.URB.TOTL            2627996
    34  SP.URB.GROW   6.11272936104507
    35  SP.POP.TOTL            3275333
    36  SP.POP.GROW   5.58038700101525
    37  SP.URB.TOTL           33045629
    38  SP.URB.GROW   1.34664689815167
    39  SP.POP.TOTL           37070774
    40  SP.POP.GROW   1.13327702210541
    41  SP.URB.TOTL            2048957
    42  SP.URB.GROW  -1.61037490478258
    43  SP.POP.TOTL            3168523
    44  SP.POP.GROW  -1.17678628879226
    45  SP.URB.TOTL              51584
    46  SP.URB.GROW    1.6142305917451
    47  SP.POP.TOTL              58230
    48  SP.POP.GROW   1.09822902395737
    49  SP.URB.TOTL              24113
    50  SP.URB.GROW   0.53641727893261
    51  SP.POP.TOTL              75055
    52  SP.POP.GROW   1.65779341376666
    53  SP.URB.TOTL           16028911
    54  SP.URB.GROW  0.984333804176905
    55  SP.POP.TOTL           19028802
    56  SP.POP.GROW   1.14447285147721
    57  SP.URB.TOTL            4824004
    58  SP.URB.GROW -0.220175991539749
    59  SP.POP.TOTL            8011566
    60  SP.POP.GROW  0.240466652446524
    61  SP.URB.TOTL            4135854
    62  SP.URB.GROW   1.21344378120309
    63  SP.POP.TOTL            8048600
    64  SP.POP.GROW  0.821519963674211
    65  SP.URB.TOTL             520130
    66  SP.URB.GROW   4.62153763088361
    67  SP.POP.TOTL            6307659
    68  SP.POP.GROW    2.0417212041938
    69  SP.URB.TOTL            9956937
    70  SP.URB.GROW  0.308431312307231
    71  SP.POP.TOTL           10251250
    72  SP.POP.GROW  0.242517956221334
    73  SP.URB.TOTL            2682552
    74  SP.URB.GROW   3.87148751501267
    75  SP.POP.TOTL            6998023
    76  SP.POP.GROW   3.03845662138601
    77  SP.URB.TOTL            2120383
    78  SP.URB.GROW   6.85756484832227
    79  SP.POP.TOTL           11882888
    80  SP.POP.GROW   2.98388558736392
    81  SP.URB.TOTL           30476706
    82  SP.URB.GROW   3.56396663214103
    83  SP.POP.TOTL          129193327
    84  SP.POP.GROW   1.90552404891968
    85  SP.URB.TOTL            5629167
    86  SP.URB.GROW -0.171157650642926
    87  SP.POP.TOTL            8170172
    88  SP.POP.GROW -0.493896416633318
    89  SP.URB.TOTL             628716
    90  SP.URB.GROW   2.74720093000752
    91  SP.POP.TOTL             711442
    92  SP.POP.GROW    2.7515762605179
    93  SP.URB.TOTL             266534
    94  SP.URB.GROW   1.69321459564921
    95  SP.POP.TOTL             325014
    96  SP.POP.GROW   1.46976235766802
    97  SP.URB.TOTL            1771376
    98  SP.URB.GROW   1.37816316655527
    99  SP.POP.TOTL            4179350
    100 SP.POP.GROW  0.632139635256837

But wait. There’s more. Entering the chat now is
[dbplyr](https://dbplyr.tidyverse.org), which uses dplyr verbs to
interact with a database. Sweet.

``` r
library(dbplyr)
```


    Attaching package: 'dbplyr'

    The following objects are masked from 'package:dplyr':

        ident, sql

Now say you want to interact with a particular table.

First create a connection to that table like so:

``` r
my_dbplyr_connection <- tbl(con, "my_table_name")
```

    Warning in result_create(conn@ptr, statement, immediate): Closing open result
    set, cancelling previous query

``` r
my_dbplyr_connection
```

    # Source:   table<my_table_name> [?? x 20]
    # Database: postgres  [rstudio@localhost:5432/test2]
       file      indic…¹ x2000 x2001 x2002 x2003 x2004 x2005 x2006 x2007 x2008 x2009
       <chr>     <chr>   <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr>
     1 ./data/i… SP.URB… 41625 42025 42194 42277 42317 42399 42555 42729 42906 43079
     2 ./data/i… SP.URB… 1.66… 0.95… 0.40… 0.19… 0.09… 0.19… 0.36… 0.40… 0.41… 0.40…
     3 ./data/i… SP.POP… 89101 90691 91781 92701 93540 94483 95606 96787 97996 99212
     4 ./data/i… SP.POP… 2.53… 1.76… 1.19… 0.99… 0.90… 1.00… 1.18… 1.22… 1.24… 1.23…
     5 ./data/i… SP.URB… 1155… 1197… 1242… 1288… 1336… 1387… 1440… 1492… 1553… 1617…
     6 ./data/i… SP.URB… 3.60… 3.65… 3.71… 3.70… 3.73… 3.81… 3.80… 3.61… 4.12… 4.11…
     7 ./data/i… SP.POP… 4016… 4120… 4227… 4338… 4452… 4571… 4695… 4824… 4957… 5094…
     8 ./data/i… SP.POP… 2.58… 2.58… 2.60… 2.61… 2.64… 2.66… 2.70… 2.74… 2.76… 2.75…
     9 ./data/i… SP.URB… 4314… 4364… 4674… 5061… 5299… 5542… 5828… 5987… 6162… 6443…
    10 ./data/i… SP.URB… 1.86… 1.15… 6.86… 7.95… 4.58… 4.47… 5.03… 2.68… 2.89… 4.44…
    # … with more rows, 8 more variables: x2010 <chr>, x2011 <chr>, x2012 <chr>,
    #   x2013 <chr>, x2014 <chr>, x2015 <chr>, x2016 <chr>, x2017 <chr>, and
    #   abbreviated variable name ¹​indicator

To me, one of the real time-saving features of dbplyr is how it makes
select statements much easier.

Say you just want to extract the total population for each year— you can
use
[tidyselectors](https://tidyselect.r-lib.org/reference/language.html)
that make it easier than writing a traditional select statement in SQL.

``` r
my_dbplyr_connection %>%
  filter(indicator=="SP.POP.TOTL") %>%
  select(file, starts_with("x2")) # saves me writing out all those field names in a select statement
```

    # Source:   SQL [?? x 19]
    # Database: postgres  [rstudio@localhost:5432/test2]
       file  x2000 x2001 x2002 x2003 x2004 x2005 x2006 x2007 x2008 x2009 x2010 x2011
       <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr>
     1 ./da… 89101 90691 91781 92701 93540 94483 95606 96787 97996 99212 1003… 1012…
     2 ./da… 4016… 4120… 4227… 4338… 4452… 4571… 4695… 4824… 4957… 5094… 5234… 5377…
     3 ./da… 1954… 1968… 2100… 2264… 2355… 2441… 2544… 2590… 2642… 2738… 2818… 2924…
     4 ./da… 2696… 2771… 2849… 2929… 3012… 3098… 3186… 3276… 3368… 3464… 3563… 3664…
     5 ./da… 1639… 1694… 1751… 1812… 1877… 1945… 2016… 2090… 2169… 2250… 2336… 2425…
     6 ./da… 3089… 3060… 3051… 3039… 3026… 3011… 2992… 2970… 2947… 2927… 2913… 2905…
     7 ./da… 66097 67820 70849 73907 76933 79826 80221 78168 76055 73852 71519 70567
     8 ./da… 2870… 2935… 3000… 3065… 3132… 3205… 3287… 3374… 3466… 3557… 3644… 3723…
     9 ./da… 3275… 3454… 3633… 3813… 3993… 4280… 4898… 5872… 6988… 7992… 8481… 8575…
    10 ./da… 3707… 3748… 3788… 3827… 3866… 3907… 3947… 3987… 4027… 4068… 4078… 4126…
    # … with more rows, and 6 more variables: x2012 <chr>, x2013 <chr>,
    #   x2014 <chr>, x2015 <chr>, x2016 <chr>, x2017 <chr>

# Wrangling data

OK. One thing we need to add is extract the ISO3 code of the country
buried in the filename. We can extract those from the file name using
[str_extract](https://stringr.tidyverse.org/reference/str_extract.html)
and [regular
expressions](https://cran.r-project.org/web/packages/stringr/vignettes/regular-expressions.html).

``` r
my_pattern <- "./data/importing/(\\D\\D\\D).csv"

imported_data_collection_fixed_2 <- imported_data_collection_fixed %>%
  filter(indicator=="SP.POP.TOTL") %>%
  mutate(country_code = str_extract(file, my_pattern, group=1)) %>%
  select(file, country_code, starts_with("x2"))

imported_data_collection_fixed_2
```

    # A tibble: 266 × 20
       file      count…¹ x2000 x2001 x2002 x2003 x2004 x2005 x2006 x2007 x2008 x2009
       <chr>     <chr>   <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr> <chr>
     1 ./data/i… ABW     89101 90691 91781 92701 93540 94483 95606 96787 97996 99212
     2 ./data/i… AFE     4016… 4120… 4227… 4338… 4452… 4571… 4695… 4824… 4957… 5094…
     3 ./data/i… AFG     1954… 1968… 2100… 2264… 2355… 2441… 2544… 2590… 2642… 2738…
     4 ./data/i… AFW     2696… 2771… 2849… 2929… 3012… 3098… 3186… 3276… 3368… 3464…
     5 ./data/i… AGO     1639… 1694… 1751… 1812… 1877… 1945… 2016… 2090… 2169… 2250…
     6 ./data/i… ALB     3089… 3060… 3051… 3039… 3026… 3011… 2992… 2970… 2947… 2927…
     7 ./data/i… AND     66097 67820 70849 73907 76933 79826 80221 78168 76055 73852
     8 ./data/i… ARB     2870… 2935… 3000… 3065… 3132… 3205… 3287… 3374… 3466… 3557…
     9 ./data/i… ARE     3275… 3454… 3633… 3813… 3993… 4280… 4898… 5872… 6988… 7992…
    10 ./data/i… ARG     3707… 3748… 3788… 3827… 3866… 3907… 3947… 3987… 4027… 4068…
    # … with 256 more rows, 8 more variables: x2010 <chr>, x2011 <chr>,
    #   x2012 <chr>, x2013 <chr>, x2014 <chr>, x2015 <chr>, x2016 <chr>,
    #   x2017 <chr>, and abbreviated variable name ¹​country_code

Cool huh?

# Pivot Data

OK. So remember how we imported all that data as character? We need to
convert all those pop fields to integers.

We could write a mutate statement to do each of the years individually.
Or we could pivot the data, making a wide dataset into a long one.

``` r
imported_data_collection_fixed_2 %>%
  pivot_longer(cols=starts_with("x"), names_to = "the_year", values_to = "pop_tot")
```

    # A tibble: 4,788 × 4
       file                     country_code the_year pop_tot
       <chr>                    <chr>        <chr>    <chr>  
     1 ./data/importing/ABW.csv ABW          x2000    89101  
     2 ./data/importing/ABW.csv ABW          x2001    90691  
     3 ./data/importing/ABW.csv ABW          x2002    91781  
     4 ./data/importing/ABW.csv ABW          x2003    92701  
     5 ./data/importing/ABW.csv ABW          x2004    93540  
     6 ./data/importing/ABW.csv ABW          x2005    94483  
     7 ./data/importing/ABW.csv ABW          x2006    95606  
     8 ./data/importing/ABW.csv ABW          x2007    96787  
     9 ./data/importing/ABW.csv ABW          x2008    97996  
    10 ./data/importing/ABW.csv ABW          x2009    99212  
    # … with 4,778 more rows

So now we can just convert that one field to numeric, then pivot it
back.

``` r
imported_data_collection_fixed_3 <- imported_data_collection_fixed_2 %>%
  pivot_longer(cols=starts_with("x"), names_to = "the_year", values_to = "pop_tot") %>%
  mutate(pop_tot_num = as.numeric(pop_tot)) %>%
  select(!pop_tot) %>% # drop the old character field
  pivot_wider(names_from = the_year, values_from = pop_tot_num)

imported_data_collection_fixed_3
```

    # A tibble: 266 × 20
       file   count…¹  x2000  x2001  x2002  x2003  x2004  x2005  x2006  x2007  x2008
       <chr>  <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
     1 ./dat… ABW     8.91e4 9.07e4 9.18e4 9.27e4 9.35e4 9.45e4 9.56e4 9.68e4 9.80e4
     2 ./dat… AFE     4.02e8 4.12e8 4.23e8 4.34e8 4.45e8 4.57e8 4.70e8 4.82e8 4.96e8
     3 ./dat… AFG     1.95e7 1.97e7 2.10e7 2.26e7 2.36e7 2.44e7 2.54e7 2.59e7 2.64e7
     4 ./dat… AFW     2.70e8 2.77e8 2.85e8 2.93e8 3.01e8 3.10e8 3.19e8 3.28e8 3.37e8
     5 ./dat… AGO     1.64e7 1.69e7 1.75e7 1.81e7 1.88e7 1.95e7 2.02e7 2.09e7 2.17e7
     6 ./dat… ALB     3.09e6 3.06e6 3.05e6 3.04e6 3.03e6 3.01e6 2.99e6 2.97e6 2.95e6
     7 ./dat… AND     6.61e4 6.78e4 7.08e4 7.39e4 7.69e4 7.98e4 8.02e4 7.82e4 7.61e4
     8 ./dat… ARB     2.87e8 2.94e8 3.00e8 3.07e8 3.13e8 3.21e8 3.29e8 3.37e8 3.47e8
     9 ./dat… ARE     3.28e6 3.45e6 3.63e6 3.81e6 3.99e6 4.28e6 4.90e6 5.87e6 6.99e6
    10 ./dat… ARG     3.71e7 3.75e7 3.79e7 3.83e7 3.87e7 3.91e7 3.95e7 3.99e7 4.03e7
    # … with 256 more rows, 9 more variables: x2009 <dbl>, x2010 <dbl>,
    #   x2011 <dbl>, x2012 <dbl>, x2013 <dbl>, x2014 <dbl>, x2015 <dbl>,
    #   x2016 <dbl>, x2017 <dbl>, and abbreviated variable name ¹​country_code

Voila!

# Easy charting

So let’s take our last dataset and do some charting. How about we chart
China and India’s population?

We’re going to filter for those two countries, then pivot longer because
it’s a time series, then we’re going to turn our year field name into an
integer.

``` r
ready_to_chart <- imported_data_collection_fixed_3 %>%
  filter(country_code %in% c("IND", "CHN")) %>%
  pivot_longer(cols = starts_with("x"), names_to = "the_year", values_to = "pop") %>%
  mutate(the_year = as.integer(str_remove(the_year, "x")))

ready_to_chart
```

    # A tibble: 36 × 4
       file                     country_code the_year        pop
       <chr>                    <chr>           <int>      <dbl>
     1 ./data/importing/CHN.csv CHN              2000 1262645000
     2 ./data/importing/CHN.csv CHN              2001 1271850000
     3 ./data/importing/CHN.csv CHN              2002 1280400000
     4 ./data/importing/CHN.csv CHN              2003 1288400000
     5 ./data/importing/CHN.csv CHN              2004 1296075000
     6 ./data/importing/CHN.csv CHN              2005 1303720000
     7 ./data/importing/CHN.csv CHN              2006 1311020000
     8 ./data/importing/CHN.csv CHN              2007 1317885000
     9 ./data/importing/CHN.csv CHN              2008 1324655000
    10 ./data/importing/CHN.csv CHN              2009 1331260000
    # … with 26 more rows

``` r
p1 <- ggplot(data=ready_to_chart, aes(x=the_year, y=pop, group=country_code, color=country_code)) + 
  geom_line() +
  geom_point()

p1
```

![](ten_things_files/figure-commonmark/unnamed-chunk-29-1.png)

# Do GIS stuff

OK. So we’ve all traditionally worked with desktop applications, such as
QGIS or ArcGIS.

We can do all of that stuff in R.

For vectors, we can use SF. For rasters, we can use stars.

``` r
#install.packages("stars")
#install.packages("sf")
```

``` r
library(stars)
```

    Loading required package: abind

    Loading required package: sf

    Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE

``` r
library(sf)
```

Let’s say we have a raster of 2020 population density in Switzerland. We
can read it into a stars object like so.

``` r
swiss_raster <- read_stars("./data/che_pd_2020_1km.tif")
```

Let’s take a look.

``` r
ggplot() + 
  geom_stars(data=swiss_raster)
```

![](ten_things_files/figure-commonmark/unnamed-chunk-33-1.png)

Let’s say we wanted to see just the area around Zurich.

One simple way is to use
[tribble](https://tibble.tidyverse.org/reference/tribble.html), which
makes it easy to create an SF point on the fly. Let’s create a dot for
Zurich.

``` r
zurich_dot <- tribble(
  ~site, ~x, ~y,
  "Zurich", 8.541111, 47.374444
  ) %>%
  st_as_sf(coords = c("x", "y"), crs = 4326)
```

Let’s make sure the dot is where the dot is supposed to be — in Zurich.

``` r
ggplot() + 
  geom_stars(data=swiss_raster) +
  geom_sf(data=zurich_dot, color="red")
```

![](ten_things_files/figure-commonmark/unnamed-chunk-35-1.png)

Now we’ve got a dot. Let’s buffer that dot.

``` r
zurich_25k_buffer <- zurich_dot %>%
  st_buffer(25000) # base unit in meters, so 25,000m = 25km
```

Let’s see how it looks.

``` r
ggplot() + 
  geom_stars(data=swiss_raster) +
  geom_sf(data=zurich_dot, color="red") +
  geom_sf(data=zurich_25k_buffer, fill=NA, color="red")
```

![](ten_things_files/figure-commonmark/unnamed-chunk-37-1.png)

Now let’s crop the raster by our buffer.

``` r
swiss_raster_cropped <- swiss_raster %>%
  st_crop(zurich_25k_buffer) 
```

How does it look?

``` r
ggplot() + 
  geom_stars(data=swiss_raster_cropped)
```

![](ten_things_files/figure-commonmark/unnamed-chunk-39-1.png)

I won’t go too far down the rabbit hole, but you can use [tidyverse
methods](https://r-spatial.github.io/stars/articles/stars3.html) with
stars objects.

Here we rename the attribute to pop_density_20, then we create a new
band with a yes or no whether it’s high density. Then we select just our
newly created high_density band.

``` r
new_band_example <- swiss_raster_cropped %>%
  rename(pop_density_20 = 1) %>%
  mutate(high_density = if_else(pop_density_20 > 3000, "Yes", "No")) %>%
  select(high_density) 
```

How does it look?

``` r
ggplot() + 
  geom_stars(data=new_band_example)
```

![](ten_things_files/figure-commonmark/unnamed-chunk-41-1.png)

What if you’re like me and prefer to work in vectors?

Let’s return to our swiss_raster_cropped stars object.

It takes just one line of code to turn this raster into an SF vector
object.

``` r
swiss_raster_cropped_sf <- swiss_raster_cropped %>%
  st_as_sf()
```

Now let’s take a look.

``` r
ggplot() + 
  geom_sf(data=swiss_raster_cropped_sf)
```

![](ten_things_files/figure-commonmark/unnamed-chunk-43-1.png)

What about a spatial filter?

Let’s create another smaller buffer as an example.

``` r
zurich_10k_buffer <- zurich_dot %>%
  st_buffer(10000) 
```

Now we can use st_filter.

``` r
smaller_zurich_area <- swiss_raster_cropped_sf %>%
  st_filter(zurich_10k_buffer)
```

How does it look?

``` r
ggplot() + 
  geom_sf(data=swiss_raster_cropped_sf) +
  geom_sf(data=smaller_zurich_area, color="red")
```

![](ten_things_files/figure-commonmark/unnamed-chunk-46-1.png)
