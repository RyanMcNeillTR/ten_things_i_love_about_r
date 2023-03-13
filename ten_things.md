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

showing_off 
```

                             file   indicator               x2000
    1    ./data/importing/ABW.csv SP.URB.TOTL               41625
    2    ./data/importing/ABW.csv SP.URB.GROW    1.66422212392663
    3    ./data/importing/ABW.csv SP.POP.TOTL               89101
    4    ./data/importing/ABW.csv SP.POP.GROW    2.53923444445246
    5    ./data/importing/AFE.csv SP.URB.TOTL           115551653
    6    ./data/importing/AFE.csv SP.URB.GROW    3.60226233978761
    7    ./data/importing/AFE.csv SP.POP.TOTL           401600588
    8    ./data/importing/AFE.csv SP.POP.GROW     2.5835792421522
    9    ./data/importing/AFG.csv SP.URB.TOTL             4314700
    10   ./data/importing/AFG.csv SP.URB.GROW    1.86137729962901
    11   ./data/importing/AFG.csv SP.POP.TOTL            19542982
    12   ./data/importing/AFG.csv SP.POP.GROW     1.4438030241194
    13   ./data/importing/AFW.csv SP.URB.TOTL            95272890
    14   ./data/importing/AFW.csv SP.URB.GROW    4.14518949083499
    15   ./data/importing/AFW.csv SP.POP.TOTL           269611898
    16   ./data/importing/AFW.csv SP.POP.GROW    2.74959971917366
    17   ./data/importing/AGO.csv SP.URB.TOTL             8211294
    18   ./data/importing/AGO.csv SP.URB.GROW    5.64866955326432
    19   ./data/importing/AGO.csv SP.POP.TOTL            16394062
    20   ./data/importing/AGO.csv SP.POP.GROW    3.24412146672851
    21   ./data/importing/ALB.csv SP.URB.TOTL             1289391
    22   ./data/importing/ALB.csv SP.URB.GROW   0.742478629285177
    23   ./data/importing/ALB.csv SP.POP.TOTL             3089027
    24   ./data/importing/ALB.csv SP.POP.GROW  -0.637356833943492
    25   ./data/importing/AND.csv SP.URB.TOTL               61070
    26   ./data/importing/AND.csv SP.URB.GROW   0.377327984395132
    27   ./data/importing/AND.csv SP.POP.TOTL               66097
    28   ./data/importing/AND.csv SP.POP.GROW   0.670960073758389
    29   ./data/importing/ARB.csv SP.URB.TOTL           152305719
    30   ./data/importing/ARB.csv SP.URB.GROW    2.76137514575139
    31   ./data/importing/ARB.csv SP.POP.TOTL           287065982
    32   ./data/importing/ARB.csv SP.POP.GROW    2.28593431739384
    33   ./data/importing/ARE.csv SP.URB.TOTL             2627996
    34   ./data/importing/ARE.csv SP.URB.GROW    6.11272936104507
    35   ./data/importing/ARE.csv SP.POP.TOTL             3275333
    36   ./data/importing/ARE.csv SP.POP.GROW    5.58038700101525
    37   ./data/importing/ARG.csv SP.URB.TOTL            33045629
    38   ./data/importing/ARG.csv SP.URB.GROW    1.34664689815167
    39   ./data/importing/ARG.csv SP.POP.TOTL            37070774
    40   ./data/importing/ARG.csv SP.POP.GROW    1.13327702210541
    41   ./data/importing/ARM.csv SP.URB.TOTL             2048957
    42   ./data/importing/ARM.csv SP.URB.GROW   -1.61037490478258
    43   ./data/importing/ARM.csv SP.POP.TOTL             3168523
    44   ./data/importing/ARM.csv SP.POP.GROW   -1.17678628879226
    45   ./data/importing/ASM.csv SP.URB.TOTL               51584
    46   ./data/importing/ASM.csv SP.URB.GROW     1.6142305917451
    47   ./data/importing/ASM.csv SP.POP.TOTL               58230
    48   ./data/importing/ASM.csv SP.POP.GROW    1.09822902395737
    49   ./data/importing/ATG.csv SP.URB.TOTL               24113
    50   ./data/importing/ATG.csv SP.URB.GROW    0.53641727893261
    51   ./data/importing/ATG.csv SP.POP.TOTL               75055
    52   ./data/importing/ATG.csv SP.POP.GROW    1.65779341376666
    53   ./data/importing/AUS.csv SP.URB.TOTL            16028911
    54   ./data/importing/AUS.csv SP.URB.GROW   0.984333804176905
    55   ./data/importing/AUS.csv SP.POP.TOTL            19028802
    56   ./data/importing/AUS.csv SP.POP.GROW    1.14447285147721
    57   ./data/importing/AUT.csv SP.URB.TOTL             4824004
    58   ./data/importing/AUT.csv SP.URB.GROW  -0.220175991539749
    59   ./data/importing/AUT.csv SP.POP.TOTL             8011566
    60   ./data/importing/AUT.csv SP.POP.GROW   0.240466652446524
    61   ./data/importing/AZE.csv SP.URB.TOTL             4135854
    62   ./data/importing/AZE.csv SP.URB.GROW    1.21344378120309
    63   ./data/importing/AZE.csv SP.POP.TOTL             8048600
    64   ./data/importing/AZE.csv SP.POP.GROW   0.821519963674211
    65   ./data/importing/BDI.csv SP.URB.TOTL              520130
    66   ./data/importing/BDI.csv SP.URB.GROW    4.62153763088361
    67   ./data/importing/BDI.csv SP.POP.TOTL             6307659
    68   ./data/importing/BDI.csv SP.POP.GROW     2.0417212041938
    69   ./data/importing/BEL.csv SP.URB.TOTL             9956937
    70   ./data/importing/BEL.csv SP.URB.GROW   0.308431312307231
    71   ./data/importing/BEL.csv SP.POP.TOTL            10251250
    72   ./data/importing/BEL.csv SP.POP.GROW   0.242517956221334
    73   ./data/importing/BEN.csv SP.URB.TOTL             2682552
    74   ./data/importing/BEN.csv SP.URB.GROW    3.87148751501267
    75   ./data/importing/BEN.csv SP.POP.TOTL             6998023
    76   ./data/importing/BEN.csv SP.POP.GROW    3.03845662138601
    77   ./data/importing/BFA.csv SP.URB.TOTL             2120383
    78   ./data/importing/BFA.csv SP.URB.GROW    6.85756484832227
    79   ./data/importing/BFA.csv SP.POP.TOTL            11882888
    80   ./data/importing/BFA.csv SP.POP.GROW    2.98388558736392
    81   ./data/importing/BGD.csv SP.URB.TOTL            30476706
    82   ./data/importing/BGD.csv SP.URB.GROW    3.56396663214103
    83   ./data/importing/BGD.csv SP.POP.TOTL           129193327
    84   ./data/importing/BGD.csv SP.POP.GROW    1.90552404891968
    85   ./data/importing/BGR.csv SP.URB.TOTL             5629167
    86   ./data/importing/BGR.csv SP.URB.GROW  -0.171157650642926
    87   ./data/importing/BGR.csv SP.POP.TOTL             8170172
    88   ./data/importing/BGR.csv SP.POP.GROW  -0.493896416633318
    89   ./data/importing/BHR.csv SP.URB.TOTL              628716
    90   ./data/importing/BHR.csv SP.URB.GROW    2.74720093000752
    91   ./data/importing/BHR.csv SP.POP.TOTL              711442
    92   ./data/importing/BHR.csv SP.POP.GROW     2.7515762605179
    93   ./data/importing/BHS.csv SP.URB.TOTL              266534
    94   ./data/importing/BHS.csv SP.URB.GROW    1.69321459564921
    95   ./data/importing/BHS.csv SP.POP.TOTL              325014
    96   ./data/importing/BHS.csv SP.POP.GROW    1.46976235766802
    97   ./data/importing/BIH.csv SP.URB.TOTL             1771376
    98   ./data/importing/BIH.csv SP.URB.GROW    1.37816316655527
    99   ./data/importing/BIH.csv SP.POP.TOTL             4179350
    100  ./data/importing/BIH.csv SP.POP.GROW   0.632139635256837
    101  ./data/importing/BLR.csv SP.URB.TOTL             6983033
    102  ./data/importing/BLR.csv SP.URB.GROW   0.233049857909096
    103  ./data/importing/BLR.csv SP.POP.TOTL             9979610
    104  ./data/importing/BLR.csv SP.POP.GROW  -0.471131334643547
    105  ./data/importing/BLZ.csv SP.URB.TOTL              109140
    106  ./data/importing/BLZ.csv SP.URB.GROW    2.84002286407907
    107  ./data/importing/BLZ.csv SP.POP.TOTL              240406
    108  ./data/importing/BLZ.csv SP.POP.GROW    3.24372870048271
    109  ./data/importing/BMU.csv SP.URB.TOTL               61833
    110  ./data/importing/BMU.csv SP.URB.GROW   0.890208773614165
    111  ./data/importing/BMU.csv SP.POP.TOTL               61833
    112  ./data/importing/BMU.csv SP.POP.GROW   0.890208773614165
    113  ./data/importing/BOL.csv SP.URB.TOTL             5309144
    114  ./data/importing/BOL.csv SP.URB.GROW    2.65383928729288
    115  ./data/importing/BOL.csv SP.POP.TOTL             8592656
    116  ./data/importing/BOL.csv SP.POP.GROW    1.80379049207336
    117  ./data/importing/BRA.csv SP.URB.TOTL           142795391
    118  ./data/importing/BRA.csv SP.URB.GROW    2.22769397356534
    119  ./data/importing/BRA.csv SP.POP.TOTL           175873720
    120  ./data/importing/BRA.csv SP.POP.GROW    1.36677132973907
    121  ./data/importing/BRB.csv SP.URB.TOTL               89526
    122  ./data/importing/BRB.csv SP.URB.GROW  -0.771096201563446
    123  ./data/importing/BRB.csv SP.POP.TOTL              264657
    124  ./data/importing/BRB.csv SP.POP.GROW    0.18418126822941
    125  ./data/importing/BRN.csv SP.URB.TOTL              237635
    126  ./data/importing/BRN.csv SP.URB.GROW    2.78031536453181
    127  ./data/importing/BRN.csv SP.POP.TOTL              333926
    128  ./data/importing/BRN.csv SP.POP.GROW    2.08216354349989
    129  ./data/importing/BTN.csv SP.URB.TOTL              149256
    130  ./data/importing/BTN.csv SP.URB.GROW     6.9578805637825
    131  ./data/importing/BTN.csv SP.POP.TOTL              587207
    132  ./data/importing/BTN.csv SP.POP.GROW    2.80057018423756
    133  ./data/importing/BWA.csv SP.URB.TOTL              919084
    134  ./data/importing/BWA.csv SP.URB.GROW    3.67700580793188
    135  ./data/importing/BWA.csv SP.POP.TOTL             1726985
    136  ./data/importing/BWA.csv SP.POP.GROW    2.07271158015976
    137  ./data/importing/CAF.csv SP.URB.TOTL             1414914
    138  ./data/importing/CAF.csv SP.URB.GROW    3.01842513430412
    139  ./data/importing/CAF.csv SP.POP.TOTL             3759170
    140  ./data/importing/CAF.csv SP.POP.GROW    2.80036047898137
    141  ./data/importing/CAN.csv SP.URB.TOTL            24388404
    142  ./data/importing/CAN.csv SP.URB.GROW    1.40422595439655
    143  ./data/importing/CAN.csv SP.POP.TOTL            30685730
    144  ./data/importing/CAN.csv SP.POP.GROW   0.931281553452429
    145  ./data/importing/CEB.csv SP.URB.TOTL            66669587
    146  ./data/importing/CEB.csv SP.URB.GROW  -0.608882078841546
    147  ./data/importing/CEB.csv SP.POP.TOTL           108447824
    148  ./data/importing/CEB.csv SP.POP.GROW  -0.562187468913606
    149  ./data/importing/CHE.csv SP.URB.TOTL             5272018
    150  ./data/importing/CHE.csv SP.URB.GROW   0.487032192288853
    151  ./data/importing/CHE.csv SP.POP.TOTL             7184250
    152  ./data/importing/CHE.csv SP.POP.GROW   0.561954617400164
    153  ./data/importing/CHI.csv SP.URB.TOTL               44267
    154  ./data/importing/CHI.csv SP.URB.GROW   0.391575969156542
    155  ./data/importing/CHI.csv SP.POP.TOTL              145306
    156  ./data/importing/CHI.csv SP.POP.GROW   0.341241968130063
    157  ./data/importing/CHL.csv SP.URB.TOTL            13213754
    158  ./data/importing/CHL.csv SP.URB.GROW    1.49236285157488
    159  ./data/importing/CHL.csv SP.POP.TOTL            15351799
    160  ./data/importing/CHL.csv SP.POP.GROW    1.14904172536735
    161  ./data/importing/CHN.csv SP.URB.TOTL           452999147
    162  ./data/importing/CHN.csv SP.URB.GROW    3.64925271115854
    163  ./data/importing/CHN.csv SP.POP.TOTL          1262645000
    164  ./data/importing/CHN.csv SP.POP.GROW   0.787956592953992
    165  ./data/importing/CIV.csv SP.URB.TOTL             7249898
    166  ./data/importing/CIV.csv SP.URB.GROW    3.70221408199557
    167  ./data/importing/CIV.csv SP.POP.TOTL            16799670
    168  ./data/importing/CIV.csv SP.POP.GROW    2.73825090531507
    169  ./data/importing/CMR.csv SP.URB.TOTL             6873014
    170  ./data/importing/CMR.csv SP.URB.GROW    3.95780816719243
    171  ./data/importing/CMR.csv SP.POP.TOTL            15091594
    172  ./data/importing/CMR.csv SP.POP.GROW    2.63602726719356
    173  ./data/importing/COD.csv SP.URB.TOTL            17075023
    174  ./data/importing/COD.csv SP.URB.GROW    4.22587294178986
    175  ./data/importing/COD.csv SP.POP.TOTL            48616317
    176  ./data/importing/COD.csv SP.POP.GROW    2.89884128806625
    177  ./data/importing/COG.csv SP.URB.TOTL             1839519
    178  ./data/importing/COG.csv SP.URB.GROW    4.61063543293432
    179  ./data/importing/COG.csv SP.POP.TOTL             3134030
    180  ./data/importing/COG.csv SP.POP.GROW    3.81697887153797
    181  ./data/importing/COL.csv SP.URB.TOTL            29002337
    182  ./data/importing/COL.csv SP.URB.GROW    2.19886492761409
    183  ./data/importing/COL.csv SP.POP.TOTL            39215135
    184  ./data/importing/COL.csv SP.POP.GROW    1.61983140551157
    185  ./data/importing/COM.csv SP.URB.TOTL              150722
    186  ./data/importing/COM.csv SP.URB.GROW    1.78397456253272
    187  ./data/importing/COM.csv SP.POP.TOTL              536758
    188  ./data/importing/COM.csv SP.POP.GROW    1.94042775607481
    189  ./data/importing/CPV.csv SP.URB.TOTL              244866
    190  ./data/importing/CPV.csv SP.URB.GROW    3.65530991464629
    191  ./data/importing/CPV.csv SP.POP.TOTL              458251
    192  ./data/importing/CPV.csv SP.POP.GROW    1.89987187752495
    193  ./data/importing/CRI.csv SP.URB.TOTL             2349793
    194  ./data/importing/CRI.csv SP.URB.GROW    3.50078660942677
    195  ./data/importing/CRI.csv SP.POP.TOTL             3979193
    196  ./data/importing/CRI.csv SP.POP.GROW    1.97358823214158
    197  ./data/importing/CSS.csv SP.URB.TOTL             3317222
    198  ./data/importing/CSS.csv SP.URB.GROW   0.991089510416259
    199  ./data/importing/CSS.csv SP.POP.TOTL             6582141
    200  ./data/importing/CSS.csv SP.POP.GROW   0.715212644247117
    201  ./data/importing/CUB.csv SP.URB.TOTL             8365215
    202  ./data/importing/CUB.csv SP.URB.GROW   0.633089365845533
    203  ./data/importing/CUB.csv SP.POP.TOTL            11105791
    204  ./data/importing/CUB.csv SP.POP.GROW    0.32194458830191
    205  ./data/importing/CUW.csv SP.URB.TOTL              121481
    206  ./data/importing/CUW.csv SP.URB.GROW   -3.46254083518543
    207  ./data/importing/CUW.csv SP.POP.TOTL              133860
    208  ./data/importing/CUW.csv SP.POP.GROW   -4.07538613162288
    209  ./data/importing/CYM.csv SP.URB.TOTL               39658
    210  ./data/importing/CYM.csv SP.URB.GROW    3.49733538150567
    211  ./data/importing/CYM.csv SP.POP.TOTL               39658
    212  ./data/importing/CYM.csv SP.POP.GROW    3.49733538150567
    213  ./data/importing/CYP.csv SP.URB.TOTL              650946
    214  ./data/importing/CYP.csv SP.URB.GROW    1.94647761607177
    215  ./data/importing/CYP.csv SP.POP.TOTL              948237
    216  ./data/importing/CYP.csv SP.POP.GROW    1.77009330000432
    217  ./data/importing/CZE.csv SP.URB.TOTL             7587516
    218  ./data/importing/CZE.csv SP.URB.GROW  -0.458662204702604
    219  ./data/importing/CZE.csv SP.POP.TOTL            10255063
    220  ./data/importing/CZE.csv SP.POP.GROW  -0.280414108078884
    221  ./data/importing/DEU.csv SP.URB.TOTL            61629857
    222  ./data/importing/DEU.csv SP.URB.GROW   0.411941893833571
    223  ./data/importing/DEU.csv SP.POP.TOTL            82211508
    224  ./data/importing/DEU.csv SP.POP.GROW   0.135431600393084
    225  ./data/importing/DJI.csv SP.URB.TOTL              567893
    226  ./data/importing/DJI.csv SP.URB.GROW    3.16311796731286
    227  ./data/importing/DJI.csv SP.POP.TOTL              742033
    228  ./data/importing/DJI.csv SP.POP.GROW     3.1016497493543
    229  ./data/importing/DMA.csv SP.URB.TOTL               44606
    230  ./data/importing/DMA.csv SP.URB.GROW  -0.382623560679492
    231  ./data/importing/DMA.csv SP.POP.TOTL               68346
    232  ./data/importing/DMA.csv SP.POP.GROW  -0.513704757719484
    233  ./data/importing/DNK.csv SP.URB.TOTL             4544013
    234  ./data/importing/DNK.csv SP.URB.GROW   0.350679180938332
    235  ./data/importing/DNK.csv SP.POP.TOTL             5339616
    236  ./data/importing/DNK.csv SP.POP.GROW   0.334233618829261
    237  ./data/importing/DOM.csv SP.URB.TOTL             5274195
    238  ./data/importing/DOM.csv SP.URB.GROW    2.86364158534241
    239  ./data/importing/DOM.csv SP.POP.TOTL             8540791
    240  ./data/importing/DOM.csv SP.POP.GROW    1.52687888895349
    241  ./data/importing/DZA.csv SP.URB.TOTL            18439845
    242  ./data/importing/DZA.csv SP.URB.GROW    2.74811230466512
    243  ./data/importing/DZA.csv SP.POP.TOTL            30774621
    244  ./data/importing/DZA.csv SP.POP.GROW    1.40229085828763
    245  ./data/importing/EAP.csv SP.URB.TOTL           664869369
    246  ./data/importing/EAP.csv SP.URB.GROW    3.59317184748342
    247  ./data/importing/EAP.csv SP.POP.TOTL          1817064785
    248  ./data/importing/EAP.csv SP.POP.GROW    1.00015598826199
    249  ./data/importing/EAR.csv SP.URB.TOTL           957507719
    250  ./data/importing/EAR.csv SP.URB.GROW    2.77926436781644
    251  ./data/importing/EAR.csv SP.POP.TOTL          2499206970
    252  ./data/importing/EAR.csv SP.POP.GROW    1.88565238059226
    253  ./data/importing/EAS.csv SP.URB.TOTL           848763919
    254  ./data/importing/EAS.csv SP.URB.GROW    2.94994040900316
    255  ./data/importing/EAS.csv SP.POP.TOTL          2048148696
    256  ./data/importing/EAS.csv SP.POP.GROW    0.94409698205007
    257  ./data/importing/ECA.csv SP.URB.TOTL           238079874
    258  ./data/importing/ECA.csv SP.URB.GROW   0.203129460542058
    259  ./data/importing/ECA.csv SP.POP.TOTL           370646199
    260  ./data/importing/ECA.csv SP.POP.GROW  0.0268381789157814
    261  ./data/importing/ECS.csv SP.URB.TOTL           591738229
    262  ./data/importing/ECS.csv SP.URB.GROW   0.304399863956235
    263  ./data/importing/ECS.csv SP.POP.TOTL           862786211
    264  ./data/importing/ECS.csv SP.POP.GROW   0.108791382445531
    265  ./data/importing/ECU.csv SP.URB.TOTL             7613657
    266  ./data/importing/ECU.csv SP.URB.GROW    2.55075761062712
    267  ./data/importing/ECU.csv SP.POP.TOTL            12626507
    268  ./data/importing/ECU.csv SP.POP.GROW    1.71308817043169
    269  ./data/importing/EGY.csv SP.URB.TOTL            30544806
    270  ./data/importing/EGY.csv SP.URB.GROW    2.18171596199955
    271  ./data/importing/EGY.csv SP.POP.TOTL            71371371
    272  ./data/importing/EGY.csv SP.POP.GROW    2.07183470577985
    273  ./data/importing/EMU.csv SP.URB.TOTL           234381189
    274  ./data/importing/EMU.csv SP.URB.GROW   0.588520975014291
    275  ./data/importing/EMU.csv SP.POP.TOTL           321324622
    276  ./data/importing/EMU.csv SP.POP.GROW   0.328616767600721
    277  ./data/importing/ERI.csv SP.URB.TOTL              636195
    278  ./data/importing/ERI.csv SP.URB.GROW    4.79009738761001
    279  ./data/importing/ERI.csv SP.POP.TOTL             2392880
    280  ./data/importing/ERI.csv SP.POP.GROW    1.53299543125693
    281  ./data/importing/ESP.csv SP.URB.TOTL            30937864
    282  ./data/importing/ESP.csv SP.URB.GROW   0.553405611176717
    283  ./data/importing/ESP.csv SP.POP.TOTL            40567864
    284  ./data/importing/ESP.csv SP.POP.GROW   0.447137014535835
    285  ./data/importing/EST.csv SP.URB.TOTL              969061
    286  ./data/importing/EST.csv SP.URB.GROW    0.23183289495095
    287  ./data/importing/EST.csv SP.POP.TOTL             1396985
    288  ./data/importing/EST.csv SP.POP.GROW   0.483707161729931
    289  ./data/importing/ETH.csv SP.URB.TOTL             9880497
    290  ./data/importing/ETH.csv SP.URB.GROW    4.23557684937443
    291  ./data/importing/ETH.csv SP.POP.TOTL            67031867
    292  ./data/importing/ETH.csv SP.POP.GROW    2.95880518919236
    293  ./data/importing/EUU.csv SP.URB.TOTL           304085189
    294  ./data/importing/EUU.csv SP.URB.GROW   0.340804127803992
    295  ./data/importing/EUU.csv SP.POP.TOTL           429342455
    296  ./data/importing/EUU.csv SP.POP.GROW   0.119792808014381
    297  ./data/importing/FCS.csv SP.URB.TOTL           227304933
    298  ./data/importing/FCS.csv SP.URB.GROW    2.84312307960302
    299  ./data/importing/FCS.csv SP.POP.TOTL           612395529
    300  ./data/importing/FCS.csv SP.POP.GROW    2.25004229908427
    301  ./data/importing/FIN.csv SP.URB.TOTL             4253964
    302  ./data/importing/FIN.csv SP.URB.GROW   0.496415301116337
    303  ./data/importing/FIN.csv SP.POP.TOTL             5176209
    304  ./data/importing/FIN.csv SP.POP.GROW     0.2076065154133
    305  ./data/importing/FJI.csv SP.URB.TOTL              398838
    306  ./data/importing/FJI.csv SP.URB.GROW    1.92111334652935
    307  ./data/importing/FJI.csv SP.POP.TOTL              832509
    308  ./data/importing/FJI.csv SP.POP.GROW    1.09752051437681
    309  ./data/importing/FRA.csv SP.URB.TOTL            46221663
    310  ./data/importing/FRA.csv SP.URB.GROW    1.02609018706937
    311  ./data/importing/FRA.csv SP.POP.TOTL            60921384
    312  ./data/importing/FRA.csv SP.POP.GROW   0.686782586857032
    313  ./data/importing/FRO.csv SP.URB.TOTL               16591
    314  ./data/importing/FRO.csv SP.URB.GROW    2.32320439136337
    315  ./data/importing/FRO.csv SP.POP.TOTL               45660
    316  ./data/importing/FRO.csv SP.POP.GROW    1.00591847845505
    317  ./data/importing/FSM.csv SP.URB.TOTL               24945
    318  ./data/importing/FSM.csv SP.URB.GROW   -1.70516264054059
    319  ./data/importing/FSM.csv SP.POP.TOTL              111709
    320  ./data/importing/FSM.csv SP.POP.GROW   0.152297026498527
    321  ./data/importing/GAB.csv SP.URB.TOTL             1004078
    322  ./data/importing/GAB.csv SP.URB.GROW    3.54974250712575
    323  ./data/importing/GAB.csv SP.POP.TOTL             1272935
    324  ./data/importing/GAB.csv SP.POP.GROW    2.55979388649606
    325  ./data/importing/GBR.csv SP.URB.TOTL            46319551
    326  ./data/importing/GBR.csv SP.URB.GROW   0.433615661051976
    327  ./data/importing/GBR.csv SP.POP.TOTL            58892514
    328  ./data/importing/GBR.csv SP.POP.GROW   0.357300887421605
    329  ./data/importing/GEO.csv SP.URB.TOTL             2146120
    330  ./data/importing/GEO.csv SP.URB.GROW   -2.40145855729194
    331  ./data/importing/GEO.csv SP.POP.TOTL             4077131
    332  ./data/importing/GEO.csv SP.POP.GROW    -1.9446291567975
    333  ./data/importing/GHA.csv SP.URB.TOTL             8638858
    334  ./data/importing/GHA.csv SP.URB.GROW    4.22698334801559
    335  ./data/importing/GHA.csv SP.POP.TOTL            19665502
    336  ./data/importing/GHA.csv SP.POP.GROW    2.51651873851715
    337  ./data/importing/GIB.csv SP.URB.TOTL               27741
    338  ./data/importing/GIB.csv SP.URB.GROW    0.16595717785441
    339  ./data/importing/GIB.csv SP.POP.TOTL               27741
    340  ./data/importing/GIB.csv SP.POP.GROW    0.16595717785441
    341  ./data/importing/GIN.csv SP.URB.TOTL             2573538
    342  ./data/importing/GIN.csv SP.URB.GROW    2.85469725493748
    343  ./data/importing/GIN.csv SP.POP.TOTL             8336967
    344  ./data/importing/GIN.csv SP.POP.GROW    1.96313638467112
    345  ./data/importing/GMB.csv SP.URB.TOTL              688121
    346  ./data/importing/GMB.csv SP.URB.GROW     4.8093581717006
    347  ./data/importing/GMB.csv SP.POP.TOTL             1437539
    348  ./data/importing/GMB.csv SP.POP.GROW    2.89642334291935
    349  ./data/importing/GNB.csv SP.URB.TOTL              446097
    350  ./data/importing/GNB.csv SP.URB.GROW    3.04871478018358
    351  ./data/importing/GNB.csv SP.POP.TOTL             1230849
    352  ./data/importing/GNB.csv SP.POP.GROW    2.00021192650957
    353  ./data/importing/GNQ.csv SP.URB.TOTL              336269
    354  ./data/importing/GNQ.csv SP.URB.GROW    8.08360381858775
    355  ./data/importing/GNQ.csv SP.POP.TOTL              684977
    356  ./data/importing/GNQ.csv SP.POP.GROW    4.47057515918696
    357  ./data/importing/GRC.csv SP.URB.TOTL             7857551
    358  ./data/importing/GRC.csv SP.URB.GROW   0.572820545681015
    359  ./data/importing/GRC.csv SP.POP.TOTL            10805808
    360  ./data/importing/GRC.csv SP.POP.GROW   0.409041838238246
    361  ./data/importing/GRD.csv SP.URB.TOTL               38351
    362  ./data/importing/GRD.csv SP.URB.GROW    1.26477676537637
    363  ./data/importing/GRD.csv SP.POP.TOTL              107432
    364  ./data/importing/GRD.csv SP.POP.GROW   0.568483013326884
    365  ./data/importing/GRL.csv SP.URB.TOTL               45859
    366  ./data/importing/GRL.csv SP.URB.GROW   0.329813441256315
    367  ./data/importing/GRL.csv SP.POP.TOTL               56200
    368  ./data/importing/GRL.csv SP.POP.GROW   0.178094437099469
    369  ./data/importing/GTM.csv SP.URB.TOTL             5253870
    370  ./data/importing/GTM.csv SP.URB.GROW    3.18010727540115
    371  ./data/importing/GTM.csv SP.POP.TOTL            11589761
    372  ./data/importing/GTM.csv SP.POP.GROW    2.43394364463754
    373  ./data/importing/GUM.csv SP.URB.TOTL              149181
    374  ./data/importing/GUM.csv SP.URB.GROW    1.32186821669843
    375  ./data/importing/GUM.csv SP.POP.TOTL              160188
    376  ./data/importing/GUM.csv SP.POP.GROW    1.12562122360417
    377  ./data/importing/GUY.csv SP.URB.TOTL              217802
    378  ./data/importing/GUY.csv SP.URB.GROW   -0.16973493319816
    379  ./data/importing/GUY.csv SP.POP.TOTL              759051
    380  ./data/importing/GUY.csv SP.POP.GROW   0.136711368532353
    381  ./data/importing/HIC.csv SP.URB.TOTL           839091772
    382  ./data/importing/HIC.csv SP.URB.GROW   0.899429047689139
    383  ./data/importing/HIC.csv SP.POP.TOTL          1101322358
    384  ./data/importing/HIC.csv SP.POP.GROW   0.612160081391139
    385  ./data/importing/HKG.csv SP.URB.TOTL             6665000
    386  ./data/importing/HKG.csv SP.URB.GROW   0.881594075853745
    387  ./data/importing/HKG.csv SP.POP.TOTL             6665000
    388  ./data/importing/HKG.csv SP.POP.GROW   0.881594075853745
    389  ./data/importing/HND.csv SP.URB.TOTL             3026014
    390  ./data/importing/HND.csv SP.URB.GROW    3.85074216674986
    391  ./data/importing/HND.csv SP.POP.TOTL             6656725
    392  ./data/importing/HND.csv SP.POP.GROW    2.73138300029784
    393  ./data/importing/HPC.csv SP.URB.TOTL           136443921
    394  ./data/importing/HPC.csv SP.URB.GROW    3.85451058826591
    395  ./data/importing/HPC.csv SP.POP.TOTL           475127894
    396  ./data/importing/HPC.csv SP.POP.GROW     2.7230069331732
    397  ./data/importing/HRV.csv SP.URB.TOTL             2387324
    398  ./data/importing/HRV.csv SP.URB.GROW  -0.555021490838623
    399  ./data/importing/HRV.csv SP.POP.TOTL             4468302
    400  ./data/importing/HRV.csv SP.POP.GROW  -0.986434858645568
    401  ./data/importing/HTI.csv SP.URB.TOTL             2976240
    402  ./data/importing/HTI.csv SP.URB.GROW     3.5487427661698
    403  ./data/importing/HTI.csv SP.POP.TOTL             8360225
    404  ./data/importing/HTI.csv SP.POP.GROW    1.82614145032597
    405  ./data/importing/HUN.csv SP.URB.TOTL             6593735
    406  ./data/importing/HUN.csv SP.URB.GROW  -0.456240196646938
    407  ./data/importing/HUN.csv SP.POP.TOTL            10210971
    408  ./data/importing/HUN.csv SP.POP.GROW  -0.259764908288633
    409  ./data/importing/IBD.csv SP.URB.TOTL          1742482689
    410  ./data/importing/IBD.csv SP.URB.GROW    2.48383423347501
    411  ./data/importing/IBD.csv SP.POP.TOTL          3999140171
    412  ./data/importing/IBD.csv SP.POP.GROW    1.21533207508904
    413  ./data/importing/IBT.csv SP.URB.TOTL          2059347840
    414  ./data/importing/IBT.csv SP.URB.GROW    2.67425581888314
    415  ./data/importing/IBT.csv SP.POP.TOTL          5093906989
    416  ./data/importing/IBT.csv SP.POP.GROW    1.49126530696937
    417  ./data/importing/IDA.csv SP.URB.TOTL           316865151
    418  ./data/importing/IDA.csv SP.URB.GROW    3.73418464477022
    419  ./data/importing/IDA.csv SP.POP.TOTL          1094766818
    420  ./data/importing/IDA.csv SP.POP.GROW    2.51215321548015
    421  ./data/importing/IDB.csv SP.URB.TOTL           125693668
    422  ./data/importing/IDB.csv SP.URB.GROW    3.80699666984914
    423  ./data/importing/IDB.csv SP.POP.TOTL           370910722
    424  ./data/importing/IDB.csv SP.POP.GROW    2.74314664989879
    425  ./data/importing/IDN.csv SP.URB.TOTL            89914698
    426  ./data/importing/IDN.csv SP.URB.GROW     4.3702128806786
    427  ./data/importing/IDN.csv SP.POP.TOTL           214072421
    428  ./data/importing/IDN.csv SP.POP.GROW    1.44708848408903
    429  ./data/importing/IDX.csv SP.URB.TOTL           191171483
    430  ./data/importing/IDX.csv SP.URB.GROW    3.68636697238043
    431  ./data/importing/IDX.csv SP.POP.TOTL           723856096
    432  ./data/importing/IDX.csv SP.POP.GROW    2.39419199741633
    433  ./data/importing/IMN.csv SP.URB.TOTL               39158
    434  ./data/importing/IMN.csv SP.URB.GROW    1.20494219286421
    435  ./data/importing/IMN.csv SP.POP.TOTL               75562
    436  ./data/importing/IMN.csv SP.POP.GROW    1.19018911438933
    437  ./data/importing/IND.csv SP.URB.TOTL           293168849
    438  ./data/importing/IND.csv SP.URB.GROW    2.59867555250261
    439  ./data/importing/IND.csv SP.POP.TOTL          1059633675
    440  ./data/importing/IND.csv SP.POP.GROW    1.82218400218023
    441  ./data/importing/INX.csv SP.URB.TOTL                <NA>
    442  ./data/importing/INX.csv SP.URB.GROW                <NA>
    443  ./data/importing/INX.csv SP.POP.TOTL                <NA>
    444  ./data/importing/INX.csv SP.POP.GROW                <NA>
    445  ./data/importing/IRL.csv SP.URB.TOTL             2250951
    446  ./data/importing/IRL.csv SP.URB.GROW     1.7481026700676
    447  ./data/importing/IRL.csv SP.POP.TOTL             3805174
    448  ./data/importing/IRL.csv SP.POP.GROW    1.33304266586712
    449  ./data/importing/IRN.csv SP.URB.TOTL            41975934
    450  ./data/importing/IRN.csv SP.URB.GROW    2.78233953467196
    451  ./data/importing/IRN.csv SP.POP.TOTL            65544383
    452  ./data/importing/IRN.csv SP.POP.GROW    1.64539194870845
    453  ./data/importing/IRQ.csv SP.URB.TOTL            16869783
    454  ./data/importing/IRQ.csv SP.URB.GROW    3.41511557664929
    455  ./data/importing/IRQ.csv SP.POP.TOTL            24628858
    456  ./data/importing/IRQ.csv SP.POP.GROW    3.33624669433987
    457  ./data/importing/ISL.csv SP.URB.TOTL              259836
    458  ./data/importing/ISL.csv SP.URB.GROW    1.51427858666661
    459  ./data/importing/ISL.csv SP.POP.TOTL              281205
    460  ./data/importing/ISL.csv SP.POP.GROW    1.36919283329812
    461  ./data/importing/ISR.csv SP.URB.TOTL             5735757
    462  ./data/importing/ISR.csv SP.URB.GROW    2.71253095110511
    463  ./data/importing/ISR.csv SP.POP.TOTL             6289000
    464  ./data/importing/ISR.csv SP.POP.GROW    2.64233191305756
    465  ./data/importing/ITA.csv SP.URB.TOTL            38277624
    466  ./data/importing/ITA.csv SP.URB.GROW   0.134599938879629
    467  ./data/importing/ITA.csv SP.POP.TOTL            56942108
    468  ./data/importing/ITA.csv SP.POP.GROW   0.045303631138613
    469  ./data/importing/JAM.csv SP.URB.TOTL             1353488
    470  ./data/importing/JAM.csv SP.URB.GROW    1.07611505949566
    471  ./data/importing/JAM.csv SP.POP.TOTL             2612205
    472  ./data/importing/JAM.csv SP.POP.GROW   0.611850749006637
    473  ./data/importing/JOR.csv SP.URB.TOTL             3957467
    474  ./data/importing/JOR.csv SP.URB.GROW    2.12192984544585
    475  ./data/importing/JOR.csv SP.POP.TOTL             5056174
    476  ./data/importing/JOR.csv SP.POP.GROW    2.10659408273097
    477  ./data/importing/JPN.csv SP.URB.TOTL            99760751
    478  ./data/importing/JPN.csv SP.URB.GROW   0.327609574744949
    479  ./data/importing/JPN.csv SP.POP.TOTL           126843000
    480  ./data/importing/JPN.csv SP.POP.GROW   0.167275578113187
    481  ./data/importing/KAZ.csv SP.URB.TOTL             8349417
    482  ./data/importing/KAZ.csv SP.URB.GROW  -0.169987079196277
    483  ./data/importing/KAZ.csv SP.POP.TOTL            14883626
    484  ./data/importing/KAZ.csv SP.POP.GROW  -0.300201486690526
    485  ./data/importing/KEN.csv SP.URB.TOTL             6137001
    486  ./data/importing/KEN.csv SP.URB.GROW     4.6496699104578
    487  ./data/importing/KEN.csv SP.POP.TOTL            30851606
    488  ./data/importing/KEN.csv SP.POP.GROW    2.91544684198394
    489  ./data/importing/KGZ.csv SP.URB.TOTL             1729037
    490  ./data/importing/KGZ.csv SP.URB.GROW    1.18545832829557
    491  ./data/importing/KGZ.csv SP.POP.TOTL             4898400
    492  ./data/importing/KGZ.csv SP.POP.GROW    1.19112592398474
    493  ./data/importing/KHM.csv SP.URB.TOTL             2252408
    494  ./data/importing/KHM.csv SP.URB.GROW    2.45675764547543
    495  ./data/importing/KHM.csv SP.POP.TOTL            12118841
    496  ./data/importing/KHM.csv SP.POP.GROW    1.83064817889597
    497  ./data/importing/KIR.csv SP.URB.TOTL               38158
    498  ./data/importing/KIR.csv SP.URB.GROW    5.10202289784141
    499  ./data/importing/KIR.csv SP.POP.TOTL               88826
    500  ./data/importing/KIR.csv SP.POP.GROW    1.74549473198221
    501  ./data/importing/KNA.csv SP.URB.TOTL               14901
    502  ./data/importing/KNA.csv SP.URB.GROW   0.835640687182618
    503  ./data/importing/KNA.csv SP.POP.TOTL               45461
    504  ./data/importing/KNA.csv SP.POP.GROW     1.4088796051579
    505  ./data/importing/KOR.csv SP.URB.TOTL            37428328
    506  ./data/importing/KOR.csv SP.URB.GROW    1.13428440765118
    507  ./data/importing/KOR.csv SP.POP.TOTL            47008111
    508  ./data/importing/KOR.csv SP.POP.GROW   0.836180864297766
    509  ./data/importing/KWT.csv SP.URB.TOTL             1915552
    510  ./data/importing/KWT.csv SP.URB.GROW    3.15487927706308
    511  ./data/importing/KWT.csv SP.POP.TOTL             1934901
    512  ./data/importing/KWT.csv SP.POP.GROW    3.01539399732843
    513  ./data/importing/LAC.csv SP.URB.TOTL           348963364
    514  ./data/importing/LAC.csv SP.URB.GROW    2.12103007005746
    515  ./data/importing/LAC.csv SP.POP.TOTL           468884433
    516  ./data/importing/LAC.csv SP.POP.GROW     1.4796413383072
    517  ./data/importing/LAO.csv SP.URB.TOTL             1193539
    518  ./data/importing/LAO.csv SP.URB.GROW    6.27632532532244
    519  ./data/importing/LAO.csv SP.POP.TOTL             5430853
    520  ./data/importing/LAO.csv SP.POP.GROW    1.68600720393729
    521  ./data/importing/LBN.csv SP.URB.TOTL             3715752
    522  ./data/importing/LBN.csv SP.URB.GROW    1.78182968975603
    523  ./data/importing/LBN.csv SP.POP.TOTL             4320642
    524  ./data/importing/LBN.csv SP.POP.GROW     1.6480313517987
    525  ./data/importing/LBR.csv SP.URB.TOTL             1283482
    526  ./data/importing/LBR.csv SP.URB.GROW    4.48806596410507
    527  ./data/importing/LBR.csv SP.POP.TOTL             2895224
    528  ./data/importing/LBR.csv SP.POP.GROW    3.71130059206802
    529  ./data/importing/LBY.csv SP.URB.TOTL             3937589
    530  ./data/importing/LBY.csv SP.URB.GROW    2.03048728445198
    531  ./data/importing/LBY.csv SP.POP.TOTL             5154790
    532  ./data/importing/LBY.csv SP.POP.GROW    1.89556266562551
    533  ./data/importing/LCA.csv SP.URB.TOTL               44300
    534  ./data/importing/LCA.csv SP.URB.GROW   0.076778903123659
    535  ./data/importing/LCA.csv SP.POP.TOTL              159500
    536  ./data/importing/LCA.csv SP.POP.GROW   0.782994722741428
    537  ./data/importing/LCN.csv SP.URB.TOTL           393557252
    538  ./data/importing/LCN.csv SP.URB.GROW    2.07993257427606
    539  ./data/importing/LCN.csv SP.POP.TOTL           521281149
    540  ./data/importing/LCN.csv SP.POP.GROW    1.47294661249624
    541  ./data/importing/LDC.csv SP.URB.TOTL           165527209
    542  ./data/importing/LDC.csv SP.URB.GROW    3.87123702795616
    543  ./data/importing/LDC.csv SP.POP.TOTL           662018171
    544  ./data/importing/LDC.csv SP.POP.GROW    2.43963206303424
    545  ./data/importing/LIC.csv SP.URB.TOTL           110204801
    546  ./data/importing/LIC.csv SP.URB.GROW    3.55150788904555
    547  ./data/importing/LIC.csv SP.POP.TOTL           400821583
    548  ./data/importing/LIC.csv SP.POP.GROW    2.67574122770131
    549  ./data/importing/LIE.csv SP.URB.TOTL                4997
    550  ./data/importing/LIE.csv SP.URB.GROW  -0.598565234837027
    551  ./data/importing/LIE.csv SP.POP.TOTL               33026
    552  ./data/importing/LIE.csv SP.POP.GROW    1.25228247927372
    553  ./data/importing/LKA.csv SP.URB.TOTL             3451324
    554  ./data/importing/LKA.csv SP.URB.GROW   0.523631902158173
    555  ./data/importing/LKA.csv SP.POP.TOTL            18777606
    556  ./data/importing/LKA.csv SP.POP.GROW   0.610633602585677
    557  ./data/importing/LMC.csv SP.URB.TOTL           831329755
    558  ./data/importing/LMC.csv SP.URB.GROW    2.93139596726328
    559  ./data/importing/LMC.csv SP.POP.TOTL          2459549814
    560  ./data/importing/LMC.csv SP.POP.GROW    1.87795753419972
    561  ./data/importing/LMY.csv SP.URB.TOTL          2005521539
    562  ./data/importing/LMY.csv SP.URB.GROW     2.7356904534672
    563  ./data/importing/LMY.csv SP.POP.TOTL          5018572610
    564  ./data/importing/LMY.csv SP.POP.GROW    1.51464413776077
    565  ./data/importing/LSO.csv SP.URB.TOTL              390692
    566  ./data/importing/LSO.csv SP.URB.GROW    2.86328747333908
    567  ./data/importing/LSO.csv SP.POP.TOTL             1998630
    568  ./data/importing/LSO.csv SP.POP.GROW   0.219741612882212
    569  ./data/importing/LTE.csv SP.URB.TOTL           933800752
    570  ./data/importing/LTE.csv SP.URB.GROW    2.44315831076814
    571  ./data/importing/LTE.csv SP.POP.TOTL          2046027286
    572  ./data/importing/LTE.csv SP.POP.GROW   0.759458776514506
    573  ./data/importing/LTU.csv SP.URB.TOTL             2344199
    574  ./data/importing/LTU.csv SP.URB.GROW  -0.792940722309791
    575  ./data/importing/LTU.csv SP.POP.TOTL             3499536
    576  ./data/importing/LTU.csv SP.POP.GROW  -0.703385440488978
    577  ./data/importing/LUX.csv SP.URB.TOTL              367434
    578  ./data/importing/LUX.csv SP.URB.GROW    2.26003451290749
    579  ./data/importing/LUX.csv SP.POP.TOTL              436300
    580  ./data/importing/LUX.csv SP.POP.GROW    1.34408299573143
    581  ./data/importing/LVA.csv SP.URB.TOTL             1611520
    582  ./data/importing/LVA.csv SP.URB.GROW   -1.18848873897458
    583  ./data/importing/LVA.csv SP.POP.TOTL             2367550
    584  ./data/importing/LVA.csv SP.POP.GROW  -0.963935407092365
    585  ./data/importing/MAC.csv SP.URB.TOTL              431896
    586  ./data/importing/MAC.csv SP.URB.GROW     1.4877568229253
    587  ./data/importing/MAC.csv SP.POP.TOTL              431896
    588  ./data/importing/MAC.csv SP.POP.GROW     1.4877568229253
    589  ./data/importing/MAF.csv SP.URB.TOTL                <NA>
    590  ./data/importing/MAF.csv SP.URB.GROW                <NA>
    591  ./data/importing/MAF.csv SP.POP.TOTL               29610
    592  ./data/importing/MAF.csv SP.POP.GROW    1.74083327876863
    593  ./data/importing/MAR.csv SP.URB.TOTL            15229497
    594  ./data/importing/MAR.csv SP.URB.GROW    1.94743958694502
    595  ./data/importing/MAR.csv SP.POP.TOTL            28554415
    596  ./data/importing/MAR.csv SP.POP.GROW    1.33056292746569
    597  ./data/importing/MCO.csv SP.URB.TOTL               32465
    598  ./data/importing/MCO.csv SP.URB.GROW   0.218936549624505
    599  ./data/importing/MCO.csv SP.POP.TOTL               32465
    600  ./data/importing/MCO.csv SP.POP.GROW   0.218936549624527
    601  ./data/importing/MDA.csv SP.URB.TOTL             1304080
    602  ./data/importing/MDA.csv SP.URB.GROW   -1.10310446057351
    603  ./data/importing/MDA.csv SP.POP.TOTL             2924668
    604  ./data/importing/MDA.csv SP.POP.GROW  -0.203371722054682
    605  ./data/importing/MDG.csv SP.URB.TOTL             4398058
    606  ./data/importing/MDG.csv SP.URB.GROW    4.03296351284223
    607  ./data/importing/MDG.csv SP.POP.TOTL            16216431
    608  ./data/importing/MDG.csv SP.POP.GROW    3.03990100767654
    609  ./data/importing/MDV.csv SP.URB.TOTL               78271
    610  ./data/importing/MDV.csv SP.URB.GROW    3.65346941142273
    611  ./data/importing/MDV.csv SP.POP.TOTL              282507
    612  ./data/importing/MDV.csv SP.POP.GROW    1.56830144656694
    613  ./data/importing/MEA.csv SP.URB.TOTL           187755039
    614  ./data/importing/MEA.csv SP.URB.GROW    2.73654015765081
    615  ./data/importing/MEA.csv SP.POP.TOTL           321037453
    616  ./data/importing/MEA.csv SP.POP.GROW    2.08773003994465
    617  ./data/importing/MEX.csv SP.URB.TOTL            73132993
    618  ./data/importing/MEX.csv SP.URB.GROW    1.96131969556176
    619  ./data/importing/MEX.csv SP.POP.TOTL            97873442
    620  ./data/importing/MEX.csv SP.POP.GROW    1.58455078746177
    621  ./data/importing/MHL.csv SP.URB.TOTL               37187
    622  ./data/importing/MHL.csv SP.URB.GROW    1.49278742965663
    623  ./data/importing/MHL.csv SP.POP.TOTL               54224
    624  ./data/importing/MHL.csv SP.POP.GROW   0.721837704584486
    625  ./data/importing/MIC.csv SP.URB.TOTL          1895316738
    626  ./data/importing/MIC.csv SP.URB.GROW    2.68864932692271
    627  ./data/importing/MIC.csv SP.POP.TOTL          4617751027
    628  ./data/importing/MIC.csv SP.POP.GROW    1.41509812019633
    629  ./data/importing/MKD.csv SP.URB.TOTL             1186387
    630  ./data/importing/MKD.csv SP.URB.GROW  0.0990892723551782
    631  ./data/importing/MKD.csv SP.POP.TOTL             2026350
    632  ./data/importing/MKD.csv SP.POP.GROW   0.455448702114895
    633  ./data/importing/MLI.csv SP.URB.TOTL             3186959
    634  ./data/importing/MLI.csv SP.URB.GROW    5.44357002629568
    635  ./data/importing/MLI.csv SP.POP.TOTL            11239101
    636  ./data/importing/MLI.csv SP.POP.GROW    2.90782929642605
    637  ./data/importing/MLT.csv SP.URB.TOTL              360316
    638  ./data/importing/MLT.csv SP.URB.GROW   0.952299180882739
    639  ./data/importing/MLT.csv SP.POP.TOTL              390087
    640  ./data/importing/MLT.csv SP.POP.GROW   0.645267230900854
    641  ./data/importing/MMR.csv SP.URB.TOTL            12306734
    642  ./data/importing/MMR.csv SP.URB.GROW    1.77244215299467
    643  ./data/importing/MMR.csv SP.POP.TOTL            45538332
    644  ./data/importing/MMR.csv SP.POP.GROW    1.09671263838614
    645  ./data/importing/MNA.csv SP.URB.TOTL           156981676
    646  ./data/importing/MNA.csv SP.URB.GROW    2.66762541081491
    647  ./data/importing/MNA.csv SP.POP.TOTL           283899110
    648  ./data/importing/MNA.csv SP.POP.GROW    1.99105888034856
    649  ./data/importing/MNE.csv SP.URB.TOTL              354162
    650  ./data/importing/MNE.csv SP.URB.GROW    1.59440537638767
    651  ./data/importing/MNE.csv SP.POP.TOTL              604950
    652  ./data/importing/MNE.csv SP.POP.GROW -0.0279740186992493
    653  ./data/importing/MNG.csv SP.URB.TOTL             1400318
    654  ./data/importing/MNG.csv SP.URB.GROW    1.81852607522354
    655  ./data/importing/MNG.csv SP.POP.TOTL             2450979
    656  ./data/importing/MNG.csv SP.POP.GROW   0.921869510816351
    657  ./data/importing/MNP.csv SP.URB.TOTL               72426
    658  ./data/importing/MNP.csv SP.URB.GROW    5.36607586626903
    659  ./data/importing/MNP.csv SP.POP.TOTL               80338
    660  ./data/importing/MNP.csv SP.POP.GROW    5.23958252727563
    661  ./data/importing/MOZ.csv SP.URB.TOTL             5170280
    662  ./data/importing/MOZ.csv SP.URB.GROW     3.0669112660627
    663  ./data/importing/MOZ.csv SP.POP.TOTL            17768505
    664  ./data/importing/MOZ.csv SP.POP.GROW    2.45330550582502
    665  ./data/importing/MRT.csv SP.URB.TOTL             1026554
    666  ./data/importing/MRT.csv SP.URB.GROW    2.47685978414761
    667  ./data/importing/MRT.csv SP.POP.TOTL             2695003
    668  ./data/importing/MRT.csv SP.POP.GROW    2.79918208413242
    669  ./data/importing/MUS.csv SP.URB.TOTL              506439
    670  ./data/importing/MUS.csv SP.URB.GROW   0.694890998843174
    671  ./data/importing/MUS.csv SP.POP.TOTL             1186873
    672  ./data/importing/MUS.csv SP.POP.GROW   0.982676166064297
    673  ./data/importing/MWI.csv SP.URB.TOTL             1640613
    674  ./data/importing/MWI.csv SP.URB.GROW    2.90504995399615
    675  ./data/importing/MWI.csv SP.POP.TOTL            11229387
    676  ./data/importing/MWI.csv SP.POP.GROW    2.30093521781424
    677  ./data/importing/MYS.csv SP.URB.TOTL            14220716
    678  ./data/importing/MYS.csv SP.URB.GROW    4.55739567987587
    679  ./data/importing/MYS.csv SP.POP.TOTL            22945150
    680  ./data/importing/MYS.csv SP.POP.GROW    2.54459366705921
    681  ./data/importing/NAC.csv SP.URB.TOTL           247519374
    682  ./data/importing/NAC.csv SP.URB.GROW    1.51255525280388
    683  ./data/importing/NAC.csv SP.POP.TOTL           312909974
    684  ./data/importing/NAC.csv SP.POP.GROW     1.1009288191018
    685  ./data/importing/NAM.csv SP.URB.TOTL              588911
    686  ./data/importing/NAM.csv SP.URB.GROW    3.89762918276995
    687  ./data/importing/NAM.csv SP.POP.TOTL             1819141
    688  ./data/importing/NAM.csv SP.POP.GROW    2.27194936669291
    689  ./data/importing/NCL.csv SP.URB.TOTL              132030
    690  ./data/importing/NCL.csv SP.URB.GROW    2.48459985865308
    691  ./data/importing/NCL.csv SP.POP.TOTL              213230
    692  ./data/importing/NCL.csv SP.POP.GROW    1.90137437804069
    693  ./data/importing/NER.csv SP.URB.TOTL             1881245
    694  ./data/importing/NER.csv SP.URB.GROW    3.93790676397728
    695  ./data/importing/NER.csv SP.POP.TOTL            11622665
    696  ./data/importing/NER.csv SP.POP.GROW    3.42375005583503
    697  ./data/importing/NGA.csv SP.URB.TOTL            42801631
    698  ./data/importing/NGA.csv SP.URB.GROW    4.15328596481643
    699  ./data/importing/NGA.csv SP.POP.TOTL           122851984
    700  ./data/importing/NGA.csv SP.POP.GROW    2.60286876961431
    701  ./data/importing/NIC.csv SP.URB.TOTL             2827250
    702  ./data/importing/NIC.csv SP.URB.GROW    1.71230408284756
    703  ./data/importing/NIC.csv SP.POP.TOTL             5123222
    704  ./data/importing/NIC.csv SP.POP.GROW    1.44194534225615
    705  ./data/importing/NLD.csv SP.URB.TOTL            12229998
    706  ./data/importing/NLD.csv SP.URB.GROW    1.71592210438949
    707  ./data/importing/NLD.csv SP.POP.TOTL            15925513
    708  ./data/importing/NLD.csv SP.POP.GROW   0.714770362784392
    709  ./data/importing/NOR.csv SP.URB.TOTL             3414033
    710  ./data/importing/NOR.csv SP.URB.GROW    1.24407427476763
    711  ./data/importing/NOR.csv SP.POP.TOTL             4490967
    712  ./data/importing/NOR.csv SP.POP.GROW   0.649044821192665
    713  ./data/importing/NPL.csv SP.URB.TOTL             3290236
    714  ./data/importing/NPL.csv SP.URB.GROW    5.82399866046924
    715  ./data/importing/NPL.csv SP.POP.TOTL            24559500
    716  ./data/importing/NPL.csv SP.POP.GROW    1.70977588565454
    717  ./data/importing/NRU.csv SP.URB.TOTL               10377
    718  ./data/importing/NRU.csv SP.URB.GROW -0.0578034698175499
    719  ./data/importing/NRU.csv SP.POP.TOTL               10377
    720  ./data/importing/NRU.csv SP.POP.GROW -0.0578034698175388
    721  ./data/importing/NZL.csv SP.URB.TOTL             3318432
    722  ./data/importing/NZL.csv SP.URB.GROW   0.680613208426653
    723  ./data/importing/NZL.csv SP.POP.TOTL             3857700
    724  ./data/importing/NZL.csv SP.POP.GROW   0.587564086381346
    725  ./data/importing/OED.csv SP.URB.TOTL           907363550
    726  ./data/importing/OED.csv SP.URB.GROW    1.04531648004067
    727  ./data/importing/OED.csv SP.POP.TOTL          1200179492
    728  ./data/importing/OED.csv SP.POP.GROW   0.726363968929135
    729  ./data/importing/OMN.csv SP.URB.TOTL             1677758
    730  ./data/importing/OMN.csv SP.URB.GROW      1.331482999273
    731  ./data/importing/OMN.csv SP.POP.TOTL             2344253
    732  ./data/importing/OMN.csv SP.POP.GROW    1.35943683310458
    733  ./data/importing/OSS.csv SP.URB.TOTL            10743131
    734  ./data/importing/OSS.csv SP.URB.GROW     3.0077552589358
    735  ./data/importing/OSS.csv SP.POP.TOTL            21437978
    736  ./data/importing/OSS.csv SP.POP.GROW    1.88388912656646
    737  ./data/importing/PAK.csv SP.URB.TOTL            50914288
    738  ./data/importing/PAK.csv SP.URB.GROW    3.68074020517277
    739  ./data/importing/PAK.csv SP.POP.TOTL           154369924
    740  ./data/importing/PAK.csv SP.POP.GROW    3.07555291166784
    741  ./data/importing/PAN.csv SP.URB.TOTL             1867017
    742  ./data/importing/PAN.csv SP.URB.GROW    3.18016218103103
    743  ./data/importing/PAN.csv SP.POP.TOTL             3001731
    744  ./data/importing/PAN.csv SP.POP.GROW    1.97188791278722
    745  ./data/importing/PER.csv SP.URB.TOTL            19468935
    746  ./data/importing/PER.csv SP.URB.GROW    2.08471804520449
    747  ./data/importing/PER.csv SP.POP.TOTL            26654439
    748  ./data/importing/PER.csv SP.POP.GROW    1.52044227070086
    749  ./data/importing/PHL.csv SP.URB.TOTL            35966026
    750  ./data/importing/PHL.csv SP.URB.GROW    2.03272225924168
    751  ./data/importing/PHL.csv SP.POP.TOTL            77958223
    752  ./data/importing/PHL.csv SP.POP.GROW    2.21679406368577
    753  ./data/importing/PLW.csv SP.URB.TOTL               13875
    754  ./data/importing/PLW.csv SP.URB.GROW     1.4226849525191
    755  ./data/importing/PLW.csv SP.POP.TOTL               19726
    756  ./data/importing/PLW.csv SP.POP.GROW    1.76959560691815
    757  ./data/importing/PNG.csv SP.URB.TOTL              727316
    758  ./data/importing/PNG.csv SP.URB.GROW    2.16539899567433
    759  ./data/importing/PNG.csv SP.POP.TOTL             5508297
    760  ./data/importing/PNG.csv SP.POP.GROW     3.4521329401203
    761  ./data/importing/POL.csv SP.URB.TOTL            23611695
    762  ./data/importing/POL.csv SP.URB.GROW  -0.973016273174752
    763  ./data/importing/POL.csv SP.POP.TOTL            38258629
    764  ./data/importing/POL.csv SP.POP.GROW   -1.04433539837844
    765  ./data/importing/PRE.csv SP.URB.TOTL           177859062
    766  ./data/importing/PRE.csv SP.URB.GROW    4.05197468592706
    767  ./data/importing/PRE.csv SP.POP.TOTL           553089230
    768  ./data/importing/PRE.csv SP.POP.GROW    2.82034676005263
    769  ./data/importing/PRI.csv SP.URB.TOTL             3596716
    770  ./data/importing/PRI.csv SP.URB.GROW   0.370913652828193
    771  ./data/importing/PRI.csv SP.POP.TOTL             3810605
    772  ./data/importing/PRI.csv SP.POP.GROW   0.276558688867414
    773  ./data/importing/PRK.csv SP.URB.TOTL            13882837
    774  ./data/importing/PRK.csv SP.URB.GROW   0.829486690742089
    775  ./data/importing/PRK.csv SP.POP.TOTL            23367059
    776  ./data/importing/PRK.csv SP.POP.GROW   0.698115634060685
    777  ./data/importing/PRT.csv SP.URB.TOTL             5597602
    778  ./data/importing/PRT.csv SP.URB.GROW    1.91610690404258
    779  ./data/importing/PRT.csv SP.POP.TOTL            10289898
    780  ./data/importing/PRT.csv SP.POP.GROW   0.702859953319024
    781  ./data/importing/PRY.csv SP.URB.TOTL             2835060
    782  ./data/importing/PRY.csv SP.URB.GROW      3.086699849536
    783  ./data/importing/PRY.csv SP.POP.TOTL             5123819
    784  ./data/importing/PRY.csv SP.POP.GROW    1.92694524157704
    785  ./data/importing/PSE.csv SP.URB.TOTL             2103044
    786  ./data/importing/PSE.csv SP.URB.GROW    2.86415437260134
    787  ./data/importing/PSE.csv SP.POP.TOTL             2922153
    788  ./data/importing/PSE.csv SP.POP.GROW    2.55523569844846
    789  ./data/importing/PSS.csv SP.URB.TOTL              701485
    790  ./data/importing/PSS.csv SP.URB.GROW    2.20693011315124
    791  ./data/importing/PSS.csv SP.POP.TOTL             2035672
    792  ./data/importing/PSS.csv SP.POP.GROW    1.46191499849726
    793  ./data/importing/PST.csv SP.URB.TOTL           780442277
    794  ./data/importing/PST.csv SP.URB.GROW   0.785471978025228
    795  ./data/importing/PST.csv SP.POP.TOTL          1020793420
    796  ./data/importing/PST.csv SP.POP.GROW    0.50042197994982
    797  ./data/importing/PYF.csv SP.URB.TOTL              140637
    798  ./data/importing/PYF.csv SP.URB.GROW    1.50879753546036
    799  ./data/importing/PYF.csv SP.POP.TOTL              250927
    800  ./data/importing/PYF.csv SP.POP.GROW     1.6498682624854
    801  ./data/importing/QAT.csv SP.URB.TOTL              622108
    802  ./data/importing/QAT.csv SP.URB.GROW    5.39848881165345
    803  ./data/importing/QAT.csv SP.POP.TOTL              645937
    804  ./data/importing/QAT.csv SP.POP.GROW    5.18445021336724
    805  ./data/importing/ROU.csv SP.URB.TOTL            11895672
    806  ./data/importing/ROU.csv SP.URB.GROW  -0.419565624342385
    807  ./data/importing/ROU.csv SP.POP.TOTL            22442971
    808  ./data/importing/ROU.csv SP.POP.GROW  -0.129440039806255
    809  ./data/importing/RUS.csv SP.URB.TOTL           107528803
    810  ./data/importing/RUS.csv SP.URB.GROW  -0.426068727524834
    811  ./data/importing/RUS.csv SP.POP.TOTL           146596869
    812  ./data/importing/RUS.csv SP.POP.GROW  -0.420614990249978
    813  ./data/importing/RWA.csv SP.URB.TOTL             1210497
    814  ./data/importing/RWA.csv SP.URB.GROW    7.19439451604905
    815  ./data/importing/RWA.csv SP.POP.TOTL             8109989
    816  ./data/importing/RWA.csv SP.POP.GROW    1.24573125745531
    817  ./data/importing/SAS.csv SP.URB.TOTL           385843630
    818  ./data/importing/SAS.csv SP.URB.GROW    2.85881008890094
    819  ./data/importing/SAS.csv SP.POP.TOTL          1406946728
    820  ./data/importing/SAS.csv SP.POP.GROW    1.96243373580489
    821  ./data/importing/SAU.csv SP.URB.TOTL            17205160
    822  ./data/importing/SAU.csv SP.URB.GROW     2.8182109540427
    823  ./data/importing/SAU.csv SP.POP.TOTL            21547390
    824  ./data/importing/SAU.csv SP.POP.GROW    2.52723635699494
    825  ./data/importing/SDN.csv SP.URB.TOTL             8545786
    826  ./data/importing/SDN.csv SP.URB.GROW    2.72286675787171
    827  ./data/importing/SDN.csv SP.POP.TOTL            26298773
    828  ./data/importing/SDN.csv SP.POP.GROW    2.55963690837227
    829  ./data/importing/SEN.csv SP.URB.TOTL             3912769
    830  ./data/importing/SEN.csv SP.URB.GROW    2.70878831646509
    831  ./data/importing/SEN.csv SP.POP.TOTL             9704287
    832  ./data/importing/SEN.csv SP.POP.GROW    2.35349186373024
    833  ./data/importing/SGP.csv SP.URB.TOTL             4027887
    834  ./data/importing/SGP.csv SP.URB.GROW    1.73204223254257
    835  ./data/importing/SGP.csv SP.POP.TOTL             4027887
    836  ./data/importing/SGP.csv SP.POP.GROW    1.73204223254257
    837  ./data/importing/SLB.csv SP.URB.TOTL               67992
    838  ./data/importing/SLB.csv SP.URB.GROW    4.55567808839687
    839  ./data/importing/SLB.csv SP.POP.TOTL              429978
    840  ./data/importing/SLB.csv SP.POP.GROW    2.53167345846883
    841  ./data/importing/SLE.csv SP.URB.TOTL             1633120
    842  ./data/importing/SLE.csv SP.URB.GROW    3.08641814084455
    843  ./data/importing/SLE.csv SP.POP.TOTL             4584067
    844  ./data/importing/SLE.csv SP.POP.GROW    2.40478419249569
    845  ./data/importing/SLV.csv SP.URB.TOTL             3510261
    846  ./data/importing/SLV.csv SP.URB.GROW    1.52944395884384
    847  ./data/importing/SLV.csv SP.POP.TOTL             5958482
    848  ./data/importing/SLV.csv SP.POP.GROW   0.582883767568532
    849  ./data/importing/SMR.csv SP.URB.TOTL               25063
    850  ./data/importing/SMR.csv SP.URB.GROW    1.92154797473259
    851  ./data/importing/SMR.csv SP.POP.TOTL               26823
    852  ./data/importing/SMR.csv SP.POP.GROW    1.57442145947664
    853  ./data/importing/SOM.csv SP.URB.TOTL             2899625
    854  ./data/importing/SOM.csv SP.URB.GROW    5.05656066455098
    855  ./data/importing/SOM.csv SP.POP.TOTL             8721465
    856  ./data/importing/SOM.csv SP.POP.GROW    3.94049698165867
    857  ./data/importing/SRB.csv SP.URB.TOTL             3966301
    858  ./data/importing/SRB.csv SP.URB.GROW   0.105872502528175
    859  ./data/importing/SRB.csv SP.POP.TOTL             7516346
    860  ./data/importing/SRB.csv SP.POP.GROW  -0.319524801286905
    861  ./data/importing/SSA.csv SP.URB.TOTL           210783626
    862  ./data/importing/SSA.csv SP.URB.GROW    3.84742064131463
    863  ./data/importing/SSA.csv SP.POP.TOTL           671131355
    864  ./data/importing/SSA.csv SP.POP.GROW    2.65041731976174
    865  ./data/importing/SSD.csv SP.URB.TOTL             1009127
    866  ./data/importing/SSD.csv SP.URB.GROW     5.1972225048576
    867  ./data/importing/SSD.csv SP.POP.TOTL             6114440
    868  ./data/importing/SSD.csv SP.POP.GROW     4.4186739597559
    869  ./data/importing/SSF.csv SP.URB.TOTL           210824543
    870  ./data/importing/SSF.csv SP.URB.GROW    3.84691171077253
    871  ./data/importing/SSF.csv SP.POP.TOTL           671212486
    872  ./data/importing/SSF.csv SP.POP.GROW    2.65020165426746
    873  ./data/importing/SST.csv SP.URB.TOTL            14761838
    874  ./data/importing/SST.csv SP.URB.GROW    2.50959605762182
    875  ./data/importing/SST.csv SP.POP.TOTL            30055791
    876  ./data/importing/SST.csv SP.POP.GROW    1.59709265877478
    877  ./data/importing/STP.csv SP.URB.TOTL               76778
    878  ./data/importing/STP.csv SP.URB.GROW    3.14353175850983
    879  ./data/importing/STP.csv SP.POP.TOTL              143714
    880  ./data/importing/STP.csv SP.POP.GROW    1.33511835347824
    881  ./data/importing/SUR.csv SP.URB.TOTL              318265
    882  ./data/importing/SUR.csv SP.URB.GROW    1.90866252229937
    883  ./data/importing/SUR.csv SP.POP.TOTL              478998
    884  ./data/importing/SUR.csv SP.POP.GROW    1.79897337334452
    885  ./data/importing/SVK.csv SP.URB.TOTL             3030239
    886  ./data/importing/SVK.csv SP.URB.GROW   -0.24377578251167
    887  ./data/importing/SVK.csv SP.POP.TOTL             5388720
    888  ./data/importing/SVK.csv SP.POP.GROW  -0.135376487794425
    889  ./data/importing/SVN.csv SP.URB.TOTL             1009459
    890  ./data/importing/SVN.csv SP.URB.GROW   0.347322840072504
    891  ./data/importing/SVN.csv SP.POP.TOTL             1988925
    892  ./data/importing/SVN.csv SP.POP.GROW    0.29607496005045
    893  ./data/importing/SWE.csv SP.URB.TOTL             7454878
    894  ./data/importing/SWE.csv SP.URB.GROW   0.193899954777834
    895  ./data/importing/SWE.csv SP.POP.TOTL             8872109
    896  ./data/importing/SWE.csv SP.POP.GROW   0.160575484575313
    897  ./data/importing/SWZ.csv SP.URB.TOTL              233778
    898  ./data/importing/SWZ.csv SP.URB.GROW   0.607975030947652
    899  ./data/importing/SWZ.csv SP.POP.TOTL             1030496
    900  ./data/importing/SWZ.csv SP.POP.GROW    1.18369298761011
    901  ./data/importing/SXM.csv SP.URB.TOTL               30519
    902  ./data/importing/SXM.csv SP.URB.GROW   -1.83437768672748
    903  ./data/importing/SXM.csv SP.POP.TOTL               30519
    904  ./data/importing/SXM.csv SP.POP.GROW   -1.83437768672749
    905  ./data/importing/SYC.csv SP.URB.TOTL               40917
    906  ./data/importing/SYC.csv SP.URB.GROW    1.28148544485373
    907  ./data/importing/SYC.csv SP.POP.TOTL               81131
    908  ./data/importing/SYC.csv SP.POP.GROW 0.89265856676617406
    909  ./data/importing/SYR.csv SP.URB.TOTL             8471337
    910  ./data/importing/SYR.csv SP.URB.GROW    3.23687225720868
    911  ./data/importing/SYR.csv SP.POP.TOTL            16307654
    912  ./data/importing/SYR.csv SP.POP.GROW    2.52399271863257
    913  ./data/importing/TCA.csv SP.URB.TOTL               15848
    914  ./data/importing/TCA.csv SP.URB.GROW     5.1466002517883
    915  ./data/importing/TCA.csv SP.POP.TOTL               18744
    916  ./data/importing/TCA.csv SP.POP.GROW     4.1391227494214
    917  ./data/importing/TCD.csv SP.URB.TOTL             1787029
    918  ./data/importing/TCD.csv SP.URB.GROW    3.56712410956491
    919  ./data/importing/TCD.csv SP.POP.TOTL             8259137
    920  ./data/importing/TCD.csv SP.POP.GROW    3.41450024149477
    921  ./data/importing/TEA.csv SP.URB.TOTL           650945325
    922  ./data/importing/TEA.csv SP.URB.GROW    3.65378531016158
    923  ./data/importing/TEA.csv SP.POP.TOTL          1793649873
    924  ./data/importing/TEA.csv SP.POP.GROW    1.00406124764849
    925  ./data/importing/TEC.csv SP.URB.TOTL           275974565
    926  ./data/importing/TEC.csv SP.URB.GROW  0.0683359303701394
    927  ./data/importing/TEC.csv SP.POP.TOTL           435816101
    928  ./data/importing/TEC.csv SP.POP.GROW -0.0860993080108301
    929  ./data/importing/TGO.csv SP.URB.TOTL             1647994
    930  ./data/importing/TGO.csv SP.URB.GROW     4.2049137134671
    931  ./data/importing/TGO.csv SP.POP.TOTL             5008035
    932  ./data/importing/TGO.csv SP.POP.GROW     2.8372567506566
    933  ./data/importing/THA.csv SP.URB.TOTL            19794084
    934  ./data/importing/THA.csv SP.URB.GROW     2.3318014212256
    935  ./data/importing/THA.csv SP.POP.TOTL            63066603
    936  ./data/importing/THA.csv SP.POP.GROW   0.994280693095973
    937  ./data/importing/TJK.csv SP.URB.TOTL             1662407
    938  ./data/importing/TJK.csv SP.URB.GROW   0.366344815044205
    939  ./data/importing/TJK.csv SP.POP.TOTL             6272998
    940  ./data/importing/TJK.csv SP.POP.GROW    1.33895806972214
    941  ./data/importing/TKM.csv SP.URB.TOTL             2097826
    942  ./data/importing/TKM.csv SP.URB.GROW     1.9897034243335
    943  ./data/importing/TKM.csv SP.POP.TOTL             4569132
    944  ./data/importing/TKM.csv SP.POP.GROW    1.50061066998589
    945  ./data/importing/TLA.csv SP.URB.TOTL           380881145
    946  ./data/importing/TLA.csv SP.URB.GROW    2.13185638871187
    947  ./data/importing/TLA.csv SP.POP.TOTL           505304844
    948  ./data/importing/TLA.csv SP.POP.GROW    1.50987572037849
    949  ./data/importing/TLS.csv SP.URB.TOTL              213116
    950  ./data/importing/TLS.csv SP.URB.GROW    2.80748909258887
    951  ./data/importing/TLS.csv SP.POP.TOTL              878360
    952  ./data/importing/TLS.csv SP.POP.GROW    1.34224818594807
    953  ./data/importing/TMN.csv SP.URB.TOTL           154878632
    954  ./data/importing/TMN.csv SP.URB.GROW    2.66440207287941
    955  ./data/importing/TMN.csv SP.POP.TOTL           280976957
    956  ./data/importing/TMN.csv SP.POP.GROW    1.98488554415559
    957  ./data/importing/TON.csv SP.URB.TOTL               23611
    958  ./data/importing/TON.csv SP.URB.GROW   0.726874042601457
    959  ./data/importing/TON.csv SP.POP.TOTL              102603
    960  ./data/importing/TON.csv SP.POP.GROW   0.607084495200795
    961  ./data/importing/TSA.csv SP.URB.TOTL           385843630
    962  ./data/importing/TSA.csv SP.URB.GROW    2.85881008890094
    963  ./data/importing/TSA.csv SP.POP.TOTL          1406946728
    964  ./data/importing/TSA.csv SP.POP.GROW    1.96243373580489
    965  ./data/importing/TSS.csv SP.URB.TOTL           210824543
    966  ./data/importing/TSS.csv SP.URB.GROW    3.84691171077253
    967  ./data/importing/TSS.csv SP.POP.TOTL           671212486
    968  ./data/importing/TSS.csv SP.POP.GROW    2.65020165426746
    969  ./data/importing/TTO.csv SP.URB.TOTL              744768
    970  ./data/importing/TTO.csv SP.URB.GROW   0.626582981569608
    971  ./data/importing/TTO.csv SP.POP.TOTL             1332203
    972  ./data/importing/TTO.csv SP.POP.GROW   0.386573317672307
    973  ./data/importing/TUN.csv SP.URB.TOTL             6275528
    974  ./data/importing/TUN.csv SP.URB.GROW    1.68467997407744
    975  ./data/importing/TUN.csv SP.POP.TOTL             9893316
    976  ./data/importing/TUN.csv SP.POP.GROW    1.06953869345051
    977  ./data/importing/TUR.csv SP.URB.TOTL            41507751
    978  ./data/importing/TUR.csv SP.URB.GROW    2.26122886011657
    979  ./data/importing/TUR.csv SP.POP.TOTL            64113547
    980  ./data/importing/TUR.csv SP.POP.GROW    1.45790187647314
    981  ./data/importing/TUV.csv SP.URB.TOTL                4435
    982  ./data/importing/TUV.csv SP.URB.GROW   0.860512556286972
    983  ./data/importing/TUV.csv SP.POP.TOTL                9638
    984  ./data/importing/TUV.csv SP.POP.GROW -0.0207490404313245
    985  ./data/importing/TZA.csv SP.URB.TOTL             7688508
    986  ./data/importing/TZA.csv SP.URB.GROW    4.47278383914827
    987  ./data/importing/TZA.csv SP.POP.TOTL            34463704
    988  ./data/importing/TZA.csv SP.POP.GROW    2.83680794271469
    989  ./data/importing/UGA.csv SP.URB.TOTL             3551700
    990  ./data/importing/UGA.csv SP.URB.GROW    5.92654401983794
    991  ./data/importing/UGA.csv SP.POP.TOTL            24020697
    992  ./data/importing/UGA.csv SP.POP.GROW    3.13535567369772
    993  ./data/importing/UKR.csv SP.URB.TOTL            33019561
    994  ./data/importing/UKR.csv SP.URB.GROW  -0.948477352408885
    995  ./data/importing/UKR.csv SP.POP.TOTL            49176500
    996  ./data/importing/UKR.csv SP.POP.GROW   -1.00657902702927
    997  ./data/importing/UMC.csv SP.URB.TOTL          1063986983
    998  ./data/importing/UMC.csv SP.URB.GROW    2.49977831743374
    999  ./data/importing/UMC.csv SP.POP.TOTL          2158201213
    1000 ./data/importing/UMC.csv SP.POP.GROW    0.89271112761358
    1001 ./data/importing/URY.csv SP.URB.TOTL             3029768
    1002 ./data/importing/URY.csv SP.URB.GROW   0.713783964461946
    1003 ./data/importing/URY.csv SP.POP.TOTL             3292224
    1004 ./data/importing/URY.csv SP.POP.GROW   0.403611037153845
    1005 ./data/importing/USA.csv SP.URB.TOTL           223069137
    1006 ./data/importing/USA.csv SP.URB.GROW    1.51201139138713
    1007 ./data/importing/USA.csv SP.POP.TOTL           282162411
    1008 ./data/importing/USA.csv SP.POP.GROW    1.11276899679534
    1009 ./data/importing/UZB.csv SP.URB.TOTL            11370244
    1010 ./data/importing/UZB.csv SP.URB.GROW    2.43420878873601
    1011 ./data/importing/UZB.csv SP.POP.TOTL            24650400
    1012 ./data/importing/UZB.csv SP.POP.GROW    1.38374682096563
    1013 ./data/importing/VCT.csv SP.URB.TOTL               51428
    1014 ./data/importing/VCT.csv SP.URB.GROW   0.647655496271435
    1015 ./data/importing/VCT.csv SP.POP.TOTL              113813
    1016 ./data/importing/VCT.csv SP.POP.GROW  -0.159783711477875
    1017 ./data/importing/VEN.csv SP.URB.TOTL            21388675
    1018 ./data/importing/VEN.csv SP.URB.GROW    2.24404422397847
    1019 ./data/importing/VEN.csv SP.POP.TOTL            24427729
    1020 ./data/importing/VEN.csv SP.POP.GROW     1.9042706267462
    1021 ./data/importing/VGB.csv SP.URB.TOTL                8398
    1022 ./data/importing/VGB.csv SP.URB.GROW    3.26786914258755
    1023 ./data/importing/VGB.csv SP.POP.TOTL               20104
    1024 ./data/importing/VGB.csv SP.POP.GROW    2.61037749426884
    1025 ./data/importing/VIR.csv SP.URB.TOTL              100587
    1026 ./data/importing/VIR.csv SP.URB.GROW   0.426410138693815
    1027 ./data/importing/VIR.csv SP.POP.TOTL              108642
    1028 ./data/importing/VIR.csv SP.POP.GROW  0.0395873712251061
    1029 ./data/importing/VNM.csv SP.URB.TOTL            19255738
    1030 ./data/importing/VNM.csv SP.URB.GROW    3.42860197272881
    1031 ./data/importing/VNM.csv SP.POP.TOTL            79001142
    1032 ./data/importing/VNM.csv SP.POP.GROW    1.11686737378671
    1033 ./data/importing/VUT.csv SP.URB.TOTL               41628
    1034 ./data/importing/VUT.csv SP.URB.GROW    3.79704140998283
    1035 ./data/importing/VUT.csv SP.POP.TOTL              192074
    1036 ./data/importing/VUT.csv SP.POP.GROW    2.44646014761845
    1037 ./data/importing/WLD.csv SP.URB.TOTL          2866001986
    1038 ./data/importing/WLD.csv SP.URB.GROW    2.18773892428798
    1039 ./data/importing/WLD.csv SP.POP.TOTL          6144322697
    1040 ./data/importing/WLD.csv SP.POP.GROW    1.35330175380903
    1041 ./data/importing/WSM.csv SP.URB.TOTL               40439
    1042 ./data/importing/WSM.csv SP.URB.GROW      1.394479630087
    1043 ./data/importing/WSM.csv SP.POP.TOTL              184008
    1044 ./data/importing/WSM.csv SP.POP.GROW   0.981387870498847
    1045 ./data/importing/XKX.csv SP.URB.TOTL                <NA>
    1046 ./data/importing/XKX.csv SP.URB.GROW                <NA>
    1047 ./data/importing/XKX.csv SP.POP.TOTL             1700000
    1048 ./data/importing/XKX.csv SP.POP.GROW   -3.58212764518173
    1049 ./data/importing/YEM.csv SP.URB.TOTL             4893201
    1050 ./data/importing/YEM.csv SP.URB.GROW    4.77890817937106
    1051 ./data/importing/YEM.csv SP.POP.TOTL            18628700
    1052 ./data/importing/YEM.csv SP.POP.GROW    2.79878090986914
    1053 ./data/importing/ZAF.csv SP.URB.TOTL            26632535
    1054 ./data/importing/ZAF.csv SP.URB.GROW    1.81016232925667
    1055 ./data/importing/ZAF.csv SP.POP.TOTL            46813266
    1056 ./data/importing/ZAF.csv SP.POP.GROW   0.962864025570883
    1057 ./data/importing/ZMB.csv SP.URB.TOTL             3442313
    1058 ./data/importing/ZMB.csv SP.URB.GROW    1.46484423776558
    1059 ./data/importing/ZMB.csv SP.POP.TOTL             9891136
    1060 ./data/importing/ZMB.csv SP.POP.GROW    2.76660559103692
    1061 ./data/importing/ZWE.csv SP.URB.TOTL             3995150
    1062 ./data/importing/ZWE.csv SP.URB.GROW    2.22892978426192
    1063 ./data/importing/ZWE.csv SP.POP.TOTL            11834676
    1064 ./data/importing/ZWE.csv SP.POP.GROW     1.0039687523875
                       x2001                x2002                x2003
    1                  42025                42194                42277
    2      0.956373099407145    0.401335154395215    0.196517211141057
    3                  90691                91781                92701
    4       1.76875662143885     1.19471805545637    0.997395547284924
    5              119775502            124227507            128833965
    6       3.65537739213475      3.7169579134805     3.70808213997242
    7              412001885            422741118            433807484
    8       2.58996060035651     2.60659802563768       2.617764283814
    9                4364773              4674867              5061866
    10      1.15383861520503     6.86345297807244     7.95344768841497
    11              19688632             21000256             22645130
    12     0.742516834396313     6.44932147797304     7.54101896480183
    13              99624942            104163982            108909607
    14      4.56798570926105     4.55612812301563     4.55591741874845
    15             277160097            284952322            292977949
    16      2.79965352270915      2.8114526890211     2.81648064619036
    17               8686629              9189142              9722803
    18      5.62744231595919     5.62376227868434     5.64513817273351
    19              16941587             17516139             18124342
    20      3.28521723299089     3.33513160843876     3.41332121240757
    21               1298584              1327220              1354848
    22     0.710442617598151     2.18120890441126     2.06027418181457
    23               3060173              3051010              3039616
    24     -0.93847042771206   -0.299876697084691   -0.374149169291299
    25                 62432                64927                67408
    26      2.20572172056573     3.91855974083943     3.75001433875657
    27                 67820                70849                73907
    28      2.57337766489402     4.36937150135481      4.2256694355319
    29             156476716            160709287            165028788
    30      2.73856886490258     2.70492064774672     2.68777310921678
    31             293512340            300018650            306573087
    32      2.24560150077274     2.21670748153213     2.18467651927638
    33               2785983              2945695              3106888
    34      5.83791948495437     5.57440090696271     5.32767996564432
    35               3454198              3633655              3813443
    36      5.31707596147158     5.06487258360989     4.82934268529322
    37              33480950             33910889             34330154
    38      1.30873169770709     1.27595518811745     1.22879210827292
    39              37480493             37885028             38278164
    40       1.0991714603689     1.07353834961783     1.03236085902218
    41               2017268              1994552              1978050
    42     -1.55867624443429    -1.13246570248286    -0.83079527809499
    43               3133133              3105037              3084102
    44     -1.12320863167984   -0.900783081509756   -0.676510249442033
    45                 51611                51425                51160
    46    0.0523281178663842   -0.361039252380598   -0.516645882837897
    47                 58324                58177                57941
    48      0.16129866050102   -0.252358482783624   -0.406483620024703
    49                 24191                24006                23785
    50     0.322954901436329   -0.767686406077838   -0.924866921890812
    51                 76215                77195                78075
    52      1.53371156016655     1.27763943800851     1.13352150752522
    53              16210024             16419256             16633061
    54      1.12357871219199     1.28249760868533     1.29375488089444
    55              19274701             19495210             19720737
    56      1.28396809127349      1.1375387361946     1.15019273319182
    57               4820068              4821291              4822095
    58   -0.0816252765125744   0.0253698675997875   0.0166746410827647
    59               8042293              8081957              8121423
    60     0.382799394483217     0.49198046425522    0.487133894826838
    61               4184325              4232008              4280744
    62      1.16515638336764     1.13311843032047     1.14502413490342
    63               8111200              8171950              8234100
    64     0.774765939386613    0.746173582716985    0.757650955616913
    65                547065               577261               611164
    66      5.04888450132963     5.37268785774085     5.70708316726612
    67               6465729              6648938              6860846
    68      2.47511575866868     2.79413757787033     3.13736150828626
    69               9997106             10047703             10095562
    70     0.402615693084428    0.504840005818933    0.475187017156648
    71              10286570             10332785             10376133
    72     0.343951157623668    0.448268894821562    0.418641508069053
    73               2787454              2901071              3028144
    74       3.8360054170946     3.99513447510548     4.28699114281374
    75               7212041              7431783              7659208
    76       3.0124309700982     3.00138127284262     3.01427811282378
    77               2271106              2432722              2605597
    78      6.86702047633563     6.87438575377423     6.86510285676784
    79              12249764             12632269             13030591
    80      3.04072894648829     3.07479003510637     3.10451751268277
    81              31727320             33207655             34711400
    82      4.02154852620664     4.56022822596011      4.4287741624036
    83             131670484            134139826            136503206
    84      1.89925268099487     1.85802653007171     1.74653680101613
    85               5539603              5448708              5433321
    86     -1.60386362919759    -1.65443211908207   -0.282796714742491
    87               8009142              7837161              7775327
    88     -1.99063220438276    -2.17069878030004    -0.79211363763553
    89                645321               661316               687846
    90      2.60682230104421     2.44839217402731     3.93331865129447
    91                730257               748324               778256
    92       2.6102630692956     2.44395122899014     3.92194796267708
    93                270455               274189               278015
    94      1.46039106465837     1.37119237681649     1.38574194491706
    95                329626               334002               338490
    96      1.40904200942925     1.31883025634015       1.334756883574
    97               1791152              1805904              1812822
    98      1.11023425698696    0.820230916996715    0.382344970451888
    99               4194932              4198410              4183757
    100    0.372139796167984   0.0828752170247773   -0.349623550237269
    101              6995457              6998521              6996348
    102    0.177758874835104   0.0437902653407329  -0.0310542387708375
    103              9928549              9865548              9796749
    104   -0.512966688341309   -0.636565666436346   -0.699809190918934
    105               112590               116126               119715
    106     3.11214400569533     3.09229066920039     3.04381094890388
    107               248100               255987               263998
    108     3.15027299454963     3.12947712423959     3.08148654198932
    109                62504                62912                63325
    110     1.07933521651731    0.650636983025823    0.654327136303748
    111                62504                62912                63325
    112     1.07933521651731    0.650636983025823     0.65432713630377
    113              5449335              5588053              5728610
    114     2.60629658958734     2.51373430779305     2.48419924420098
    115              8746084              8900583              9057378
    116     1.76981723900868     1.75107228754869     1.74628939852943
    117            145337135            147774310            150126745
    118     1.76433391214426     1.66300648431476     1.57937265407228
    119            178211881            180476685            182629278
    120     1.32069475807143     1.26284153622856     1.18566943930002
    121                89244                89079                88901
    122   -0.315489437717616    -0.18505750473986   -0.200022541124515
    123               265377               266455               267499
    124    0.271680851742656    0.405391731352261    0.391045419782759
    125               244153               250333               256403
    126     2.70591949230629     2.49969535768787      2.3958393908868
    127               340748               347463               354045
    128     2.02237896105741     1.95149833621625     1.87658391830951
    129               159736               170672               182062
    130     6.78595000148985     6.62211333985306     6.46037025231088
    131               603234               619048               634627
    132     2.69277829428078     2.58776329516912     2.48546106253177
    133               952535               979602              1005304
    134     3.57493298542873     2.80195149929971     2.58988955896447
    135              1761930              1795130              1826863
    136      2.0032685597646     1.86676435735896     1.75228455517665
    137              1450248              1485864              1525528
    138     2.46658244274902     2.42618449553929     2.63441583980616
    139              3844773              3930648              4026841
    140     2.25163751502407     2.20897342817341     2.41778998181238
    141             24757782             25052940             25304780
    142     1.50320904037894     1.18513220773898     1.00021247806731
    143             31020902             31360079             31644028
    144     1.08635096562717     1.08744792718674    0.901372739095955
    145             66206030             65839890             65682785
    146    -0.69530504216263   -0.553031196705192   -0.238616741309855
    147            107660041            106959751            106624167
    148    -0.72641660380387   -0.650464177326484   -0.313747925609889
    149              5304905              5347082              5388808
    150    0.621865281288162    0.791912727997122    0.777321863401719
    151              7229854              7284753              7339001
    152    0.632771238018431    0.756469145838096     0.74191960337232
    153                44536                44970                45421
    154    0.605837241485205    0.969774990140949    0.997895238514865
    155               146044               147167               148341
    156    0.506608257016431    0.766005021042782    0.794568117674261
    157             13406973             13591764             13744299
    158     1.45166879283817      1.3689076320046     1.11600988887849
    159             15523978             15693790             15859112
    160     1.11531304820862     1.08792963607244     1.04791320216262
    161            471767321            491993700            512473984
    162     4.05956575767596     4.19800114764631     4.07840352649768
    163           1271850000           1280400000           1288400000
    164    0.726380637838525    0.669999567758626    0.622860936133583
    165              7513505              7777908              8043523
    166     3.57146681501481     3.45853394778769     3.35797638657461
    167             17245468             17683897             18116451
    168     2.61901409623771     2.51050671514096     2.41659690314912
    169              7148432              7437901              7741811
    170     3.92903028631021     3.96956534280202     4.00469541761859
    171             15493253             15914033             16354326
    172     2.62667387658374     2.67966597627532     2.72911509031776
    173             17831456             18626760             19433892
    174     4.33473362099478     4.36351677809616     4.24192956856511
    175             50106657             51662071             53205639
    176     3.01946582164363     3.05700023925831     2.94405110630979
    177              1924996              1985936              2057395
    178     4.54197654124402     3.11664497091615     3.53502799353932
    179              3254101              3331158              3424653
    180     3.75963307173171     2.34039442531377     2.76801614231128
    181             29631013             30258416             30879897
    182     2.14451363772357     2.09528113345893     2.03310290004829
    183             39837875             40454050             41057687
    184     1.57553237214944     1.53486691614177     1.48113157054236
    185               153565               156488               159334
    186     1.86868497860821     1.88553995588983      1.8023298190953
    187               547741               559047               570130
    188     2.02552063213452     2.04310008455716     1.96308566673178
    189               252973               260968               268835
    190     3.25716415213695     3.11150307770424     2.97000140639019
    191               465958               473231               480089
    192     1.66784323760593     1.54881400616773     1.43878620109395
    193              2448551              2545843              2642175
    194     4.11691818523769     3.89654118540643     3.71406087288222
    195              4053222              4122623              4188610
    196     1.84330854695316     1.69774908835068     1.58793253810516
    197              3344658              3369635              3393441
    198    0.827077596856654    0.746772913702969    0.706486014063842
    199              6626803              6671842              6716747
    200    0.678533018359829    0.679648995148938    0.673052509336998
    201              8416167              8465223              8505469
    202    0.607246251474507    0.581186005219123    0.474300935380461
    203             11139127             11170051             11199217
    204    0.299718119482692    0.277231369135209    0.260768627095581
    205               117447               117440               119730
    206     -3.3770703956836 -0.00596031283304224     1.93116428625467
    207               129047               129205               131897
    208     -3.6617798581649    0.122361119924304     2.06210246711066
    209                41054                42456                43862
    210     3.45955801183733     3.35799722306069     3.25800960597195
    211                41054                42456                43862
    212     3.45955801183736     3.35799722306069     3.25800960597195
    213               663504               674708               685770
    214     1.91081929307819     1.67451222946572     1.62622898552981
    215               964830               982194              1000350
    216     1.73474490066302     1.78369248341346     1.83163728954308
    217              7547721              7526242              7517054
    218   -0.525860172574256    -0.28498165395158   -0.122154096316381
    219             10216605             10196916             10193998
    220   -0.375719704335905   -0.192901617564167   -0.028620590719166
    221             61902439             62174878             62376854
    222    0.441313650311012    0.439144630807111    0.324324959275838
    223             82349925             82488495             82534176
    224    0.168225361337335    0.168128319406931   0.0553633035873241
    225               586197               604662               618275
    226     3.17228898627621      3.1013713669934     2.22637180741589
    227               765490               789129               806411
    228     3.11224350831744     3.04136539252469     2.16637325167082
    229                44560                44841                45168
    230   -0.103178350673752    0.628630385275869    0.726597203934633
    231                68153                68262                68442
    232   -0.282786142924393    0.159806506904842    0.263342848818132
    233              4563004              4582981              4601394
    234    0.417063623089164    0.436848120119594    0.400964130257149
    235              5358783              5375931              5390574
    236    0.358315678955486    0.319487125340996    0.272010444031566
    237              5423785              5573543              5762670
    238     2.79678501279131      2.7237026688676     3.33699710741464
    239              8669040              8795101              8919852
    240     1.49044315561471      1.4436803902991     1.40844926048238
    241             18942742             19449504             19965686
    242     2.69070377933956     2.64007182073424     2.61935316011717
    243             31200985             31624696             32055883
    244     1.37593080261361     1.34886695233422     1.35423891111813
    245            690009240            716688851            743711842
    246     3.78117449414337     3.86655851159325     3.77053319056027
    247           1834160201           1850531105           1866298799
    248    0.940825893557772     0.89255584060075    0.852063170264856
    249            983588177           1010287668           1036921694
    250     2.72378566589917     2.71449897673995     2.63628141207796
    251           2545843129           2591204247           2635731526
    252     1.86603828973797     1.78177192000899     1.71840097327534
    253            876880694            906856237            936864583
    254     3.31267321461152     3.41842889290479     3.30905217118776
    255           2066530023           2083969920           2100610344
    256    0.897460571876366    0.843921782209691    0.798496362173978
    257            238586715            239168943            239983417
    258    0.212886957425056    0.244032028354965    0.340543378995491
    259            370739184            370807684            371072025
    260   0.0250872665768185   0.0184766010597741    0.071287897043689
    261            593846263            596658392            600019903
    262    0.356244348715222     0.47354495181861    0.563389545017912
    263            863909133            865474170            867701442
    264    0.130150666026353     0.18115759403598    0.257347021691004
    265              7809948              7988495              8155754
    266     2.54546973479341     2.26040758867352     2.07213088277276
    267             12845521             13070609             13301184
    268     1.71968558942167     1.73709315382378      1.7486932014909
    269             31212951             31906739             32614852
    270     2.16384500441365     2.19841365576193     2.19505260500755
    271             72854261             74393759             75963322
    272      2.0564198428314     2.09110329331686     2.08785646198219
    273            235945597            237890076            240000854
    274    0.667463121368499    0.824121757186248    0.887291321896086
    275            322562386            324141533            325904835
    276    0.385206708498046    0.489563280946228    0.543991380456646
    277               675922               721965               775957
    278     6.05725646498872     6.58989764932487     7.21204450132512
    279              2461927              2547424              2653390
    280     2.84467152232301     3.41382758677749     4.07554152440981
    281             31186430             31708814             32390830
    282    0.800225838968891      1.6611622094209     2.12806716477212
    283             40850412             41431558             42187645
    284    0.694068084334464     1.41259540475782     1.80845446818345
    285               961159               953352               945646
    286   -0.818771346779428   -0.815565256432265   -0.811590354293914
    287              1388115              1379350              1370720
    288    -0.63696312445507   -0.633433796423808   -0.627622448059203
    289             10302456             10744849             11202149
    290     4.18194992753438     4.20441635341404     4.16791579475958
    291             69018932             71073215             73168838
    292     2.92127105923873     2.93296991314103     2.90590770385319
    293            305289150            306973917            309030225
    294    0.395928852687405    0.551859442105965    0.669864078386823
    295            429910140            430898141            432434810
    296    0.132221957877434    0.229815700555463    0.356620011502898
    297            234229073            241663006            249479349
    298     3.04618993904543     3.17378748282029     3.23439782090603
    299            626177896            641576379            657778626
    300     2.25056623494714     2.45912273466773     2.52538084791306
    301              4273258              4290649              4307931
    302    0.452527984505473    0.406146958535345    0.401973890354926
    303              5188008              5200598              5213014
    304    0.227687341886374    0.242381050233372    0.238457240068217
    305               406358               413829               421300
    306     1.86792250611192     1.82183005541335     1.78923239638711
    307               841320               849891               858306
    308     1.05280543999033     1.01360189605196    0.985257329508455
    309             46717151             47215240             47708761
    310     1.06627720582144     1.06053654800967     1.03983279598815
    311             61367388             61816234             62256970
    312    0.729430789508821    0.728746206873967    0.710448089652969
    313                17192                17799                18347
    314     3.55837796789911     3.46981166044066     3.03237974519666
    315                46245                46813                47392
    316     1.27307089045739     1.22075921220517     1.22924959571133
    317                24998                25008                24966
    318    0.212242035502941   0.0399952011090631   -0.168087445026153
    319               111948               111992               111805
    320    0.213720197420152   0.0392962405699845   -0.167115773241494
    321              1040477              1078294              1117646
    322     3.56095543549978     3.57009007263004      3.5844525165144
    323              1306590              1341696              1378398
    324     2.60954321589185     2.65137953739362     2.69874699554435
    325             46557334             46930583             47323791
    326    0.512040255282785    0.798501015283694    0.834359753141167
    327             59119673             59370479             59647577
    328    0.384975969564228    0.423337079636281    0.465641113602993
    329              2103451              2086373              2087702
    330     -2.0082231364141   -0.815217755242416   0.0636787812825021
    331              4014373              3978515              3951736
    332    -1.55123839698929    -0.89725367267161   -0.675365805865971
    333              9007429              9398332              9801125
    334      4.1779283134726      4.2482545433143     4.19649482844463
    335             20195577             20758326             21329514
    336     2.65976868907588     2.74837991725016     2.71443282052613
    337                27721                27892                28301
    338  -0.0721214556522241    0.614966065167533     1.45572303551829
    339                27721                27892                28301
    340  -0.0721214556522241    0.614966065167533     1.45572303551829
    341              2630334              2695142              2780629
    342      2.1829228439361     2.43400588158008     3.12262679627876
    343              8445717              8577790              8772254
    344     1.29599678025488     1.55168555991374     2.24174813408231
    345               721601               756286               791821
    346     4.75076599940829     4.69472569610685     4.59157446514491
    347              1479449              1522223              1566257
    348     2.87370974027546     2.85020452877525     2.85169300452624
    349               460478               475752               491883
    350     3.17286632335731     3.26316296604027     3.33441732781763
    351              1257380              1285678              1315653
    352      2.1326015824877     2.22560146575472     2.30469146620396
    353               365619               396461               428852
    354     8.36803726991838     8.09858679863775      7.8534195479421
    355               719270               754115               789681
    356     4.88515477320107     4.73080674836867     4.60841901726829
    357              7919906              7991509              8052786
    358    0.790435678644777    0.900026595348849    0.763851549019637
    359             10862132             10902022             10928070
    360     0.51988446396193    0.366566462148184    0.238643175320258
    361                38770                38869                39045
    362     1.08661485902397    0.255026606877499    0.451780936302554
    363               107936               108231               108740
    364    0.468036960036568    0.272937296750141    0.469187987103342
    365                46122                46473                46739
    366    0.571858789801961    0.758143919728567    0.570743513870947
    367                56350                56609                56765
    368    0.266548358613988     0.45857426832132     0.27519554474986
    369              5421506              5588466              5750534
    370     3.14086887299519     3.03311942684078     2.85878891596839
    371             11871565             12147518             12415334
    372     2.40240090043075     2.29788241650457     2.18074524550455
    373               150594               151727               152659
    374    0.942714011869787    0.749537944364224    0.612382246615482
    375               161524               162563               163385
    376    0.830561297136285    0.641188051128184     0.50437600022459
    377               217351               216829               215292
    378   -0.207283489477126    -0.24045338408381   -0.711377823229739
    379               759809               760323               760562
    380   0.0998117091672832   0.0676257087659106   0.0314290695177204
    381            847957779            857542072            866916014
    382     1.05661946593369     1.13027950652247     1.09311744648721
    383           1108051953           1114863356           1121696635
    384    0.611046797617092    0.614718739636572    0.612925248930679
    385              6714300              6744100              6730800
    386    0.736962668138432     0.44284682728195   -0.197404126472125
    387              6714300              6744100              6730800
    388    0.736962668138432     0.44284682728195   -0.197404126472125
    389              3142954              3271979              3404113
    390     3.79168800847597     4.02318786170448     3.95894057728677
    391              6837861              7019908              7201881
    392     2.68473415634022     2.62751491094002      2.5592129363433
    393            141861935            147771882            154013600
    394     3.97087239965788     4.16598504736312     4.22388746459899
    395            488040621            502494432            517663749
    396     2.71773708996341     2.96159999353824     3.01880300237833
    397              2306414              2315000              2322932
    398    -3.44791359884732    0.371575014866115    0.342049332916028
    399              4299642              4302174              4303399
    400     -3.8476707266781   0.0588712912047699   0.0284699231131846
    401              3170704              3372373              3581737
    402     6.32928863710659     6.16630056150639     6.02312274074243
    403              8511728              8661546              8812245
    404     1.79596365914658       1.744825137965     1.72490029683628
    405              6588305              6611324              6633742
    406  -0.0823848252186118    0.348782883752427    0.338511282726665
    407             10187576             10158608             10129552
    408   -0.229379183308447    -0.28475137719698   -0.286433268128688
    409           1786838564           1833372562           1880578389
    410     2.54555613550775     2.60426425405937     2.57480819656774
    411           4046339602           4092406561           4137913546
    412     1.18023947603211     1.13848474253695     1.11198592617055
    413           2116234292           2175866342           2236475647
    414     2.76235276503847     2.81783780866924     2.78552518737385
    415           5168697790           5243235123           5317497279
    416     1.46824041274225     1.44209114226427     1.41634228215786
    417            329395728            342493780            355897258
    418     3.95454563572375     3.97638793906884     3.91349530493663
    419           1122358188           1150828562           1179583733
    420     2.52029651852308     2.53665668450577     2.49864940352427
    421            130849818            135879840            140867968
    422      4.1021557267308     3.84411845341657     3.67098459933423
    423            381142622            390704440            399995493
    424     2.75858835916853      2.5087244113045     2.37802595742194
    425             92887214             95899743             98949418
    426     3.25245836016092     3.19172975254904     3.13054882618811
    427            217112437            220115092            223080121
    428      1.4100988547982     1.37351928060869     1.33804389134874
    429            198545910            206613940            215029290
    430     3.85749322245934     4.06355890181771     4.07298268451781
    431            741215566            760124122            779588240
    432     2.39819352160295     2.55101982032579     2.56064995658696
    433                39599                40020                40426
    434     1.11991215812507     1.05754637392555     1.00938131692189
    435                76398                77183                77939
    436     1.10030065051098     1.02227077083137    0.974724391824046
    437            301227098            310207535            319267849
    438     2.71157391943668     2.93770825491266     2.87888612843243
    439           1078970907           1098313039           1117415123
    440     1.80844642123748     1.77676787418134     1.72426903226703
    441                 <NA>                 <NA>                 <NA>
    442                 <NA>                 <NA>                 <NA>
    443                 <NA>                 <NA>                 <NA>
    444                 <NA>                 <NA>                 <NA>
    445              2296510              2345367              2394915
    446     2.00377856420569      2.1051313624531     2.09058467764551
    447              3866243              3931947              3996521
    448     1.59215149039299     1.68514885730282     1.62895096193019
    449             43177300             44077717             44966983
    450     2.82184401269272     2.06394771243319      1.9974138054089
    451             66674851             67327117             67954699
    452      1.7100319119575    0.973524734407785    0.927820930669199
    453             17429292             18012216             18584912
    454     3.26282056809803     3.28979552871636     3.12998743639086
    455             25425663             26255343             27068823
    456     3.18401734533544      3.2110492891941     3.05131116311828
    457               263687               266425               268644
    458     1.47121309326768       1.032998440819    0.829430492672495
    459               284968               287523               289521
    460     1.32929525456819    0.892596377462957    0.692497620476186
    461              5876682              6000381              6113918
    462     2.42725785439453     2.08306502848675      1.8744844325013
    463              6439000              6570000              6689700
    464     2.35711729946802     2.01405839404727     1.80551975888109
    465             38333314             38447500             38686985
    466    0.145383965430874    0.297433898475445    0.620956423105583
    467             56974100             57059007             57313203
    468   0.0561676014374784    0.148916429490753    0.444507312655549
    469              1366602              1378509              1390119
    470    0.964240271979112    0.867511344659901    0.838687469284281
    471              2625405              2638244              2651027
    472    0.504047760220551    0.487837447460561    0.483356775927262
    473              4041994              4130531              4225591
    474     2.11339604260065     2.16678355702441     2.27531625465002
    475              5163310              5275532              5396117
    476     2.09677758965608      2.1501680651457     2.26000934810202
    477            101706485            104055019            106256267
    478     1.93162375891114     2.28287211011819     2.09340014933777
    479            127149000            127445000            127718000
    480    0.240952587521069    0.232527187105684    0.213980948779682
    481              8346075              8357267              8396312
    482  -0.0400347596181894     0.13400913032763     0.46611020475722
    483             14858335             14858948             14909019
    484   -0.170069526322763  0.00412554539450457    0.336408913793247
    485              6436071              6749693              7073537
    486     4.75820744129826     4.75787616026489     4.68636153255088
    487             31800343             32779823             33767122
    488     3.02882685482353     3.03360977756994     2.96744335273422
    489              1745422              1761468              1779932
    490    0.943175410303354    0.915119207043952     1.04276090284424
    491              4945100              4990700              5043300
    492    0.948856613551314    0.917899316903102      1.0484449238834
    493              2307612              2364127              2421609
    494     2.42133574360616     2.41955970099951     2.40233746644263
    495             12338192             12561779             12787710
    496     1.79381433398266     1.79592987916956     1.78257622928652
    497                39357                40187                41033
    498     3.09384154618839     2.08697104738101     2.08330607716413
    499                90531                92400                94302
    500     1.90129335529483     2.04346451592179     2.03754196746088
    501                14991                15015                15003
    502    0.602169623685447    0.159968040511599  -0.0799520330417284
    503                45986                46264                46431
    504     1.14821867970338    0.602711851610802    0.360321874281502
    505             37867709             38258247             38626122
    506     1.16708927807075   1.0260401227104201    0.956963846404653
    507             47370164             47644736             47892330
    508    0.767241774760276    0.577957276814842    0.518321452985646
    509              1989842              2047364              2101506
    510     3.80494068502559     2.84978734732264     2.61011184898608
    511              1991674              2047364              2101506
    512     2.89193287688669      2.7577620921541     2.61011184898608
    513            355715223            362317890            368798552
    514     1.93483319354979     1.85616655489609     1.78866740474781
    515            475577026            482114041            488449381
    516     1.42734382482688     1.37454389985609     1.31407498252058
    517              1269146              1347931              1429434
    518     6.14213900725156     6.02265909005981     5.87077374884571
    519              5519707              5606101              5689065
    520     1.62285673222063     1.55306898212099     1.46904422700425
    521              3779891              3834805              3890622
    522     1.71140929408291     1.44234128319211     1.44504559515846
    523              4389200              4446666              4504807
    524     1.57429758089142     1.30076237598121     1.29904469608094
    525              1331992              1377790              1399465
    526     3.70988690161441     3.38052001090116     1.56092545257143
    527              2981648              3060599              3085173
    528     2.94136849764656     2.61344806491648    0.799708513941131
    529              4036181              4142047              4254975
    530     2.47303407974439     2.58911622159408     2.68987764831276
    531              5275916              5405326              5542641
    532     2.32259340851935      2.4232450219976     2.50863381723218
    533                44120                42600                41114
    534   -0.407148266578819     -3.5058941109977    -3.55055571718435
    535               160594               161799               163047
    536    0.683551868949005    0.747538322118724    0.768367866645381
    537            401043709            408305152            415412489
    538     1.90225360146586       1.810636306478     1.74069246130895
    539            528688278            535935546            542976557
    540     1.42094702910502     1.37080171843719     1.31377943720122
    541            172249228            179649044            187402727
    542     4.06097525633989     4.29599370976572     4.31601684448708
    543            678096204            695718323            713914587
    544     2.42863922839362     2.59876384737881     2.61546424730285
    545            114205930            118653082            123289408
    546     3.63063039331652     3.89397643362301     3.90746360890988
    547            411561975            423857982            436823058
    548     2.67959422733979     2.98764408446625     3.05882549122313
    549                 4994                 5022                 5049
    550  -0.0600540504486441    0.559106887781182    0.536194314138537
    551                33376                33693                34000
    552     1.05419487775652    0.945302183064675    0.907042396715275
    553              3473139              3497774              3524535
    554    0.630087045681519    0.706797016569711    0.762174650750614
    555             18911727             19062476             19224036
    556    0.711721622429264    0.793958951013405    0.843957574114613
    557            855629549            880827966            906393615
    558     2.92300303866786     2.94501481738799     2.90245655074943
    559           2505434958           2550242901           2594524373
    560     1.86559116383076     1.78842970386941     1.73636291596524
    561           2062167338           2121574002           2181879692
    562      2.8244921781416     2.88078774720657      2.8424975958015
    563           5093407382           5167898454           5241904688
    564     1.49115650635171     1.46249978478552     1.43203730991885
    565               401234               411432               421267
    566     2.66252724769804     2.50989591135165     2.36230795620831
    567              1999473              1997534              1993030
    568   0.0421699997470092  -0.0970226047692506    -0.22573259882656
    569            958547615            984633804           1011141189
    570     2.65012241069601     2.72142860634001     2.69210592733215
    571           2060700691           2074533503           2088012323
    572    0.717165655629486    0.671267402413861    0.649727757132297
    573              2322637              2300933              2279211
    574   -0.924058759665489   -0.938848527101684   -0.948536240967633
    575              3470818              3443067              3415213
    576   -0.824008723044179   -0.802765710748508   -0.812278097406687
    577               374603               380583               387241
    578     1.93230878327534     1.58374890277717     1.73429506782531
    579               441525               446175               451630
    580     1.19045634528707     1.04766081596443     1.21520087964639
    581              1588107              1567452              1551233
    582    -1.46350920861011    -1.30913698611288    -1.04012724739849
    583              2337170              2310173              2287955
    584    -1.29148694354873     -1.1618382239436   -0.966400801237561
    585               439122               449665               462533
    586     1.65924602742688      2.3725582288621      2.8215044674889
    587               439122               449665               462533
    588     1.65924602742688      2.3725582288621      2.8215044674889
    589                 <NA>                 <NA>                 <NA>
    590                 <NA>                 <NA>                 <NA>
    591                30387                31160                31929
    592     2.59027432592699     2.51203356297776     2.43794667244693
    593             15524469             15820051             16111112
    594     1.91832848949412     1.88607617656353     1.82310341550727
    595             28930097             29301817             29661270
    596     1.30709067761953     1.27670558488657     1.21926266518875
    597                32444                32386                32316
    598  -0.0647059752351783   -0.178929555681925   -0.216376703381685
    599                32444                32386                32316
    600  -0.0647059752351783   -0.178929555681925   -0.216376703381685
    601              1289495              1274866              1259669
    602    -1.12471424560406    -1.14095938118688    -1.19920870085719
    603              2918135              2911385              2903198
    604   -0.223625637302563   -0.231580062051396   -0.281602495320467
    605              4576944              4761337              4951463
    606     3.98684466499167     3.94969846480372     3.91545773120084
    607             16709665             17211934             17724310
    608     2.99623070466098     2.96156859041379     2.93341631578179
    609                82919                87808                92889
    610     5.76870636558263     5.72883849851975     5.62526192566503
    611               287324               292284               297226
    612     1.69071698288354     1.71154334670149     1.67668598570671
    613            192854627            197697731            202561806
    614     2.71608582499883     2.51127187111773     2.46035954757619
    615            327691655            333949565            340215354
    616     2.07271828810578     1.90969464876973     1.87626805263244
    617             74590443             76056158             77520574
    618     1.97327835219045     1.94595993878443      1.9071383624649
    619             99394288            100917081            102429341
    620     1.54194113014521     1.52045522318023     1.48740053113001
    621                37600                37939                38216
    622     1.10448125585416    0.897555635722556    0.727466933458637
    623                54413                54496                54493
    624    0.347948103651383    0.152420857906842 -0.00550514272221927
    625           1947961408           2002920920           2058590284
    626      2.7776185871472     2.82138607953367     2.77940898435472
    627           4681845407           4744040472           4805081630
    628     1.38799990786079     1.32843055661363     1.28669134170067
    629              1187130              1174317              1173927
    630   0.0626075165477599    -1.08519274348444  -0.0332163100244931
    631              2034882              2020157              2026773
    632    0.420168685369998   -0.726260079023056    0.326964187374614
    633              3367997              3562610              3770161
    634      5.5251035053994     5.61752166677191     5.66242838072937
    635             11583824             11952660             12342165
    636     3.02107830973822     3.13442055916044     3.20676010440896
    637               364105               367875               371319
    638     1.04608633718595     1.03009189754651    0.931832487956391
    639               393028               395969               398582
    640     0.75110649818537    0.745506921127166    0.657732334391546
    641             12519714             12731400             12939374
    642     1.71579297197093     1.67668609954142     1.62035278213787
    643             46014826             46480230             46924293
    644      1.0409216084204     1.00634119882684     0.95084548671382
    645            161152792            165098686            169073058
    646     2.65707189927058     2.44854212640635     2.40727052182596
    647            289544085            294813993            300102895
    648     1.98837361624697      1.8200710264898      1.7939792973124
    649               361755               369361               376961
    650     2.12127515430801     2.08072984646732     2.03672482401899
    651               607389               609828               612267
    652    0.402363248322291    0.400750772330341    0.399151168806326
    653              1439598              1479408              1519385
    654     2.76645542616522      2.7278099807709     2.66636399655483
    655              2472601              2494617              2516454
    656    0.878309644537485    0.886457738285741     0.87155573049131
    657                71718                69690                67458
    658   -0.982358882314607    -2.86849266734714    -3.25516534547499
    659                79479                77162                74623
    660    -1.07498985987344    -2.95857279553887    -3.34583371838907
    661              5334479              5506970              5686378
    662     3.12643773416067     3.18233396506061     3.20589274371998
    663             18220716             18694946             19186754
    664     2.51316804647212     2.56940314170631     2.59669251607104
    665              1066506              1114008              1163451
    666     3.81803227567564     4.35764380153659     4.34262657552538
    667              2761823              2821703              2883326
    668      2.4491651403647     2.14496344480591     2.16038855352539
    669               509128               511325               513680
    670    0.529557633124118    0.430593745045558    0.459510762156298
    671              1196287              1204621              1213370
    672    0.790047581068153    0.694240135210974    0.723661765578573
    673              1690096              1742456              1798085
    674     2.97153795699944     3.05102802023059     3.14265974240467
    675             11498818             11784498             12087965
    676      2.3710066148072     2.45406914818211     2.54253905729233
    677             14813423             15416400             16026190
    678     4.08339544653752     3.98981486531171     3.87923811766046
    679             23542517             24142445             24739411
    680     2.57014353027485     2.51634742159783     2.44260643657749
    681            250612588            253516142            256244701
    682      1.2496856104686      1.1585826646505     1.07628610094579
    683            316052361            319048184            321815286
    684     1.00424635233902    0.947888188691621    0.867299091099042
    685               610719               637679               664825
    686     3.63618829150714     4.31980703497694     4.16888261952338
    687              1856402              1888525              1915425
    688     2.02757940882814     1.71558953640231     1.41434290218581
    689               135341               138724               141907
    690      2.4768350084254     2.46888281045825     2.26855657837876
    691               217324               221490               225296
    692      1.9017932212971     1.89881141880703   1.7037651435771901
    693              1956311              2024807              2096507
    694     3.91267652245929     3.44138304455692     3.47982391468998
    695             12031430             12456517             12900790
    696     3.45653213318518     3.47215473567164     3.50446100307726
    697             44997399             47308171             49728233
    698     5.00284787615202     5.00783408931391     4.98898112302915
    699            126152678            129583026            133119801
    700     2.65126549448063     2.68288995606035     2.69276790616125
    701              2873416              2917907              2961379
    702     1.61970581259597     1.53650125528195     1.47884597786304
    703              5192764              5259006              5323062
    704     1.34825800349086     1.26759174707907     1.21066661820809
    705             12488742             12775902             13035570
    706     2.09358123215972     2.27331417624889     2.01210346020247
    707             16046180             16148929             16225302
    708    0.754840057734904    0.638291666268526    0.471814398788212
    709              3447513              3482266              3518773
    710    0.975881112027497     1.00301297792155     1.04291179907128
    711              4513751              4538159              4564855
    712    0.506046910557235    0.539290805050494    0.586532691986894
    713              3480623              3607302              3733781
    714     5.62520060247632     3.57488237525447     3.44612686537792
    715             24956071             25332178             25682908
    716      1.6018374486186     1.49583260634063     1.37502669708874
    717                10363                10351                10344
    718   -0.135004842106104   -0.115863680046723  -0.0676491932056767
    719                10363                10351                10344
    720   -0.135004842106115   -0.115863680046712  -0.0676491932056878
    721              3341111              3402067              3472292
    722    0.681100420036239     1.80798016687309     2.04317065670572
    723              3880500              3948500              4027200
    724    0.589286034969052     1.73717486434422     1.97355854434158
    725            918438906            930300368            941906450
    726     1.22060843197855     1.29148078576715     1.24756287315581
    727           1209244377           1218395986           1227425785
    728    0.755294108958154    0.756803932609884    0.741121860524572
    729              1699040              1719313              1738813
    730     1.26050073376893     1.18614067584545     1.12779040932188
    731              2374653              2403659              2431600
    732     1.28845205052626     1.21408381409051     1.15573172024923
    733             11056997             11375365             11701754
    734     2.92155052377187     2.87933513955009     2.86926177753418
    735             21823886             22208079             22590088
    736      1.8001137980457     1.76042433506115     1.72013527149286
    737             52828442             54497125             56037189
    738     3.69061284870334     3.10982280624176     2.78676116269803
    739            159217727            163262807            166876680
    740     3.09207918437975     2.50885975954029      2.1893881210377
    741              1913048              1959794              2007308
    742     2.43558115876792     2.41415840553212     2.39551552418952
    743              3061024              3120990              3181608
    744     1.95603782398984     1.94007586747458     1.92364701840572
    745             19841910             20186530             20510054
    746     1.89762490340588     1.72191829085133     1.58996546499862
    747             27014909             27334503             27623341
    748     1.34331954460691     1.17608515721159     1.05113533724533
    749             36667813             37362913             38054174
    750     1.93245696242633     1.87792411411585     1.83321946443007
    751             79626086             81285572             82942837
    752      2.1168672092615     2.06267818983041     2.01831250528467
    753                13898                13865                13836
    754    0.165628525963743    -0.23772658243943    -0.20937879928191
    755                19828                19851                19880
    756    0.515751762652575    0.115930353970459    0.145981753039584
    757               751175               775701               800720
    758     3.22776011615142     3.21284893991127     3.17441862565887
    759              5698489              5892596              6090980
    760     3.39455509979259     3.34955956126701     3.31123407239589
    761             23622394             23621395             23563051
    762   0.0453020282998503 -0.00422912734687445   -0.247301959358141
    763             38248076             38230364             38204570
    764  -0.0275871247282958  -0.0463189372499392   -0.067492701646486
    765            185515371            193777253            202537854
    766     4.30470559886345      4.4534757176536     4.52096459433244
    767            568734640            586023563            604077372
    768     2.82873163160309     3.03989273450971     3.08073090228285
    769              3602364              3604909              3605100
    770    0.156908970557385   0.0706231082346651  0.00529819033637695
    771              3818774              3823701              3826095
    772    0.214145951376015    0.128937305848895   0.0625899064015263
    773             13987834             14081401             14185313
    774    0.753462281030614    0.666689680485504    0.735228495886114
    775             23512522             23638411             23781707
    776    0.620583476817431    0.533984339747177     0.60436980005948
    777              5704057              5800192              5886957
    778     1.88393843039725     1.67133444032998     1.48482053370472
    779             10362722             10419631             10458821
    780    0.705230637323876    0.547667939660689    0.375411414855816
    781              2916691              2992166              3050124
    782     2.83866535468301     2.55477853328691     1.91847056739792
    783              5211541              5286512              5353254
    784     1.69755299380957     1.42830817376899     1.25459291928333
    785              2164070              2226847              2291359
    786     2.86048889723178     2.85959756593569     2.85584077010789
    787              2997784              3075373              3154969
    788      2.5552675698799     2.55528456623402     2.55524755562296
    789               716368               730330               744178
    790     2.12164194530176     1.94899828021353     1.89612914709789
    791              2064193              2091909              2119122
    792     1.40106068168153     1.34270390414075     1.30086920606966
    793            787725024            795862783            803847286
    794    0.933156392807774     1.03307102758741     1.00325120995134
    795           1025680403           1030818658           1035935060
    796    0.478743583594039     0.50096062915614    0.496343557646384
    797               142743               144826               148014
    798     1.48637079048582     1.44872112878611     2.17738420408675
    799               255049               259137               263173
    800     1.62936233841902     1.59011958183693     1.54547312227047
    801               655113               689594               725081
    802     5.16940287323627     5.12952787760609     5.01803541363528
    803               678831               713186               748525
    804     4.96702253375353     4.93700548143404     4.83623474765467
    805             11697189             11469356             11415407
    806    -1.68260799572644    -1.96697734313015   -0.471484890969484
    807             22131970             21730496             21574326
    808    -1.39542998635748    -1.83065499370162     -0.7212622974018
    809            107067910            106569238            106133030
    810   -0.429544052386161   -0.466841006203021   -0.410158871957287
    811            145976482            145306497            144648618
    812   -0.424090524785415   -0.460024258081391   -0.453780668020263
    813              1301768              1404622              1448333
    814     7.26923214269629     7.60448869371561     3.06450124573438
    815              8223941              8372306              8567992
    816     1.39530227863448     1.78798197593577     2.31040451162765
    817            397344050            409950738            422611571
    818     2.98059086786013     3.17273858762954     3.08837912129822
    819           1434306106           1462021914           1489278936
    820     1.94459231863682     1.93234957893989     1.86433744521834
    821             17685749             18167507             18643377
    822     2.75498352081463     2.68754947754543     2.58562935641189
    823             22085929             22623415             23150847
    824     2.46860133878166     2.40447229697642     2.30459354525526
    825              8770792              8988199              9204829
    826      2.5988814220842     2.44853845356237     2.38157429421509
    827             26947253             27570318             28188977
    828     2.43590785577774     2.28583965848042     2.21912557576201
    829              4021323              4134178              4267503
    830     2.73656454691076     2.76775649384436     3.17403607699238
    831              9938027             10180950             10434504
    832     2.38007633836581     2.41498169831971     2.45996800523196
    833              4138012              4175950              4114826
    834      2.6973558432636    0.912639805776796     -1.4745328419194
    835              4138012              4175950              4114826
    836     2.69735584326358    0.912639805776796    -1.47453284191939
    837                71348                74813                78411
    838     4.81792614997319     4.74223539749058     4.69725569658651
    839               440395               450760               461216
    840     2.39380068808573      2.3262995615241     2.29314328165436
    841              1742095              1856043              1945162
    842     6.45961162875014      6.3358390238528     4.68984618913006
    843              4857096              5140113              5350907
    844       5.785413309402     5.66343352057259     4.01910155879324
    845              3560761              3607607              3649920
    846      1.4283892376963     1.30703855910826     1.16605778902949
    847              5988095              6011275              6026849
    848      0.4957580845353    0.386354097488165    0.258744779316872
    849                25627                26270                26878
    850   2.2253828200937402     2.47811205040359     2.28805044274239
    851                27335                27969                28601
    852      1.8908203925672     2.29288216357038     2.23449282668431
    853              3049313              3198834              3391588
    854     5.03349010597156     4.78700490589674     5.85118798833874
    855              9070747              9411103              9758281
    856     3.92673917655211     3.68355422936353     3.62260954076201
    857              3976219              3989274              3997403
    858    0.249744543421309    0.327789169840433    0.203564080708459
    859              7503433              7496522              7480591
    860   -0.171946646904854  -0.0921469496277098    -0.21273800909834
    861            219359318            228348894            237701252
    862       4.068481106782     4.09810537430646     4.09564409801784
    863            689080780            707609717            726702652
    864     2.67450251970421     2.68893539593427     2.69822962309603
    865              1063522              1120657              1181081
    866     5.25004412108781     5.23290786061641     5.25150003442859
    867              6394431              6686100              6992367
    868     4.47742684509793      4.4603289201929     4.47883810222676
    869            219400444            228391489            237743572
    870     4.06779062720415     4.09800674788062     4.09475985333236
    871            689161982            707693440            726785433
    872     2.67418982429359     2.68898437290757     2.69777730312153
    873             15118023             15475330             15839373
    874     2.41287704146328     2.36345056493168     2.35240864007424
    875             30514882             30971830             31425957
    876     1.52746271092983     1.49745950189157     1.46625820947617
    877                79531                83244                87285
    878     3.52287419782208     4.56291714296761     4.74025725855784
    879               146258               149841               153762
    880      1.7546971939805     2.42025464283049     2.58312205225329
    881               324200               330069               335840
    882      1.8476240217507     1.79411142115836     1.73331325591636
    883               487394               495666               503780
    884     1.73764088610001     1.68294819033744     1.62373520363101
    885              3020933              3011770              3001728
    886    -0.30757702264983   -0.303777827167022   -0.333982295641792
    887              5378867              5376912              5373374
    888    -0.18301227099618  -0.0363525509687338  -0.0658215070322074
    889              1011568              1014358              1019460
    890    0.208705847624412     0.27542978013819    0.501717528541028
    891              1992060              1994530              1995733
    892    0.157498742047657    0.123915442323272   0.0602967792572922
    893              7478923              7508835              7542470
    894    0.322021443548705    0.399152958219952    0.446938733743388
    895              8895960              8924958              8958229
    896    0.268470527954234    0.325438067703728    0.372092942799084
    897               234887               235644               236069
    898     0.47326003431111    0.321764412171086    0.180194361825068
    899              1041396              1050809              1058797
    900     1.05218810260185    0.899822293950695    0.757301427499704
    901                30600                30777                31472
    902    0.265056847861417    0.576764881535037     2.23306038730179
    903                30600                30777                31472
    904    0.265056847861417    0.576764881535037     2.23306038730179
    905                41126                42595                42320
    906    0.509490028647897     3.50963505954771   -0.647708794475981
    907                81202                83723                82781
    908   0.0874745178458057     3.05738532610948    -1.13151642070391
    909              8751059              9042350              9342648
    910     3.24863741711485     3.27443747560705     3.26706274625283
    911             16727948             17164021             17611356
    912     2.54462854098906     2.57345370585541     2.57285306276557
    913                16712                17716                18828
    914     5.30837148580148     5.83411618066871      6.0876937477944
    915                19578                20598                21739
    916     4.35327856337439     5.07874968643713     5.39138987714319
    917              1850359              1918103              1998830
    918     3.48252099570091     3.59570024738027     4.12253325886443
    919              8538804              8838369              9196366
    920      3.3300848498951     3.44814062098756     3.97060486574728
    921            675980158            702566376            729485713
    922     3.84591947104005     3.93298792654799     3.83157206487206
    923           1810599718           1826844868           1842469495
    924    0.944991843455512    0.897224816644979    0.855279354787555
    925            276212712            276574694            277284807
    926   0.0862930973366929    0.131051897423177    0.256752702038597
    927            435418872            435070718            435154320
    928   -0.091146012983117  -0.0799584084173546   0.0192157266718311
    929              1716308              1785635              1857506
    930     4.06166814354079     3.95986221278719     3.94606334853757
    931              5145426              5281538              5421001
    932     2.70645421009308     2.61091784886825     2.60631415651139
    933             20716767             21664845             22633716
    934     4.55602672226528      4.4747528371425     4.37497522003223
    935             63649892             64222580             64776956
    936    0.920626978729791    0.895723403218221    0.859506057256063
    937              1698527              1733892              1768677
    938     2.14948527377572     2.06071878102999     1.98632167912758
    939              6408810              6541755              6672492
    940     2.14192159842275     2.05318718218435     1.97879252509967
    941              2138493              2178536              2217070
    942     1.91998049519795     1.85517162137461     1.75334127642956
    943              4635094              4698968              4758988
    944      1.4333226775008     1.36864308511031      1.2692130459731
    945            388308206            395511206            402568285
    946     1.94996814557464     1.85496981230419     1.78429305995442
    947            512664811            519865563            526861169
    948     1.45653996540749     1.40457309444631     1.34565674241438
    949               219848               227219               234859
    950     3.10997800666531      3.2977911637264     3.30710239610896
    951               893001               909639               926721
    952     1.65311683189099     1.84601168099225     1.86047318846223
    953            158988722            162871839            166781699
    954     2.65374890449704     2.44238519006399     2.40057460148159
    955            286546301            291738620            296947926
    956     1.98213549590119     1.81203490740576     1.78560726721749
    957                23781                23947                24117
    958    0.717423738830206    0.695611252765245    0.707393096315252
    959               103210               103804               104410
    960     0.58985755025645    0.573875805693953    0.582095066723764
    961            397344050            409950738            422611571
    962     2.98059086786013     3.17273858762954     3.08837912129822
    963           1434306106           1462021914           1489278936
    964     1.94459231863682     1.93234957893989     1.86433744521834
    965            219400444            228391489            237743572
    966     4.06779062720415     4.09800674788062     4.09475985333236
    967            689161982            707693440            726785433
    968     2.67418982429359     2.68898437290757     2.69777730312153
    969               745823               747414               749094
    970    0.141554615070771     0.21309418425198    0.224522783138701
    971              1338567              1345964              1353548
    972    0.476567584984784     0.55108461926576    0.561880810521628
    973              6378688              6480910              6583664
    974     1.63048086718413     1.58984990271606     1.57304966624616
    975              9995123             10094561             10193798
    976     1.02378964217481    0.989948991480907    0.978273219919924
    977             42518057             43535361             44534977
    978     2.40486754052128     2.36446481382506     2.27013760110559
    979             65072018             65988663             66867327
    980     1.48389425307121      1.3988328763027     1.32275076403145
    981                 4467                 4501                 4607
    982    0.718942660843874    0.758255194081403     2.32772916429843
    983                 9621                 9609                 9668
    984   -0.176540883901345   -0.124805008399676     0.61213035461059
    985              8029877              8377308              8814911
    986      4.3442463794078     4.23574115527111     5.09180975374054
    987             35414469             36353531             37333918
    988     2.72137531815627     2.61708727425256      2.6610905029697
    989              3764273              3991676              4232834
    990     5.81283862136826     5.86564445134828     5.86605522528284
    991             24763325             25545090             26354736
    992     3.04478942602344     3.10813979944162     3.12028672115224
    993             32692860             32432068             32238837
    994   -0.994343799303611   -0.800901903575437   -0.597584261528737
    995             48662400             48202470             47812949
    996    -1.05092091504282   -0.949639340714078   -0.811376216452802
    997           1092331859           1122092954           1152196669
    998     2.66402469700139     2.72454700966476     2.68281828993644
    999           2176410449           2193797571           2210557257
    1000   0.843722813717093    0.798889842124623    0.763957724338283
    1001             3046866              3060739              3072794
    1002   0.562747242049909    0.454286872148488    0.393085535610219
    1003             3300939              3306441              3310202
    1004   0.264364920924121    0.166541088046771    0.113683038515289
    1005           225792302            228400290            230876596
    1006    1.21338042170418     1.14841884658312     1.07836038438036
    1007           284968955            287625193            290107933
    1008   0.989741382223669    0.927797485710314    0.859481712840946
    1009            11634932             11900009             12162787
    1010    2.30121847876892     2.25272041174372     2.18418879362993
    1011            24964450             25271850             25567650
    1012    1.26596853291421     1.22383151928578     1.16367524443693
    1013               51761                52089                52350
    1014   0.645419872219124    0.631682408591678    0.499814328643544
    1015              113641               113450               113108
    1016  -0.151239400305238   -0.168214543982812   -0.301909674146042
    1017            21857258             22259807             22662919
    1018    2.16714639554835     1.82496352521521     1.79473890535752
    1019            24880203             25330929             25782029
    1020    1.83535055290849     1.79537122177653      1.7651559616598
    1021                8686                 9011                 9368
    1022    3.37189518506287     3.67335191402701     3.88535730425159
    1023               20657                21288                21982
    1024    2.71354447748245     3.00892886592725     3.20804029713205
    1025              100749               100952               101182
    1026   0.160925055589447    0.201288113138855    0.227571907014354
    1027              108549               108509               108505
    1028 -0.0856389129235212  -0.0368565088158972 -0.00368639811298412
    1029            19904159             20572659             21258672
    1030    3.31196106294403     3.30342563630772     3.28019450083254
    1031            79817777             80642308             81475825
    1032    1.02839410111569     1.02771758753322     1.02829255325908
    1033               43255                44942                46677
    1034    3.83398158567246      3.8259935917403     3.78787684511947
    1035              197034               202125               207258
    1036    2.54955882382133     2.55100152185343     2.50780760969082
    1037          2931982375           3001375881           3071458625
    1038    2.30217527141659     2.36677773344391     2.33502056319084
    1039          6226339538           6308092739           6389383352
    1040    1.33483941265074     1.31302188872051     1.28867181196335
    1041               40943                40948                40671
    1042    1.23861896617603   0.0122113541322226   -0.678766129216231
    1043              185530               186630               187440
    1044    0.82373591140834    0.591145315625182    0.433074700450214
    1045                <NA>                 <NA>                 <NA>
    1046                <NA>                 <NA>                 <NA>
    1047             1701154              1702310              1703466
    1048  0.0678593232934242   0.0679307931245331   0.0678846785222277
    1049             5127958              5370307              5622379
    1050      4.686083870137     4.61775470894098     4.58698075972799
    1051            19143457             19660653             20188799
    1052    2.72575839716245     2.66583425909229     2.65086168454782
    1053            27094742             27595063             28114892
    1054    1.72060982345533     1.82971929904417     1.86625204598229
    1055            47229714             47661514             48104048
    1056   0.885660412204392    0.910100943022538    0.924209372808693
    1057             3567391              3727817              3896360
    1058    3.56908871355332     4.39882913425441     4.42199763398519
    1059            10191964             10508294             10837973
    1060    2.99605641046682     3.05652834733399     3.08911357914686
    1061             4069981              4144889              4163625
    1062    1.85572056919021      1.8237677837638    0.451008008382556
    1063            11910978             11984644             12075828
    1064   0.642662969955232    0.616566778386599    0.757960496752579
                       x2004                 x2005               x2006
    1                  42317                 42399               42555
    2     0.0945693618486456     0.193588048559493   0.367257987480091
    3                  93540                 94483               95606
    4      0.900989229759957      1.00307718391638    1.18156554573069
    5              133647466             138745587           144026740
    6       3.73620496737797      3.81460356307841    3.80635745913851
    7              445281555             457153837           469508516
    8       2.64496843028186      2.66624158730311    2.70252112091536
    9                5299549               5542073             5828215
    10      4.58865325754718      4.47468958963317    5.03421600301046
    11              23553551              24411191            25442944
    12      3.93317768847308      3.57650800354462    4.13967803534352
    13             113875177             119079574           124507242
    14      4.55934984688724      4.57026468551614    4.55801764960965
    15             301265247             309824829           318601484
    16      2.82864223341259      2.84121122009138    2.83277974471181
    17              10291645              10892537            11444951
    18      5.68584496286432      5.67454748217163    4.94707962134254
    19              18771125              19450959            20162340
    20      3.50638882370405      3.55765898910381     3.5920133134417
    21               1381828               1407298             1430886
    22      1.97179893742068      1.82642935665107    1.66222789724062
    23               3026939               3011487             2992547
    24    -0.417931377925999    -0.511790116421897  -0.630911244851673
    25                 69817                 72071               72044
    26      3.51138278860428      3.17742111163013  -0.037470076419389
    27                 76933                 79826               80221
    28      4.01273671344563      3.69143527252581   0.493606005740852
    29             169454014             174496874           180242023
    30      2.68148730511189      2.97594602863759    3.29240797746326
    31             313249616             320558754           328703057
    32      2.17779357781657      2.33332704229076    2.54065842793987
    33               3269387               3521630             4048888
    34      5.09809234355025      7.43214450465337    13.9518324907705
    35               3993339               4280993             4898954
    36        4.609526743863      6.95572689795874    13.4836720911499
    37              34747780              35175563            35608120
    38      1.20915917460343      1.22359234802314    1.22220931788621
    39              38668796              39070501            39476851
    40      1.01533668354561      1.03347622351885    1.03467173566483
    41               1963242               1948348             1932078
    42    -0.751432255097974    -0.761535435957295  -0.838572638294868
    43               3065745               3047246             3026486
    44    -0.596992234773189    -0.605237484238947  -0.683602136047238
    45                 50825                 50441               50017
    46    -0.656961719623165     -0.75840230789254  -0.844138879424085
    47                 57626                 57254               56837
    48    -0.545139668053888    -0.647634575615904  -0.730998688583939
    49                 23554                 23336               23141
    50    -0.975947246488804    -0.929842485393645   -0.83912965218856
    51                 78941                 79869               80895
    52      1.10308348343215      1.16870545018922    1.27642249519442
    53              16835775              17065978            17321968
    54      1.21137453896552      1.35808044012136    1.48886315447039
    55              19932722              20176844            20450966
    56      1.06919812205752      1.21729073052406    1.34945083098893
    57               4829142               4839033             4839801
    58     0.146033119948255     0.204609524563365  0.0158696802034512
    59               8171966               8227829             8268641
    60     0.620413127334181     0.681267248299787   0.494797775098466
    61               4335079               4396406             4461940
    62      1.26130075641327      1.40475551381099    1.47962600704689
    63               8306500               8391850             8484550
    64     0.875427280482454      1.02226559239828    1.09858657441812
    65                650742                692707              736488
    66      6.27479146975711       6.2493860070609    6.12858321286989
    67               7120496               7388874             7658190
    68      3.71466281696618      3.69979693670807    3.58003081168824
    69              10144977              10206487            10279407
    70      0.48827848090044     0.604479238401223   0.711907500572579
    71              10421137              10478617            10547958
    72     0.432788248129175     0.550055708089009   0.659558214980699
    73               3160900               3304019             3449196
    74       4.2906906587585      4.42828090728256    4.30015541586328
    75               7894554               8149419             8402631
    76      3.02645705105477        3.177348170646    3.05982347255495
    77               2790981               2988501             3198105
    78      6.87313233222337      6.83787769629743    6.77865229952647
    79              13445977              13876127            14316242
    80      3.13802065225682      3.14899280596388    3.12248153649463
    81              36243549              37777256            39247175
    82      4.31932435855494      4.14458215704186    3.81722387019357
    83             138789725             140912590           142628831
    84      1.66119164263389      1.51797511096499    1.21058994053819
    85               5419782               5406009             5391557
    86    -0.249495596294176    -0.254448057069833  -0.267690118865882
    87               7716860               7658972             7601022
    88    -0.754796951273604    -0.752977445493645  -0.759505915531059
    89                736762                797497              858852
    90      6.86999335666605      7.92131636610236     7.4118540879233
    91                833451                901921              970981
    92      6.85193933235192      7.89520208580362    7.37799675652121
    93                281937                285957              290101
    94      1.40085726547946      1.41578081415146    1.43876883427672
    95                343089                347804              352664
    96      1.34953389082619      1.36492139898865    1.38766595682613
    97               1808276               1800058             1797002
    98    -0.251084195362237    -0.455501858145626  -0.169916583835945
    99               4142860               4094297             4058086
    100   -0.982327609602979     -1.17913407901923  -0.888359591107583
    101              6994710               6992229             6993921
    102  -0.0234149555735391   -0.0354759540457049  0.0241953648181244
    103              9730146               9663915             9604924
    104   -0.682169472243919    -0.683005560010688  -0.612296198031047
    105               123356                127046              130783
    106     2.99605658716917      2.94747417349311    2.89902355546765
    107               272128                280375              288729
    108     3.03310164793291      2.98554490009989    2.93605390781473
    109                63740                 64154               64523
    110    0.653211310145151     0.647413398667903   0.573530641034438
    111                63740                 64154               64523
    112    0.653211310145151     0.647413398667903   0.573530641034438
    113              5871784               6017470             6167232
    114       2.468558749287        2.450839930909    2.45832096863023
    115              9216279               9377388             9542663
    116     1.73917027368374       1.7329882488039    1.74713275481179
    117            152434477             154731704           156991180
    118     1.52549407216353      1.49578304716073    1.44969502534998
    119            184722043             186797334           188820682
    120     1.13939301199192      1.11720287765336    1.07735384590771
    121                88706                 88499               88283
    122   -0.219586027976693    -0.233627825573633  -0.244368892182542
    123               268505                269477              270425
    124    0.375370773814513     0.361350772944923    0.35117520374422
    125               262394                268301              274122
    126      2.3096765508764       2.2262293640862    2.14637781700089
    127               360461                366717              372808
    128     1.79597440947835      1.72066656308508    1.64731083840717
    129               193925                205398              213504
    130     6.31241979706994       5.7478073971772    3.87060075074648
    131               649991                663323              673260
    132     2.39210919099831      2.03035343996667    1.48695338651854
    133              1031569               1058912             1087789
    134     2.57909608333065      2.61610217385376    2.69052298976928
    135              1859085               1892807             1928704
    136     1.74841429333915      1.79764812587178    1.87873623072759
    137              1562353               1602261             1640142
    138     2.38524385586521      2.52227385216219  2.3367066729423698
    139              4115138               4208834             4294352
    140     2.16901718078334      2.25132803027483    2.01150212315221
    141             25566897              25834340            26126316
    142      1.0305118096742      1.04061858829188    1.12384675481251
    143             31940655              32243753            32571174
    144    0.933020777495183     0.944466927450734    1.01033450270045
    145             65551568              65430714            65329206
    146   -0.199773806789707    -0.184364773699997  -0.155138151174697
    147            106331716             106041911           105772481
    148   -0.274282095915453    -0.272548032611454  -0.254078785886833
    149              5427827               5464643             5500991
    150    0.721465950344645     0.675992501186956   0.662946312521288
    151              7389625               7437115             7483934
    152    0.687425960233006     0.640601540243491   0.627558473346134
    153                45880                 46349               46819
    154     1.00547390934054      1.01704245449199    1.00893866347325
    155               149538                150763              151985
    156    0.803686357858662     0.815852624411776   0.807276438769338
    157             13891581              14037420            14185272
    158     1.06588532302092      1.04436478447545    1.04776222341866
    159             16017966              16175311            16334575
    160    0.996674255969576      0.97751001174206   0.979795955667856
    161            533257098             554367818           575118254
    162     3.97537212954358      3.88247297575862    3.67472809325703
    163           1296075000            1303720000          1311020000
    164    0.593932815112141     0.588124989556992   0.558374367373002
    165              8311269               8580987             8853775
    166     3.27451334940999      3.19366370589233    3.12949798951071
    167             18544903              18970215            19394057
    168     2.33745599030944      2.26751371599435    2.20965613638884
    169              8058430               8385541             8723538
    170     4.00831089018868      3.97901647072019    3.95159764080866
    171             16809407              17275171            17751333
    172     2.74462206750134    2.7331598125050802    2.71903433909344
    173             20281775              21195598            22168089
    174     4.27041474167126      4.40708190602944    4.48603037406706
    175             54815607              56550247            58381630
    176     2.98105657054334      3.11546176922533     3.1871715188242
    177              2144691               2239991             2342944
    178     4.15548665988047      4.34763619428637    4.49364102566441
    179              3543012               3672839             3813323
    180     3.39770601248237      3.59877196603076    3.75360549915507
    181             31494420              32098047            32688341
    182     1.97049976240107      1.89847998807834    1.82232828200747
    183             41648268              42220940            42772910
    184     1.42817053070487      1.36565233876962    1.29886517669187
    185               162159                165210              168476
    186     1.75747074608288      1.86400565666597    1.95759144709188
    187               581154                592683              604658
    188       1.915137697063      1.96439041104988    2.00033228573184
    189               276603                284307              291992
    190     2.84854566528668      2.74713750733129    2.66717644005577
    191               486583                492827              498884
    192     1.34359900654352      1.27507062329957    1.22154041180103
    193              2738293               2834329             2930529
    194     3.57322919534328      3.44704913035542    3.33777279453659
    195              4252800               4315887             4378172
    196     1.52086528848105      1.47252758666781    1.43284202712965
    197              3416335               3437801             3459290
    198    0.674654428941011     0.628334165121402    0.62507981119326
    199              6760372               6802474             6844183
    200     0.64949595391937     0.622776379761362   0.613144570637104
    201              8535826               8562116             8583640
    202    0.356276117639966     0.307522633060114   0.251071009136873
    203             11225294              11246114            11260630
    204    0.232575967917871     0.185302193413535   0.128992465660705
    205               121652                124627              127696
    206     1.59253019739367      2.41607665949769    2.43271628359874
    207               134192                137658              141239
    208     1.72502952808587      2.55007380740239    2.56811425717747
    209                45286                 46727               48177
    210     3.19495923650165      3.13242221135565    3.05595726835774
    211                45286                 46727               48177
    212     3.19495923650163      3.13242221135563    3.05595726835774
    213               696882                707981              719017
    214     1.60738049010172      1.58011577343477    1.54677442160317
    215              1018684               1037062             1055438
    216     1.81616594339173      1.78801171115289    1.75641313375661
    217              7512306               7515659             7528974
    218  -0.0631829985834922     0.044623474377798   0.177006690378909
    219             10197101              10211216            10238905
    220   0.0304348483027166     0.138325980046718   0.270795629232185
    221             62529997              62660267            62753557
    222    0.245211652539077     0.208115300433845   0.148771505773609
    223             82516260              82469422            82376451
    224   -0.021709727650009   -0.0567782618352031  -0.112797497644931
    225               627823                637777              650515
    226     1.53249373075503      1.57304128563813    1.97756644074892
    227               818373                830861              846947
    228     1.47246847937649      1.51442896223044    1.91756063912333
    229                45464                 45738               45990
    230    0.653193248368682     0.600865823937137   0.549451931764078
    231                68574                 68674               68742
    232    0.192678286970014     0.145721639216111  0.0989695603866779
    233              4624434               4652908             4681382
    234    0.499468403225543     0.613841537988877   0.610096500558927
    235              5404523               5419432             5437272
    236    0.258432282052875     0.275481733409308   0.328645158919929
    237              5968735               6174304             6379152
    238      3.5134103182523      3.38611514561565     3.2639009551271
    239              9043127               9164768             9284168
    240     1.37256671456504      1.33615456062027    1.29440166190212
    241             20501248              21036255            21596721
    242      2.6470662285033      2.57616153899167    2.62941198808677
    243             32510186              32956690            33435080
    244     1.40727321515229      1.36408196251206    1.44113729642515
    245            771106741             798883707           826410060
    246     3.68353674809443      3.60222061656184    3.44560200174418
    247           1881619185            1896845515          1911778513
    248    0.820896740018753     0.809214219401142   0.787254306263321
    249           1064154356            1091784280          1120094037
    250     2.62629879937684      2.59642070196064     2.5929808221822
    251           2680567394            2725013738          2769421149
    252     1.70107871601184      1.65809462949842    1.62962154578319
    253            967022565             997364788          1027397885
    254     3.21903320364925      3.13769544767553    3.01124496887692
    255           2116651449            2132517760          2148405823
    256    0.763640198469858     0.749594885237045   0.745037781068717
    257            240967900             242028778           243192777
    258    0.410229595155727     0.440256980286577   0.480934130899087
    259            371533136             372077135           372780224
    260    0.124264554839442     0.146420049058563   0.188963237421191
    261            603649419             607362884           611119312
    262    0.604899267816464     0.615169150026134   0.618481652230813
    263            870270838             872913705           875627934
    264    0.296115216090655     0.303683277044371   0.310938983367208
    265              8325670               8497612             8672589
    266      2.0619822695318      2.04416697398271    2.03821792841116
    267             13534593              13770012            14009061
    268     1.73957986039204      1.72443317741318    1.72111503482542
    269             33319914              34023734            34729618
    270     2.13874688144954      2.09031036470576    2.05345276493424
    271             77522427              79075310            80629670
    272     2.03166566101876      1.98334143708702    1.94660064062847
    273            242128814             244182997           246112194
    274     0.88664684501498     0.848384364530858   0.790061971432038
    275            327702192             329398861           330939600
    276    0.551497494659742     0.517747223369184   0.467742661684539
    277               833474                880640              923099
    278     7.15054016663028      5.50464079218039    4.70875718243989
    279              2763140               2831732             2880093
    280      4.0529646475642      2.45208243654581    1.69340467080078
    281             33059302              33727737            34408810
    282     2.04276209944672      2.00175761706271    1.99920810283443
    283             42921895              43653155            44397319
    284      1.7254663034483      1.68934904712031    1.69035255626114
    285               938279                931205              924006
    286   -0.782094568681874    -0.756789976535161  -0.776088125147073
    287              1362550               1354775             1346810
    288   -0.597820510395586    -0.572255524659659  -0.589655559840225
    289             11674671              12162781            12670080
    290     4.13159883909853      4.09589277615261    4.08627573930679
    291             75301026              77469940            79691050
    292     2.87241402666675      2.83962299391293    2.82672932299161
    293            311139276             313196512           315160438
    294    0.682474020138329     0.661194570626947   0.627058707473722
    295            434059923             435600393           437014860
    296    0.375805315025417     0.354898003333972   0.324716649188133
    297            257404681             265676736           274057951
    298     3.17674870956954      3.21363813892724     3.1546665041835
    299            673751123             690141498           706884119
    300     2.42824810181655      2.43270466504291     2.4259693191207
    301              4327463               4349276             4372951
    302    0.452371560777971     0.502793640628433   0.542867277881872
    303              5228172               5246096             5266268
    304    0.290350361518584     0.342248594286999   0.383777136305104
    305               428832                436333              443873
    306     1.77200648528469      1.73404794617401    1.71327721805212
    307               866694                874923              883083
    308    0.972529404319471      0.94499091797119   0.928331143605677
    309             48218178              48737209            49233640
    310     1.06210370954027      1.07066966413981    1.01343461629162
    311             62716306              63188395            63628261
    312    0.735098067563242     0.749918325024531   0.693706612114177
    313                18834                 19211               19396
    314     2.61976736912888      1.98192845584341   0.958382759680014
    315                47989                 48291               48313
    316     1.25183798931429     0.627338991367072  0.0455467690371607
    317                24884                 24773               24624
    318   -0.328987257608594    -0.447067623413369  -0.603277332211627
    319               111438                110940              110301
    320   -0.328790000684937    -0.447886780617126  -0.577652222559237
    321              1158884               1202441             1248711
    322      3.6232785326017      3.68961842776437    3.77581619166747
    323              1417110               1458353             1502534
    324     2.76976312159116      2.86881302760127    2.98452993581194
    325             47767769              48269624            48798541
    326     0.93379733417362      1.04513367651541    1.08979547464129
    327             59987905              60401206            60846820
    328    0.568943113421317      0.68661130607346    0.73504867842129
    329              2090052               2091919             2095077
    330    0.112500652161642    0.0892880473841929    0.15084803514687
    331              3927340               3902469             3880347
    332   -0.619262410024726    -0.635292218459481  -0.568484712403705
    333             10214975              10642858            11084549
    334     4.13576058789423      4.10342759818215    4.06630997622966
    335             21906444              22496951            23098586
    336     2.66890927022094      2.65989488010389    2.63916146161943
    337                28716                 29155               29587
    338     1.45573190002895      1.51719659598826    1.47086509948869
    339                28716                 29155               29587
    340     1.45573190002895      1.51719659598826    1.47086509948869
    341              2865471               2948327             3036092
    342     3.00555734579613      2.85051562744497    2.93332712695019
    343              8961039               9140114             9330625
    344     2.12923941806619       1.9786677950531    2.06291427897393
    345               826878                863723              902725
    346     4.33218068112684      4.35949525678906     4.4165851065872
    347              1612225               1660368             1711294
    348     2.89265160161859      2.94240520384811    3.02105449420899
    349               508792                526457              545061
    350     3.37984052976644      3.41303681918736    3.47280581816997
    351              1347009               1379713             1414091
    352     2.35534585979133      2.39889276052679    2.46114153135878
    353               463031                499293              538099
    354      7.6682135427641      7.53990912923087    7.48494603222043
    355               826355                864726              905418
    356     4.53953968288754       4.5388230472664    4.59840217738441
    357              8114802               8180275             8246206
    358    0.767168289554225      0.80359672232117   0.802744715055919
    359             10955141              10987314            11020362
    360    0.247413542822812     0.293249074052818    0.30033180096664
    361                39317                 39576               39832
    362    0.694216820371209     0.656587860705694   0.644773541447411
    363               109516                110254              110988
    364    0.711094558594151     0.671613754423162   0.663529251217564
    365                46997                 47175               47220
    366    0.550483595457193     0.378032136899694  0.0953440402753015
    367                56911                 56935               56774
    368    0.256870544844429    0.0421622197627189  -0.283179181223717
    369              5911891               6074432             6238013
    370     2.76730263364391      2.71227405142158    2.65732152717023
    371             12682108              12948292            13213330
    372     2.12598597102736      2.07717063066365    2.02622809680574
    373               153400                153957              154345
    374    0.484221308867501     0.362445371187286   0.251701386850063
    375               164004                164430              164675
    376    0.378143882417747     0.259412995547093   0.148888680062194
    377               213405                211366              209173
    378   -0.880347745400763    -0.960054081498194   -1.04295671923629
    379               760424                759709              758367
    380  -0.0181461236827469   -0.0940707231179221  -0.176802788474664
    381            876369501             886016450           896315848
    382     1.09047322316506      1.10078556921391    1.16243868835618
    383           1128753775            1136164408          1144695047
    384    0.629148718093447     0.656532289338301   0.750827867862583
    385              6783500               6813200             6857100
    386     0.77991856230718     0.436871406105885   0.642270482876128
    387              6783500               6813200             6857100
    388     0.77991856230718     0.436871406105885   0.642270482876128
    389              3538645               3675267             3814124
    390     3.87594791097303      3.78818975773254    3.70852353614482
    391              7383407               7564613             7745200
    392     2.48929432977264      2.42460040894439    2.35921073577649
    393            160315018             166983301           173994752
    394     4.09146854563494        4.159487416207    4.19889351690324
    395            532571131             547936900           563930944
    396     2.87974230932674      2.88520501874518    2.91895727409488
    397              2330812               2341055             2348849
    398    0.338652385305638     0.438497774560809   0.332373847105583
    399              4304600               4310145             4311159
    400   0.0279042785156658     0.128732789322745  0.0235231237565747
    401              3735059               3885132             4040472
    402      4.1915736705384        3.939334509696    3.92045567555494
    403              8961489               9111900             9266288
    404     1.67941646661672      1.66448553299086    1.68016163386105
    405              6660205               6694281             6737243
    406    0.398121565451941     0.510331493937622   0.639721099642082
    407             10107146              10087065            10071370
    408   -0.221439378654807    -0.198878843237103  -0.155716484651578
    409           1928771388            1977642793          2026037928
    410      2.5626689789638      2.53381013965975     2.4471120452742
    411           4183400394            4228483560          4272296273
    412     1.09927013927033      1.07766796753808    1.03613298664452
    413           2298373932            2361462994          2424716421
    414      2.7676708701492      2.74494333239765    2.67856947835787
    415           5391691968            5465865380          5539552227
    416       1.395293407916       1.3756982490881    1.34812773233723
    417            369602544             383820201           398678493
    418     3.85091081539044      3.84674219125505    3.87115945468435
    419           1208291574            1237381820          1267255954
    420     2.43372642372645      2.40755183814598    2.41430199774553
    421            146072614             151416693           156947515
    422     3.69469800260056      3.65850850043662    3.65271615065585
    423            409622323             419337831           429250772
    424     2.40673461788231      2.37182093222981    2.36395103593694
    425            102009016             105117659           108337481
    426      3.0452411261964      3.00190831666375    3.01708943811632
    427            225938595             228805144           231797427
    428     1.27322648868512      1.26074829560286    1.29930889873663
    429            223529930             232403508           241730978
    430     3.95324748549373      3.96974937539684    4.01348072594499
    431            798669251             818043989           838005182
    432     2.44757553038512       2.4258775426425    2.44011242285407
    433                40823                 41220               41656
    434    0.977250574198138     0.967792739552552     1.0521840398285
    435                78677                 79415               80228
    436    0.942439421736094     0.933640362384973     1.0185313836682
    437            328414552             337558628           346659205
    438     2.82462858995052      2.74625194074982    2.66029713783098
    439           1136264583            1154638713          1172373788
    440     1.67281087022876      1.60412916927926    1.52430796041014
    441                 <NA>                  <NA>                <NA>
    442                 <NA>                  <NA>                <NA>
    443                 <NA>                  <NA>                <NA>
    444                 <NA>                  <NA>                <NA>
    445              2450379               2515791             2595694
    446     2.28949668426784      2.63445605550239    3.12666512170803
    447              4070262               4159914             4273591
    448     1.82831381790901      2.17870301355631    2.69600564359576
    449             46180851              47413957            48637866
    450     2.66367222859408      2.63514051880762    2.54857265046887
    451             69061674              70182594            71275760
    452     1.61586394638623      1.61004001098983    1.54559660977253
    453             19142440              19734937            19892550
    454     2.95577908767335      3.04826571983735   0.795477296474372
    455             27858948              28698684            28905607
    456     2.87715845950826      2.96970606467236   0.718432208523802
    457               271378                276072              283000
    458     1.01255999170572      1.71490183110168    2.47851963814189
    459               292074                296734              303782
    460      0.8779361576874      1.58289197780465    2.34742243028387
    461              6227239               6342289             6459778
    462     1.83652448597197      1.83066879022281    1.83552075799557
    463              6809000               6930100             7053700
    464     1.76762364154994      1.76289766113006    1.76780587358185
    465             39006818              39267369            39454178
    466    0.823321280014444     0.665741727415553    0.47460792573121
    467             57685327              57969484            58143979
    468    0.647182705470772     0.491389107503214    0.30055968851778
    469              1401943               1413678             1425466
    470    0.846977659155608     0.833568718884464   0.830395903655452
    471              2664024               2676863             2689660
    472     0.48906495556364     0.480782489174805   0.476920503498281
    473              4333049               4513583             4919371
    474     2.51123185726129      4.08198436075783    8.60893819392188
    475              5532423               5678534             6075548
    476     2.49462542491271      2.60672257408209    6.75790900912969
    477            108136910             109856670           111383848
    478     1.75443198804852      1.57784066748167      1.380580980785
    479            127761000             127773000           127854000
    480   0.0336622582725446   0.00939209655766336  0.0633735894181335
    481              8465822               8552467             8654579
    482    0.824455477952229      1.01826627411614    1.18687657005792
    483             15012984              15147029            15308085
    484    0.694909484311257     0.888898042702025    1.05767130866502
    485              7414140               7768972             8140172
    486     4.70283500500608       4.6748864150129    4.66734581179976
    487             34791836              35843010            36925253
    488     2.98951543624097      2.97658073983056    2.97471111652036
    489              1801551               1821882             1841521
    490     1.20727992201011      1.12220721976998    1.07218278400268
    491              5104700               5162600             5218400
    492     1.21010542490902      1.12786445855189    1.07505129401544
    493              2480269               2539900             2600268
    494     2.39348267671201       2.3757688140057    2.34898064617692
    495             13016371              13246583            13477779
    496     1.77233194606874      1.75317594902836    1.73026969167735
    497                41888                 42751               44067
    498     2.06227687150349      2.03931971945611    3.03186160953369
    499                96224                 98164              100083
    500     2.01764084838712      1.99607425625382    1.93602923599118
    501                14985                 14965               14946
    502   -0.120048033624978    -0.133555946396425  -0.127043579656394
    503                46580                 46725               46874
    504     0.32039248574627     0.310808888543171   0.318379738805617
    505             38947802              39195731            39490771
    506    0.829355606254432     0.634549840258114   0.749916123325417
    507             48082519              48184561            48438292
    508    0.396331436697806     0.211997784117548   0.525199940498775
    509              2153481               2235403             2363409
    510     2.44313720641217      3.73359223835886    5.56835432784761
    511              2153481               2235403             2363409
    512     2.44313720641219      3.73359223835884    5.56835432784763
    513            375172385             381509245           387764152
    514     1.72826952964826      1.68905288698153    1.63951649454785
    515            494678215             500836315           506853151
    516      1.2752261016787      1.24486985949036    1.20135777294823
    517              1513682               1591188             1650061
    518     5.72665315988699      4.99358137273328    3.63313498750379
    519              5768167               5852970             5946593
    520     1.38084411454621      1.45948723125249    1.58692257953256
    521              3956925               4022130             4095048
    522     1.68981664504185      1.63444045850362     1.7966825792201
    523              4574797               4643044             4719864
    524     1.54172773032086      1.48078594660326    1.64097999986534
    525              1427146               1504172             1603166
    526      1.9586625076045      5.25659348860487    6.37378434077194
    527              3122447               3266318             3455397
    528     1.20092557138433    4.5046367323306296    5.62739999338956
    529              4374817               4499920             4614607
    530     2.77758037958912      2.81949284771596    2.51670883918496
    531              5687563               5837986             5973369
    532     2.58107591705621      2.61040132087158    2.29252154954785
    533                39636                 38179               36737
    534    -3.66108999068283     -3.74521699654759   -3.85012046502119
    535               164239                165386              166470
    536    0.728418123935715     0.695945167676197   0.653297728777027
    537            422401468             429345391           436193125
    538     1.68241908586431      1.64391545154383    1.59492430652412
    539            549897638             556739532           563424119
    540     1.27465558333488      1.24421229101553    1.20066684971816
    541            195243007             203451706           211849033
    542     4.18365310126998      4.20434981315361    4.12743012339251
    543            731736892             749852827           768211301
    544     2.49641978529849      2.47574438272275    2.44827696035345
    545            127977256             132975732           138434334
    546     3.80231203640787      3.90575337855346    4.10496104657653
    547            449503454             462591596           476615219
    548     2.90286782434457      2.91168886101596    3.03153432125904
    549                 5075                  5100                5123
    550    0.513632105107241     0.491401480242916   0.449966522678504
    551                34300                 34603               34889
    552    0.878482955573281     0.879502933541775   0.823120977302667
    553              3551533               3577319             3602000
    554    0.763082859199326     0.723429591725973   0.687560957543116
    555             19387153              19544988            19695977
    556    0.844925912065483     0.810825506471785   0.769551641465596
    557            932764512             959518064           986522345
    558      2.9094310202086      2.86820002860486    2.81435879251984
    559           2639297493            2683610483          2727263371
    560     1.72567737138772      1.67896912407667    1.62664769259659
    561           2243471653            2306245136          2369177564
    562     2.82288529591392      2.79805108818998    2.72878312099776
    563           5315840366            5389738377          5463138510
    564      1.4104735282436      1.39014729397537    1.36184964586083
    565               430610                439957              451101
    566     2.19359711344273      2.14741867941586    2.50142663513041
    567              1985384               1977424             1976780
    568   -0.384374749020571    -0.401735875452327 -0.0325729277343265
    569           1038119044            1065718318          1093457314
    570     2.66806013774206      2.65858469310578    2.60284500430255
    571           2101307817            2114766197          2128345264
    572    0.636753617473744     0.640476368627134   0.642107246619659
    573              2250685               2213967             2181225
    574    -1.25947161177743     -1.64486853006564   -1.48992834414436
    575              3377075               3322528             3269909
    576    -1.12299127483243   -1.6284011259342701   -1.59637831830094
    577               394773                402818              411237
    578     1.92636779365475      2.01739308726993    2.06848436020593
    579               458095                465158              472637
    580     1.42133256471314      1.53005466340868    1.59505191774492
    581              1536660               1522383             1507751
    582   -0.943886863627841    -0.933435957787239  -0.965773376970411
    583              2263122               2238799             2218357
    584    -1.09131295557421     -1.08057145691172   -0.91727295748817
    585               475529                488619              501863
    586     2.77099628005097      2.71551761234388    2.67441293965373
    587               475529                488619              501863
    588     2.77099628005097      2.71551761234388    2.67441293965373
    589                 <NA>                  <NA>                <NA>
    590                 <NA>                  <NA>                <NA>
    591                32697                 33452               34183
    592     2.37686428482674      2.28282449439949    2.16168689426467
    593             16411301              16790498            17188176
    594     1.84609626289302      2.28429482760974    2.34085744006463
    595             30033125              30431902            30833022
    596      1.2458784781145      1.31905268782034    1.30947924386268
    597                32236                 32141               32011
    598   -0.247862315519727    -0.295136676013591  -0.405287997089757
    599                32236                 32141               32011
    600    -0.24786231551975    -0.295136676013591  -0.405287997089734
    601              1245000               1235763             1231441
    602    -1.17134581227451     -0.74469368399036  -0.350356469998636
    603              2896023               2888985             2880967
    604   -0.247447154059097    -0.243318711825946  -0.277922768195992
    605              5148726               5414776             5693825
    606     3.90662169635098      5.03822075912781    5.02507412544639
    607             18250774              18792171            19350299
    608      2.9270346495312      2.92328568439833    2.92675246207207
    609                98164                103619              109393
    610     5.52343174486409      5.40811613248245    5.42261918724769
    611               302135                307018              314401
    612     1.63811451653174      1.60324404314335    2.37628634213334
    613            207835182             213719274           220272056
    614     2.60334171783599      2.83113375867229    3.06606974530523
    615            347044925             354462370           362633997
    616     2.00742586120907      2.13731550749519    2.30535811177926
    617             78995700              80460988            81892383
    618     1.88500485100268      1.83790252338506    1.76335370311728
    619            103945813             105442402           106886790
    620      1.4696529746789      1.42951167105736    1.36053857389621
    621                38451                 38654               38832
    622      0.6130427328136     0.526555914747926    0.45943864209696
    623                54435                 54337               54208
    624   -0.106492371991223    -0.180193480890142  -0.237689549465288
    625           2115494397            2173269404          2230743230
    626     2.76422722103939       2.7310404169319    2.64457898750227
    627           4866336912            4927146781          4986523291
    628      1.2748021098655      1.24960252649274    1.20508912437857
    629              1173001               1171843             1170744
    630  -0.0789116707501944   -0.0987699048755907 -0.0938278988679569
    631              2032544               2036855             2040228
    632    0.284333738311281     0.211874117661892   0.165461471818753
    633              3991374               4225685             4472881
    634     5.70178264070663      5.70458453195544    5.68513421829838
    635             12751995              13180551            13623541
    636     3.26662811589565      3.30546040144123    3.30569182799157
    637               374812                378170              380114
    638    0.936303549430444     0.891926304114185   0.512737776078232
    639               401268                403834              405308
    640     0.67162845452858     0.637436918202842   0.364336947162944
    641             13141153              13337081            13528193
    642     1.54738457659192      1.47994444083795    1.42276772489526
    643             47338446              47724471            48088274
    644    0.878726142111789     0.812150802847322   0.759407839480453
    645            173437036             178039708           182689261
    646      2.5811196956052      2.65379996461654    2.61152585130053
    647            305945780             311948573           317983066
    648     1.94696055831119      1.96204471262848    1.93445122763873
    649               381058                383698              386254
    650     1.08098606908643     0.690419032192582   0.663939931159043
    651               613353                614261              615025
    652    0.177216475615812     0.147929262061512    0.12429981279666
    653              1559392               1599381             1639992
    654     2.59903539816971      2.53206776883662    2.50746843851331
    655              2537949               2559255             2581242
    656    0.850550681669034      0.83599259855571   0.855447793558829
    657                65052                 62508               59875
    658    -3.63182319294761     -3.98924015151467   -4.30354928199755
    659                71898                 69025               66060
    660    -3.72003227551706     -4.07796901122163   -4.39053378535746
    661              5872479               6063132             6258534
    662     3.22033714641782      3.19496366981429    3.17194741299357
    663             19694411              20211114            20735982
    664      2.6114744795382       2.5897761215382     2.5637800990406
    665              1214843               1268535             1324867
    666     4.32242617730309      4.32478409840937      4.344938572132
    667              2946575               3012360             3081229
    668     2.16989899804321      2.20803458075482    2.26047186619935
    669               515544                517242              518278
    670     0.36221503539923     0.328819625736774   0.200092773805189
    671              1221003               1228254             1233996
    672    0.627103982385528     0.592099659929956   0.466403525955087
    673              1857233               1920235             1986636
    674     3.23655360080743      3.33598286934557    3.39951816651334
    675             12411342              12755648            13118307
    676     2.64004024351051        2.736342194188    2.80345816457226
    677             16642423              17263520            17889347
    678      3.7730778753259      3.66405668341837    3.56097914085245
    679             25333247              25923536            26509413
    680      2.3720084635771      2.30336382151543    2.23485955352487
    681            259163359             262099001           265190165
    682     1.13901204146268      1.13273805808328    1.17938793669801
    683            324809693             327824506           331015609
    684    0.930473824664759     0.928178273300475   0.973418076316719
    685               691709                719037              747144
    686     3.96414988140249      3.87474698155934    3.83451208805757
    687              1939406               1962865             1986558
    688     1.24422104996521      1.20234000182313    1.19983522844956
    689               144893                148559              152384
    690     2.08236255172329      2.49866467538063    2.54214651296246
    691               228750                232250              235750
    692     1.52146109806749      1.51846735383173    1.49575438196191
    693              2171718               2250365             2332616
    694     3.52459328670792      3.55738658936001    3.58979592679752
    695             13366885              13855221            14365168
    696     3.54918298730086      3.58817494323712    3.61442583471313
    697             52257527              54895345            57649638
    698     4.96110976626493      4.92446165064167    4.89554130315558
    699            136756848             140490722           144329764
    700     2.69550346507859      2.69369341754265    2.69592582302795
    701              3004543               3051074             3103994
    702     1.44704383282578      1.53681832282723    1.71960095461742
    703              5386223               5454678             5529811
    704      1.1795695481415      1.26291919571256    1.36800486065092
    705             13271929              13485107            13671225
    706     1.79694284127559      1.59346880368831    1.37073677080826
    707             16281779              16319868            16346101
    708    0.347475412048658      0.23366314793854   0.160613668857538
    709              3554092               3591141             3632998
    710    0.998726642610231      1.03703635791594    1.15882234278512
    711              4591910               4623291             4660677
    712    0.590930939690253     0.681072964187373   0.805392739160981
    713              3859248               3981931             4100363
    714      3.3050952685761      3.12945316717111     2.9308628860514
    715             26003965              26285110            26518971
    716     1.24233141085399      1.07535927644443   0.885774450092644
    717                10335                 10318               10294
    718  -0.0870448335824783    -0.164625031127346  -0.232874159189534
    719                10335                 10318               10294
    720  -0.0870448335824672    -0.164625031127357  -0.232874159189534
    721              3526736               3569251             3615494
    722     1.55579026598741      1.19829725539141    1.28727292519303
    723              4087500               4133900             4184600
    724     1.48621908244575      1.12877350967992  1.2189848602842701
    725            953421995             964733077           975975235
    726     1.22257842060642      1.18636679868078    1.16531279667112
    727           1236496893            1245447053          1254649810
    728    0.739035150707707     0.723831984590362   0.738911941525956
    729              1773304               1820999             1868429
    730     1.96417766701787      2.65407784012507    2.57127192305095
    731              2468855               2515192             2560649
    732     1.52050035159895      1.85946621776946    1.79115994007007
    733             12043207              12447890            12960370
    734     2.91796426416073      3.36025943920086    4.11700296194778
    735             22986615              23450481            24035320
    736     1.75531410059138       2.0179830740629    2.49393178758253
    737             57646810              59255126            60871443
    738      2.8319351661406      2.75173797684307    2.69118574430988
    739            170648620             174372098           178069984
    740     2.23514920732307      2.15849215504399     2.0985131417675
    741              2055838               2105243             2155078
    742     2.38890297943089      2.37473491874482    2.33960178946282
    743              3243311               3305868             3368573
    744     1.92079922199436      1.91043496107333    1.87901413538404
    745             20821131              21120020            21405293
    746     1.50531795351236      1.42530213293439    1.34168220864779
    747             27893911              28147267            28381078
    748    0.974731754515074     0.904184155760706   0.827239266792745
    749             38746005              39430017            40105212
    750     1.80168776528927      1.74997253229173     1.6978921895599
    751             84607501              86261250            87901835
    752     1.98712673013663      1.93575552038701    1.88401982804789
    753                14011                 14110               14108
    754     1.25688443141721     0.704103047562818  -0.014175349091706
    755                19907                 19831               19619
    756    0.135722744336851    -0.382505876810364    -1.0747885465001
    757               826104                851930              878173
    758     3.12093506747836      3.07836902440688    3.03392491202946
    759              6293166               6498818             6708217
    760     3.26552943968256       3.2156031499028    3.17128786595142
    761             23506503              23453429            23396235
    762   -0.240274327668896    -0.226039605840911    -0.2441598232723
    763             38182222              38165445            38141267
    764  -0.0585127351273288    -0.043948953332248 -0.0633705742926632
    765            211541579             221073122           230552678
    766     4.44545294727968      4.50575392556752    4.28797309878313
    767            621974508             640493810           659115651
    768      2.9627224639694      2.97750177246814    2.90741935507542
    769              3603694               3596360             3579032
    770   -0.039007912220217    -0.203720764774072  -0.482985004438138
    771              3826878               3821362             3805214
    772   0.0204626359646878    -0.144242360500613  -0.423467205495354
    773             14303977              14413592            14513343
    774    0.833047690844374       0.7634039799809   0.689678290443282
    775             23948930              24100982            24235761
    776    0.700697475242568     0.632894010472634   0.557668311641366
    777              5966051               6041725             6117343
    778     1.33460087073959       1.2604332701246    1.24382847284665
    779             10483861              10503330            10522288
    780    0.239128989611917     0.185532266654625    0.18033244147767
    781              3103824               3156489             3207898
    782     1.74526541518773      1.68254342645055    1.61555600405723
    783              5416324               5476878             5534675
    784     1.17127562545308      1.11178740173656    1.04976165973902
    785              2357720               2425915             2496025
    786     2.85499590394494      2.85137224443691    2.84906936727212
    787              3236626               3320396             3406334
    788     2.55527589181892       2.5552623734323    2.55525867519272
    789               758417                772655              787479
    790     1.91338631348953      1.87733133619103    1.91857944360679
    791              2146140               2172802             2199540
    792     1.27496198897468      1.24232342717623    1.23057692325395
    793            811888035             819758200           827660842
    794     1.00028315577345      0.96936580670264   0.964021097928637
    795           1041241254            1046468599          1052106032
    796    0.512212995281772     0.502030147184314   0.538710192105825
    797               151814                155635              159448
    798     2.53492234379594      2.48574348598075    2.42043280777465
    799               267132                271060              274901
    800     1.49313076537124      1.45972803316776    1.40708342631452
    801               755305                826610              991419
    802     4.08382684316999      9.02113585706445    18.1804250465987
    803               777943                848710             1015060
    804     3.85486532773939      8.70642929198329     17.898545339652
    805             11378651              11336529            11297334
    806   -0.322505381518045    -0.370871358103029  -0.346339811096976
    807             21451748              21319685            21193760
    808   -0.569786272532879     -0.61753095660306  -0.592402560029107
    809            105771342             105433226           105152927
    810    -0.34136940906381    -0.320178954508602  -0.266208543667034
    811            144067316             143518814           143049637
    812   -0.402681471328549    -0.381452794741516  -0.327445270108383
    813              1486527               1526528             1568217
    814     2.60292868626807      2.65533488760916    2.69434295715566
    815              8791853               9026299             9270066
    816     2.57920977781251      2.63169301631945    2.66480719140037
    817            435307330             448001350           460631298
    818     3.00412006466335      2.91610527210742    2.81917632614277
    819           1515599723            1541135031          1565718156
    820     1.76735105585351      1.68483192577094    1.59513115369579
    821             19107856              19756968            20611906
    822      2.4608597012664       3.3406679862066    4.23626307545006
    823             23661808              24397644            25382870
    824     2.18309041865558      3.06242987376478    3.95879667991777
    825              9429935               9677493             9953157
    826     2.41609660745069      2.59136764155049    2.80869071358679
    827             28831550              29540577            30332968
    828     2.25392586427754      2.42945361033328    2.64703640708656
    829              4419201               4577718             4744589
    830     3.49300318442015      3.52417101634635    3.58041901367083
    831             10698691              10974057            11263387
    832     2.50033904791585      2.54126351828797    2.60233440516001
    833              4166664               4265762             4401365
    834      1.2519166730553      2.35051128857175     3.1293891560754
    835              4166664               4265762             4401365
    836      1.2519166730553      2.35051128857175     3.1293891560754
    837                82152                 86037               90095
    838     4.66069662591661      4.62062463921632    4.60872328906206
    839               471785                482486              493430
    840     2.26568934347868      2.24285314785611    2.24291000270575
    841              2024977               2098230             2167220
    842      4.0213078378927      3.55357897134132    3.23511080610452
    843              5533329               5683334             5809774
    844     3.35235443124014       2.6748408801911    2.20036392928469
    845              3688208               3722193             3752635
    846      1.0435453690315     0.917230748141819   0.814524917663258
    847              6035655               6037817             6034436
    848     0.14600619513778     0.035814056190455 -0.0560127440734922
    849                27393                 27871               28382
    850     1.89793981774402      1.72992154233341    1.81684196866987
    851                29093                 29508               29959
    852     1.70559131630084      1.41638176095381    1.51683672327943
    853              3594797               3800778             4001225
    854     5.81892739745874      5.57182603557635    5.13947816162133
    855             10117354              10467292            10784973
    856     3.61359093578303      3.40031804628923    2.98984286765814
    857              4004805               4009310             4010029
    858    0.184998993055132     0.112426649093949   0.017931652519655
    859              7463157               7440769             7411569
    860   -0.233328451597809    -0.300431132565912     -0.393204593401
    861            247480261             257782348           268490016
    862     4.11399137266639      4.16279139126978    4.15376308078316
    863            746464327             766895808           788025400
    864   2.7193618938382502      2.73710079115408    2.75521026188737
    865              1245593               1314452             1388063
    866     5.31816009327971      5.38081269953823    5.44894013679409
    867              7317118               7662654             8029517
    868     4.53974090656815      4.61418643398604    4.67659777703664
    869            247522643             257825161           268533982
    870     4.11328513226847      4.16225274388331    4.15352053246659
    871            746546802             766978666           788110000
    872     2.71901005478739      2.73684971193542     2.7551397368333
    873             16217959              16658346            17207139
    874     2.39015774172373      2.71542800176027    3.29440269760275
    875             31893127              32425757            33079043
    876     1.48657366265728      1.67004633945113     2.0147131800192
    877                91420                 95658              100012
    878     4.62856461268554       4.5315057749685     4.4510848135384
    879               157697                161680              165725
    880     2.52695179043068      2.49436027695914    2.47107150320897
    881               340740                344226              347746
    882     1.44848670450733      1.01786952854415    1.01739098183848
    883               510572                516220              522023
    884     1.33920010200757      1.10013653489516    1.11786170080844
    885              2993058               2985293             2977317
    886   -0.289251561103309    -0.259770775137602  -0.267534008003681
    887              5372280               5372807             5373054
    888  -0.0203617202597358   0.00980913417308711 0.00459711883568263
    889              1024627               1030904             1038715
    890    0.505556857920171     0.610744329465034   0.754828505825282
    891              1997012               2000474             2006868
    892   0.0640662022616923     0.173208905073743    0.31911453657107
    893              7577769               7613645             7666670
    894    0.466911457263983     0.472320307976532   0.694032980903411
    895              8993531               9029572             9080505
    896    0.393298991357658     0.399942762807279   0.562483906481893
    897               236237                236222              236132
    898   0.0711403230470545   -0.0063497573355704 -0.0381070134217559
    899              1065764               1071886             1077735
    900    0.655855489540335     0.572780084427945   0.544190295359054
    901                32488                 33011               33441
    902     3.17725287372934      1.59700488726039    1.29418528189955
    903                32488                 33011               33441
    904     3.17725287372934      1.59700488726039    1.29418528189955
    905                42382                 42813               43966
    906    0.146395624833013      1.01180509879897    2.65748131515615
    907                82475                 82858               84600
    908   -0.370334934558794     0.463308214458562     2.0805967352947
    909              9659934               9994609            10521656
    910      3.3397092066766      3.40590317244439    5.13897617424851
    911             18084007              18583557            19432009
    912     2.64840352675744      2.72492009593396    4.46444973791979
    913                19939                 21054               22183
    914     5.73324892858665      5.44129530043708    5.22356639349639
    915                22869                 23995               25128
    916     5.06743991992045      4.80631936359624    4.61372869261365
    917              2092667               2181193             2263228
    918     4.58773196573542      4.14326457839646    3.69201379695375
    919              9613503              10005012            10365614
    920     4.43602666694242      3.99174947439079    3.54078145175603
    921            756762274             784429992           811856994
    922     3.73914944650878      3.65606465208121    3.49642444573945
    923           1857622964            1872697597          1887496209
    924    0.822454268096308     0.811501219146209   0.790229667817528
    925            278183866             279159791           280235195
    926    0.324236661116444     0.350820129877704   0.385228831182218
    927            435471706             435872410           436426410
    928      0.0729364240254    0.0920160815224023   0.127101414838336
    929              1932466               2009625             2094108
    930     3.95621797816319      3.91512299667463    4.11795495485976
    931              5565218               5711597             5874240
    932      2.6255672619467      2.59625119615377    2.80780206331491
    933             23622396              24624429            25647750
    934     4.27545921125585      4.15437538893834    4.07168494989182
    935             65311166              65821360            66319525
    936    0.821309424269957     0.778138864421305   0.753994284335371
    937              1802863               1836916             1871062
    938     1.91441473909979      1.87121215952415    1.84181056434948
    939              6801204               6929145             7057417
    940     1.91062519876215      1.86367736343867    1.83426915710039
    941              2256241               2298855             2343652
    942     1.75136498363422      1.87110187201695    1.92992236374485
    943              4819792               4885775             4954029
    944     1.26957330067416      1.35971482322199    1.38732630688867
    945            409518503             416432907           423265716
    946     1.72646933674866      1.68842285497415     1.6407946838841
    947            533741619             540553499           547224720
    948     1.30593226543138      1.27625048478748    1.23414629862566
    949               243214                252419              262306
    950     3.49563801525127      3.71486909467818    3.84213560108899
    951               945989                969313              994564
    952     2.05783916860514      2.43566320717601    2.57168770210506
    953            171079316             175613793           180193236
    954     2.57679171381989      2.65051153232341    2.60767842990558
    955            302709154             308628177           314576732
    956     1.94014757994975      1.95534985374113    1.92741798815084
    957                24292                 24459               24619
    958    0.723009202139544     0.685116831355445   0.652025619827523
    959               105036                105633              106190
    960    0.597769223628224     0.566767391590509   0.525911996154802
    961            435307330             448001350           460631298
    962     3.00412006466335      2.91610527210742    2.81917632614277
    963           1515599723            1541135031          1565718156
    964     1.76735105585351      1.68483192577094    1.59513115369579
    965            247522643             257825161           268533982
    966     4.11328513226847      4.16225274388331    4.15352053246659
    967            746546802             766978666           788110000
    968     2.71901005478739      2.73684971193542     2.7551397368333
    969               750754                752539              754262
    970    0.221355854434767     0.237478766799675   0.228696523425577
    971              1361172               1369075             1376919
    972    0.561680029157224     0.578923597211151   0.571306516699516
    973              6684800               6777044             6869247
    974     1.52448631143633      1.37047263011151     1.3513474558849
    975             10292225              10388344            10483558
    976    0.960925967691636     0.929565256440937   0.912371670625613
    977             45568517              46609279            47642112
    978     2.29421809703669      2.25825800751552    2.19174345166622
    979             67785075              68704715            69601333
    980     1.36315757853756      1.34757906352045    1.29658908503988
    981                 4766                  4926                5087
    982   3.3930492722770502      3.30199210217958    3.21609661422819
    983                 9791                  9912               10030
    984     1.26421335306978       1.2282547954907    1.18344576470028
    985              9292156               9798745            10334400
    986     5.27258840303019      5.30837128488774    5.32238200445735
    987             38360879              39439505            40562052
    988     2.71359210283265      2.77298146987654    2.80649707734517
    989              4481004               4739741             5013735
    990     5.69753833779394      5.61353644023847    5.61986534611436
    991             27146084              27946588            28773227
    992     2.95848070670913      2.90623167656329    2.91502197083402
    993             32075876              31932595            31801190
    994    -0.50676226681521    -0.447694616281555  -0.412356448619737
    995             47451626              47105171            46787786
    996   -0.758571027336172    -0.732800943705215  -0.676059672626708
    997           1182729885            1213751340          1244220885
    998     2.65000037072664       2.6228689570992    2.51036138917877
    999           2227039419            2243536298          2259259920
    1000   0.745611177806296     0.740753794443719   0.700840989914767
    1001             3084420               3096012             3108094
    1002   0.377638776778915      0.37511984900266   0.389484488932747
    1003             3313801               3317665             3322282
    1004    0.10866542041562     0.116535331321663   0.139067397498697
    1005           233532722             236200507           238999326
    1006    1.14388530085121      1.13588459113456    1.17796815968651
    1007           292805298             295516599           298379912
    1008   0.925483968943482     0.921713167161207   0.964253917136075
    1009            12428855              12700677            12984805
    1010    2.16397410749235      2.16345136994461    2.21245285438773
    1011            25864350              26167000            26488250
    1012    1.15376921505544       1.1633502238127    1.22021635439699
    1013               52538                 52696               52830
    1014   0.358477998106102     0.300283404078497    0.25396598378326
    1015              112608                112043              111427
    1016  -0.443035334521563    -0.503003495436714   -0.55130582200281
    1017            23060812              23456263            23844411
    1018    1.74046621072965      1.70028114142018    1.64123121255606
    1019            26226927              26668785            27102081
    1020    1.71089322511207      1.67071492636166    1.61167342545955
    1021                9747                 10153               10585
    1022    3.96599190596567      4.08096829640126    4.16686762677428
    1023               22715                 23497               24323
    1024     3.2801562564658      3.38472542151015     3.4549651327021
    1025              101373                101581              101716
    1026   0.188590809024712     0.204972627121298   0.132810636500791
    1027              108466                108453              108369
    1028 -0.0359495051596945 -0.011986040886740299  -0.077482916127075
    1029            21963105              22681995            23412478
    1030    3.25990938485259      3.22074378651575    3.16976901467855
    1031            82311227              83142095            83951800
    1032    1.02011634543028      1.00436180353196   0.969169266037147
    1033               48451                 50271               52146
    1034    3.73014399577226      3.68753921280386    3.66191065966812
    1035              212422                217632              222923
    1036    2.46104681641835      2.42307008348654    2.40208573602407
    1037          3142901966            3215717849          3289337823
    1038    2.32603950509018      2.31683596204158    2.28937915131122
    1039          6470821068            6552571570          6634935638
    1040    1.27457864888491       1.2633713888996    1.25697319167169
    1041               40355                 40023               39734
    1042  -0.780000498902632    -0.826101391155689    -0.7247044518907
    1043              188073                188626              189379
    1044   0.337139113464018     0.293603326396777    0.39840795546279
    1045                <NA>                  <NA>                <NA>
    1046                <NA>                  <NA>                <NA>
    1047             1704622               1705780             1719536
    1048  0.0678386264869726    0.0679098853744036   0.803200285953362
    1049             5886214               6169349             6477861
    1050    4.58581226648359      4.69803152070819    4.87970414632164
    1051            20733406              21320671            21966298
    1052    2.66182702320952      2.79308557038497    2.98322980970804
    1053            28644683              29182849            29733162
    1054    1.86684409245862      1.86133323997528    1.86818148741063
    1055            48556071              49017147            49491756
    1056   0.935290168887955     0.945094240817718   0.963593457802097
    1057             4075803               4268709             4476767
    1058    4.50249985192117      4.62436571109042     4.7589694519562
    1059            11188040              11564870            11971567
    1060    3.17893646862542      3.31267042148986    3.45623668042667
    1061             4170453               4169863             4183242
    1062   0.163857394368567   -0.0141481450099478   0.320336244803581
    1063            12160881              12224753            12330490
    1064   0.701855595519876     0.523850608635102   0.861222619917424
                       x2007               x2008                x2009
    1                  42729               42906                43079
    2      0.408048969163451   0.413382967375507    0.402396309678717
    3                  96787               97996                99212
    4       1.22771081489399    1.24139737678817     1.23323132057229
    5              149231302           155383774            161776165
    6       3.61360813971072     4.1227757967293     4.11393727635938
    7              482406426           495748900            509410477
    8       2.74710885116298    2.76581597609149     2.75574529766985
    9                5987030             6162823              6443215
    10      2.68846842633562    2.89394890008399     4.44926873597444
    11              25903301            26427199             27385307
    12      1.79319572747206    2.00233326232255     3.56128837446668
    13             130133054           135972155            142060079
    14      4.51846166506525    4.48702371958471     4.47733140656629
    15             327612838           336893835            346475221
    16       2.8284092989347    2.83291615086219     2.84403720240236
    17              12028087            12642253             13287180
    18      4.96958261730382    4.98001182832443     4.97550440874176
    19              20909684            21691522             22507674
    20      3.63958930439587    3.67090920225351     3.69348247645612
    21               1452398             1473392              1495260
    22      1.49221506952818    1.43512421072918     1.47328791317413
    23               2970017             2947314              2927519
    24     -0.75571875541991  -0.767342959142321   -0.673894046451936
    25                 69810               67692                65663
    26      -3.1499777492501   -3.08092614364707     -3.0432403598387
    27                 78168               76055                73852
    28     -2.59249693414403   -2.74035938783357    -2.93936722302096
    29             186027900           192585424            199211212
    30      3.21005995366575    3.52502178436676     3.44044105850918
    31             337498222           346630443            355754908
    32      2.67571743331854    2.70585751411751     2.63233226748061
    33               4875629             5827655              6693200
    34      18.5806846251198    17.8365567832908     13.8477396037629
    35               5872624             6988685              7992644
    36      18.1279839755253    17.3990859957442     13.4229206026704
    37              36034446            36459843             36897033
    38      1.19016125791417    1.17361488361579     1.19196781572589
    39              39876111            40273769             40684338
    40      1.00629733064648   0.992294094354656     1.01428389337816
    41               1914970             1898649              1883514
    42    -0.889415093262459  -0.855937609056045    -0.80033995301265
    43               3004393             2983421              2964296
    44    -0.732665962263764  -0.700492224911231     -0.6431061166878
    45                 49561               49072                48554
    46    -0.915871352131617  -0.991562674803217    -1.06120267389806
    47                 56383               55891                55366
    48    -0.801982747478947  -0.876432921866332   -0.943767847051186
    49                 22966               22815                22670
    50    -0.759107468953717  -0.659664697463174   -0.637574985992819
    51                 82016               83251                84534
    52      1.37623327243605    1.49457906187611      1.5293679498773
    53              17666406            18049707             18451571
    54      1.96893475127689    2.14645819019787     2.20200639528942
    55              20827622            21249199             21691653
    56      1.82499679529254    2.00391140524272     2.06083316277243
    57               4832038             4823638              4812679
    58     -0.16052793407859  -0.173990969023818   -0.227452142237554
    59               8295487             8321496              8343323
    60     0.324146535285463   0.313041437661508    0.261953189906348
    61               4530068             4643726              4759039
    62      1.51533013846802    2.47801108138936     2.45286958706445
    63               8581300             8763400              8947243
    64      1.13385546615645    2.09985403259251     2.07614826384645
    65                783656              837579               903684
    66      6.20772060591364    6.65454390587286       7.596415415179
    67               7944609             8278109              8709366
    68      3.67179220330916    4.11209752356589     5.07844376468302
    69              10360589            10448114             10537701
    70     0.786651495882078   0.841239515661802    0.853791422542778
    71              10625700            10709973             10796493
    72     0.734330830765919   0.789976845480149    0.804599572619861
    73               3593837             3746862              3905656
    74      4.10792735599933    4.16982557036081     4.15070692699142
    75               8647761             8906469              9172514
    76      2.87555724136521    2.94774231330122     2.94335369072878
    77               3393537             3576677              3768369
    78      5.93142934061169     5.2561416766791     5.22081247892827
    79              14757074            15197915             15650022
    80      3.03278641140169    2.94356863526933      2.9314075345183
    81              40699664            42125647             43585126
    82      3.63403697703824    3.44369107188052     3.40591977571723
    83             144135934           145421318            146706810
    84      1.05111719423699   0.887833017097369    0.880093475599957
    85               5378166             5366322              5357245
    86    -0.248678761764876  -0.220466620603334   -0.169290725144742
    87               7545338             7492561              7444443
    88    -0.735282283991309   -0.70192274454598   -0.644281362547054
    89                920746              982987              1044748
    90      6.95875972555163    6.54116842065566     6.09350917548755
    91               1040532             1110343              1179453
    92      6.91804991621101     6.4936855776179        6.03817950789
    93                294366              298736               303224
    94      1.45947528462747    1.47363491680767     1.49115660854148
    95                357666              362795               368057
    96      1.40838277430791    1.42383464201547     1.43998810307513
    97               1787553             1771372              1754255
    98    -0.527207527074408  -0.909325793425457   -0.971012219476719
    99               4007876             3943392              3877750
    100    -1.24500087046096   -1.62201585384377    -1.67861781190568
    101              7005597             7024602              7049739
    102    0.166805781560358   0.270915781365443    0.357203605168211
    103              9560953             9527985              9504583
    104   -0.458847535622636  -0.345415054298517   -0.245915441295321
    105               134557              138353               142147
    106     2.84484399316828    2.78204890432151     2.70533432749151
    107               297173              305671               314171
    108        2.88259288843     2.8194899348079     2.74280635114816
    109                64888               65273                65636
    110    0.564095738182979   0.591576774606153    0.554585139879867
    111                64888               65273                65636
    112    0.564095738183002   0.591576774606153    0.554585139879867
    113              6320212             6475247              6632261
    114     2.45026370100787    2.42340016762147      2.3959018126589
    115              9711152             9880593             10051317
    116     1.75023288737608    1.72976143588269     1.71311404940389
    117            159201638           161361139            163480329
    118     1.39819368757787    1.34733900695877      1.3047718800979
    119            190779453           192672317            194517549
    120     1.03202723327326   0.987284202776065    0.953147864558066
    121                88086               87941                87786
    122   -0.223395360781282  -0.164747490778413   -0.176410045351384
    123               271444              272635               273791
    124    0.376106107267713   0.437804768187742    0.423113733464088
    125               279864              285549               291203
    126     2.07305081204229    2.01098722049407     1.96069753761838
    127               378748              384568               390311
    128     1.58075344729259    1.52495500833732     1.48232308730953
    129               221313              229241               237280
    130      3.5922421262198    3.51958627960986     3.44670264751531
    131               681614              689737               697678
    132     1.23319303054383    1.18468503010311     1.14473127595882
    133              1139430             1193231              1248597
    134      4.6380941562096    4.61366165127325     4.53557670741519
    135              1966977             2007320              2048997
    136     1.96495716378833    2.03026526526897     2.05499060689998
    137              1677549             1720153              1766203
    138     2.25509761175561    2.50794407863865     2.64188042553996
    139              4375569             4467233              4564540
    140      1.8735897550277    2.07326328004891     2.15485409702233
    141             26441461            26789863             27158023
    142     1.19901884209158    1.30902990520455     1.36489394771959
    143             32889025            33247118             33628895
    144    0.971135141368055    1.08290711607014     1.14175809912162
    145             65168162            65021934             64961672
    146   -0.246511491353502  -0.224385644020458  -0.0926794948916836
    147            105378748           105001883            104800475
    148   -0.372245215653024   -0.35762903541044   -0.191813702998061
    149              5552336             5625324              5697988
    150    0.929048227949449    1.30598063263603     1.28345836397255
    151              7551117             7647675              7743831
    152    0.893690977823134    1.27061807385776     1.24948463127384
    153                47296               47778                48261
    154     1.01366218849144    1.01395571776263     1.00584985729178
    155               153225              154475               155721
    156    0.812559760565262   0.812484157524789    0.803367355727897
    157             14334623            14488641             14647961
    158     1.04735564599465    1.06871633922945      1.0936181774877
    159             16495538            16661462             16833447
    160    0.980589313160657    1.00084678976019     1.02694120751704
    161            595670841           616481190            637407288
    162     3.51125555182775    3.43395768889026     3.33810246782671
    163           1317885000          1324655000           1331260000
    164    0.522271866392275    0.51238693163744    0.497381400884935
    165              9130213             9411847              9699938
    166     3.07451022731112    3.03801908722743     3.01502788469479
    167             19817700            20244449             20677762
    168     2.16087988372223    2.13051547484823     2.11781907640103
    169              9079573             9454260              9843943
    170     4.00022754976239    4.04382685038063      4.0390908603398
    171             18251866            18777081             19319274
    172     2.78067096838021    2.83697090124967     2.84662200490483
    173             23193341            24262452             25380957
    174     4.52113892616682    4.50647585211136     4.50691982996456
    175             60289422            62249724             64270232
    176     3.21553799022491    3.19974361408747     3.19424675479088
    177              2448691             2549703              2673540
    178     4.41453378024999    4.04232857379959     4.74265551565953
    179              3956329             4089602              4257230
    180     3.68155876257458     3.3131079690881     4.01710592389119
    181             33266384            33827174             34381839
    182     1.75289276355675    1.67170463604494     1.62640449458684
    183             43306582            43815313             44313917
    184     1.23996744251378    1.16787354604705     1.13154130595269
    185               171911              175534               179390
    186     2.01835946203309    2.08558553022451     2.17294503750045
    187               616899              629470               642493
    188     2.00423062675791     2.0172880272233     2.04777262901454
    189               299625              307192               314622
    190     2.58052881218523     2.4941266513212     2.38989589813538
    191               504733              510336               515638
    192     1.16559727886391    1.10397557326972     1.03356365735023
    193              3026939             3123883              3220427
    194     3.23689250633054    3.15249014205758     3.04371801687519
    195              4440019             4501921              4563127
    196     1.40273700394929    1.38455388597677     1.35039395502168
    197              3480295             3500786              3522052
    198    0.607205524833134   0.588771928816385    0.607463581035802
    199              6884705             6923924              6964240
    200    0.592064823515102   0.569654037464204    0.582270978133209
    201              8601178             8616795              8632201
    202    0.204110466445062   0.181403516925037    0.178630734178103
    203             11269887            11276609             11283185
    204   0.0821729969258322  0.0596278925041623   0.0582984063263004
    205               130064              131527               132200
    206     1.83741978705634     1.1185516731597    0.510377368177239
    207               144056              145880               146833
    208     1.97486219793502    1.25822530732751     0.65115206175087
    209                49647               51123                52602
    210     3.00562369574523    2.92965280884495     2.85196480411381
    211                49647               51123                52602
    212     3.00562369574523    2.92965280884495     2.85196480411381
    213               730040              741066               752074
    214     1.52143257703564    1.49903629937856     1.47450332081723
    215              1073873             1092390              1110974
    216     1.73158929128947    1.70962168593317     1.68691516238933
    217              7565828             7621676              7657912
    218    0.488301512379783   0.735450034824268    0.474306871788842
    219             10298828            10384603             10443936
    220    0.583542205385113   0.829412602993033    0.569729451418061
    221             62833410            62875807             62877220
    222    0.127167671405373  0.0674524949232638  0.00224726200833851
    223             82266372            82110097             81902307
    224   -0.133718572600486  -0.190142844695674   -0.253383410163402
    225               664929              678931               693345
    226     2.19159111666247    2.08392343270082     2.10082084431839
    227               865196              882886               901103
    228     2.13179519553233    2.02400160866773     2.04234815532952
    229                46219               46429                46637
    230    0.496698740478761   0.453329504443147    0.446995350242368
    231                68775               68782                68787
    232   0.0479940671094341  0.0101775991131422  0.00726907951967227
    233              4712839             4751268              4785983
    234    0.669712104371388   0.812104326450457    0.727990766226651
    235              5461438             5493621              5523095
    236    0.443466054321374   0.587547590258978    0.535079062079429
    237              6583425             6789862              6998896
    238     3.15199534705477    3.08754907254629      3.0321804973215
    239              9402206             9522948              9648061
    240      1.2633758696885    1.27601221390825     1.30524977858784
    241             22207751            22849463             23520104
    242     2.78998744748105    2.84862438842223     2.89279288193908
    243             33983827            34569592             35196037
    244     1.62790867702938    1.70897167476718     1.79590393152832
    245            853873812           881700219            909762514
    246     3.32325964182962     3.2588430057157     3.18274787680414
    247           1926362850          1940824032           1955097471
    248    0.762867502737748   0.750698758543848    0.735431897207661
    249           1148947196          1177775174           1206885159
    250     2.57595862908786    2.50907771047817     2.47160796411896
    251           2814101547          2857856055           2901516356
    252     1.61334790182177    1.55483045900191      1.5277291843868
    253           1057376409          1087741858           1117968806
    254      2.9179078950508    2.87177288442794     2.77887145536326
    255           2164155292          2179914111           2195169711
    256     0.73307700209115   0.728174131415329    0.699825737308601
    257            244580289           246269946            248153732
    258     0.57053997125908   0.690839399572369    0.764927280245558
    259            373795966           375281256            377054106
    260    0.272477437000518   0.397353137834557    0.472405688175371
    261            615204410           619583863            623834147
    262    0.668461611306427   0.711869571936248    0.685990106233604
    263            878767025           882357452            885846236
    264    0.358495986492827   0.408575526602178    0.395393498642989
    265              8850960             9031505              9213492
    266     2.03585655806213     2.0193092495047     1.99499112930922
    267             14251835            14496797             14742766
    268      1.7181335805958    1.70420581194769     1.68247933289289
    269             35418195            36101887             36798803
    270     1.96328035872334    1.91194642943421     1.91201820310785
    271             82218755            83844783             85501064
    272     1.95167431348286    1.95838290327697     1.95615522230144
    273            248153262           250125886            251703061
    274    0.829324206503969   0.794921648057965    0.630552489077445
    275            332660837           334290264            335377620
    276    0.520106085823514   0.489816298995251    0.325273008848399
    277               966133             1021965              1066563
    278      4.5565018205489    5.61810177071226     4.27140842948172
    279              2926168             3005779              3083888
    280     1.58711319244676    2.68430517830464     2.56543695688394
    281             35159317            35833174             36260460
    282     2.15770109034777    1.89844652332989     1.18537790236856
    283             45226803            45954106             46362946
    284     1.85108130845179    1.59533049906372     0.88573598057307
    285               918084              913914               910446
    286   -0.642967542479925  -0.455241457878645   -0.380188578712754
    287              1340680             1337090              1334515
    288   -0.456188535090031  -0.268133719541207   -0.192768077420229
    289             13214505            13927358             14670369
    290     4.20717814789972    5.25400172848094     5.19746380080012
    291             81996185            84357105             86755585
    292     2.85154384116412    2.83863160180599     2.80357593483215
    293            317183435           319163672            320838559
    294    0.641894335735117   0.624319173540684    0.524773696675609
    295            438484072           439892213            440934530
    296    0.336192686903146   0.321138460874366    0.236948272598767
    297            282189955           291572706            301538672
    298     2.96725709665691    3.32497696454148     3.41800374140644
    299            723581503           741028426            759511035
    300     2.36211050032091     2.4111897454073     2.49418353621971
    301              4398523             4426008              4454167
    302    0.583073550661644   0.622924637607349    0.634201339099274
    303              5288720             5313399              5338871
    304    0.425429832100631   0.465549284507694    0.478246393482911
    305               451176              458642               465636
    306     1.63190217996907    1.64124420294959     1.51342637065858
    307               890648              896731               901383
    308    0.853009376352656   0.680664088099223    0.517432196093315
    309             49694312            50131182             50550197
    310    0.931335018090846   0.875272981984948    0.832363289760852
    311             64021737            64379696             64710879
    312    0.616493932446319   0.557563757631959    0.513102876178491
    313                19509               19624                19726
    314    0.580903831201854   0.587740940106377    0.518425557561371
    315                48361               48411                48429
    316   0.0993028196710854   0.103335684804041   0.0371747216177147
    317                24444               24253                24060
    318   -0.733679006385432  -0.784446596058745   -0.798961052554881
    319               109532              108704               107868
    320   -0.699624839506227  -0.758815202826041   -0.772033459891083
    321              1297812             1349725              1404767
    322     3.85679503175068    3.92210984170203     3.99705847382003
    323              1549774             1599978              1653542
    324     3.09560977258065    3.18807653024551      3.2929774631171
    325             49351705            49913475             50463084
    326     1.12718995503098    1.13186919618862     1.09510526789302
    327             61322463            61806995             62276270
    328    0.778666112441256   0.787032622318939    0.756390859593407
    329              2099115             2107603              2103652
    330    0.192552045667768   0.403545544001272   -0.187640081602738
    331              3860158             3848449              3814419
    332   -0.521646734447263  -0.303790529341209   -0.888185042750245
    333             11538602            12005167             12483116
    334     4.01459532645514    3.96390305529739     3.90398708167934
    335             23708320            24326087             24950762
    336     2.60546378550717    2.57232722220713     2.53550492031822
    337                29996               30398                30819
    338     1.37289637701367    1.33127773351264     1.37545661279669
    339                29996               30398                30819
    340     1.37289637701367    1.33127773351264     1.37545661279669
    341              3133543             3237696              3346220
    342     3.15931511582049    3.26976524709282     3.29693848189259
    343              9547082             9779785             10021323
    344     2.29335572388692    2.40819421004136     2.43976227038861
    345               943877              986934              1031782
    346     4.45778944316756    4.46073068507279     4.44395155203353
    347              1764883             1820542              1878119
    348     3.08345897475552    3.10498597160244     3.11364851085582
    349               564766              585325               606541
    350     3.55137710613252    3.57557624567978     3.56050785649872
    351              1450572             1488431              1527196
    352     2.54710395639989    2.57645836046792      2.5710829245713
    353               579725              624295               671695
    354     7.45112950674857    7.40691598950807     7.31813553802184
    355               948814              994971              1043686
    356     4.68160680952619    4.75008073899975     4.78003659146047
    357              8308341             8371303              8433780
    358    0.750673416564129   0.754959746748096    0.743552267452411
    359             11048473            11077841             11107017
    360    0.254757581935775   0.265457836726293    0.263026401346927
    361                40090               40354                40624
    362    0.645631731453399   0.656359573807059    0.666850256983647
    363               111725              112478               113249
    364    0.661840745939182   0.671715214592327    0.683128733664499
    365                47213               47196                47360
    366  -0.0148253259195849 -0.0360135160498233     0.34688473380775
    367                56555               56328                56323
    368   -0.386485846698679  -0.402186876651222 -0.00887697402292811
    369              6402661             6568347              6734651
    370     2.60519830178315    2.55485174643189     2.50037869960926
    371             13477017            13739299             14000190
    372     1.97596216713401    1.92744763752889     1.88106343818981
    373               154584              154703               154718
    374    0.154728139917005  0.0769511850625091   0.0096955281068424
    375               164763              164725               164580
    376   0.0534243178340463 -0.0230660905213468   -0.088064262231673
    377               206856              204412               201856
    378    -1.11387617506875   -1.18853339920571    -1.25829931206321
    379               756521              754150               751258
    380   -0.243714507438187  -0.313900504723792   -0.384215250307409
    381            907142967           918189482            928347146
    382      1.2079580009836    1.21772591552265     1.10627100387543
    383           1153978848          1163599191           1172260321
    384    0.811028319230587   0.833667186939621    0.744339637479172
    385              6916300             6957800              6972800
    386    0.859633272217027   0.598238786949717    0.215353334344996
    387              6916300             6957800              6972800
    388    0.859633272217027   0.598238786949717    0.215353334344996
    389              3954703             4096745              4240048
    390     3.61944851348056    3.52872523806306      3.4381833970701
    391              7924462             8101777              8277302
    392     2.28811343765187    2.21289893099486     2.14336495538161
    393            180774103           188502254            196609636
    394     3.89629625150994    4.27503213776146     4.30094697965787
    395            579864063           596301357            613535903
    396      2.8253670364292    2.83468058271443     2.89024094909112
    397              2355577             2362537              2367290
    398    0.286028713681515   0.295033356489087    0.200979934356861
    399              4310217             4309705              4305181
    400  -0.0218526602655707 -0.0118794578559279    -0.10502751669287
    401              4199239             4361621              4527471
    402     3.85418015642172    3.79404588592128     3.73197281986112
    403              9420826             9575247              9730638
    404     1.65399026348443    1.62585605595915      1.6098133370454
    405              6779707             6820246              6861506
    406    0.628309488410444   0.596165590682043    0.603140958545005
    407             10055780            10038188             10022650
    408   -0.154915158060948   -0.17509736747488   -0.154908813950621
    409           2074174495          2123356756           2173482087
    410     2.37589663721241    2.37117277830572     2.36066458725601
    411           4314977758          4358318999           4402357503
    412    0.999029146684833    1.00443718208385     1.01044700973252
    413           2487970052          2553174003           2619813336
    414     2.60870221573842    2.62076912652485     2.61005841833335
    415           5612664364          5686502257           5761813095
    416     1.31982033933444    1.31555867608265     1.32437893447231
    417            413795557           429817247            446331249
    418     3.79179320315129     3.8718854586445      3.8420985000632
    419           1297686606          1328183258           1359455592
    420     2.40130274424419    2.35007835166019     2.35451951465571
    421            162751283           168812982            175167463
    422     3.69790372278274    3.72451687523716       3.764213465526
    423            439576756           450269469            461410153
    424     2.40558309351159    2.43250191327222     2.47422593957842
    425            111639888           115006628            118403751
    426     3.00272263603897     2.9711356042071     2.91106412062858
    427            234858289           237936543            240981299
    428     1.31184759614566    1.30217053507895     1.27153208994292
    429            251044274           261004265            271163786
    430     3.85275237665238    3.96742408870875     3.89247317471997
    431            858109850           877913789            898045439
    432     2.39911022411792    2.30785592310821       2.293123795553
    433                42123               42604                43096
    434     1.11484939400169    1.13542357803043     1.14820397973132
    435                81100               81997                82915
    436     1.08103799975669    1.09997001077714     1.11333254369235
    437            355789232           364989009            374274816
    438     2.59963313831962    2.55287302199413     2.51230889183139
    439           1189691809          1206734806           1223640160
    440     1.46637175001498    1.42239150883577     1.39119493033754
    441                 <NA>                <NA>                 <NA>
    442                 <NA>                <NA>                 <NA>
    443                 <NA>                <NA>                 <NA>
    444                 <NA>                <NA>                 <NA>
    445              2680715             2744952              2782090
    446     3.22296317427124    2.36800384846691     1.34388553349095
    447              4398942             4489544              4535375
    448     2.89096000464164    2.03870801117202     1.01566327592341
    449             49802044            50925490             52059325
    450     2.36536640248403    2.23075566946806     2.20203496139747
    451             72319418            73318394             74322685
    452      1.4536370817991    1.37188502638988     1.36047026604448
    453             19739613            20139353             20893380
    454   -0.771786082657502    2.00483327003265     3.67566007342798
    455             28660887            29218381             30289040
    456   -0.850221916193164    1.92646265531762      3.5987933376422
    457               290623              296449               297832
    458     2.65799969542739    1.98483023000493    0.465437224707066
    459               311566              317414               318499
    460     2.53008548940662    1.85957217059918    0.341241978970882
    461              6579915             6702316              6869011
    462     1.84268754417262     1.8431310987376     2.45669983200117
    463              7180100             7308800              7485600
    464      1.7761008691798     1.7765791059698     2.39020733801215
    465             39722857            40056298             40308358
    466    0.678681719512836   0.835914949481381    0.627292740575372
    467             58438310            58826731             59095365
    468    0.504933687397673   0.662469252941527    0.455613449577732
    469              1436617             1447087              1458036
    470    0.779226627841202   0.726152615294569      0.7537754402416
    471              2701221             2711373              2722401
    472     0.42891014375515   0.375125564741813    0.405906286917041
    473              5332251             5551184              5758537
    474     8.05927986742454    4.02377628622861     3.66722110511884
    475              6473457             6632873              6780493
    476     6.34380871094335    2.43277656402626     2.20117695037558
    477            112827761           114107975            115228215
    478     1.28800909827127     1.1282732314255    0.976949089372748
    479            128001000           128063000            128047000
    480    0.114908847726211  0.0484253945978966  -0.0124946312294096
    481              8765446             8942684              9133481
    482     1.27288562353003    2.00183665637905     2.11111246374793
    483             15484192            15776938             16092822
    484     1.14385123349431    1.87296209612182     1.98240805691531
    485              8527849             8934612              9357689
    486      4.6525850875403    4.65955620465768     4.62656353605351
    487             38036793            39186895             40364444
    488     2.96582480091057    2.97884515479232     2.96069187652137
    489              1859060             1876703              1899444
    490    0.947912305045165   0.944553022910198     1.20446979040125
    491              5268400             5318700              5383300
    492    0.953586964300523   0.950220144755314     1.20726564406139
    493              2662452             2728958              2821239
    494     2.36329860655678    2.46723486509103     3.32562988061287
    495             13714791            13943888             14155740
    496     1.74325561857738    1.65663905731394     1.50789194873236
    497                45762               47519                49338
    498   3.7742849392769098    3.76755786517659     3.75649441548909
    499               101998              103966               105996
    500     1.89533635203291     1.9110717364553     1.93374349375449
    501                14924               14903                14878
    502     -0.1473050149187  -0.140812039225567   -0.167892319750605
    503                47015               47156                47286
    504    0.300354899935738   0.299455470084081    0.275301416968664
    505             39740941            40093884             40351067
    506    0.631491671818636    0.88418882208575    0.639403400754965
    507             48683638            49054708             49307835
    508    0.505234032920336   0.759316681850927    0.514682827535803
    509              2506769             2650930              2795550
    510     5.88896046397856    5.59158488996994     5.31183453845193
    511              2506769             2650930              2795550
    512     5.88896046397853    5.59158488996994     5.31183453845195
    513            393943321           400026850            406062201
    514     1.59353797098809    1.54426504415848     1.50873647606404
    515            512746342           518513539            524224926
    516     1.16270185720913    1.12476609340686     1.10149235659591
    517              1710668             1772834              1836272
    518     3.60716805821068    3.56954585790763     3.51580332863139
    519              6041348             6135861              6229930
    520     1.58087153393389    1.55232447601189     1.52146861670776
    521              4179597             4254276              4316746
    522     2.04363915574689    1.77097643807313     1.45772829006783
    523              4809608             4887613              4951135
    524     1.88355982920981    1.60884613336982     1.29127979774658
    525              1698015             1781757              1852759
    526     5.74794976737039    4.81400343414898     3.90759233676875
    527              3632740             3783887              3905066
    528     5.00498279274666    4.07646036851064     3.15228963928178
    529              4721471             4835208              4950582
    530     2.28936963539516    2.38037440420652     2.35809977983508
    531              6097177             6228370              6360191
    532     2.05147873321277    2.12887841520837     2.09437470204341
    533                35319               33936                32602
    534    -3.93633588167955   -3.99446649418428    -4.01027616382286
    535               167518              168576               169688
    536    0.627569517206789   0.629587850277381    0.657477003615403
    537            442953513           449611195            456215612
    538     1.54986119966929    1.50302047610128     1.46891738316258
    539            569972223           576386180            582738079
    540     1.16219802794775    1.12531045219022     1.10202139128319
    541            220009033           229089026            238596629
    542     3.85179950290355    4.12710009047674     4.15017828047337
    543            786317083           804637337            823699833
    544     2.35687524727001    2.32988121409032     2.36907922655844
    545            143828135           149837719            155951332
    546     3.89628847421623    4.17830906310508     4.08015621220181
    547            490959927           505273533            519948553
    548     3.00970414459216     2.9154326479277     2.90437140312274
    549                 5142                5159                 5180
    550    0.370190388663247    0.33006534188151    0.406229400887893
    551                35150               35401                35675
    552    0.745302497633524   0.711545007164178    0.771009503458442
    553              3625538             3648476              3670930
    554    0.651344433373979   0.630685506862606    0.613548980302319
    555             19842044            19983984             20123508
    556    0.738871905202862   0.712803199874156    0.695753116801849
    557           1014041518          1042088543           1070808942
    558     2.78951339921247    2.76586554910665     2.75604210342037
    559           2770966712          2814667244           2858820038
    560     1.60246133412394    1.57708614148086     1.56866834238114
    561           2432204155          2497190572           2563534727
    562     2.66027299758778    2.67191456220499      2.6567517811372
    563           5536137785          5609875336           5684902603
    564     1.33621497727687    1.33193128248703     1.33741415818159
    565               462227              474734               488087
    566     2.43648511888669    2.66985347719485      2.7739021475837
    567              1983465             1995014              2009169
    568    0.337605696087044   0.580575262050289    0.707013588718218
    569           1121439436          1150003096           1178727308
    570     2.55905023833422    2.54705328554185     2.49775084083774
    571           2141896101          2155744556           2169498384
    572    0.636684152200601   0.646551202625318    0.638008244609495
    573              2157761             2137953              2114156
    574    -1.08155355773235  -0.922228080815704    -1.11931507180531
    575              3231294             3198231              3162916
    576      -1.187947957259   -1.02848317546539    -1.11034575437238
    577               419557              429035               438935
    578     2.00297020821596    2.23391079618646     2.28128376496061
    579               479993              488650               497783
    580     1.54438684650549    1.78749663225793     1.85177523043645
    581              1494021             1476965              1452351
    582   -0.914799375192887   -1.14818362507299    -1.68056838595402
    583              2200325             2177322              2141669
    584   -0.816175506582982    -1.0509395443743    -1.65102496759116
    585               515330              529038               543021
    586     2.64802980741037    2.62527907737455     2.60877302924839
    587               515330              529038               543021
    588     2.64802980741037    2.62527907737455     2.60877302924839
    589                 <NA>                <NA>                 <NA>
    590                 <NA>                <NA>                 <NA>
    591                34887               35541                36132
    592     2.03858225170899    1.85726926617157     1.64919396468654
    593             17589282            17995882             18409274
    594     2.30680337069635    2.28532146753545     2.27116054330186
    595             31232633            31634992             32042877
    596     1.28772191974814    1.28003720148764     1.28110624620562
    597                31823               31862                32401
    598   -0.589029493886282   0.122477835833443      1.6775209368822
    599                31823               31862                32401
    600   -0.589029493886293   0.122477835833443      1.6775209368822
    601              1227699             1224475              1222042
    602   -0.304334270369286  -0.262950496795147   -0.198895066093037
    603              2874299             2868833              2865213
    604   -0.231718324871942  -0.190349156188775    -0.12626339171085
    605              5986055             6291521              6609861
    606     5.00503432164096    4.97702563872023     4.93597710170821
    607             19924958            20513599             21117092
    608     2.92652452166253    2.91149152987358      2.8994727895916
    609               114444              119964               125727
    610     4.51387178188176    4.71060773882683     4.69211918935939
    611               325126              336883               349037
    612     3.35435582926146    3.55228895579949     3.54422447127984
    613            227159256           234297106            241543564
    614     3.12667894651149     3.1422228289038     3.09284998168096
    615            371324802           380294099            389332369
    616     2.39657756081817    2.41548556726895     2.37665270740897
    617             83306647            84700556             86085517
    618     1.71223590367281    1.65938240735703     1.62190199212404
    619            108302973           109684489            111049428
    620     1.31623681916458    1.26753574174522     1.23674369230562
    621                38975               39077                39173
    622     0.36757659595946   0.261364367510436     0.24536753524391
    623                54038               53816                53593
    624   -0.314099624496591  -0.411668206131852   -0.415235819000647
    625           2288376020          2347352853           2407583395
    626     2.58356897490171    2.57723523077296     2.56589212495358
    627           5045177858          5104601803           5164954050
    628     1.17626176750971    1.17783647420424      1.1823105764005
    629              1170224             1170314              1171282
    630  -0.0444260672125847 0.00769053952931664   0.0826786563644444
    631              2043559             2046898              2050671
    632     0.16313293218841   0.163258078088837     0.18415802473701
    633              4732876             5006021              5291488
    634     5.65003315248508    5.61083364140145     5.54581037942845
    635             14080912            14551117             15032635
    636     3.30208691127537    3.28476390707979      3.2555744003116
    637               381739              384526               387733
    638    0.426592128487567   0.727427793133083    0.830555146775326
    639               406724              409379               412477
    640    0.348755082369901   0.650655460565174    0.753906931031973
    641             13719323            13891315             14065094
    642     1.40293988563575    1.24585476543478     1.24323004644903
    643             48445647            48729486             49015836
    644    0.740412549138029   0.584181978347596    0.585912068881096
    645            187275549           191965087            196838003
    646     2.51043108658698    2.50408450277723     2.53843866931906
    647            324080392           330293304            336675037
    648     1.91750022310937    1.91708975716125     1.93214119775192
    649               388857              391621               394521
    650    0.671648263371455   0.708286858855469    0.737783544066644
    651               615875              616969               618294
    652    0.138110347731053   0.177475866720179    0.214529295893828
    653              1682307             1727329              1775498
    654     2.54747019289925    2.64102190803345     2.75046621723598
    655              2605643             2633887              2666713
    656    0.940879931171425    1.07812241088145     1.23859259914584
    657                57196               54506                51848
    658    -4.57750898604785   -4.81731785179343    -4.99944262272516
    659                63050               60032                57056
    660    -4.66353575517151   -4.90503090271691    -5.08445117030295
    661              6461615             6735426              7028957
    662     3.19333141668851    4.15017729826855     4.26572711294931
    663             21280513            21845571             22436660
    664     2.59213192117964    2.62064294405699        2.66980274555
    665              1384075             1448147              1517970
    666     4.37199694371064     4.5252761669145     4.70891077844988
    667              3153508             3233336              3322616
    668     2.31869406123366    2.49989376010022     2.72380028814205
    669               519269              519756               519754
    670    0.191027548893521   0.093741735889452  -3.8479668305094e-4
    671              1239630             1244121              1247429
    672    0.455526400702738   0.361630847401639    0.265537672832765
    673              2056034             2128693              2206182
    674     3.43361283727671    3.47292919379471     3.57552433356195
    675             13495463            13889423             14298932
    676     2.83448193292953    2.87740603419066     2.90572339354206
    677             18520504            19150609             19773206
    678     3.46730466546394    3.34560740169375      3.1993272243952
    679             27092604            27664296             28217204
    680     2.17608980403121    2.08818532017883     1.97892341382596
    681            268301627           271462240            274499918
    682     1.17329464311015    1.17800739240393     1.11900572249017
    683            334185120           337406357            340466060
    684    0.957511039909903   0.963907968134549    0.906830276466891
    685               776396              807205               839596
    686     3.84047616588734    3.89149639738792     3.93431604897639
    687              2011492             2038552              2067919
    688      1.2473242341721    1.33630166361813      1.4303035696071
    689               156245              160140               163862
    690     2.50216372401023    2.46231445399878     2.29761775360097
    691               239250              242750               245950
    692     1.47371088194967    1.45230768383707     1.30961560699145
    693              2418521             2508220              2602152
    694     3.61658117612962    3.64171419105866     3.67654567649365
    695             14897873            15455175             16037915
    696     3.64120636284668    3.67254476327787     3.70117077417358
    697             60532139            63546553             66691001
    698     4.87904797043417    4.85983082808399     4.82972708690892
    699            148294028           152382506            156595758
    700     2.70962693730599     2.7196868027914     2.72738484297876
    701              3158566             3214997              3273164
    702     1.74284578038208    1.77082974299414     1.79306766360908
    703              5607453             5687744              5770639
    704     1.39429680103578  1.4217078710387001     1.44691351664322
    705             13848922            14044865             14263741
    706     1.29141366585093    1.40494528793482     1.54638742479771
    707             16381696            16445593             16530388
    708    0.217521601194242   0.389292461499527    0.514284544827762
    709              3684159             3744286              3805760
    710     1.39840743411394      1.618866751592     1.62847639699914
    711              4709153             4768212              4828726
    712     1.03473451527827    1.24633301535167      1.2611272893768
    713              4215682             4329273              4441607
    714     2.77358750728389    2.65882480851806      2.5616618363182
    715             26713655            26881544             27026941
    716    0.731449423498353   0.626509658660505    0.539422885134866
    717                10267               10243                10233
    718    -0.26263329137351  -0.234032286243018   -0.097675334977918
    719                10267               10243                10233
    720   -0.262633291373521  -0.234032286243007   -0.097675334977918
    721              3646829             3675355              3709702
    722     0.86295237642558   0.779170376466414    0.930182330420022
    723              4223800             4259800              4302600
    724    0.932407687512324   0.848701402422591    0.999728031208421
    725            987351758           998729288           1009413551
    726     1.16565693390775    1.15232792242621      1.0697856895131
    727           1264209610          1273851101           1282681964
    728    0.761949663069728   0.762649715975499     0.69324138379028
    729              1915893             1963882              2013010
    730     2.50858616507577    2.47392941831475     2.47079876942857
    731              2605700             2651028              2697537
    732     1.74406111328877    1.72461361834632     1.73916451569952
    733             13558604            14172038             14748396
    734     4.61587130614328    4.52431533511857     4.06686744701079
    735             24697686            25375277             26013714
    736     2.75580271034461    2.74354042722868     2.51598041668669
    737             62558385            64313863             66149573
    738     2.73361322023607     2.7674926074888     2.81432287523418
    739            181924521           185931955            190123222
    740     2.14152397156301    2.17888921699852      2.2291625346925
    741              2205430             2256550              2308270
    742     2.30955834281551    2.29145970246075     2.26612279567452
    743              3431614             3495276              3559343
    744     1.85414918870892     1.8381638086969     1.81636347456987
    745             21679951            21907680             22117000
    746     1.27496885275026    1.04493438570835    0.950928330821102
    747             28600387            28806185             29009326
    748    0.769759369616088   0.716987182658426    0.702724346623641
    749             40786251            41478745             42170961
    750     1.68387383948539    1.68360880350824      1.6550728852387
    751             89561377            91252326             92946951
    752     1.87034863111627    1.87043157688419     1.84004284987183
    753                14070               14019                13952
    754   -0.269714124750785  -0.363131873989889   -0.479068521977503
    755                19366               19102                18826
    756    -1.29795332493524   -1.37259095767935    -1.45541484875174
    757               904791              931864               959378
    758      2.9860364852927    2.94829034882128     2.90982765154533
    759              6921066             7137988              7358890
    760     3.12366111372057    3.08611399149295      3.0478162226257
    761             23340838            23300939             23274004
    762   -0.237058174108073  -0.171087005249751   -0.115663063732749
    763             38120560            38125759             38151603
    764  -0.0543050208038707  0.0136373796827091    0.067763227021235
    765            239611575           250170269            261593710
    766     3.92920918489612    4.40658761998456     4.56626642552797
    767            677333522           697003893            718121906
    768     2.76398701386624    2.90408939777824     3.02982712321808
    769              3555977             3533033              3511647
    770   -0.646252309966882  -0.647314085202659    -0.60715506445333
    771              3782995             3760866              3740410
    772   -0.585620757159528   -0.58667739163101    -0.54540189449208
    773             14604648            14691216             14796839
    774    0.627140102183045   0.590992994541443    0.716381271778334
    775             24356506            24469047             24581509
    776    0.496973102800538   0.460993034958389    0.458556258017028
    777              6193886             6267228              6337144
    778      1.2434824546765    1.17714751506627     1.10940421451619
    779             10542964            10558177             10568247
    780    0.196304384487306   0.144191279853498   0.0953308592370666
    781              3258272             3308734              3360926
    782      1.5581099509185    1.53686453877243      1.5650892359349
    783              5590145             5645148              5702574
    784    0.997237882485964   0.979118914218707     1.01212367069034
    785              2568140             2647431              2727093
    786      2.8482434993004     3.0407834355184     2.96464708614345
    787              3494496             3591977              3689099
    788     2.55525196026943    2.75135880229092     2.66795074941001
    789               802659              818295               833815
    790     1.92767045216445    1.94802525107175     1.89662652221998
    791              2226078             2251506              2276149
    792     1.20652500068196    1.14227803338427     1.09451185117872
    793            835817852           844082403            851509459
    794    0.985549827425558   0.988798095209859    0.879897030622018
    795           1058252873          1064631968           1070085408
    796    0.584241589064476   0.602794961653743    0.512237107649966
    797               162971              165995               168642
    798     2.18544206073036    1.83853965532446     1.58204574442311
    799               278178              280558               282283
    800     1.18501637126492   0.851928064290659    0.612963661508526
    801              1206233             1417399              1583479
    802     19.6120309069375    16.1321221569127     11.0800823270074
    803              1231893             1444277              1610274
    804      19.360428664312    15.9056839638554     10.8795500522929
    805             11159030            11001524             10936933
    806    -1.23177330661917   -1.42152269565098   -0.588839831638732
    807             20882982            20537875             20367487
    808    -1.47722298197717   -1.66638264306554   -0.833088754778975
    809            105037446           105055527            105149987
    810   -0.109882303334064  0.0172123796302606    0.089873956739492
    811            142805114           142742366            142785349
    812    -0.17108203294568 -0.0439492591634852   0.0301077605196216
    813              1611415             1655603              1700405
    814      2.7173369783368    2.70526179293714     2.67011650600764
    815              9523168             9781996             10043737
    816      2.6937067262308    2.68159866469225     2.64057029628242
    817            473211288           485918296            498928274
    818     2.73103240153689    2.68527153139256     2.67740031752169
    819           1589218004          1612407426           1636052663
    820       1.500898990661    1.45917186576247     1.46645547637164
    821             21496783            22402324             23318830
    822     4.20344260259126    4.12614074480669     4.00964852177366
    823             26400068            27437353             28483797
    824     3.92920488472079    3.85387472835296     3.74300660963915
    825             10251288            10555557             10870585
    826     2.95135689486631    2.92490948797874     2.94080664258584
    827             31191163            32065241             32948155
    828     2.78996436965565    2.76377910390166     2.71626568948597
    829              4918692             5099067              5287643
    830     3.60378306820491    3.60149403176997     3.63150071059154
    831             11563869            11872929             12195029
    832     2.63281190597884    2.63754388814397     2.67674749775798
    833              4588599             4839396              4987573
    834     4.16600286360931    5.32151708302625     3.01594991475788
    835              4588599             4839396              4987573
    836     4.16600286360933    5.32151708302623      3.0159499147579
    837                94333               98752               103397
    838     4.59664062212028    4.57805813407154     4.59642913730803
    839               504619              516001               527833
    840      2.2422684243981    2.23050143791026     2.26712423337321
    841              2238411             2319217              2407911
    842     3.23209988976844    3.54633895708249     3.75299379183658
    843              5939163             6090860              6259842
    844     2.20265426987169    2.52210725035976     2.73656583182459
    845              3794143             3864044              3933080
    846     1.10003019993797    1.82557415552717     1.77085305006433
    847              6044131             6068099              6091188
    848    0.160532322783851   0.395765788534978    0.379776014021418
    849                28856               29246                29664
    850     1.65628021888922    1.34248685884495     1.41913771937573
    851                30372               30700                31059
    852     1.36913510961371    1.07415232421642     1.16259668589342
    853              3825068             4121412              4415772
    854    -4.50243193366773    7.46195781952307     6.89868541136301
    855             11118092            11444870             11730037
    856     3.04199150056875    2.89679033014887     2.46112222974988
    857              4010264             4009546              4009752
    858  0.00586013503665218 -0.0179056611545222  0.00513760680910835
    859              7381579             7350222              7320807
    860   -0.405458541825536  -0.425705551624613   -0.400994905201716
    861            279319896           291310174            303790003
    862     4.03362484808373    4.29266879005282     4.28403472101184
    863            809934231           832555779            855798400
    864     2.78021888634554    2.79301048581067     2.79171937619809
    865              1466385             1549034              1633665
    866     5.48909382549184    5.48313224803031     5.31944462200473
    867              8417823             8823888              9229227
    868     4.72268670452429    4.71113454589955     4.49127071476695
    869            279364356           291355929            303836244
    870     4.03314840056257    4.29244917701669     4.28352875564788
    871            810019264           832642735            855885698
    872     2.77997538414689    2.79295468706285     2.79146890052431
    873             17841558            18491119             19104263
    874     3.68695225859452    3.64071904482782      3.3158836953026
    875             33808469            34550707             35254103
    876     2.20510006894699    2.19542032500793     2.03583677752239
    877               104489              109066               113687
    878     4.37916239177486    4.28714008659474     4.14958546754157
    879               169845              174004               178128
    880     2.45564683066573     2.4192031021932     2.34241055303069
    881               351332              354957               358616
    882     1.02593171383605    1.02650100188308     1.02555227559956
    883               527946              533938               539987
    884     1.12823575341268    1.12857216018121     1.12653378438719
    885              2970070             2964549              2960369
    886   -0.243703784090505  -0.186060860612821   -0.141099022585442
    887              5374622             5379233              5386406
    888   0.0291784011934372  0.0857552995195914    0.133257321027673
    889              1049080             1055309              1069460
    890    0.992921733830878   0.592002542509754     1.33202331472936
    891              2018122             2021316              2039669
    892    0.559207815536831   0.158140843634363    0.903875535210913
    893              7738188             7813274              7894625
    894    0.928518962327078    0.96565304883884     1.03580660075872
    895              9148092             9219637              9298515
    896    0.741552514625445   0.779033290770134    0.851904412936677
    897               236628              240262               243744
    898    0.209831704030784    1.52407068363579     1.43884996164704
    899              1084008             1089870              1094886
    900    0.580366640025196   0.539314000922246    0.459182518639854
    901                33811               33964                34238
    902     1.10035012550169   0.451494680650414    0.803499821546966
    903                33811               33964                34238
    904     1.10035012550167   0.451494680650392    0.803499821546966
    905                44460               45755                46241
    906     1.11733005981253     2.8711166831291     1.05657750605133
    907                85033               86956                87298
    908    0.510514982843497    2.23628271543202     0.39253094807516
    909             11285415            11783890             12056702
    910     7.00755747028532    4.32221603529221     2.28873434714227
    911             20703005            21474059             21827220
    912     6.33572039641122    3.65667896993469     1.63121662489933
    913                23325               24486                25657
    914     5.01995169693105    4.85757789183161     4.67149132168307
    915                26268               27422                28581
    916     4.43687060923807     4.2994142605737     4.13965504582889
    917              2344847             2430718              2521561
    918   3.5428041775584602    3.59665323927383     3.66914672313974
    919             10722731            11098664             11496128
    920     3.38718987073326    3.44588600538683     3.51855423082844
    921            839229870           866970174            894927354
    922     3.37163764090207    3.30544764809193     3.22469916940879
    923           1901960228          1916309337           1930470829
    924    0.766307181494312   0.754437910359911    0.738998225733738
    925            281435734           282934946            284731959
    926     0.42840407679698   0.532701366202488    0.635132925573643
    927            437109725           438254595            439878377
    928    0.156570497188753   0.261918217445285    0.370511118086498
    929              2184128             2276619              2371202
    930     4.20889771742969    4.14747834290649     4.07055519211195
    931              6047537             6222482              6398624
    932     2.90743917538544    2.85177808134201       2.791410472239
    933             26697288            27767512             28849963
    934     4.01061387426357    3.93047170053379     3.82420062386752
    935             66826754            67328239             67813654
    936    0.761916078459878    0.74762381126891    0.718381364649211
    937              1905930             1942198              1980522
    938     1.84638943269497    1.88502429879477     1.95401249066004
    939              7188391             7324627              7468596
    940     1.83882431198902    1.87748684470733     1.94647975674617
    941              2390895             2441308              2495771
    942      1.9957376302811    2.08661884151999      2.2063738256255
    943              5024894             5100083              5180957
    944     1.42031742897792    1.48524549123721     1.57329741344839
    945            430021395           436677002            443278595
    946     1.59608462122645    1.54773857240289     1.51177940898293
    947            553771247           560186837            566539590
    948     1.19631419428568    1.15852710568774     1.13404181969381
    949               272261              282131               291841
    950     3.72494038549419    3.56103370431122     3.38376313348189
    951              1019362             1043076              1065540
    952     2.46277702115281    2.29970986950706     2.13076728178527
    953            184707409           189317656            194110910
    954      2.5051844898329    2.49597296879412     2.53185788440145
    955            320585896           326701327            332985938
    956     1.91023791295537    1.90757955240801     1.92365640437085
    957                24769               24895                25002
    958    0.607436872212521   0.507410882486229    0.428884157422539
    959               106638              106932               107144
    960    0.420997859015547   0.275319741267334    0.198060566623799
    961            473211288           485918296            498928274
    962     2.73103240153689    2.68527153139256     2.67740031752169
    963           1589218004          1612407426           1636052663
    964       1.500898990661    1.45917186576247     1.46645547637164
    965            279364356           291355929            303836244
    966     4.03314840056257    4.29244917701669     4.28352875564788
    967            810019264           832642735            855885698
    968     2.77997538414689    2.79295468706285     2.79146890052431
    969               755996              757713               759628
    970    0.229629740341825   0.226860081612185    0.252415378845168
    971              1384861             1392803              1401191
    972    0.575137910503384   0.571848981207468    0.600432585058036
    973              6962852             7058984              7158322
    974     1.35346672883516    1.37119712723396     1.39744641638699
    975             10580395            10680380             10784504
    976    0.919463523549751   0.940565309661396    0.970187555948585
    977             48660868            49675599             50732011
    978     2.11580992581424    2.06386714883569     2.10432452861863
    979             70468869            71320726             72225639
    980     1.23873183716008    1.20159345885056     1.26081212492584
    981                 5251                5420                 5598
    982     3.17302688150943    3.16772807958325      3.2313575641707
    983                10149               10272                10408
    984     1.17945764928369    1.20465684809241     1.31529942625988
    985             10896766            11479108             12061617
    986     5.29879116679841    5.20626395601047     4.94995743890031
    987             41716497            42870884             43957933
    988     2.80637123627598     2.7296238795863     2.50402075339375
    989              5303439             5608933              5930056
    990     5.61743317338077     5.6005027307773     5.56731511060272
    991             29629804            30509862             31412520
    992     2.93354099227772    2.92692289648893      2.9156563947553
    993             31694730            31605908             31547453
    994   -0.335328946357134   -0.28063558630836   -0.185120841608538
    995             46509355            46258189             46053331
    996   -0.596871080118823  -0.541496740974727   -0.443841324029999
    997           1274334502          1305264310           1336774453
    998     2.42027901661528    2.42713415915973     2.41408140547412
    999           2274211146          2289934559           2306134012
    1000   0.661775383507006   0.691378767871015     0.70741991015997
    1001             3121576             3135892              3150496
    1002    0.43283259139241   0.457566062741001    0.464623763980443
    1003             3328651             3336126              3344156
    1004    0.19152206004265   0.224313675740569    0.240409128463508
    1005           241795278           244607104            247276259
    1006    1.16306776101756    1.15618567133535     1.08529029428822
    1007           301231207           304093966            306771529
    1008   0.951055242772428   0.945865287282592    0.876651298802912
    1009            13301003            13648397             14014762
    1010    2.40596182438203    2.57826329193129     2.64891238299525
    1011            26868000            27302800             27767400
    1012    1.42347489680303    1.60532736295221       1.687340968602
    1013               52967               53150                53348
    1014   0.258986694473885    0.34490266669937    0.371838397219122
    1015              110824              110316               109840
    1016  -0.542631060920161  -0.459438268596005   -0.432421320990234
    1017            24223737            24590433             24944692
    1018    1.57831699448907    1.50244446623473     1.43035892158866
    1019            27525097            27933833             28327892
    1020     1.5487692066186    1.47403998165424     1.40082958318807
    1021               11041               11530                12029
    1022    4.21777118701715    4.33367178239677     4.23680667309679
    1023               25191               26115                27044
    1024    3.50643831160466    3.60230740187263     3.49553073401693
    1025              101894              102152               102354
    1026   0.174844109175561   0.252884288340362    0.197549280405744
    1027              108337              108397               108404
    1028 -0.0295331003595483  0.0553674103119773  0.00645753479219667
    1029            24160637            24932764             25739048
    1030    3.14556105122946    3.14580232139383     3.18264544200066
    1031            84762269            85597241             86482923
    1032    0.96076781166711   0.980254857535931     1.02939211623888
    1033               54088               56113                58211
    1034    3.65648731049987    3.67551649505018     3.67068255760404
    1035              228345              233952               239689
    1036    2.40312228258581  2.4258322931578302     2.42262835797189
    1037          3363570859          3439970487           3516826565
    1038    2.25677750339115     2.2713845256322     2.23420748202483
    1039          6717641730          6801408360           6885490816
    1040    1.24652440524548    1.24696483329723     1.23625066382576
    1041               39524               39362                39215
    1042  -0.529916201315545  -0.410719843147077   -0.374155723661575
    1043              190478              191787               193176
    1044   0.578640418070375   0.684867888236991    0.721630924369264
    1045                <NA>                <NA>                 <NA>
    1046                <NA>                <NA>                 <NA>
    1047             1733404             1747383              1761474
    1048   0.803261832927916   0.803213477569189    0.803171844793122
    1049             6804009             7143108              7495790
    1050    4.91216340050244    4.86359784720767     4.81935538462532
    1051            22641538            23329004             24029589
    1052    3.02768141948774    2.99112094190605     2.95885526727336
    1053            30305632            30923017             31565718
    1054    1.90705812608283    2.01672235144201     2.05708649293564
    1055            49996094            50565812             51170779
    1056    1.01387721267549    1.13308133870917     1.18929507013328
    1057             4698029             4931812              5175542
    1058    4.82419249319878    4.85634071080162     4.82376018015044
    1059            12402073            12852966             13318087
    1060    3.53292144460649    3.57109657699196      3.5548434351805
    1061             4201195             4211896              4232267
    1062   0.428246462759909   0.254389397924437    0.482488069423371
    1063            12450568            12550347             12679810
    1064   0.969118750298355   0.798207023794335     1.02626500717403
                       x2010               x2011                 x2012
    1                  43206               43493                 43864
    2      0.294373510368369   0.662063111068539     0.849393249627712
    3                 100341              101288                102112
    4       1.13154104048985   0.939355909629835     0.810230587788097
    5              168456076           175415651             182558745
    6       4.12910702883826    4.13138852884119      4.07209616660715
    7              523459657           537792950             552530654
    8       2.75792914247424    2.73818484544645      2.74040483423965
    9                6691382             7004588               7360701
    10      3.77927906385172    4.57449331406255      4.95898098432825
    11              28189672            29249157              30466479
    12      2.89490410370988    3.68950830230738      4.07762772858795
    13             148404549           154982054             161743874
    14      4.46604707294298    4.43214513592842       4.3629696635715
    15             356337762           366489204             376797999
    16       2.8465357411519    2.84882577221777      2.81285093462125
    17              13967811            14683555              15432363
    18      4.99558074843681     4.9972692003532      4.97386377959222
    19              23364185            24259111              25188292
    20      3.73479765843852    3.75879638504539      3.75870252194126
    21               1519519             1546929               1575788
    22      1.60937326447224    1.78778378467889      1.84837893643447
    23               2913021             2905195               2900401
    24     -0.49646196338738  -0.269017331764615    -0.165151040121679
    25                 63522               62611                 62940
    26     -3.31492983581545   -1.44453210701534     0.524091011711652
    27                 71519               70567                 71013
    28     -3.20999418688968   -1.34005355374823     0.630034574524577
    29             205465837           210892180             216302063
    30      3.13969526976223    2.64099525216935      2.56523641606816
    31             364427661           372351065             380383408
    32      2.43784493340004     2.1742048828725      2.15719619332899
    33               7132067             7239445               7343475
    34      6.35090085977911    1.49434517646557      1.42676178340442
    35               8481771             8575205               8664969
    36      5.93976536580988    1.09556271332912      1.04134460783085
    37              37055902            37543830              38027774
    38     0.429649599145157    1.30814131603172      1.28077370283392
    39              40788453            41261490              41733271
    40     0.255582398485167    1.15305928006386      1.13690569378967
    41               1869128             1855213               1843080
    42    -0.766716914319411   -0.74724976842942    -0.656142903197955
    43               2946293             2928976               2914421
    44    -0.609179750926293  -0.589489609644367    -0.498170160314836
    45                 48044               47522                 46937
    46     -1.05593229406713   -1.09244959814226     -1.23864847065879
    47                 54849               54310                 53691
    48    -0.938173192497606  -0.987558296772346      -1.1462982343373
    49                 22485               22292                 22380
    50    -0.819404429676686  -0.862055051538081     0.393983317648936
    51                 85695               86729                 87674
    52       1.3640660545341    1.19938337496143       1.0837075607137
    53              18767085            19056040              19414834
    54       1.6955021612661    1.52795741614347      1.86533013697943
    55              22031750            22340024              22733465
    56      1.55570626204128    1.38952731561803      1.74582000067359
    57               4800510             4792887               4817487
    58    -0.253173145767403  -0.158921841854284     0.511947895650249
    59               8363404             8391643               8429991
    60     0.240394299500766   0.337080841831645      0.45593747231891
    61               4835557             4920166               5008847
    62      1.59505668203145    1.73459458477812      1.78634791175657
    63               9054332             9173082               9295784
    64      1.18978770786007    1.30300085487651      1.32876382733755
    65                971253             1032093               1096506
    66      7.21072487195949    6.07570677801742      6.05399815388462
    67               9126605             9455733               9795479
    68      4.67947760941816    3.54274498118795      3.52997284776032
    69              10639649            10784163              10856360
    70     0.962809676840454    1.34911743933303     0.667241432726619
    71              10895586            11038264              11106932
    72     0.913639391631709     1.3010029017038     0.620163579337378
    73               4070440             4241480               4418712
    74       4.1325342779679    4.11611632222051      4.09359859670972
    75               9445710             9726380              10014078
    76      2.93492666293192    2.92811115740547      2.91501212217311
    77               3970062             4183204               4409695
    78      5.21394298500172    5.22957483914856       5.2728065972188
    79              16116845            16602651              17113732
    80      2.93926755839508    2.96973830872596      3.03188007722477
    81              45202909            46903386              48658361
    82      3.64454978121574    3.69284258339114      3.67337850835789
    83             148391139           150211005             152090649
    84       1.1415513368569    1.21893869127295      1.24357124646827
    85               5347166             5337678               5331472
    86    -0.188314947521282  -0.177597398592412    -0.116335428781667
    87               7395599             7348328               7305888
    88    -0.658275446635908  -0.641228920515067    -0.579220596364389
    89               1075702             1075052               1087280
    90      2.91977636980506 -0.0604439205775808      1.13101321621974
    91               1213645             1212077               1224939
    92      2.85774564487366   -0.12928111452105      1.05556298562381
    93                307677              311718                315323
    94      1.45787250945238    1.30484015374869      1.14985773504039
    95                373272              377950                382061
    96      1.40695589315095     1.2454535327817      1.08183699838334
    97               1736255             1717279               1697524
    98      -1.0313770515203   -1.09894334172017     -1.15703426450386
    99               3811088             3743142               3674374
    100     -1.7340375845377   -1.79893469913603      -1.8542587666769
    101              7081770             7114020               7150971
    102    0.453328159855854   0.454360841936171       0.5180666800968
    103              9483836             9461643               9446836
    104   -0.218522755959381  -0.234282914157686    -0.156617600248906
    105               145682              149043                152506
    106     2.45644306384036    2.28086908696299      2.29690846205685
    107               322106              329538                337059
    108     2.49432602566338    2.28109896258836      2.25663156677437
    109                65124               64564                 64798
    110   -0.783118104363585  -0.863616495823483     0.361775877335588
    111                65124               64564                 64798
    112   -0.783118104363585  -0.863616495823495     0.361775877335588
    113              6791318             6952386               7114992
    114     2.36992600149234     2.3439878294354      2.31191971440345
    115             10223270            10396246              10569697
    116     1.69628237644672    1.67782849865133      1.65463539887486
    117            165594717           167726203             169827068
    118      1.2850668095719      1.278956660978      1.24477663663604
    119            196353492           198185302             199977707
    120     0.93941802248079   0.928589629509177     0.900343360601223
    121                87550               87329                 87148
    122   -0.269197589221943  -0.252746319051398    -0.207477277725948
    123               274711              275486                276197
    124    0.335459437797166   0.281717462132222     0.257756861693629
    125               296885              302374                307643
    126     1.93242399648658    1.83198033408753      1.72753580260807
    127               396053              401506                406634
    128     1.46041834447089    1.36744365525444      1.26910407285782
    129               245470              253839                262288
    130     3.39338617853067    3.35254676028094      3.27429289063623
    131               705516              713331                721145
    132     1.11717717159158    1.10160983275966      1.08946783207717
    133              1305449             1362903               1408979
    134     4.45265219502154    4.30699407357279      3.32483449805582
    135              2091664             2134037               2175425
    136     2.06095163216795     2.0055569612499      1.92085566899296
    137              1812952             1851877               1880110
    138     2.61244113541796    2.12432633644236      1.51305664368655
    139              4660067             4732022               4773306
    140       2.071208345015    1.53227695856897     0.868655128872181
    141             27522537            27847821              28166078
    142     1.33326871439499    1.17495264216968      1.13636233305053
    143             34004889            34339328              34714222
    144     1.11186407517168   0.978697786570341      1.08581726304756
    145             64768515            64654506              64524893
    146   -0.297339945314206  -0.176025341942761    -0.200470172952834
    147            104421447           104174038             103935318
    148   -0.361666299699507   -0.23693312734882    -0.229154983893395
    149              5759681             5825978               5889928
    150     1.07689611583261    1.14447911246303      1.09168911257723
    151              7824909             7912398               7996861
    152      1.0415580576588    1.11187894263206      1.06181932886789
    153                48734               49068                 49266
    154    0.975315736828003   0.683015272594517     0.402709678420115
    155               156933              157819                158621
    156     0.77530181672538   0.562984406870747      0.50689022702539
    157             14806204            14963678              15120117
    158     1.07451372348701    1.05795158197495      1.04003108729206
    159             17004162            17173573              17341771
    160      1.0090337555584   0.991361115471139     0.974635115583974
    161            658498663           679390629             700996454
    162     3.25536549409244    3.12337717694482      3.13065659552565
    163           1337705000          1345035000            1354190000
    164    0.482959688678361   0.546457594880274     0.678345458846803
    165              9996116            10296507              10603000
    166     3.00771238108969    2.96080939584703      2.93322684379616
    167             21120042            21562914              22010712
    168      2.1163624191381     2.0752447134814      2.05543493401974
    169             10248917            10666950              11098737
    170     4.03156992826644    3.99781348132645       3.9681141781799
    171             19878036            20448873              21032684
    172     2.85121531529997    2.83123673541846      2.81498378244419
    173             26565134            27835187              29167345
    174     4.56004356901677    4.67013356521778      4.67488217943795
    175             66391257            68654269              70997870
    176     3.24688069782067    3.35179391507681      3.35665613143748
    177              2807228             2920696               3024450
    178     4.87940826333228    3.96244244534646      3.49073127278007
    179              4437884             4584216               4713257
    180     4.15589722236467    3.24414124523082      2.77600777570712
    181             34940430            35492726              36031220
    182     1.61161225163206    1.56831626358983       1.5058010780647
    183             44816108            45308899              45782417
    184     1.12688471543895    1.09358307436582      1.03966499994087
    185               183510              187888                192517
    186      2.2706955211584    2.35768787001422      2.43384208941777
    187               656024              670071                684553
    188     2.08414518723241    2.11863032791113      2.13823935913559
    189               322218              328762                335379
    190     2.38564168551052    2.01057496227175      1.99271508951728
    191               521212              527521                533864
    192     1.07518998845602    1.20318062074503      1.19524498910285
    193              3315819             3410168               3502521
    194     2.91906924871365    2.80569049966737       2.6721437984372
    195              4622252             4679926               4736593
    196      1.2873898093698    1.24002656207939      1.20358030533586
    197              3543701             3566759               3594354
    198    0.614670084371255   0.650675663663506     0.773671560091387
    199              7004428             7044357               7088996
    200    0.577062249434263   0.570053686039756     0.633684522235313
    201              8648121             8664868               8683386
    202    0.184255877917833     0.1934617439347     0.213485541924243
    203             11290417            11298710              11309290
    204   0.0640748450108155  0.0734247089104104    0.0935951943767368
    205               133691              135409                136353
    206     1.12152395404942    1.27686610304306     0.694728326126186
    207               148703              150831                152088
    208     1.26551415533004    1.42089766276671     0.829929595036991
    209                54074               55492                 56860
    210     2.75993369995976    2.58853873843781      2.43532400043746
    211                54074               55492                 56860
    212     2.75993369995976    2.58853873843781      2.43532400043746
    213               763114              771857                777911
    214     1.45727069828883    1.13918693597077     0.781282204142003
    215              1129686             1145086               1156556
    216     1.67026099599159    1.35400253526204     0.996688012391421
    217              7673029             7681562               7693579
    218    0.197209093679863   0.111145922134052     0.156317297473518
    219             10474410            10496088              10510785
    220     0.29136167418042   0.206747667335417     0.139925655740972
    221             62940432            61940177              62064608
    222    0.100481934472227   -1.60197231472939     0.200687484378332
    223             81776930            80274983              80425823
    224   -0.153198446937304    -1.8537146287573     0.187727800567293
    225               707774              721944                736126
    226     2.05971228873621    1.98227398617218      1.94537261709032
    227               919199              936811                954297
    228      1.9883070131748    1.89789156839079      1.84933894374145
    229                46818               47012                 47316
    230    0.387352685920391   0.413514390792854      0.64456164128827
    231                68755               68742                 68888
    232  -0.0465312422058471 -0.0189095035364085     0.212163125432016
    233              4815111             4844002               4872608
    234    0.606766049074667   0.598214062880133     0.588807879418738
    235              5547683             5570572               5591572
    236    0.444197154509531   0.411737855195019     0.376272242619955
    237              7209913             7410669               7603609
    238     2.97044623184602    2.74638339836508      2.57022843655067
    239              9775755             9903737              10030882
    240     1.31483782216629    1.30068199219472      1.27563736367599
    241             24217375            24935851              25678117
    242      2.9214805958999    2.92362164020675      2.93325834377157
    243             35856344            36543541              37260563
    244     1.85870199052442    1.89839428094337      1.94310213435967
    245            938080077           965894856             994473135
    246      3.1126324248616    2.96507512332553      2.95873601795019
    247           1969241762          1984322091            2001317846
    248    0.723457076171513   0.765793682167498     0.856501828865646
    249           1236639202          1266355697            1295870129
    250     2.46535826363576    2.40300444559254      2.33065891912673
    251           2946338808          2991353712            3035261244
    252     1.54479404906031    1.52782510544185      1.46781478311514
    253           1148291153          1177159568            1206613250
    254     2.71227129390942    2.51403269323977      2.50209765954177
    255           2210203758          2225992094            2243776727
    256     0.68486946246864   0.714338483176164     0.798953107153295
    257            250188825           252203843             254248697
    258    0.820093650656844   0.805398882224267     0.810794147970228
    259            379132241           381278903             383476086
    260    0.551150343393942   0.566204022727774      0.57626660764916
    261            627940793           630961782             634989639
    262    0.658291313444238   0.481094560773343     0.638367824313008
    263            889222824           891279641             894762079
    264    0.381170892055366   0.231305016525312     0.390723386892674
    265              9396971             9577064               9752988
    266     1.97184718035533    1.89836698895406      1.82026264040711
    267             14989585            15237728              15483883
    268     1.66031048053538    1.64188311720888      1.60252187198822
    269             37535116            38356023              39184092
    270     1.98116039971524    2.16346474675773      2.13592791674744
    271             87252413            89200054              91240376
    272     2.02763965508429    2.20764281063758      2.26158734403113
    273            253074480           253287677             254568545
    274     0.54485590860574  0.0842427889212729     0.505696927371631
    275            336171621           335442349             336183135
    276      0.2367483554806  -0.216934433022828     0.220838544151732
    277              1107213             1147380               1183034
    278     3.74047182272589    3.56350351789818      3.06012427527278
    279              3147727             3207570               3252596
    280     2.04894664970192    1.88330360533393      1.39398056061833
    281             36535850            36773882              36904876
    282    0.756607893274885   0.649389460315482     0.355581830036495
    283             46576897            46742697              46773055
    284    0.460408305051304   0.355338396471428    0.0649259625617232
    285               906655              902194                899089
    286    -0.41725864442961  -0.493242835001277     -0.34475460828661
    287              1331475             1327439               1322696
    288    -0.22805796852935  -0.303582823643979    -0.357944411443807
    289             15455093            16283910              17152352
    290     5.21088478232665    5.22389106918417       5.1957803337979
    291             89237791            91817929              94451280
    292     2.82098186260834    2.85029677836164      2.82765624318832
    293            322210410           322501582             323810708
    294    0.427582957695563  0.0903670368688694     0.405928551383042
    295            441552554           440769682             441419873
    296    0.140162304821075  -0.177299846396082     0.147512641307301
    297            311855774           321936916             332341553
    298     3.42148552010602    3.23262958087798      3.23188689550595
    299            778427274           797489095             817111040
    300     2.49058119346482    2.44876067895842      2.46046561928223
    301              4492880             4543014               4593267
    302    0.865385899923003    1.10967469180396      1.10008671573527
    303              5363352             5388272               5413971
    304    0.457494535465831   0.463558707499131     0.475809486683209
    305               472236              478549                484647
    306     1.40746476113547    1.32797483667207      1.26621818914863
    307               905169              908355                911059
    308    0.419141607071656   0.351360475814814     0.297238780271001
    309             50963811            51375729              51793062
    310    0.814895002170066   0.805006988460646     0.809033957422758
    311             65030575            65345233              65662240
    312    0.492821125690305   0.482694781758347     0.483953489768883
    313                19812               19881                 19950
    314    0.435025219418949    0.34766870482082     0.346464156167298
    315                48410               48386                 48392
    316  -0.0392403891899056 -0.0495888270007087    0.0123995123017319
    317                23990               24064                 24162
    318   -0.291363369492964   0.307987091582606      0.40642033299429
    319               107588              107887                108232
    320   -0.259914003442924   0.277526573301423     0.319268822321146
    321              1463559             1525981               1591211
    322     4.09996879177844    4.17663412806507       4.1857879622497
    323              1711105             1772500               1836705
    324     3.42197068660406    3.52516187757304      3.55822259981442
    325             51030310            51600211              52130345
    326     1.11777108530512    1.11059917110884      1.02214552018811
    327             62766365            63258810              63700215
    328    0.783888646646516   0.781506562236639     0.695353132322269
    329              2102941             2100564               2099468
    330  -0.0338040788243874  -0.113096106861441   -0.0521900797364174
    331              3786695             3756441               3728874
    332   -0.729475256932528  -0.802164039694326    -0.736565518814038
    333             12969707            13468281              13986163
    334     3.82393962157783    3.77209578725329      3.77311183733799
    335             25574719            26205941              26858762
    336     2.46999616717291    2.43818163019966      2.46059584044277
    337                31262               31701                 32160
    338     1.42719195710431    1.39449236537241      1.43752181837621
    339                31262               31701                 32160
    340     1.42719195710431    1.39449236537241      1.43752181837621
    341              3458976             3575843               3695666
    342     3.31412409568336    3.32283610348839      3.29598291863036
    343             10270728            10527712              10788692
    344     2.45827846352306    2.47131110815721      2.44875299812511
    345              1078326             1126672               1176942
    346     4.41224342084751    4.38583159000324      4.36513946955446
    347              1937275             1998212               2061014
    348     3.10116024088171    3.09704342340045      3.09453140006576
    349               628628              651781                676093
    350     3.57673397127386    3.61689491312817      3.66220246124671
    351              1567220             1609017               1652717
    352     2.58699749262038    2.63200843432739      2.67971667869759
    353               721729              772460                823609
    354     7.18453537266005    6.79305053235993       6.4115675232704
    355              1094524             1144588               1193636
    356     4.75608876236201     4.4725181364733      4.19593639444133
    357              8484693             8511794               8505100
    358    0.601864644518792    0.31890146437525     -0.07867476103786
    359             11121341            11104899              11045011
    360    0.128880432668041  -0.147951277402184    -0.540752950543684
    361                40900               41208                 41576
    362    0.677103808643345   0.750234921452796     0.889066544285665
    363               114039              114918                115912
    364     0.69515608509071   0.767833539558842     0.861245196984192
    365                48018               48217                 48402
    366      1.3797950037214   0.413571535602637     0.382947921390193
    367                56905               56890                 56810
    368     1.02802332572502 -0.0263631971297131     -0.14072121935205
    369              6902116             7072123               7243152
    370      2.4562041594304    2.43326864607613      2.38957530543333
    371             14259687            14521515              14781942
    372     1.83655642844003    1.81948775605672      1.77749578673149
    373               155174              156021                156869
    374    0.294296289751992   0.544354563240186     0.542044856834603
    375               164905              165649                166392
    376    0.197277633521513   0.450154139027106     0.447535840524607
    377               199204              196477                196407
    378    -1.32251466303424   -1.37840491576083   -0.0356339279485514
    379               747932              744230                743966
    380   -0.443706996010097   -0.49619379069059   -0.0354791980047339
    381            937411084           943832637             951139649
    382    0.976352223309362   0.685030624195178      0.77418513765592
    383           1179656335          1184675562            1191244446
    384    0.630919077231141   0.425482138406011     0.554488014331127
    385              7024200             7071600               7150100
    386    0.734446396338105   0.672543291818633       1.1039579972368
    387              7024200             7071600               7150100
    388    0.734446396338105   0.672543291818633       1.1039579972368
    389              4384767             4530522               4677715
    390     3.35618987880396    3.27006759218059      3.19725781901586
    391              8450933             8622504               8792367
    392     2.07597795876773    2.00986802562652      1.95084289919704
    393            204980402           213704750             222680240
    394     4.25755632852093    4.25618640361532      4.19994876108277
    395            630953183           648927117             667520863
    396     2.83883631175208    2.84869535240937      2.86530575050696
    397              2369143             2368126               2368751
    398   0.0782445394894119  -0.042936131160119    0.0263886946453169
    399              4295427             4280622               4267558
    400   -0.226821270818422  -0.345264228485821    -0.305655944795909
    401              4676254             4827045               5001402
    402     3.23338566121347    3.17371171293843      3.54837939338699
    403              9842880             9954312              10108539
    404      1.1468886718608    1.12574729606877      1.53746880756048
    405              6891116             6916190               6912310
    406    0.430609461286473   0.363199418675683   -0.0561159938072453
    407             10000023             9971727               9920362
    408   -0.226013875689698  -0.283360435946948    -0.516437606548041
    409           2223866615          2274477704            2326601285
    410     2.31814783758095    2.27581495484613      2.29167254127545
    411           4446099709          4491533041            4539500289
    412    0.993608673766076     1.0218693905589      1.06794823865575
    413           2687691737          2755915238            2825400876
    414      2.5909632593763    2.53836777710792      2.52132710911772
    415           5838366631          5917205336            5997941575
    416      1.3286362250527    1.35035550150944      1.36443193053999
    417            463825122           481437534             498799591
    418     3.91948200785734    3.79720958710814      3.60629485111976
    419           1392266922          1425672295            1458441286
    420     2.41356394376433    2.39935119280238      2.29849391861822
    421            181950769           188744530             195246877
    422     3.87246916968822    3.73384571955286      3.44505189104024
    423            473204510           484919898             495868522
    424     2.55615463234074    2.47575577840541      2.25782114637003
    425            121798233           125020092             128304189
    426     2.82654450900766    2.61086126042535      2.59294607419067
    427            244016173           247099697             250222695
    428     1.25151729140412    1.25573808861315      1.25594145835811
    429            281874353           292693004             303552714
    430     3.94985154839222    3.83811116011678      3.71027317072463
    431            919062412           940752397             962572764
    432     2.34030173611292    2.36001219468869       2.3194590914234
    433                43586               43872                 43895
    434     1.13058124953867   0.654030550628454    0.0524114998268163
    435                83828               84350                 84338
    436     1.09510938916344   0.620772843428076   -0.0142274495165514
    437            383721793           393333604             403171286
    438     2.49274628981713    2.47403243101329      2.47033820164956
    439           1240613620          1257621191            1274487215
    440     1.37759581159767    1.36158808453343      1.33219204981494
    441                 <NA>                <NA>                  <NA>
    442                 <NA>                <NA>                  <NA>
    443                 <NA>                <NA>                  <NA>
    444                 <NA>                <NA>                  <NA>
    445              2806411             2827835               2849043
    446    0.870399899564205   0.760495841159739     0.747174720500851
    447              4560155             4580084               4599533
    448    0.544884384079014   0.436072439258428       0.4237438032978
    449             53233539            54356195              55484160
    450      2.2304696365765    2.08699604728169      2.05389840988477
    451             75373855            76342971              77324451
    452     1.40442428093574    1.27755003393578      1.27742540846959
    453             21604967            22427635              23512763
    454     3.34908795739805    3.73706618704935      4.72494762751816
    455             31264875            32378061              33864447
    456     3.17093320217157     3.4985800882497       4.4884639176917
    457               297604              298556                300190
    458  -0.0765825415807209    0.31937761983904     0.545808759140286
    459               318041              319014                320716
    460   -0.143903000294037   0.305468368224399     0.532100734916982
    461              7000447             7136149               7274496
    462     1.89538675313443    1.91992709451763      1.92012572026188
    463              7623600             7765800               7910500
    464     1.82675240987847    1.84807786159597      1.84615132126711
    465             40502481            40641670              40894259
    466    0.480438950517546   0.343066345015609     0.619579150261878
    467             59277417            59379449              59539717
    468     0.30759122234095   0.171978290995717     0.269541239520996
    469              1469278             1481009               1493944
    470    0.768079903155089   0.795248857099847      0.86959904994976
    471              2733896             2746169               2759817
    472    0.421348654823536   0.447915144043708     0.495752366688783
    473              5966981             6197912               6359781
    474     3.55576549646754    3.79713567811131      2.57814816605706
    475              6931258             7109980               7211863
    476     2.19915131357546    2.54581045628308      1.42278781713891
    477            116302928           116416235             116331281
    478    0.928359532764857   0.097376604051583   -0.0730009971601782
    479            128070000           127833000             127629000
    480   0.0179605415195644  -0.185226486410846    -0.159710675844347
    481              9275230             9421048               9566957
    482     1.54005135512754    1.55989290243788      1.53688470149352
    483             16321872            16557202              16792090
    484     1.41327147745347    1.43151244887822      1.40867688701824
    485              9786183            10219218              10658602
    486     4.47731345811679    4.32985723379387      4.20972004582599
    487             41517895            42635144              43725806
    488      2.8175240671163    2.65543505812038      2.52595644302294
    489              1923436             1949521               1986238
    490     1.25519582081825    1.34705313986617      1.86586959396653
    491              5447900             5514600               5607200
    492     1.19286442880631    1.21689072909974      1.66523655939962
    493              2914935             3011402               3110665
    494     3.26713709365215    3.25582299519282       3.2430778650594
    495             14363532            14573885              14786640
    496     1.45722988351436     1.4538734073168      1.44928419333369
    497                51179               52996                 54784
    498     3.66347164415668    3.48871473474964      3.31817415134002
    499               107995              109871                111618
    500     1.86835722052213    1.72220205807577      1.57753769269312
    501                14848               14837                 14825
    502   -0.201843572530488 -0.0741115075188128    -0.080911608486745
    503                47403               47581                 47727
    504    0.247124923790409   0.374800405061299     0.306375360312754
    505             40602657            40909592              41089082
    506    0.621566983144705   0.753105096374867      0.43778826636442
    507             49554112            49936638              50199853
    508     0.49822508440111   0.768971758582589     0.525713660600414
    509              2943356             3143825               3394663
    510     5.15215597247121      6.588978450283      7.67642810205362
    511              2943356             3143825               3394663
    512     5.15215597247121      6.588978450283      7.67642810205362
    513            411926011           418269352             424604252
    514     1.44406693988243    1.53992242067957      1.51455036562183
    515            529726237           535759003             541792264
    516     1.04941805075481    1.13884598847989      1.12611472065174
    517              1901072             1967246               2034912
    518     3.46805082182015    3.42166575438843      3.38179795806196
    519              6323418             6416327               6508803
    520      1.4894787970459    1.45859508479977      1.43097327467587
    521              4363032             4413617               4538087
    522     1.06653508305746    1.15273061821696      2.78110229153644
    523              4995800             5045056               5178337
    524    0.898071621012191    0.98111944066669      2.60752084121081
    525              1922062             2015022               2104489
    526     3.67226886107611    4.72315453608514      4.34425688668906
    527              4019956             4181150               4331740
    528     2.89962727401251    3.93153710180299      3.53829805549154
    529              5067126             4843884               4608552
    530     2.32686475429922   -4.50569147166858     -4.98031703671854
    531              6491988             6188132               5869870
    532     2.05103928741641   -4.79355371289299     -5.28007770057696
    533                31538               31761                 31941
    534    -3.31804682007029   0.704595404595719     0.565132897122641
    535               170935              172145                173124
    536    0.732191056391629   0.705377764391326     0.567095582689076
    537            462643953           469524671             476387252
    538     1.40905765408132    1.48725990156842      1.46160179088864
    539            588873862           595510008             602139396
    540     1.05292295477399    1.12692147304034      1.11322864619264
    541            248577636           259055552             269849448
    542      4.1832137536193    4.21514830079082      4.16663372649894
    543            843512714           864134636             885375140
    544     2.40535207198469    2.44476718106705      2.45800863836685
    545            162363734           168676091             174842240
    546     4.11179687775926    3.88778752772463      3.65561530590604
    547            534960135           550299855             565535976
    548     2.88712833479123    2.86745104847859       2.7686943511915
    549                 5196                5215                  5245
    550    0.308404253696132   0.364998964438266     0.573615339552481
    551                35926               36189                 36505
    552    0.701110398374398   0.729393790471232     0.869403225669288
    553              3692904             3714770               3716533
    554    0.596810444747985   0.590362467558503    0.0474479383514771
    555             20261738            20398496              20425000
    556    0.684559602845539   0.672689270325415     0.129846810394944
    557           1100575524          1130922560            1161654015
    558     2.77982194885331    2.75737878393886      2.71737925185612
    559           2904506894          2950807764            2996702087
    560     1.59810185295757     1.5941043244086      1.55531388929882
    561           2631184964          2699096714            2768319922
    562     2.63894365414619    2.58103291593605      2.56468053334082
    563           5761260544          5839761629            5920180850
    564     1.34317061051678    1.36256786861955      1.37709766440879
    565               501601              515736                528145
    566     2.73113142852235    2.77900237112312      2.37758605685104
    567              2022747             2037677               2054718
    568    0.673528482706333   0.735394495013464     0.832817872899488
    569           1207115396          1234916745            1263628106
    570     2.40836772061957    2.30312272481363      2.32496329135128
    571           2182361368          2196050826            2211789266
    572    0.592901294366669   0.627277324494855     0.716670115903767
    573              2067653             2020994               1997745
    574    -2.22415308852885   -2.28246780062813     -1.15704251361283
    575              3097282             3028115               2987773
    576    -2.09694341985475   -2.25846389882827     -1.34120198821595
    577               448892              460842                473864
    578     2.24309859567021    2.62729267734573      2.78651096329972
    579               506953              518347                530946
    580      1.8254058035237    2.22266050326725      2.40154189982875
    581              1423002             1397904               1381242
    582    -2.04148981383623   -1.77947526622434     -1.19908775139121
    583              2097555             2059709               2034319
    584    -2.08130508988111   -1.82076700208005     -1.24035915329271
    585               557297              571003                582766
    586      2.5950318244663    2.42961521187371       2.0391269983749
    587               557297              571003                582766
    588      2.5950318244663    2.42961521187371       2.0391269983749
    589                 <NA>                <NA>                  <NA>
    590                 <NA>                <NA>                  <NA>
    591                36458               36350                 36026
    592    0.898201382313273  -0.296670913120137     -0.89533039781617
    593             18835465            19275316              19725140
    594     2.28869695421819    2.30837846881745      2.30686506905046
    595             32464865            32903699              33352169
    596     1.30835154510463    1.34266554348538      1.35377246657382
    597                33178               33945                 34700
    598     2.36977191505584     2.2854562083527      2.19981191992032
    599                33178               33945                 34700
    600     2.36977191505581    2.28545620835272      2.19981191992032
    601              1219935             1218343               1217297
    602   -0.172565140692547  -0.130583979860009   -0.0858911878853553
    603              2862354             2860699               2860324
    604  -0.0998329740168972 -0.0578362648793712   -0.0131095435392298
    605              6940464             7281030               7630993
    606     4.88060061752283    4.79037046438401      4.69456452788776
    607             21731053            22348158              22966240
    608     2.86594914827117    2.80016501090231      2.72814099743688
    609               131736              137977                144424
    610     4.66870302490599     4.6287084684408      4.56664130342513
    611               361575              374440                387539
    612     3.52915551848168    3.49620875437484      3.43849123867636
    613            248421309           254476115             260609701
    614     2.84741389342089    2.43731345928944      2.41027964451594
    615            397997557           406045323             414117603
    616     2.22565311542333    2.02206417060997      1.98802437628373
    617             87567088            89164082              90758420
    618     1.70640336814944    1.80730707479904      1.77229608781534
    619            112532401           114150481             115755909
    620     1.32657895725526     1.4276397653994      1.39661546186197
    621                39297               39219                 38890
    622    0.316044611223791   -0.19868568357889    -0.842417508017679
    623                53416               52971                 52203
    624   -0.330813597773069  -0.836573256679686     -1.46046294872198
    625           2468821230          2530420623            2593477682
    626     2.54353951465096    2.49509329600184      2.49195957489634
    627           5226300409          5289461774            5354644874
    628      1.1877425898881     1.2085291708688      1.23232008822529
    629              1173181             1175261               1177371
    630    0.161998753258618   0.177138762114444     0.179373617781445
    631              2055004             2058539               2061044
    632     0.21107377513823   0.171871346437471     0.121614265751721
    633              5590350             5902462               6209357
    634     5.49424052526594    5.43276549930177      5.06877959045809
    635             15529181            16039734              16514687
    636      3.2497394697786    3.23481196285064      2.91810874359862
    637               389936              391879                395713
    638    0.566566467990139   0.497049557832315     0.973608235170595
    639               414508              416268                420028
    640     0.49118281065151   0.423700885195294     0.899209211902256
    641             14266587            14477757              14696854
    642     1.42241046161034    1.46932418519949      1.50199856311732
    643             49390988            49794522              50218185
    644      0.7624548830356    0.81369995783056     0.847223411648512
    645            201883419           206452895             211056972
    646     2.56323267006525    2.26342312936556       2.2300859476928
    647            343313330           349770162             356239722
    648     1.97172117634608    1.88074025555605      1.84966034924385
    649               397301              399777                402174
    650    0.702180899874301   0.621271184000877     0.597793914643945
    651               619428              620079                620601
    652    0.183239906425096    0.10504177223011    0.0841474080023094
    653              1826012             1864863               1899635
    654     2.80534069355789    2.10532380598405      1.84741705021224
    655              2702520             2743938               2792349
    656     1.33380431981013    1.52094437973029      1.74890622320901
    657                49192               47808                 47703
    658    -5.25853524742463   -2.85380191408165    -0.219870051198252
    659                54087               52520                 52359
    660    -5.34393804525569   -2.93998117935774    -0.307020712378622
    661              7344366             7683170               8043201
    662     4.38951580157979    4.50987337364058      4.57949158706912
    663             23073723            23760421              24487611
    664     2.79982093572927    2.93267756135951      3.01461029926612
    665              1593058             1673772               1759879
    666     4.82815236234551    4.94243225185218      5.01652946123931
    667              3419461             3524249               3636113
    668     2.87305118502029    3.01844276057634      3.12478900195865
    669               519604              519046                519081
    670  -0.0288639719983694  -0.107447177332841   0.00674291296022855
    671              1250400             1252404               1255882
    672    0.237886692890185   0.160140420765724     0.277321025514609
    673              2287832             2373696               2463707
    674     3.63412245535303    3.68435898221723       3.7218892192544
    675             14718422            15146094              15581251
    676     2.89150573052371    2.86427704641241      2.83256555379816
    677             20364317            20898466              21436918
    678     2.94564136737115    2.58915565426384      2.54388167217021
    679             28717731            29184133              29660212
    680     1.75828740323151    1.61104342924472      1.61813131846729
    681            277437381           280120518             282845297
    682     1.07011434517077   0.967114449512479     0.972716679040261
    683            343397156           345987373             348656682
    684    0.860906957950519   0.754291919645382     0.771504745059005
    685               873633              909166                947033
    686      3.9739555240755    3.98673167641516      4.08062436972193
    687              2099271             2132340               2167470
    688      1.5047354003649    1.56298266563295      1.63406204857906
    689               167587              171852                176182
    690     2.24780092266579    2.51310218774107      2.48839103589867
    691               249750              254350                259000
    692     1.53321542873142    1.82508537401111      1.81167904307637
    693              2700398             2802629               2910768
    694     3.70603745675428    3.71587356805907      3.78590586541931
    695             16647543            17283112              17954407
    696     3.73070315116356    3.74672017840573        3.810576036689
    697             69982300            73409645              76952556
    698     4.81723266545861    4.78129773248268      4.71337458499345
    699            160952853           165463745             170075932
    700     2.74437885223698    2.76406237855304      2.74928887763244
    701              3332908             3393970               3456081
    702     1.80880949850974    1.81551314607331      1.81349605220586
    703              5855734             5942553               6030607
    704      1.4638532507888     1.4717486887592      1.47088300051398
    705             14477657            14669707              14842886
    706     1.48858416763162      1.317805474921      1.17360740031088
    707             16615394            16693074              16754962
    708    0.512923100553586   0.466428782200821     0.370055034770235
    709              3867496             3935476               4008535
    710     1.60915606479481    1.74245710414189      1.83939976623181
    711              4889252             4953088               5018573
    712     1.24566617956207      1.297189390693      1.31344098868985
    713              4554452             4664736               4771393
    714     2.50889682221587    2.39260249382126      2.26070547516633
    715             27161567            27266399              27330694
    716    0.496881283325948   0.385214210038506     0.235525469173116
    717                10241               10283                 10444
    718   0.0781478988682228   0.409277515375298      1.55356045898289
    719                10241               10283                 10444
    720   0.0781478988682006   0.409277515375299      1.55356045898291
    721              3748563             3774624               3798063
    722     1.04210165993295   0.692820854887079     0.619042483893705
    723              4350700             4384000               4408100
    724      1.1117260560215   0.762479795105218     0.548220797298334
    725           1019661535          1027761520            1036720452
    726     1.01524137355275   0.794379774264996     0.871693659050393
    727           1291001492          1297544164            1305615104
    728    0.648604114932411   0.506790429022999     0.622016592877998
    729              2166075             2443090               2742336
    730     7.32856596629115    12.0346857529797      11.5546480378227
    731              2881914             3206870               3535579
    732     6.61155222833165    10.6840726819136      9.75816930384769
    733             15243817            15699289              16177235
    734     3.35915173419536    2.98791306665515      3.04437990790538
    735             26565994            27072916              27617274
    736     2.12303402735957  1.9081612380097701      2.01071063050615
    737             68053241            69912136              71597051
    738     2.83719191947223    2.69488989176193      2.38146323415308
    739            194454498           198602738             202205861
    740     2.25257934512803    2.11083457768435       1.7979754297027
    741              2360424             2413758               2468484
    742     2.23429377115688    2.23436051826336      2.24193279267508
    743              3623617             3688674               3754862
    744     1.78967209984493    1.77943456083626      1.77844896646925
    745             22340162            22584945              22848874
    746     1.00395015948743    1.08974886775122      1.16183043677037
    747             29229572            29477721              29749589
    748    0.756357202975091   0.845382115888711     0.918055908591097
    749             42900709            43854945              44812533
    750     1.71564963927674    2.19991306656885      2.16003714916213
    751             94636700            96337913              98032317
    752     1.80164390376145    1.78165880217122      1.74352515093364
    753                13871               13773                 13674
    754   -0.582253738547077   -0.70901758459584    -0.721393444359247
    755                18540               18240                 17946
    756    -1.53083329873194   -1.63135754915238     -1.62497357703192
    757               987266             1014863               1042006
    758     2.86543488635204    2.75694003264108      2.63940734576892
    759              7583269             7806637               8026545
    760     3.00352670267458    2.90298961470657      2.77799045268741
    761             23165018            23134846              23086831
    762   -0.469373364884263    -0.1303330146759    -0.207759736934845
    763             38042794            38063255              38063164
    764   -0.285609121534849  0.0537697088781007   -2.3907600329836e-4
    765            273406962           285809783             298783720
    766     4.51587769445985    4.53639545579676      4.53936071180601
    767            739525992           761717101             784822780
    768     2.98056441687213    3.00072062916756      3.03336750214302
    769              3491721             3449657               3406460
    770   -0.569042031446238   -1.21199320673227     -1.26011769056927
    771              3721525             3678732               3634488
    772   -0.506170056982176   -1.15654029293994     -1.20998793422288
    773             14904929            15008319              15116334
    774     0.72783867392078   0.691268375195914     0.717123366720389
    775             24686435            24783789              24887770
    776    0.425940886169344   0.393586759082814     0.418674807001654
    777              6403809             6457743               6494283
    778     1.04647758108964   0.838690636147976     0.564237547728676
    779             10573100            10557560              10514844
    780   0.0459100367184907  -0.147084878575482    -0.405421787747567
    781              3418538             3482053               3548485
    782     1.69964430526569    1.84090880709612      1.88986894683043
    783              5768613             5843939               5923322
    784     1.15140184845157    1.29733854365491      1.34923843816846
    785              2807401             2888204               2969835
    786     2.90229376272083    2.83757113355169      2.78715400607761
    787              3786161             3882986               3979998
    788     2.59703221798976    2.52518683079236      2.46768715003228
    789               849178              864348                879357
    790      1.8424950378681    1.78643346860139      1.73645337294701
    791              2301401             2327284               2353058
    792     1.10941770507993    1.12466275977113      1.10747119818639
    793            858450100           863058781             868544525
    794    0.815098520238507   0.536860674837129     0.635616498060983
    795           1075046427          1077987051            1082488958
    796    0.463609629933387   0.273534605217577     0.417621621319469
    797               171172              173695                176132
    798     1.48907740645678    1.46319886410261      1.39328263578879
    799               283788              285265                286584
    800    0.531736671222477   0.519109227709588     0.461311408821609
    801              1687819             1778949               1880829
    802     6.38128377896359    5.25855775455397      5.56898966136271
    803              1713504             1804171               1905660
    804     6.21360458813461    5.15608097721193      5.47271987641268
    805             10898688            10871606              10826124
    806   -0.350299532205801  -0.248797870803971    -0.419233409344486
    807             20246871            20147528              20058035
    808   -0.593959183588628  -0.491866212866043    -0.445177936198383
    809            105261487           105407937             105669982
    810    0.105982823386074   0.139033012774432     0.248292323389437
    811            142849468           142960908             143201721
    812   0.0448957880908489   0.077981777433695     0.168305035277726
    813              1745731             1791521               1836678
    814     2.63069211276196    2.58916006044361      2.48935253617643
    815             10309031            10576932              10840334
    816      2.6071051025344    2.56550960901795      2.45984040638473
    817            512293887           525925036             539682037
    818     2.67886461772258    2.66080649133336      2.61577222195599
    819           1660139325          1684436757            1708114582
    820      1.4722424616719    1.46357788374178      1.40568204188149
    821             24142488            24814831              25433937
    822     3.47120871129676    2.74682228092366      2.46428859769191
    823             29411929            30150945              30821543
    824     3.20649410639241     2.4815925615212      2.19976275661053
    825             11164206            11429725              11724384
    826     2.66522501392426    2.35046503532033      2.54533576140557
    827             33739933            34419624              35159792
    828     2.37468258916878     1.9944775499975      2.12762901437903
    829              5484810             5689594               5901794
    830     3.66098642761968    3.66564391410135      3.66174801478833
    831             12530121            12875880              13231833
    832     2.71070159961371    2.72203680436678      2.72697235540654
    833              5076732             5183688               5312437
    834     1.77183287926401    2.08490245606007       2.4533903303102
    835              5076732             5183688               5312437
    836     1.77183287926401    2.08490245606007      2.45339033031018
    837               108338              113557                119060
    838     4.66800215431446     4.7048943846165      4.73226542221584
    839               540394              553721                567763
    840     2.35185573397427    2.43624466471624      2.50431283407337
    841              2501043             2595229               2691132
    842     3.79482782397913    3.69669144820455      3.62871635861394
    843              6436698             6612385               6788587
    844     2.78607305276662     2.6928730024304      2.62984140386497
    845              4001758             4070597               4139585
    846     1.73109306184116    1.70559071762261      1.68058695225294
    847              6114034             6137349               6161289
    848    0.374364787171041   0.380610550595593     0.389311893511473
    849                30261               31183                 31863
    850     1.99255653573635     3.0013319783177      2.15723874835272
    851                31608               32495                 33132
    852     1.75216316570038    2.76759782372225      1.94133513897054
    853              4727676             5008903               5169951
    854     6.82510726297554    5.77831792083596      3.16462816739075
    855             12026649            12216837              12440326
    856     2.49721206017534    1.56901447012171      1.81282104868689
    857              4009779             3994308               3985121
    858    6.733560852961e-4   -0.38657798864192     -0.23026720481756
    859              7291436             7234099               7199077
    860   -0.402005900874718  -0.789468997859736    -0.485299545092705
    861            316812745           330350732             344254829
    862     4.28675791546702    4.27318257035398      4.20888941756606
    863            879707649           904194713             929240350
    864     2.79379454320083    2.78354565040277      2.76993844798116
    865              1734995             1847027               1949511
    866     6.01785744914684     6.2572787859913      5.40012524868226
    867              9714419            10243050              10701604
    868     5.12359802529756    5.29881502099888      4.37942099501625
    869            316860625           330397705             344302619
    870     4.28664494680891     4.2722506149194      4.20853831294016
    871            879797419           904282154             929328653
    872     2.79379840741305    2.78299691170156      2.76976592861082
    873             19636696            20130396              20650946
    874     2.78698529223556    2.51417040830087      2.58589051104607
    875             35871823            36444557              37059328
    876     1.75219321280136    1.59661247213447      1.68686643659848
    877               118302              122908                127553
    878     3.97916187644067    3.81954310133185      3.70958564738824
    879               182138              186044                189924
    880     2.22622485286332    2.12185633623001      2.06407872618616
    881               362291              365940                369514
    882     1.01955781663249    1.00216292356855     0.971924317330716
    883               546080              552146                558111
    884      1.1220419963936    1.10470189002797      1.07453632709163
    885              2948302             2937801               2935234
    886   -0.408451131443131  -0.356806910813018 -0.087416477629276207
    887              5391428             5398384               5407579
    888   0.0931912731899838   0.128936462723796     0.170183855326072
    889              1078743             1085605               1092516
    890     0.86426262161683    0.63409608733312     0.634585791340172
    891              2048583             2052843               2057159
    892     0.43607948463593   0.207732702329724     0.210024305900448
    893              7976659             8059895               8150488
    894     1.03375039522615    1.03808770233338      1.11772736224152
    895              9378126             9449213               9519374
    896    0.852524628765313   0.755150133665177     0.739763272434863
    897               247262              250919                253954
    898     1.43300090073122    1.46816746889403      1.20229703686888
    899              1099920             1105371               1111444
    900    0.458720162719754   0.494357533893043     0.547904556093655
    901                34056               33435                 34640
    902   -0.532990982673536   -1.84029730199094      3.54058281882925
    903                34056               33435                 34640
    904   -0.532990982673547   -1.84029730199093      3.54058281882925
    905                47880               46973                 47790
    906     3.48310302619174   -1.91249121348319      1.72434440351265
    907                89770               87441                 88303
    908     2.79232906929128   -2.62865635521684     0.980980190128322
    909             12419685            12406434              12106417
    910     2.96620259677862  -0.106750485312401     -2.44795664060938
    911             22337563            22730733              22605577
    912     2.31118860851256    1.74481846238131    -0.552123972765832
    913                26821               27940                 29223
    914     4.43687238976425    4.08741910626189      4.48967166281126
    915                29726               30816                 32081
    916     3.92799223339511    3.60119518890118      4.02299189665854
    917              2615056             2716552               2824191
    918      3.6407358010289    3.80779169033489      3.88585227998914
    919             11894727            12317730              12754906
    920     3.40849096495518    3.49444952100308      3.48762940257558
    921            923137345           950849298             979320308
    922     3.15221016252454    3.00193174397035      2.99427154859191
    923           1944510719          1959494275            1976386829
    924    0.727278018869242   0.770556616304248     0.862087438351921
    925            286621674           288578421             290530403
    926    0.663682084244016   0.682693312299889     0.676413015649558
    927            441717333           443770308             445864843
    928    0.418060103918222   0.464771211502352     0.471986287104187
    929              2466614             2566655               2669941
    930     3.94493603886538    3.97571347639327      3.94528800630779
    931              6571855             6748672               6926635
    932      2.6713169382967    2.65496081549196      2.60283805012609
    933             29940706            30713268              31428409
    934       3.711024875017    2.54757844318673      2.30174854622188
    935             68270489            68712846              69157023
    936    0.671403303922798   0.645857450726443     0.644344612784471
    937              2021296             2064612               2112022
    938     2.03784435202817    2.12034254519358      2.27034671600903
    939              7621779             7784819               7956382
    940     2.03027773445303    2.11657481593722      2.17988187402675
    941              2554491             2617416               2684307
    942     2.32552870848315    2.43345849689149      2.52350225314261
    943              5267970             5360811               5458682
    944       1.665530086186    1.74701805032668      1.80920990684314
    945            449702188           456600087             463478372
    946     1.44910967334211    1.53388157408743      1.50641342300051
    947            572674666           579334666             585986944
    948      1.0829033148416    1.16296396460464      1.14826168541416
    949               301859              312513                323419
    950     3.37508875166006    3.46860473794157      3.43026278790838
    951              1088486             1112976               1137676
    952      2.1306026960996    2.22497688451797      2.19500764303754
    953            199076018           203564691             208087137
    954     2.55787168274055     2.2547532571201      2.22162594985591
    955            339527169           345887176             352259724
    956     1.96441658746562    1.87319530826706      1.84237764281843
    957                25116               25227                 25193
    958     0.45492715834916   0.440975626700772    -0.134867136666362
    959               107383              107611                107502
    960    0.222815868287058   0.212099021808403    -0.101342093995982
    961            512293887           525925036             539682037
    962     2.67886461772258    2.66080649133336      2.61577222195599
    963           1660139325          1684436757            1708114582
    964      1.4722424616719    1.46357788374178      1.40568204188149
    965            316860625           330397705             344302619
    966     4.28664494680891     4.2722506149194      4.20853831294016
    967            879797419           904282154             929328653
    968     2.79379840741305    2.78299691170156      2.76976592861082
    969               761912              764482                767698
    970    0.300222368855251   0.336741683803994     0.419794635473185
    971              1410296             1420020               1430377
    972     0.64770221814423   0.687134458496019     0.726708986846704
    973              7262322             7384954               7511197
    974     1.44240164465958    1.67450747104654      1.69501534368676
    975             10895063            11032528              11174383
    976       1.019946186598    1.25382492654963      1.27759263330169
    977             51840603            52961615              54180184
    978     2.16165918899813    2.13937219559763      2.27478268615099
    979             73195345            74173854              75277439
    980     1.33367323837908     1.3279890059113      1.47687589304162
    981                 5781                5971                  6167
    982     3.21672870070772    3.23377394645163      3.22980784445003
    983                10550               10700                 10854
    984     1.35511187118714     1.4117881545785      1.42899341733525
    985             12682374            13366889              14093565
    986     5.01848935464592    5.25675234629167      5.29376313084517
    987             45110527            46416031              47786137
    988     2.58825254940751    2.85292613461509      2.90906812607356
    989              6268797             6625186               6999978
    990      5.5550813734847    5.52939767423953      5.50285592433563
    991             32341728            33295738              34273295
    992     2.91517454447869    2.90711158037377      2.89370786919312
    993             31465493            31395053              31360012
    994   -0.260137166795922  -0.224115232736355    -0.111675459635824
    995             45870741            45706086              45593342
    996   -0.397263167656909  -0.359600092788762    -0.246976481186878
    997           1368245706          1399498063            1431823667
    998     2.35426798659803    2.28411877069688      2.30979983857256
    999           2321793515          2338654010            2357942787
    1000   0.679036990847706   0.726184085323368     0.824781131262768
    1001             3165372             3180512               3193778
    1002    0.47106834749749   0.477160588686981     0.416235193346207
    1003             3352651             3361637               3371133
    1004   0.253703132150146   0.267668153797056      0.28208319300115
    1005           249849720           252208133             254614421
    1006    1.03534480069191    0.93950541093758      0.94956550837067
    1007           309327143           311583481             313877662
    1008   0.829616674709795   0.726786704810745     0.733599941250847
    1009            14554257            15007103              15199882
    1010    3.77723254773414    3.06400942093244      1.27640423101864
    1011            28562400            29339400              29774500
    1012    2.82284968340424    2.68401550537804      1.47209998811321
    1013               53518               53651                 53776
    1014   0.318155715380579   0.248206231802748     0.232716275076835
    1015              109308              108703                108083
    1016  -0.485517590904248   -0.55501929083077    -0.571994215876999
    1017            25293053            25636044              25970224
    1018    1.38687189748658      1.346955692903      1.29513209974768
    1019            28715022            29096159              29470426
    1020    1.35734981460765    1.31857718723776      1.27810796850367
    1021               12349               12626                 12932
    1022    2.62546871237498    2.21830918097486      2.39466799177527
    1023               27556               27962                 28421
    1024    1.87551285025179    1.46261478346834    1.6281862972680201
    1025              102499              102617                102691
    1026    0.14156495068734   0.115056858575086    0.0720868190275143
    1027              108357              108290                108188
    1028 -0.0433657352905335 -0.0618517700554086   -0.0942359108531572
    1029            26587808            27458906              28354957
    1030    3.24435486033937    3.22377949288483      3.21113051981827
    1031            87411012            88349117              89301326
    1032    1.06742976658398    1.06749342269972      1.07201322213276
    1033               60043               61720                 63456
    1034    3.09866318909228    2.75470552891182      2.77387248424255
    1035              245453              251294                257313
    1036    2.37632331873076    2.35180855409793      2.36696742718214
    1037          3593889101          3668565395            3745429795
    1038    2.19125210116809    2.07786862369296      2.09521684156866
    1039          6969631901          7053533350            7140895722
    1040    1.22200562383263    1.20381463744106      1.23856183369431
    1041               39086               38989                 38880
    1042  -0.329498005674477  -0.248479154423372    -0.279957547141305
    1043              194672              196351                198124
    1044   0.771440058555094   0.858778291222582     0.898922334287627
    1045                <NA>                <NA>                  <NA>
    1046                <NA>                <NA>                  <NA>
    1047             1775680             1791000               1807106
    1048   0.803248961277754   0.859067492317545     0.895254757473813
    1049             7862636             8244926               8642705
    1050    4.77803895718665    4.74760616605795      4.71176316859181
    1051            24743946            25475610              26223391
    1052    2.92949070515169      2.914067083312       2.8930274863714
    1053            32219542            32906089              33625925
    1054    2.05015046003468    2.10845537603557      2.16396302025893
    1055            51784921            52443325              53145033
    1056    1.19303600363508    1.26340561811428      1.32915852455183
    1057             5427875             5685070               5950059
    1058    4.76036450008939    4.62957272637652      4.55576952434069
    1059            13792086            14265814              14744658
    1060    3.49719132212533     3.3771096377685      3.30148019166725
    1061             4262290             4300463               4355539
    1062   0.706879123071181   0.891611678783383      1.27256771109182
    1063            12839771            13025785              13265331
    1064      1.253649854212    1.43833913255502      1.82230856245569
                       x2013               x2014               x2015
    1                  44228               44588               44943
    2      0.826413457840807   0.810669184722173   0.793025567597756
    3                 102880              103594              104257
    4      0.749301039347625   0.691615260101675    0.63795916173479
    5              190108665           198073341           206556291
    6        4.1356112521479     4.1895386514865    4.28273181901848
    7              567891875           583650827           600008150
    8       2.78015724354725     2.7749916302289    2.80258713657233
    9                7687539             8043935             8371880
    10      4.34455335199728    4.53176850303214    3.99600796676125
    11              31541209            32716210            33753499
    12      3.46678830410982    3.65757606530364    3.12134122890873
    13             168643432           175773257           183117253
    14      4.26573064522988    4.22775136597078    4.17810770838707
    15             387204553           397855507           408690375
    16      2.76183897675104    2.75073056798483    2.72331733741717
    17              16211664            17017877            17845914
    18      4.92641852853657    4.85333965972379    4.75101946993626
    19              26147002            27128337            28127721
    20      3.73552542857565    3.68442896683492    3.61767751762095
    21               1603505             1630119             1654503
    22      1.74363937043308    1.64611599636276    1.48476433256912
    23               2895092             2889104             2880703
    24    -0.183211384606402  -0.207046999760594  -0.291205786840436
    25                 63186               63342               63384
    26   0.39008659902542703   0.246585860909757  0.0662847427328072
    27                 71367               71621               71746
    28     0.497261875887559   0.355274942185653   0.174377690367456
    29             222497540           228834568           235145769
    30      2.86427087845205    2.84813396139121    2.75797535973675
    31             389131555           397922915           406501999
    32      2.29982349808486    2.25922567497771    2.15596631322425
    33               7444846             7543693             7639464
    34       1.3709817063756    1.31898690128519    1.26155937378637
    35               8751847             8835951             8916899
    36     0.997641825826328   0.956397623844095   0.911950036242349
    37              38509756            38990109            39467043
    38      1.25948243003778     1.2396386789085    1.21579706300955
    39              42202935            42669500            43131966
    40      1.11910919999772    1.09946109101836    1.07800134449604
    41               1832631             1823893             1815962
    42    -0.568544597024387  -0.477941161241734  -0.435787280070284
    43               2901385             2889930             2878595
    44     -0.44829630947071  -0.395592881362063  -0.392995248824912
    45                 46290               45579               44812
    46     -1.38803219610543   -1.54788709134401   -1.69711234332623
    47                 52995               52217               51368
    48     -1.30478202241628   -1.47894571116667   -1.63927018758043
    49                 22434               22465               22485
    50     0.240996233925256   0.138087729909168  0.0889877700547435
    51                 88497               89236               89941
    52     0.934326293295174   0.831589247616481   0.786935419380774
    53              19775013            20095657            20410546
    54      1.83817579922612    1.60845510176755    1.55480059894141
    55              23128129            23475686            23815995
    56        1.721151439989    1.49156648912847    1.43921665259925
    57               4861991             4916377             4988134
    58     0.919560199445851    1.11238523257782    1.44900148037757
    59               8479823             8546356             8642699
    60     0.589387254692989   0.781541632525841    1.12099250236958
    61               5098727             5189181             5279540
    62       1.7785151839915    1.75849805273914    1.72630914534638
    63               9416801             9535079             9649341
    64      1.29344702700591    1.24820899735588    1.19120985807018
    65               1165374             1235881             1295625
    66      6.09133047244049    5.87420105418283    4.72091281246148
    67              10149577            10494913            10727148
    68      3.55110770404353     3.3458633833966    2.18870609045616
    69              10912673            10966157            11034732
    70     0.517369105105909   0.488911943991581   0.623385919307908
    71              11159407            11209057            11274196
    72     0.471340143966627    0.44392928847521   0.579446241677791
    73               4602023             4794300             4995735
    74      4.06477385190632     4.0931722849319    4.11568361455487
    75              10308730            10614844            10932783
    76      2.89992060914312    2.92622897984691    2.95124912266827
    77               4646488             4893865             5153071
    78      5.23061393535864    5.18707144194244    5.16104677903906
    79              17636408            18169842            18718019
    80      3.00842195462805    2.97977849923872    2.97234560821631
    81              50463354            52301622            54148316
    82      3.64237554020305    3.57799744804209    3.46994901894979
    83             154030139           155961299           157830000
    84      1.26715729721878    1.24596020808779    1.19106112723824
    85               5326274             5320503             5310996
    86   -0.0975440850654849  -0.108408405007288  -0.178845947012378
    87               7265115             7223938             7177991
    88    -0.559647217410859  -0.568389264052903  -0.638069468160719
    89               1120807             1165795             1212293
    90      3.03697968951029    3.93542961597313    3.91103498856279
    91               1261673             1311134             1362142
    92      2.95475711852975    3.84537935863732    3.81660491261165
    93                318531              321687              324941
    94       1.0122291225219    0.98592215922591    1.00646042798103
    95                385650              389131              392697
    96     0.934994065798907   0.898582547265484   0.912227428271637
    97               1682852             1672688             1662529
    98    -0.868074565335509  -0.605805985621185  -0.609197627532439
    99               3617559             3571068             3524324
    100    -1.55832890226336   -1.29347782804997   -1.31760658922995
    101              7195632             7246350             7302153
    102    0.622602357749472   0.702371855942725   0.767134235936024
    103              9443211             9448515             9461076
    104  -0.0383800002667846   0.056151567510169   0.132853236725342
    105               156082              159731              163403
    106     2.31775706385302    2.31096401726445    2.27283917635128
    107               344688              352335              359871
    108     2.23816708234299    2.19427675238371    2.11632063401839
    109                65001               65138               65237
    110    0.312791570829084   0.210544188304464   0.151869636092986
    111                65001               65138               65237
    112    0.312791570829084   0.210544188304464   0.151869636092964
    113              7273140             7428682             7584842
    114     2.19840031092091    2.11603437654332    2.08033279661278
    115             10743349            10916987            11090085
    116     1.62957319915505    1.60331510672219     1.5731449401483
    117            171885100           173941724           175989923
    118     1.20455580861832    1.18940932455394    1.17064141409405
    119            201721767           203459650           205188205
    120    0.868346150167205   0.857834828724811   0.845992601345206
    121                87016               86933               86898
    122   -0.151581297253827 -0.0954302770340357 -0.0402689974434514
    123               276865              277493              278083
    124    0.241564354388319   0.226568487744075   0.212392263810218
    125               312881              318042              323086
    126     1.68829067565892    1.63605229750006    1.57350921226766
    127               411702              416656              421437
    128     1.23862687064177    1.19611546302496    1.14093587780034
    129               270775              279212              287484
    130     3.18450810377003    3.06831321854988    2.91958566075528
    131               728889              736357              743274
    132     1.06812304057562     1.0193598434941   0.934969649654716
    133              1453914             1500166             1548038
    134      3.1393901563764    3.13165384698555    3.14125540181334
    135              2217278             2260376             2305171
    136     1.90562696994558    1.92508536769659    1.96236891205249
    137              1904787             1917526             1941083
    138     1.30399055352486   0.666562218565762    1.22102512464098
    139              4802428             4798734             4819333
    140    0.608247676561223 -0.0769490230926806   0.428340361843656
    141             28479640            28781576            29011826
    142      1.1071099946107    1.05460141956621   0.796807975480466
    143             35082954            35437435            35702908
    144     1.05659125917789    1.00533757867378   0.746339477969813
    145             64402919            64285250            64165146
    146    -0.18903402133499  -0.182707557090694  -0.186829793770741
    147            103713726           103496179           103257886
    148   -0.213201830007392  -0.209757192601487  -0.230243282701295
    149              5959745             6034707             6105617
    150     1.17839214469233    1.24996083195795    1.16818639104156
    151              8089346             8188649             8282396
    152     1.14987975781229    1.22010397343287    1.13833715247736
    153                49579               49875               50217
    154    0.633316902577773    0.59525182295432   0.683373957856512
    155               159794              160912              162190
    156    0.736777668306596   0.697214600852119   0.791085579747361
    157             15276709            15441376            15611340
    158     1.03032721430236    1.07212788370275    1.09469134550671
    159             17509925            17687108            17870124
    160    0.964976309009339    1.00681495918115    1.02942555374852
    161            722694421           744357517           765822300
    162       3.048365006284    2.95349744191833    2.84287054779151
    163           1363240000          1371860000          1379860000
    164    0.666072977690069   0.630326389541893   0.581456146657648
    165             10918491            11271041            11667173
    166     2.93207940112337    3.17789189525871    3.45424789720693
    167             22469268            22995555            23596741
    168     2.06192649501195    2.31524274847183    2.58076728931826
    169             11546101            12036424            12559842
    170     3.95164861580655    4.15895815789976    4.25671955453923
    171             21632850            22299585            23012646
    172     2.81353839574426    3.03550756998046    3.14758224896897
    173             30579203            32071811            33617961
    174     4.72703758072681    4.76573449569302    4.70829953856508
    175             73460021            76035588            78656904
    176      3.4091449670035    3.44601675819051    3.38939131180859
    177              3120234             3218363             3319351
    178     3.11787422389743    3.09648461826012    3.08964366999711
    179              4828066             4944861             5064386
    180     2.40667969204573    2.39028820797079    2.38840507871515
    181             36556170            37069292            37584580
    182     1.44641969401247     1.3938936179835    1.38049419178845
    183             46237930            46677947            47119728
    184    0.990034781454962   0.947136923375947   0.941994067106625
    185               197390              202507              207892
    186      2.4997005637993    2.55929866954763    2.62442602347393
    187               699393              714612              730216
    188     2.14467454892257    2.15269220591586    2.16006427268952
    189               341874              348451              355043
    190     1.91810125041542    1.90553696063486    1.87412911681131
    191               539940              546076              552166
    192     1.13168973985358    1.13001379235817     1.1090565508692
    193              3592214             3678801             3762581
    194     2.52857305743448    2.38181583713982    2.25182742083291
    195              4791535             4844288             4895242
    196      1.1532718484809    1.09494595424381    1.04634340433544
    197              3623046             3652047             3681386
    198    0.798251925102548   0.800459061242947   0.803357678584078
    199              7135884             7181044             7224602
    200    0.661419473223006   0.632857821119288   0.606569184090787
    201              8698029             8709909             8719925
    202    0.168490349769189   0.136489477895753   0.114929391782973
    203             11321579            11332026            11339894
    204    0.108603882669157  0.0922325673162479   0.069407446002465
    205               137738              139450              141158
    206     1.01062186510087    1.23527841727163    1.21737163135625
    207               153822              155909              157980
    208     1.13367890654299    1.34764139563651    1.31959390251523
    209                58212               59559               60911
    210      2.3499412828226    2.28758988319717    2.24463644959813
    211                58212               59559               60911
    212      2.3499412828226    2.28758988319717    2.24463644959813
    213               783467              788952              794836
    214    0.711682046311523   0.697654028244905   0.743032157223161
    215              1166968             1176995             1187280
    216      0.8962308711661   0.855564798922502   0.870039642466962
    217              7705910             7723921             7748928
    218    0.160148205378629   0.233456972799232   0.323237456440875
    219             10514272            10525347            10546059
    220   0.0331699460503076   0.105277581527518   0.196588748472653
    221             62242278            62510392            63062064
    222    0.285857246854236   0.429833546326634    0.87865693932128
    223             80645605            80982500            81686611
    224    0.272900214681538   0.416877359168292   0.865702643950251
    225               750407              764703              779016
    226     1.92144262779684      1.887179714287    1.85440616629979
    227               971753              989087             1006259
    228     1.81267131886039     1.7680638456514    1.72124773942831
    229                47473               48060               48710
    230    0.331262369198145    1.22891019328509    1.34341175101285
    231                68819               69371               70007
    232   -0.100212778979274   0.798904314260286   0.912632430301816
    233              4901386             4932961             4974525
    234    0.588870489603471   0.642139401786018   0.839047238460703
    235              5614932             5643475             5683483
    236    0.416901360752097   0.507053283010163   0.706423849687002
    237              7795435             7986119             8175446
    238     2.49153029568948    2.41666036509885    2.34303622631907
    239             10157051            10282115            10405832
    240      1.2499609813316     1.2237834052777     1.1960440040584
    241             26439316            27217778            28015534
    242     2.92129941666144    2.90182179829983    2.88887793150377
    243             38000626            38760168            39543154
    244      1.9667158178046    1.97904881035906    1.99994605069585
    245           1023177946          1051853331          1080375610
    246     2.88643403122197    2.80258044185796    2.71162130302747
    247           2018121456          2034317097          2049809214
    248    0.839627250293361   0.802510718661125   0.761538947042538
    249           1326036760          1357236002          1389523758
    250     2.32790542238048    2.35281878610967    2.37893453698703
    251           3078835706          3122586392          3166642585
    252     1.43560828861557    1.42101398638255    1.41088788168906
    253           1236097364          1265610762          1294988514
    254     2.44354303253343    2.38762729049877    2.32123120963156
    255           2261274500          2278232287          2294507020
    256    0.779835746999453    0.74992164816787   0.714357929736423
    257            256569372           259075459           261665732
    258    0.912757873445472   0.976767795962814   0.999814112073039
    259            386051693           388842371           391695432
    260     0.67164735795285   0.722876767697528   0.733732024280869
    261            639571136           644384592           649265373
    262    0.721507363051629   0.752606821831307   0.757432915155732
    263            899035925           903601049           908123143
    264    0.477651668561592   0.507779931041142   0.500452495601294
    265              9925137            10095187            10267878
    266     1.74969304059626    1.69881456105916    1.69616057494596
    267             15722989            15957994            16195902
    268      1.5324232376039    1.48359860231919    1.47983523757956
    269             40052578            40950796            41811127
    270     2.19221928766594    2.21782074687789      2.079125182945
    271             93377890            95592324            97723799
    272     2.31570750353087    2.34379305041137    2.20525980686911
    273            256128892           257681423           259180525
    274    0.612937863159814   0.606152233696463   0.581765647886854
    275            337328817           338486932           339514675
    276     0.34079103938393   0.343319319795924   0.303628560762263
    277              1218964             1249275             1276083
    278     2.99189925859062    2.45620653897091    2.12318467799318
    279              3296367             3323425             3340006
    280     1.33675062099253   0.817492473750786    0.49767257206961
    281             36891840            36890017            36971015
    282  -0.0353294857608428 -0.0049415942468664   0.219325482669056
    283             46620045            46480882            46444832
    284   -0.327669039579687  -0.298951059088036  -0.077588861590047
    285               897846              897427              899949
    286   -0.138346706837646  -0.046678138378712   0.280631504498476
    287              1317997             1314545             1315407
    288   -0.355891802625531  -0.262256175097854  0.0655525295417938
    289             18033421            18949891            19908240
    290     5.00914514744772    4.95714209926538    4.93355368457339
    291             97084366            99746766           102471895
    292     2.74962070654197    2.70542811266671    2.69539316663887
    293            325407780           327011738           328571822
    294    0.493211608060847   0.492907084151454   0.477072783240587
    295            442496175           443601373           444570054
    296    0.243827264206573   0.249764418867571   0.218367448560628
    297            343357557           354372409           365740044
    298     3.31466345407611    3.20798298317342    3.20782169020389
    299            837103087           856705624           876118480
    300      2.4466744446385    2.34171123060258    2.26598909312168
    301              4629925             4651843             4669930
    302    0.794913366886938   0.472281589887817   0.388059711338671
    303              5438972             5461512             5479531
    304    0.460723772785295   0.413560207519414   0.329383885818507
    305               490579              496380              501947
    306     1.21655352939236    1.17554362508096    1.11527739986231
    307               913453              915560              917200
    308    0.262426487072664   0.230397589525045   0.178965105930181
    309             52230911            52647802            53009026
    310    0.841828230821577   0.795000594093388   0.683771061907009
    311             66002289            66312067            66548272
    312    0.516539661307546    0.46824634207038   0.355569240058298
    313                20027               20113               20326
    314    0.385221978741415   0.428500904779754    1.05344825444907
    315                48418               48465               48816
    316   0.0537134606447883  0.0970242533510219   0.721624002974263
    317                24280               24420               24581
    318     0.48718150939254   0.574950249126112   0.657131811028578
    319               108609              109024              109462
    320    0.347720569318122   0.381376448431569   0.400941558500249
    321              1657904             1723968             1787489
    322     4.10587923957758    3.90744566331908    3.61832311366208
    323              1902226             1966855             2028517
    324     3.50515739344233    3.34110410137597    3.08691638355563
    325             52650595            53209683            53802927
    326    0.993032273243824    1.05628515029885    1.10874798285391
    327             64128273            64602298            65116219
    328    0.669740535540783   0.736463978721671   0.792367505802859
    329              2107360             2122558             2140097
    330    0.375199996686463   0.718598582760596   0.822919001311653
    331              3717668             3719414             3725276
    332   -0.300972141406188  0.0469539002549217   0.157481405176007
    333             14519202            15062212            15615136
    334     3.74035654832644     3.6717041527447    3.60516095351136
    335             27525597            28196358            28870939
    336     2.45242725553535    2.40764489206261    2.36426975910316
    337                32411               32452               32520
    338    0.777442704117779   0.126420318087511   0.209321014679124
    339                32411               32452               32520
    340    0.777442704117801   0.126420318087511   0.209321014679146
    341              3819098             3948091             4085492
    342     3.28534873513428    3.32179018690041    3.42099907450637
    343             11055430            11333365            11625998
    344     2.44231614129791    2.48293200818758    2.54927671891363
    345              1228599             1281189             1334486
    346       4.295494657383    4.19140572023042    4.07576459717774
    347              2124869             2189019             2253133
    348     3.05120587130278     2.9743344848207    2.88681938435009
    349               701342              727221              753546
    350     3.66650019825562    3.62347778065125    3.55596443096807
    351              1697753             1743309             1788919
    352     2.68850117447374     2.6479419321498    2.58264954343545
    353               865086              907703              951178
    354     4.91330212264587    4.80883086286954    4.67839838456807
    355              1243941             1295183             1346973
    356     4.12804546690893     4.0367432369581    3.92078547747936
    357              8482139             8463623             8445266
    358   -0.270332500569186  -0.218532602651056   -0.21712848445539
    359             10965211            10892413            10820883
    360   -0.725120806658085  -0.666113290031891  -0.658861360878339
    361                41979               42397               42829
    362    0.964641553554523   0.990811177601195    1.01378382165339
    363               116945              117972              118980
    364    0.887245628443614   0.874356936993021   0.850810358697201
    365                48322               48308               48298
    366    -0.16541916844285 -0.0289765085334987 -0.0207026479426003
    367                56483               56295               56114
    368   -0.577265864735276  -0.333398666658893  -0.322038549275343
    369              7418337             7597137             7779195
    370     2.38984372981724    2.38165577221984    2.36813966071129
    371             15043981            15306316            15567419
    372     1.75716769812924    1.72875760277972    1.69146509302391
    373               157639              158243              158796
    374    0.489654659639951   0.382421749961497   0.348853334023769
    375               167054              167543              167978
    376    0.397066313446001   0.292292129425125   0.259298371028445
    377               197319              198400              199638
    378    0.463267160961952   0.546348640807071   0.622053166799692
    379               747420              751115              755031
    380     0.46319412619233   0.493149307612364    0.52000388065714
    381            958580837           966099548           973637991
    382    0.782344423116356   0.784358575697254   0.780296711203917
    383           1198018009          1204882945          1211637779
    384    0.568612346755913   0.573024441070814   0.560621596316153
    385              7178900             7229500             7291300
    386    0.401982535858467   0.702370950536027   0.851197911086988
    387              7178900             7229500             7291300
    388    0.401982535858444   0.702370950536049   0.851197911086988
    389              4826031             4975954             5127314
    390     3.12146478868638    3.05927202997865    2.99648242974761
    391              8960657             9127846             9294505
    392     1.89595914681821    1.84861910603263     1.8093624065659
    393            232024198           241897121           252221017
    394     4.19613253515445     4.2551264415964     4.2678870907273
    395            686620331           706617213           727129254
    396     2.86125409087026    2.91236380531821    2.90285045745129
    397              2370674             2370234             2360534
    398   0.0811490889940271 -0.0185618451682002  -0.410081982186149
    399              4255689             4238389             4203604
    400   -0.278509062850129  -0.407343184777633  -0.824099163913538
    401              5177805             5356730             5538261
    402     3.46629481629222    3.39724929923072    3.33268382810507
    403             10261206            10412740            10563757
    404     1.49898644905428    1.46596797524852    1.43989350727897
    405              6920508             6928924             6939335
    406    0.118529730670953   0.121535685159276   0.150141441052492
    407              9893082             9866468             9843028
    408    -0.27536875707442  -0.269378767165116  -0.237855001984657
    409           2380491077          2435285428          2489744230
    410     2.31624526073449    2.30180871205172    2.23623897937601
    411           4588990832          4638292836          4686050627
    412     1.09022006496893    1.07435394414404    1.02964156616696
    413           2896559639          2969073351          3042385784
    414      2.5185368775259    2.50344274026529    2.46920248620022
    415           6079383316          6160758687          6241291929
    416     1.35782818124565    1.33854647371605    1.30719682577303
    417            516068562           533787923           552641554
    418     3.46210608661065    3.43352846980824    3.53204525386013
    419           1490392484          1522465851          1555241302
    420     2.19077711984082     2.1520081015116    2.15278726800159
    421            201726825           208293654           214912950
    422     3.31884847510263    3.25530776583629    3.17786733915571
    423            506443358           516900084           527175885
    424     2.13258868648249    2.06473751404199    1.98796659510701
    425            131589503           134866535           138129702
    426      2.5283330144614    2.45984085200218    2.39074535293767
    427            253275918           256229761           259091970
    428     1.21281780987831    1.15950664180271     1.1108549322781
    429            314341737           325494269           337728604
    430      3.5542502182998    3.54790048131596    3.75869444263547
    431            983949126          1005565767          1028065417
    432     2.22075284066526    2.19692669354534    2.23751153215224
    433                43836               43762               43673
    434   -0.134502077702627  -0.168953669268612  -0.203579872449701
    435                84144               83896               83593
    436   -0.230291764982335  -0.295168031488441  -0.361815200458878
    437            413200994           423338709           433595954
    438     2.45726439138516    2.42384462717953    2.39405297719892
    439           1291132063          1307246509          1322866505
    440     1.29754883539168    1.24036218382689    1.18779532005068
    441                 <NA>                <NA>                <NA>
    442                 <NA>                <NA>                <NA>
    443                 <NA>                <NA>                <NA>
    444                 <NA>                <NA>                <NA>
    445              2873286             2903635             2940510
    446     0.84731741369742    1.05070778038882    1.26196353364427
    447              4623816             4657740             4701957
    448    0.526556158855653   0.731001376585771   0.944845307474728
    449             56721882            58236086            60000125
    450     2.20624875982667    2.63451347503895    2.98414485934981
    451             78458928            79961672            81790841
    452     1.45650590321443      1.897214219374    2.26178492558276
    453             24693559            25633615            26400640
    454     4.89990602927568    3.73621356826521    2.94836762030634
    455             35481800            36746488            37757813
    456      4.6654185971967    3.50227675046767    2.71497647013538
    457               303150              306653              309974
    458    0.981212499912214    1.14890826467864    1.07716078295576
    459               323764              327386              330815
    460    0.945885900369151    1.11250475522796    1.04194038662969
    461              7417238             7566988             7724692
    462      1.9432218377876    1.99883511928429    2.06268495608919
    463              8059500             8215700             8380100
    464     1.86605289478673    1.91954379859591    1.98128897562083
    465             41548775            42109853            42247229
    466     1.58783518674393    1.34137129947699   0.325701454305887
    467             60233948            60789140            60730582
    468      1.1592511168095   0.917504095962437 -0.0963761331392095
    469              1507196             1519915             1532278
    470     0.88313681669153   0.840344133328636   0.810110475521579
    471              2773129             2784543             2794445
    472    0.481191238304142   0.410748099114478   0.354975145947794
    473              6858003             7792223             8569127
    474     7.54223486890481    12.7709893541272    9.50396754158412
    475              7694814             8658026             9494246
    476     6.48192864156847    11.7940156559538    9.21991788769051
    477            116262976           116208079           116182717
    478   -0.058733180126223  -0.047229106454087 -0.0218270263170706
    479            127445000           127276000           127141000
    480   -0.144271883387076  -0.132694222147042  -0.106124993746747
    481              9718100             9874723            10032906
    482     1.56749440449158    1.59881336469148    1.58920310567086
    483             17035551            17288285            17542806
    484     1.43944535624798    1.47267087305755    1.46148468519161
    485             11105820            11559254            12021155
    486     4.11020295604234    4.00170330572801    3.91816860510534
    487             44792368            45831863            46851488
    488     2.40993136403743     2.2941780158286    2.20032211952888
    489              2031430             2079480             2131200
    490     2.24975812394332    2.33778839036027    2.45673389059156
    491              5719600             5835500             5956900
    492     1.98473866725772     2.0061078993716    2.05902596620676
    493              3212182             3315806             3420840
    494     3.21139272947192    3.17502743448335    3.11854036117567
    495             14999683            15210817            15417523
    496     1.43049971596447     1.3977751944463    1.34979002036239
    497                56573               58381               60243
    498     3.21336590718728    3.14586548419366    3.13958886153461
    499               113311              114985              116707
    500     1.50539234360214    1.46654343733585    1.48648352823644
    501                14791               14760               14732
    502   -0.229605719455619  -0.209806851690654  -0.189882058950755
    503                47767               47789               47790
    504   0.0837749017517704  0.0460462982726556 0.00209250986102061
    505             41240244            41463573            41645542
    506    0.367213427358424   0.540070702737516   0.437904532582115
    507             50428893            50746659            51014947
    508    0.455218622164662   0.628149861433639   0.527288497407793
    509              3646518             3761584             3908743
    510     7.15682472094729    3.10674056152789    3.83756936489801
    511              3646518             3761584             3908743
    512     7.15682472094729    3.10674056152789    3.83756936489801
    513            430842016           437037355           443212119
    514     1.46907713962318    1.43796073036665    1.41286870089172
    515            547701523           553532904           559317823
    516     1.09068722324909     1.0647005266772    1.04509035654365
    517              2103788             2173853             2247179
    518     3.32869546262147    3.27616397750432     3.3174482684055
    519              6600742             6691454             6787419
    520     1.40264986092754    1.36491228821043    1.42395616494124
    521              4985463             5518096             5637850
    522     9.40207201217122    10.1506595514831    2.14699139072685
    523              5678851             6274342             6398940
    524     9.22649612823576    9.97196954242858    1.96637326864653
    525              2168719             2232447             2297862
    526     3.00639883426674    2.89616226224001    2.88808321188897
    527              4427313             4519398             4612329
    528     2.18235448876975    2.05859453753694    2.03541363752766
    529              4713721             4817782             4908585
    530       2.256390947143    2.18360388213131    1.86720567212084
    531              5985221             6097764             6192235
    532     1.94607768885519    1.86288826279856    1.53739411600028
    533                32118               32307               32517
    534    0.552616963872035   0.586730437555436   0.647910448686892
    535               173978              174804              175623
    536    0.492075368748063   0.473649181569482   0.467430586800065
    537            483150473           489854175           496525353
    538     1.41968975273923    1.38749776200675    1.36187019330805
    539            608642242           615046755           621390109
    540      1.0799569075198     1.0522623239154    1.03136126618537
    541            281078448           292779398           304882547
    542     4.16120917912717    4.16287697731987     4.1338800074997
    543            907133963           929398042           951928215
    544     2.45758233058136    2.45433198492206    2.42416833066666
    545            180779519           186912006           193929402
    546      3.3957921152234    3.39224655200016    3.75438483068874
    547            580126164           594773454           610047256
    548     2.57988680104766    2.52484561272088    2.56800331240071
    549                 5276                5310                5343
    550    0.589299300680763   0.642360039877934   0.619545772178691
    551                36806               37096               37355
    552    0.821163785495365   0.784827172615701   0.695762433860479
    553              3746058             3785336             3828283
    554    0.791284261836151    1.04305673316786    1.12817453869436
    555             20585000            20778000            20970000
    556    0.780301447580142   0.933207942647201    0.91981102654886
    557           1193288207          1225660705          1258365298
    558     2.72320257077578     2.7128817506197     2.6683235308584
    559           3042938624          3089095464          3134736445
    560     1.54291403208143    1.51685083740946    1.47748690617999
    561           2839238809          2911524374          2984633485
    562     2.56180242884514    2.54594875115346    2.51102520909208
    563           6001328521          6082432550          6162743397
    564     1.37069581244296    1.35143458179631    1.32037381984614
    565               541319              555281              570052
    566     2.46378866535774    2.54655380524924    2.62532928147943
    567              2073939             2095242             2118521
    568    0.931108535565663    1.02193622193702    1.10491438799432
    569           1292827614          1321982899          1350567898
    570     2.31076753210489    2.25515642489982    2.16228205535963
    571           2227938007          2243581628          2258039271
    572    0.730121139850041   0.702156924961514   0.644400133231969
    573              1981238             1967853             1952971
    574   -0.829714261085855  -0.677880112448602  -0.759129788432856
    575              2957689             2932367             2904910
    576    -1.01200736818103  -0.859827343070249  -0.940753796646208
    577               486709              500042              513663
    578     2.67460478580703     2.7025686341836    2.68753135102057
    579               543360              556319              569604
    580     2.31117625275217    2.35697870484045    2.35995118136865
    581              1367090             1354815             1344323
    582    -1.02987012367514  -0.901947953010276  -0.777437303718408
    583              2012647             1993782             1977527
    584    -1.07103480329077  -0.941743354234222  -0.818626340945853
    585               593374              604167              615239
    586     1.80391580287387    1.80257581895543    1.81601604156179
    587               593374              604167              615239
    588     1.80391580287387    1.80257581895543    1.81601604156179
    589                 <NA>                <NA>                <NA>
    590                 <NA>                <NA>                <NA>
    591                35639               35261               35020
    592     -1.0800356153793   -1.06630065312841  -0.685821045858366
    593             20180706            20636838            21088840
    594     2.28330351486545    2.23507317556878    2.16662594042245
    595             33803527            34248603            34680458
    596     1.34423384293623     1.3080629397791    1.25305827289905
    597                35425               36110               36760
    598     2.06780986239307    1.91520497126222    1.78404621981199
    599                35425               36110               36760
    600     2.06780986239305    1.91520497126224    1.78404621981199
    601              1216084             1214457             1205007
    602  -0.0996966854324088  -0.133879678887659  -0.781168726225044
    603              2859558             2857815             2835978
    604  -0.0267837694756349 -0.0609720602101012  -0.767049559673864
    605              7991167             8362745             8745781
    606     4.61188256886727    4.54499157992991    4.47846901748497
    607             23588073            24215976            24850912
    608     2.67158913972617     2.6271377711217    2.58818714317603
    609               151014              158802              167825
    610      4.4619130114564    5.02855954432431    5.52636266326511
    611               400728              416738              435582
    612      3.3466404024458    3.91748340604239    4.42253409157761
    613            267522878           274769467           282176862
    614     2.65269365394805     2.7087735651528     2.6958581245856
    615            422790409           431664579           440506473
    616     2.09428576258807    2.09895253323971    2.04832511865654
    617             92306597            93808838            95260846
    618     1.69143612007338    1.61434609111677     1.5359802968669
    619            117290686           118755887           120149897
    620       1.317160792789     1.2414667155537    1.16700884975484
    621                38486               38008               37458
    622    -1.04426093692886    -1.2497874913747   -1.45763585769582
    623                51352               50419               49410
    624    -1.64360804920741   -1.83357958458324    -2.0215255305986
    625           2658459290          2724612368          2790704083
    626     2.50557806805141    2.48839913587693    2.42572909732897
    627           5421202357          5487659096          5552696141
    628        1.24298594148    1.22586715314527    1.18515096988088
    629              1180399             1184330             1188475
    630    0.256853014902241    0.33246969280435   0.349375883502964
    631              2064032             2067471             2070226
    632    0.144870083488649   0.166476984005751   0.133165884651483
    633              6529209             6879609             7243533
    634       5.022845480851    5.22760162150616     5.1547251862909
    635             17004033            17551814            18112907
    636     2.92004450187922    3.17067551792802    3.14674716699948
    637               401597              409992              420192
    638     1.47598972067765    2.06885490827702    2.45741023637283
    639               425967              434558              445053
    640      1.4040502758129     1.9967544083193    2.38639536345808
    641             14919480            15142977            15372077
    642     1.50342843462649    1.48691184984354    1.50158216777508
    643             50648334            51072436            51483949
    644    0.852912565826152   0.833860100341235   0.802515042929721
    645            216421809           222267423           228231045
    646     2.54189044273789    2.70102815747188    2.68308415129283
    647            363310225           370756356           378137339
    648     1.98475985785774    2.04952420483073    1.99079068519058
    649               404636              407105              409418
    650    0.610306674911079   0.608323988548127   0.566550161322711
    651               621207              621810              622159
    652    0.097599636675676  0.0970220113323688  0.0561107244885728
    653              1937464             1978651             2022848
    654     1.97181389729241     2.1035397948285    2.20911200501686
    655              2845153             2902823             2964749
    656     1.87336676028775    2.00668670836189    2.11086627913691
    657                47547               47331               47062
    658   -0.327559347997141  -0.455322357497164  -0.569959060973776
    659                52141               51856               51514
    660   -0.417225485626769  -0.548094105534998  -0.661703101237154
    661              8423977             8821392             9234077
    662     4.62549062487587    4.60976361616416    4.57209821029431
    663             25251731            26038704            26843246
    664     3.07273916417179     3.0689338689503    3.04301971715626
    665              1845503             1929389             2016084
    666     4.75068123756489    4.44515034214684    4.39536435239694
    667              3742959             3843174             3946220
    668     2.89612211417799    2.64221126030642    2.64595710222511
    669               518842              518383              517668
    670  -0.0460535134541202 -0.0885053927473765  -0.138024115320608
    671              1258653             1260934             1262605
    672    0.220398691741504   0.181061469451535   0.132433082605329
    673              2558516             2658225             2763250
    674     3.77602766447333    3.82312039135899    3.87489163876259
    675             16024775            16477966            16938942
    676     2.80676296771096    2.78881323875244    2.75911370133102
    677             21977315            22519314            23057113
    678     2.48962091382674    2.43625559879491    2.36009708644457
    679             30134807            30606459            31068833
    680     1.58743980435977    1.55301827143161    1.49940972361564
    681            285498217           288277446           291027807
    682    0.937940290377171   0.973466324660095   0.954067353572995
    683            351207902           353888902           356507139
    684    0.731728411274219    0.76336551220308    0.73984716254256
    685               986650             1027878             1070588
    686     4.09814271515956    4.09363953141965    4.07115472987953
    687              2204510             2243001             2282704
    688     1.69446732352455    1.73094393878771    1.75460038083267
    689               180545              184770              186949
    690      2.4462502536662     2.3131754186847    1.17240440179519
    691               263650              268050              269460
    692     1.77944042769409    1.65510642970682   0.524642605415484
    693              3023310             3141947             3270216
    694     3.79352937892563    3.84904137760406    4.00133663191279
    695             18653199            19372014            20128124
    696     3.81820594100195    3.78117877025801    3.82885938381233
    697             80580193            84275849            88019904
    698     4.60637987880964     4.4842460335748    4.34676354897871
    699            174726123           179379016           183995785
    700     2.69747404339158     2.6281238528085    2.54118746244735
    701              3518949             3582406             3646573
    702     1.80270773136487    1.78722762349928    1.77531807224623
    703              6119379             6208676             6298598
    704     1.46129517647149    1.44870479177075    1.43794002440995
    705             14987369            15126226            15275237
    706    0.968708647318031   0.922227878488258   0.980296200244103
    707             16804432            16865008            16939923
    708    0.294820793441482   0.359828172726548   0.443220089035769
    709              4078226             4145335             4207493
    710     1.72362520314388    1.63215156469894    1.48833777988021
    711              5079623             5137232             5188607
    712     1.20914158930136    1.12773667734928   0.995084736958414
    713              4878024             4993160             5123648
    714     2.21019240210938    2.33287560370288    2.57977094759409
    715             27381555            27462106            27610325
    716    0.185921856916927   0.293747924382451   0.538270619899636
    717                10694               10940               11185
    718     2.36551856896574    2.27429604672954    2.21477979482578
    719                10694               10940               11185
    720     2.36551856896574    2.27429604672952    2.21477979482578
    721              3830023             3896881             3979802
    722    0.837960846621025     1.7305681183892    2.10555797347703
    723              4442100             4516500             4609400
    724    0.768347998194806    1.66101200071152     2.0360339086492
    725           1045970703          1055611248          1065186798
    726    0.892260877284201   0.921684036880706   0.907109508177584
    727           1314072465          1323005691          1331743060
    728    0.647768318096894   0.679812281128662     0.6604180964177
    729              3010674             3213227             3410010
    730     9.33538620340283    6.51117541603445    5.94394957219363
    731              3816680             4009267             4191776
    732     7.65038581747282     4.9227496816523    4.45160788841889
    733             16705046            17303783            17934319
    734     3.26267746002328    3.58416852009866    3.64391994513569
    735             28225516            28907278            29620768
    736     2.20239694909787    2.41541022668991    2.46820195246332
    737             73126866            74593651            76003799
    738     2.11419372076131    1.98595731741215     1.8727929529853
    739            205337562           208251628           210969298
    740     1.53689764905925    1.40918297342086    1.29655172364692
    741              2524214             2581031             2639227
    742     2.23255301119704    2.22592043741403    2.22971397398894
    743              3821556             3888793             3957099
    744     1.76061359830409    1.74411571812086    1.74123543377264
    745             23126579            23425055            23757776
    746     1.20807239218484    1.28236138920965    1.41037113792164
    747             30038809            30353951            30711863
    748    0.967486220265649    1.04365112789631    1.17223066396866
    749             45765340            46703825            47687037
    750     2.10391813060709    2.02990310350283    2.08335328519515
    751             99700107           101325201           103031365
    752     1.68695605708093     1.6168406041295    1.66983002094225
    753                13685               13795               13908
    754    0.080412300104019   0.800586517660711   0.815800647636551
    755                17805               17796               17794
    756   -0.788793214612497 -0.0505603785945698 -0.0112391121219639
    757              1070117             1099324             1129724
    758     2.66202868184051    2.69274571489298    2.72779096639683
    759              8245627             8464153             8682174
    760     2.69288253584226    2.61569523033674    2.54320066295637
    761             23025350            22960228            22897449
    762   -0.266658549206173   -0.28322811788103  -0.273799474051689
    763             38040196            38011735            37986412
    764  -0.0603600174744649 -0.0748462292920506 -0.0666411009657168
    765            312300609           326105328           340184841
    766     4.52397105170255    4.42033047716535    4.31747407696447
    767            808542070           832469778           856420253
    768     3.02224790162182    2.95936462526929    2.87703837820284
    769              3366210             3310445             3251779
    770    -1.18861464908686   -1.67048609741356    -1.7880390243471
    771              3593077             3534874             3473232
    772    -1.14593097838135    -1.6331283312239   -1.75920825815177
    773             15230608            15351564            15477354
    774     0.75312062827103   0.791027077528119   0.816056522512254
    775             25001819            25126131            25258015
    776    0.457206407016513    0.49597980701513   0.523515080357903
    777              6520333             6546012             6578828
    778    0.400319683483011   0.393056100389624   0.500060471337961
    779             10457295            10401062            10358076
    780   -0.548815210952228  -0.539190466792077  -0.414141101993213
    781              3615222             3683303             3753105
    782     1.86325126778056    1.86566394540798     1.8773593411799
    783              6005652             6090721             6177950
    784     1.38035856649557    1.40654396942745     1.4220035710818
    785              3051946             3134764             3218283
    786     2.72730242340379    2.67744721945882    2.62940953806647
    787              4076708             4173398             4270092
    788     2.40084831983677     2.3440772109436    2.29048005844149
    789               894479              909747              925015
    790     1.71966561931048    1.70691542227375     1.6782687934118
    791              2379069             2405308             2431426
    792     1.10541261626361    1.10291042420376    1.08584846514459
    793            874169758           879980930           885810456
    794    0.647662018248283   0.664764703516553   0.662460492183612
    795           1087230525          1092179620          1097061415
    796    0.438024514241746   0.455201991316429   0.446977302140098
    797               177379              178657              179983
    798  0.70549723895554906   0.717908014312707   0.739463552422834
    799               288032              289873              291787
    800    0.503989817548146   0.637131095156152    0.65811880076494
    801              2010770             2189397             2389099
    802     6.68050961944342    8.51084301590232    8.72901439691289
    803              2035501             2214465             2414573
    804     6.59135749712435    8.42688590935528    8.65116205432081
    805             10778604            10730940            10678041
    806   -0.439904419368495  -0.443190037101381  -0.494176791077897
    807             19983693            19908979            19815616
    808   -0.371323062878035  -0.374575497762261  -0.470052227864654
    809            105998572           106354644           106703732
    810    0.310476221191443    0.33535855064672   0.327692643618675
    811            143506995           143819667           144096870
    812    0.212950704549247      0.217642262118   0.192557946508244
    813              1881346             1928885             1979769
    814     2.40289730368136    2.49546376515201    2.60380554687722
    815             11101350            11368451            11642959
    816      2.3792915199656     2.3775340228914    2.38595568459796
    817            553524624           567494427           581527189
    818     2.56495233321988    2.52379070312145    2.47275767520445
    819           1731137145          1753568847          1775178483
    820     1.34783481404645    1.29577844625362    1.23232321542264
    821             26048619            26651368            27241324
    822     2.38803687622185    2.28757280185453    2.18946037158262
    823             31482498            32125564            32749848
    824     2.12178788388145    2.02203271109992    1.92462213676111
    825             12059405            12466023            12937739
    826     2.81740777930866     3.3161929971543    3.71417608815008
    827             35990704            37003245            38171178
    828      2.3357525902736    2.77449286124806    3.10751167131043
    829              6120588             6347549             6584032
    830     3.64017978513299    3.64105838011659    3.65785694887359
    831             13595566            13970308            14356181
    832     2.71181928883915    2.71905100988297    2.72463609047466
    833              5399162             5469724             5535002
    834     1.61930809858873    1.29844017799226    1.18637693749822
    835              5399162             5469724             5535002
    836     1.61930809858875    1.29844017799226    1.18637693749822
    837               124818              130801              136991
    838     4.72291085790565    4.68204079495116    4.62381459857478
    839               582365              597375              612660
    840      2.5393320797232    2.54476579755385    2.52650758971794
    841              2788451             2887123             2986549
    842     3.55243215830429    3.47742600316281    3.38580356679172
    843              6964859             7140688             7314773
    844     2.56345431818352    2.49317673033998    2.40868716120881
    845              4208463             4276749             4343053
    846     1.65019566330159    1.60956436279992    1.53844154963332
    847              6185642             6209526             6231066
    848    0.394479107083525   0.385376455719377   0.346286117647337
    849                32077               32240               32475
    850    0.669380040841188   0.506865522250349   0.726264491823559
    851                33285               33389               33570
    852    0.460726231664924    0.31196593678607   0.540630657141614
    853              5413081             5680248             5952201
    854     4.59552209593586    4.81764620732388    4.67661733701472
    855             12852485            13309235            13763906
    856     3.25938851032774    3.49209773142402    3.35915033288915
    857              3973872             3963388             3951845
    858   -0.282674139565035  -0.264171920714193  -0.291665657420665
    859              7164132             7130576             7095383
    860   -0.486591387059998  -0.469489291237153  -0.494772574469842
    861            358703042           373796379           389621790
    862     4.19695289154535    4.20775271819412    4.23369831519958
    863            955006479           981414975          1008605106
    864     2.77281641934725    2.76526877887287    2.77050296690247
    865              2045176             2088586             2110349
    866     4.79052774641657    2.10034326249772    1.03660546678671
    867             11106031            11213284            11194299
    868     3.70946573244568   0.961085280307357  -0.169451605224622
    869            358752097           373846598           389673544
    870     4.19673775412089    4.20750181705559    4.23354019661295
    871            955096428           981506334          1008698525
    872     2.77273006883175    2.76515598066922    2.77045496886217
    873             21222571            21865577            22540720
    874     2.76803299955364    3.02982141042196    3.08769807446654
    875             37740469            38493630            39276796
    876      1.8379745040169    1.99563232772758     2.0345340254998
    877               132158              136694              141137
    878     3.54662119868609    3.37466745466625    3.19861988228711
    879               193757              197497              201124
    880     1.99808044739302    1.91185977393148    1.81982389154769
    881               372995              376497              380136
    882    0.937638601850554   0.934506435247963   0.961900434751637
    883               563947              569682              575475
    884     1.04024081216499     1.0118034959337    1.01174763212586
    885              2930811             2926070             2922832
    886   -0.150800099153119  -0.161895078131654  -0.110721647611686
    887              5413393             5418649             5423801
    888    0.107458009492367  0.0970454215834878  0.0950338786482762
    889              1098614             1104335             1109788
    890    0.556609214761085   0.519395851776562   0.492566223748192
    891              2059953             2061980             2063531
    892    0.135726228915631  0.0983519219451147   0.075190689011533
    893              8250182             8362604             8481489
    894     1.21574579485077    1.35346011400427    1.41161621630385
    895              9600379             9696110             9799186
    896    0.847348652246305   0.992219728591882    1.05745468551674
    897               257202              260626              264207
    898     1.27086201276789    1.32246610931627    1.36464573405613
    899              1118319             1125865             1133936
    900    0.616659419518733   0.672496411974745     0.7143137221497
    901                36607               37685               38825
    902     5.52303952564313    2.90226583447674    2.98022317393175
    903                36607               37685               38825
    904     5.52303952564313    2.90226583447676    2.98022317393175
    905                49055               50219               51754
    906     2.61257050252874    2.34513235303269    3.01082826987076
    907                89949               91359               93419
    908     1.84687609296508    1.55539570911507    2.22979505321923
    909             11291540            10337199            10018957
    910    -6.96818698489862   -8.83048301221807    -3.1269944212056
    911             21495821            20072232            19205178
    912    -5.03381015991677   -6.85211767607702   -4.41574372125213
    913                30733               32134               33686
    914     5.03809253917072    4.45776637725078    4.71676611550509
    915                33594               34985               36538
    916     4.60835250125109    4.05719186030487     4.3433416716518
    917              2940466             3064321             3183683
    918     4.03461202868256     4.1257939212315    3.82126913202443
    919             13216766            13697126            14140274
    920     3.55701932397684    3.56998546576354    3.18410081276683
    921           1007911742          1036467128          1064864629
    922     2.91951813583754    2.83312365657508    2.73983614461528
    923           1993077336          2009149689          2024511016
    924    0.844495963801023   0.806408898926932   0.764568567693232
    925            292744000           295136861           297601756
    926    0.761915784765563   0.817390279561664    0.83517016195411
    927            448331271           451001474           453701064
    928     0.55317839895261   0.595587051968096   0.598576757644921
    929              2775764             2884669             2996765
    930     3.88696498316762    3.84841373873587     3.8123211506321
    931              7106229             7288383             7473229
    932     2.55975980782637    2.53099873501229    2.50454574117742
    933             32140444            32841765            33526210
    934     2.24029456733658    2.15858537971524     2.0626492412339
    935             69578602            69960943            70294397
    936    0.607746276565484   0.548005164899527   0.475496516958531
    937              2163443             2219388             2279505
    938     2.40551516810169    2.55305480655429    2.67268319294439
    939              8136610             8326348             8524063
    940     2.23992574371256    2.30513133268516    2.34681604248609
    941              2754527             2827102             2901495
    942     2.58231459360557    2.60064186868859    2.59739626890156
    943              5560095             5663152             5766431
    944      1.8407825771759    1.83654332210519    1.80727172800112
    945            470257313           476995678           483705904
    946     1.46262294198272    1.43291019910198    1.40676872128809
    947            592507619           598949385           605335594
    948     1.11276796637981     1.0872039098623    1.06623517110715
    949               334284              345177              355594
    950     3.30422348878724     3.2066397869655    2.97323016587008
    951              1161555             1184830             1205813
    952     2.07720395195997    1.98396797115519      1.755472385901
    953            213369863           219132659           225012762
    954     2.53870857956973    2.70084815117492     2.6833531007352
    955            359233517           366582958           373867247
    956     1.97973044457392    2.04586728470551    1.98707791538962
    957                25040               24874               24700
    958   -0.609163192030913  -0.665146500034847  -0.701983759761772
    959               107089              106626              106122
    960   -0.384918761668145   -0.43328799977302  -0.473800875069268
    961            553524624           567494427           581527189
    962     2.56495233321988    2.52379070312145    2.47275767520445
    963           1731137145          1753568847          1775178483
    964     1.34783481404645    1.29577844625362    1.23232321542264
    965            358752097           373846598           389673544
    966     4.19673775412089    4.20750181705559    4.23354019661295
    967            955096428           981506334          1008698525
    968     2.77273006883175    2.76515598066922    2.77045496886217
    969               771222              774842              778552
    970     0.45798432142614   0.468286774069044   0.477664656108237
    971              1440729             1450661             1460177
    972    0.721118959711748   0.687007914129209   0.653834662094362
    973              7627127             7745627             7865762
    974     1.53163938550379    1.54171916336469    1.53909907031994
    975             11300284            11428948            11557779
    976     1.12039316984351    1.13215769298935    1.12092816818669
    977             55541423            57081960            58628348
    978     2.48138699860662     2.7359027066291    2.67302040871489
    979             76576117            78112073            79646178
    980     1.71047645801465    1.98593891469702    1.94494215050342
    981                 6312                6407                6497
    982     2.32400875342173    1.49385591133114    1.39493892415093
    983                10918               10899               10877
    984    0.587912771835275  -0.174176145239123  -0.202057379546141
    985             14872630            15703729            16612464
    986     5.38043007755989    5.43755890687981    5.62550569187383
    987             49253643            50814552            52542823
    988     3.02477579129254    3.11994358308313    3.34457430485523
    989              7392635             7813809             8267505
    990     5.45772281362953    5.54083186977244    5.64402176241208
    991             35273570            36336539            37477356
    992      2.8767478792897    2.96898607347744    3.09130915647166
    993             31330995            31223300            31183829
    994  -0.0925714977657659  -0.344325218284398  -0.126495180584707
    995             45489648            45272155            45154036
    996   -0.227691350050529  -0.479262000344597  -0.261249679711809
    997           1465171083          1498951663          1532338785
    998     2.32901695708595    2.30557239300906    2.22736481930173
    999           2378263733          2398563632          2417959696
    1000   0.861808272534631   0.853559624961918   0.808653301552269
    1001             3206813             3220213             3234208
    1002 0.40730670092015703   0.416989737295094   0.433656928554749
    1003             3381180             3391662             3402818
    1004   0.297587135386814   0.309530574282718   0.328384512874787
    1005           256953576           259430732           261950744
    1006   0.914510413562637    0.95943078191871   0.966674782180289
    1007           316059947           318386329           320738994
    1008   0.692860276624581   0.733361519937424    0.73621730882542
    1009            15408910            15640290            15884192
    1010    1.36582488760334    1.49043636045133    1.54741236865574
    1011            30243200            30757700            31298900
    1012    1.56190433294848    1.68690035915161    1.74425837484667
    1013               53890               54053               54267
    1014   0.211766096265541   0.302011476232516   0.395126067987724
    1015              107450              106912              106482
    1016  -0.587382739507594  -0.501955691417342  -0.403010939386318
    1017            26297143            26613545            26913166
    1018    1.25096511298314    1.19599946060666    1.11953125388382
    1019            29838021            30193258            30529716
    1020    1.23962007374554    1.18352016638979    1.10818499531086
    1021               13141               13389               13680
    1022    1.60322537983142    1.86963606537415    2.15014378791619
    1023               28657               28971               29366
    1024    0.82694328769179    1.08975882376595    1.35422128878554
    1025              102715              102718              102703
    1026  0.0233683535137306 0.00292066026413467 -0.0146041544210766
    1027              108041              107882              107712
    1028  -0.135966981900308  -0.147274749553357  -0.157703864887906
    1029            29272925            30212637            31168990
    1030    3.18611624041608    3.15972531846692    3.11634097364234
    1031            90267739            91235504            92191398
    1032    1.07637953190022    1.06639876535975    1.04227094610537
    1033               65254               67107               69002
    1034    2.79405946684771    2.80010134382223    2.78471292403492
    1035              263534              269927              276438
    1036    2.38891492248033    2.39691601558107    2.38350122826717
    1037          3824116789          3904237467          3985184642
    1038    2.10088022755211    2.09514202679337    2.07331586985151
    1039          7229184551          7317508753          7404910892
    1040    1.23638311546821    1.22177268233914     1.1944247960642
    1041               38758               38634               38503
    1042  -0.314279348818066  -0.320446831995619  -0.339655744820821
    1043              199939              201757              203571
    1044   0.911922272588174   0.905168292851843   0.895083542746833
    1045                <NA>                <NA>                <NA>
    1046                <NA>                <NA>                <NA>
    1047             1818117             1812771             1788196
    1048   0.607467946683114  -0.294473630866066    -1.3649323381159
    1049             9055561             9481916             9917199
    1050    4.66634316166535    4.60073614127462    4.48841170048854
    1051            26984002            27753304            28516545
    1052    2.85923731473568    2.81107242907205    2.71295489821162
    1053            34367596            35197669            36223620
    1054    2.18167968589549    2.38657151241312    2.87315334226129
    1055            53873616            54729551            55876504
    1056    1.36162110707575    1.57629422132566    2.07401685853513
    1057             6225773             6512613             6809146
    1058    4.52964758735474    4.50431468282453    4.45259503032351
    1059            15234976            15737793            16248230
    1060    3.27129894213508    3.24711800314636    3.19189626183661
    1061             4426387             4503674             4584076
    1062    1.61353072409736    1.73098324377118    1.76950505703824
    1063            13555422            13855753            14154937
    1064     2.1632674727572    2.19139105592343    2.13629423811133
                        x2016                x2017
    1                   45297                45648
    2       0.784578492708788    0.771898934070187
    3                  104874               105439
    4       0.590062487333077    0.537295706145068
    5               215083329            223732118
    6        4.12819089591419     4.02113406009259
    7               616377331            632746296
    8        2.72815977582972   2.6556727797635298
    9                 8665979              8999963
    10       3.45264290630202      3.7815566165204
    11               34636207             35643418
    12       2.58154939895659     2.86649214750073
    13              190684610            198494008
    14       4.13251994338295     4.09545269542204
    15              419778384            431138704
    16       2.71305851036988     2.70626607586351
    17               18702478             19586972
    18       4.68814539911162      4.6208622901601
    19               29154746             30208628
    20       3.58621100683242      3.5509866361995
    21                1680247              1706345
    22       1.54401448032371     1.54128496391169
    23                2876101              2873457
    24     -0.159880412127734  -0.0919722937442495
    25                  64015                65087
    26      0.990596723520156     1.66074055947853
    27                  72540                73837
    28       1.10060298979862     1.77218271287863
    29              241256824            247298631
    30       2.59883689423303      2.5043051217486
    31              415077960            423664839
    32       2.10969712845126     2.06873884607124
    33                7731918              7821224
    34       1.20295123638322     1.14841080350245
    35                8994263              9068296
    36      0.863868922846647    0.819744473334198
    37               39940546             40410674
    38       1.19260291906809     1.17019595911431
    39               43590368             44044811
    40       1.05718157762752     1.03713389686514
    41                1807826              1799649
    42     -0.449033644936831   -0.453337251352094
    43                2865835              2851923
    44     -0.444257166904663   -0.486625263030829
    45                  43990                43117
    46      -1.85136253358485    -2.00449844640788
    47                  50448                49463
    48      -1.80723076890422    -1.97181874740715
    49                  22502                22518
    50      0.075577392620948   0.0710795232058521
    51                  90564                91119
    52      0.690288328652049      0.6109561448905
    53               20755798             21127403
    54       1.67739012761928     1.77452889208332
    55               24190907             24594202
    56       1.56194049810399     1.65339053503068
    57                5058968              5110858
    58       1.41006179973647     1.02047862584527
    59                8736668              8797566
    60       1.08139629872802    0.694621103604983
    61                5368846              5453517
    62        1.6774015137061     1.56477337981076
    63                9757812              9854033
    64       1.11785721013911     0.98126180450475
    65                1350704              1417430
    66       4.16327337996974     4.82194346767499
    67               10903327             11155593
    68       1.62902475388378     2.28730144695888
    69               11095615             11143219
    70      0.550223217018871    0.428116624357897
    71               11331422             11375158
    72      0.506300002451376    0.385228018372369
    73                5205425              5423582
    74       4.11168025684283     4.10551317806747
    75               11260085             11596779
    76       2.94982814392549     2.94632156118807
    77                5422969              5701421
    78       5.10506038167532       5.007199056703
    79               19275498             19835858
    80       2.93481128912484     2.86565542084027
    81               56057220             58016080
    82       3.46460806179455     3.43472587197033
    83              159784568            161793964
    84       1.23079535447054     1.24972406577957
    85                5298039              5283539
    86     -0.244263619495968   -0.274061374886556
    87                7127822              7075947
    88     -0.701382097841331   -0.730443175298289
    89                1255867              1299292
    90       3.53125631139207     3.39933300067121
    91                1409661              1456834
    92       3.42907892150581     3.29163380324261
    93                 327995               330887
    94      0.935473697741726    0.877856299671164
    95                 395976               399020
    96      0.831528122187361    0.765793758966215
    97                1654095              1646947
    98     -0.508590553513755   -0.433076051259853
    99                3480986              3440027
    100     -1.23730578353764    -1.18362667966933
    101               7354014              7390686
    102     0.707705042568411    0.497427219485653
    103               9469379              9458989
    104    0.0877210878706449   -0.109782322950581
    105                167109               170864
    106       2.2426752004562     2.22216243141438
    107                367313               374693
    108      2.04687117105776      1.9892678402512
    109                 64554                63873
    110     -1.05247097968272    -1.06053459607618
    111                 64554                63873
    112     -1.05247097968271     -1.0605345960762
    113               7741971              7899666
    114      2.05045243494708     2.01641737518805
    115              11263015             11435533
    116      1.54728829634713     1.52010888105833
    117             177986118            179958546
    118      1.12788207695939     1.10209658803347
    119             206859578            208504960
    120       0.8112564782781     0.79226340042227
    121                 86919                86992
    122    0.0241633443244133   0.0839509913546465
    123                278649               279187
    124     0.203329499651748    0.192888292188534
    125                327973               332655
    126      1.50127462693823     1.41746289653652
    127                425994               430276
    128      1.07549615845856     1.00016015852588
    129                295616               303711
    130      2.78941082932879      2.7015276541538
    131                749761               756121
    132     0.868973710626164    0.844692720380736
    133               1598067              1650064
    134      3.18064512045547     3.20193011646315
    135               2352416              2401840
    136      2.02880248969911     2.07922251369081
    137               1991979              2047664
    138      2.58825522023716     2.75710142793745
    139               4904177              4996741
    140      1.74517542237586     1.86986089522388
    141              29357013             29729549
    142      1.18279215486163     1.26100055193218
    143              36109487             36545236
    144      1.13234865466606     1.19952071057872
    145              64046460             63954111
    146    -0.184969578343981   -0.144190639107919
    147             102994278            102740078
    148    -0.255290913083385   -0.246809827629463
    149               6174416              6234162
    150      1.12051359857279    0.962986439023895
    151               8373338              8451840
    152      1.09203117387253    0.933155888738653
    153                 50639                51075
    154     0.836841561696184    0.857311029813121
    155                163721               165215
    156     0.939527209520039    0.908389640157369
    157              15809289             16070668
    158      1.26001054213245     1.63980683517514
    159              18083879             18368577
    160      1.18906102612278     1.56205545588869
    161             787376534            809246214
    162      2.77564176920889       2.739663820353
    163            1387790000           1396215000
    164     0.573050906069647    0.605245013482969
    165              12077997             12505013
    166      3.46061956206331     3.47442364130239
    167              24213622             24848016
    168      2.58067580845527     2.58625429610958
    169              13083840             13605785
    170      4.08732995820266     3.91171890207736
    171              23711630             24393181
    172      2.99217541611777     2.83379809494988
    173              35265313             36983500
    174      4.78393685587812     4.75720209583104
    175              81430977             84283273
    176      3.46603464808537     3.44276695760094
    177               3423356              3530528
    178      3.08520745857073     3.08260783534801
    179               5186824              5312340
    180      2.38886575467523      2.3910852337636
    181              38152200             38896985
    182      1.49895632750725     1.93333180225659
    183              47625955             48351671
    184        1.068611890834     1.51228960592288
    185                213564               219237
    186      2.69178394707373     2.62167828940718
    187                746232               761664
    188       2.1696163026429     2.04689697470843
    189                361750               368695
    190      1.87144566108678     1.90163784757169
    191                558394               564954
    192      1.12160806985756     1.16795054980192
    193               3844155              3923162
    194      2.14486547932902     2.03441484263971
    195               4945205              4993842
    196      1.01547072688832    0.978713315995535
    197               3710316              3739234
    198     0.785845331079102    0.779394531355265
    199               7265272              7303634
    200     0.562937584658641    0.528018772043225
    201               8725410              8726424
    202    0.0628821421971917   0.0116205551936975
    203              11342012             11336405
    204    0.0186756792498581  -0.0494478967755684
    205                142535               142881
    206     0.970775319854354    0.242453239827214
    207                159664               160175
    208      1.06031644056836    0.319536038312865
    209                 62255                63581
    210      2.18250710253431     2.10758301710141
    211                 62255                63581
    212      2.18250710253431     2.10758301710141
    213                801155               807728
    214     0.791863239734561    0.817093172096285
    215               1197881              1208523
    216     0.888918593369278    0.884479029885329
    217               7773650              7805452
    218     0.318529833159244    0.408265433696265
    219              10566332             10594438
    220     0.192048415842684    0.265642663548985
    221              63592936             63861626
    222     0.838301068113641    0.421625391544917
    223              82348669             82657002
    224     0.807218538573871    0.373724559895838
    225                793314               807720
    226      1.81875231399816     1.79963558580576
    227               1023261              1040233
    228      1.67550925836571     1.64501401282776
    229                 48968                49410
    230      0.52826757303175    0.898580930888994
    231                 70075                70403
    232    0.0970860001159586    0.466977884157329
    233               5020143              5059173
    234     0.912853068832918      0.7744611852936
    235               5728010              5764980
    236     0.780392644135167    0.643350903737356
    237               8362698              8547288
    238      2.26458304340081     2.18329373457855
    239              10527592             10647244
    240      1.16332018268349     1.13014592909379
    241              28826081             29639704
    242      2.85214255674006     2.78342462531066
    243              40339329             41136546
    244      1.99343168879913     1.95700247075081
    245            1109067236           1138074091
    246      2.65570841607578     2.61542799736985
    247            2065223450           2080968782
    248     0.751983935613239     0.76240331282311
    249            1421569050           1453160846
    250      2.30620684356806     2.22231878219353
    251            3210110979           3252529883
    252      1.37269656531194     1.32141549863844
    253            1324556747           1354328520
    254      2.28328148708196     2.24767818120517
    255            2310721864           2327134580
    256     0.706680949705699    0.710285225396561
    257             264115588            266247392
    258     0.936254044912531    0.807148118800157
    259             394321096            396482489
    260      0.67033306632996     0.54813019691953
    261             654007193            658228229
    262     0.730336191823994    0.645411250086966
    263             912374705            915855416
    264     0.468170207176399    0.381500164452703
    265              10444726             10630944
    266      1.70767806527617     1.76718316617636
    267              16439585             16696944
    268      1.49338987255655     1.55335626648726
    269              42639712             43469157
    270      1.96235253343461     1.92656265146592
    271              99784030            101789386
    272      2.08630284573945     1.98976849465685
    273             260667414            261973277
    274       0.5736885516379    0.500969024076014
    275             340509447            341246081
    276     0.292998233434247    0.216332911315661
    277               1306640              1340124
    278      2.36637269336708      2.5303190232193
    279               3365287              3396933
    280     0.754064581294597    0.935971649156141
    281              37112875             37311863
    282     0.382971718642725    0.534737452920241
    283              46484062             46593236
    284    0.0844301500680736    0.234587922968563
    285                902145               905267
    286     0.243716597134148    0.345466678877461
    287               1315790              1317384
    288    0.0291122255540077    0.121070631466687
    289              20917553             21975004
    290      4.94549465692669     4.93169626482509
    291             105293228            108197950
    292      2.71605397227228     2.72133142564532
    293             330147530            331569232
    294     0.479562730123575    0.430626271836715
    295             445515422            446215182
    296      0.21264770118772    0.157067514488872
    297             377391735            389236616
    298      3.18578487402381     3.13861695990771
    299             895949553            916099469
    300      2.26351497573707     2.24900117785984
    301               4686120              4699884
    302     0.346086567723842    0.293287960802983
    303               5495303              5508214
    304     0.287421401687637    0.234670531705453
    305                507262               512280
    306      1.05330989712114    0.984371515788986
    307                918371               919019
    308     0.127589742794731   0.0705348376627642
    309              53323902             53654868
    310   0.59224724889287494    0.618752778579461
    311              66724104             66918020
    312     0.263868788565368     0.29020211635109
    313                 20680                21053
    314      1.72661949246537       1.787601814819
    315                 49500                50230
    316      1.39145416003308     1.46397881873087
    317                 24762                24966
    318      0.73364332056554    0.820467923035951
    319                109925               110430
    320     0.422085800331518     0.45835209921657
    321               1847523              1904278
    322      3.30339812975426     3.02571110958661
    323               2086206              2140215
    324      2.80421210921345     2.55591859788162
    325              54382825             54923317
    326      1.07205162087653    0.988958891589791
    327              65611593             66058859
    328     0.757874492811929    0.679374473924223
    329               2155877              2170854
    330     0.734644621724621    0.692303719304226
    331               3727505              3728004
    332    0.0598165991047394   0.0133860746662629
    333              16180685             16745249
    334      3.55775466805047     3.42963292943438
    335              29554303             30222262
    336      2.33938325896993     2.23494542871956
    337                 32565                32602
    338     0.138280731875502    0.113554418573327
    339                 32565                32602
    340      0.13828073187548    0.113554418573327
    341               4230727              4381346
    342      3.49316842068838     3.49821372126964
    343              11930985             12240789
    344      2.58950004128743     2.56349381271807
    345               1388423              1442972
    346      3.96223717125743     3.85363048846145
    347               2317206              2381182
    348      2.80404574599256     2.72348534857532
    349                780290               807291
    350       3.4875580813065     3.40185526099642
    351               1834552              1879826
    352      2.51887831990535     2.43789093573311
    353                995169              1039364
    354      4.52113555525365     4.34516946258918
    355               1398927              1450694
    356      3.78456615093479     3.63365484265393
    357               8446960              8466513
    358    0.0200565633644638    0.231212226024939
    359              10775971             10754679
    360    -0.415913028277122   -0.197783224759792
    361                 43274                43730
    362      1.03365495310061     1.04823726595914
    363                119966               120921
    364     0.825295754248069    0.792907050535382
    365                 48503                48629
    366     0.423549976685475    0.259440906549959
    367                 56186                56171
    368     0.128227978389047  -0.0267006062623867
    369               7964335              8153103
    370       2.3520587206727     2.34251413808824
    371              15827690             16087418
    372      1.65807331834237     1.62765379973835
    373                159284               159668
    374     0.306841287024032     0.24078869894743
    375                168346               168606
    376     0.218836685375473    0.154324682210868
    377                201021               202552
    378     0.690365369484126    0.758726349149973
    379                759087               763252
    380      0.53575873239208    0.547185665228521
    381             981367682            988569205
    382      0.79389784205739     0.73382516380849
    383            1218545401           1224733163
    384     0.570106191777953    0.507799052454018
    385               7336600              7393200
    386     0.619366345332479    0.768513877614955
    387               7336600              7393200
    388     0.619366345332479    0.768513877614955
    389               5280355              5435026
    390      2.94113949878593     2.88709740638841
    391               9460798              9626842
    392      1.77333693683233     1.73985038012617
    393             263047946            274358545
    394      4.29263553401657     4.29982410887177
    395             748356635            770390212
    396      2.91934080264636     2.94426159527536
    397               2354458              2337248
    398    -0.257731226832978   -0.733638347773647
    399               4174349              4124531
    400     -0.69838345731273    -1.20061016480925
    401               5720767              5903901
    402      3.24223347457675      3.1510431553484
    403              10713849             10863543
    404      1.41082121304761     1.38753007437198
    405               6946267              6955524
    406    0.0998444370776508    0.133177104564375
    407               9814023              9787966
    408    -0.295110604845478   -0.265860932255517
    409            2543322236           2596413659
    410      2.15194819429303     2.08748314501821
    411            4731948335           4776473096
    412     0.979453950743675    0.940939288594336
    413            3115801428           3189667379
    414      2.41309449926091     2.37068865609365
    415            6321324033           6401367406
    416       1.2823002819037     1.26624378978421
    417             572479192            593253720
    418      3.58960303589477     3.62887040966898
    419            1589375698           1624894310
    420      2.19479742186013     2.23475242793099
    421             221601322            228567551
    422      3.11213074875198     3.14358639069852
    423             537358412            547984961
    424       1.9315236697521     1.97755329826306
    425             141370295            144572428
    426      2.31895402779747     2.23979602121327
    427             261850182            264498852
    428      1.05894205868913     1.00643952446092
    429             350877870            364686169
    430      3.89344161088587     3.93535762172748
    431            1052017286           1076909349
    432      2.32980008897626      2.3661268052586
    433                 43680                43844
    434    0.0160269252687058    0.374754791682734
    435                 83450                83580
    436    -0.171213441967113    0.155660691192532
    437             444186310            455009748
    438      2.41309726336112       2.407475136258
    439            1338636340           1354195680
    440      1.18504622906796     1.15562449067646
    441                  <NA>                 <NA>
    442                  <NA>                 <NA>
    443                  <NA>                 <NA>
    444                  <NA>                 <NA>
    445               2983355              3026107
    446      1.44654704678436     1.42284687903925
    447               4755335              4807388
    448      1.12883406399428        1.08867556062
    449              61546643             62866706
    450      2.54486645662248     2.12213970743413
    451              83306231             84505076
    452      1.83580791714073     1.42882552688137
    453              27124936             27844960
    454      2.70652001281654     2.61985281061518
    455              38697943             39621162
    456      2.45940250005547     2.35769232939234
    457                314424               322016
    458      1.42539705253955     2.38588402314435
    459                335439               343400
    460      1.38808149583838     2.34558267643862
    461               7884198              8045513
    462      2.04385516915739     2.02540415954727
    463               8546000              8713300
    464       1.9603489624817     1.93872567441099
    465              42351339             42462869
    466      0.24612722181536    0.262998539787345
    467              60627498             60536709
    468    -0.169884073301448    -0.14986111697397
    469               1544229              1555222
    470     0.776923966989195    0.709354404749677
    471               2802695              2808376
    472     0.294793625837463    0.202492605820257
    473               9018612              9270152
    474      5.11245815246083     2.75093344551646
    475               9964656             10215381
    476      4.83585013508326     2.48500934190509
    477             116219897            116223820
    478    0.0319961980437917  0.00337544077423152
    479             127076000            126972000
    480   -0.0511374152133631   -0.081874296046114
    481              10189588             10342139
    482      1.54961238038118     1.48602996027008
    483              17794055             18037776
    484      1.42204614032925     1.36038126908993
    485              12502904             13001604
    486       3.9292922985166     3.91117971395226
    487              47894670             48948137
    488      2.20214556198549     2.17570842690355
    489               2185215              2239720
    490      2.50290205121746     2.46366361776899
    491               6079500              6198200
    492       2.0372244221997     1.93364715643057
    493               3528344              3637892
    494      3.09425041086978     3.05757539541515
    495              15624584             15830689
    496      1.33408515743887      1.3104826630319
    497                 62160                64107
    498      3.13253230109597     3.08418562730442
    499                118513               120362
    500      1.53561388437723     1.54812092405828
    501                 14714                14705
    502     -0.12225770727462  -0.0611849504114345
    503                 47788                47785
    504  -0.00418506350895554 -0.00627792368344132
    505              41774264             41861498
    506      0.30861280632646    0.208604619717282
    507              51217803             51361911
    508     0.396851823381145    0.280968018301691
    509               4048085              4124904
    510      3.50280908539676      1.8798817000133
    511               4048085              4124904
    512      3.50280908539676     1.87988170001332
    513             449422888            455795478
    514      1.40130847820974     1.41794959049794
    515             565136972            571144463
    516       1.0404011387994     1.06301503841443
    517               2324870              2405044
    518       3.3988471695968      3.3904075111084
    519               6891363              6997917
    520      1.51981369057872     1.53436449506028
    521               5524233              5402350
    522     -2.03583729213285    -2.23103701477218
    523               6258619              6109252
    524      -2.2172797902348     -2.4155210315248
    525               2365002              2431748
    526      2.87997422054647     2.78314727046078
    527               4706097              4796631
    528      2.01259665687601     1.90548944068251
    529               4996859              5090937
    530      1.78238021340451     1.86523850308041
    531               6282196              6378261
    532       1.4423513120791      1.5175888785251
    533                 32739                32974
    534     0.680399829733986    0.715234433992432
    535                176413               177163
    536     0.448818487957651    0.424237575434501
    537             503147437            509636804
    538      1.33368496895262     1.28975455756917
    539             627668470            633797190
    540       1.0103735011334    0.976426297150155
    541             317574026            330795768
    542       4.1627436942135      4.1633574907036
    543             975265947            999288864
    544      2.45162730048925      2.4632170408386
    545             201638507            209853209
    546      3.97521207227773     4.07397481870862
    547             626382245            643485701
    548      2.67765961396276     2.73051417669095
    549                  5379                 5424
    550     0.671519031547113    0.833106734698888
    551                 37609                37889
    552     0.677661202777028    0.741744903878247
    553               3882481              3942265
    554      1.40579824719896     1.52810489553451
    555              21203000             21444000
    556      1.10498361865849     1.13022048648777
    557            1291311833           1324700256
    558      2.61820117356733     2.58562046337261
    559            3180221060           3225575822
    560      1.45098689468932     1.42615123742374
    561            3057914041           3131897864
    562      2.45526147073969     2.41942127895152
    563            6242647248           6322861019
    564      1.29656300534752     1.28493198179184
    565                585642               601912
    566       2.6981096648983     2.74025738348507
    567               2143872              2170617
    568      1.18953362244047      1.2397919001165
    569            1379057974           1407844671
    570      2.10948861158255     2.08741746487303
    571            2272117455           2286564636
    572      0.62346940466476    0.635846574225624
    573               1932212              1909625
    574      -1.0686342214764    -1.17585729154485
    575               2868231              2828403
    576     -1.27069453298857    -1.39832220092102
    577                526490               541038
    578      2.46649296367326     2.72571786341686
    579                582014               596336
    580      2.15531198799205     2.43097641776115
    581               1332897              1322185
    582    -0.853577171409638   -0.806909776930881
    583               1959537              1942248
    584    -0.913885332226604   -0.886215573292392
    585                626688               638609
    586      1.84379989293183     1.88435643808871
    587                626688               638609
    588      1.84379989293183     1.88435643808871
    589                  <NA>                 <NA>
    590                  <NA>                 <NA>
    591                 34811                34496
    592     -0.59858980697911   -0.909005350138279
    593              21541817             21994745
    594      2.12520322523312     2.08075383299536
    595              35107264             35528115
    596      1.22317023430594      1.1916294240897
    597                 37071                37044
    598     0.842469530267675  -0.0728597482140499
    599                 37071                37044
    600     0.842469530267675  -0.0728597482140499
    601               1191662              1172526
    602     -1.11364046680987    -1.61885754537528
    603               2803186              2755189
    604     -1.16302237797705    -1.72705846625605
    605               9143976              9557640
    606      4.45238891582207     4.42455328460541
    607              25501941             26169542
    608      2.58601148442291     2.58416485189804
    609                176949               186048
    610      5.29397856367578     5.01431494748377
    611                454252               472442
    612      4.19690422374828     3.92628779480377
    613             289100440            295656568
    614      2.45363065948334     2.26776825382902
    615             448917409            456885486
    616      1.90937852574984     1.77495388689637
    617              96701350             98108030
    618       1.5008486774346     1.44418554060996
    619             121519221            122839258
    620      1.13323428792033     1.08042073770937
    621                 36840                36161
    622      -1.6636093920556    -1.86030213782625
    623                 48329                47187
    624     -2.21210382968728    -2.39133636341706
    625            2856275534           2922044655
    626      2.34963826510443     2.30261822492648
    627            5616265003           5679375318
    628       1.1448287532001     1.12370614574435
    629               1192987              1197983
    630     0.378927347892163    0.417906311754642
    631               2072490              2074502
    632     0.109300286352773   0.0970341952851146
    633               7626464              8028117
    634      5.15152332659867     5.13257007550893
    635              18700106             19311355
    636       3.1904414229907     3.21640725807966
    637                430220               442474
    638      2.35849560810573     2.80849999019216
    639                455356               467999
    640      2.28861544064371     2.73866285589301
    641              15610256             15854871
    642      1.53754523391003     1.55486375859647
    643              51892349             52288341
    644     0.790127200535222    0.760205959612812
    645             233682637            238848263
    646      2.38862859345012     2.21053051536731
    647             385054538            391607422
    648      1.82928219104012     1.70180671913027
    649                411597               413735
    650     0.530807626226144    0.518095695405891
    651                622303               622373
    652    0.0231425307711559    0.011247907097728
    653               2069095              2116539
    654        2.260489418467     2.26708938461354
    655               3029555              3096030
    656      2.16233694829923     2.17049015538793
    657                 46758                46432
    658    -0.648051724420982    -0.69964873877015
    659                 51133                50729
    660    -0.742353404784604   -0.793234215651641
    661               9673277             10129295
    662      4.64664724028885      4.6064585376107
    663              27696493             28569441
    664      3.12915549916881      3.1031851393148
    665               2105443              2197486
    666      4.33688802912309     4.27880833819171
    667               4051890              4160015
    668      2.64252802228661     2.63352411058118
    669                516887               516481
    670    -0.150982818383316  -0.0785780130389972
    671               1263473              1264613
    672    0.0687231379365168   0.0901868114668421
    673               2872972              2988658
    674      3.89395108305346     3.94774231776165
    675              17405624             17881167
    676      2.71781408401004     2.69546636843743
    677              23594371             24124786
    678      2.30338547724785     2.22316100866651
    679              31526418             31975806
    680       1.4620698661924     1.41536933264598
    681             293894567            296582138
    682     0.985046765651504    0.914467738357345
    683             359245796            361731237
    684     0.768191348897489    0.691849710608722
    685               1114303              1158740
    686      4.00210670941333     3.91041105310537
    687               2323352              2364534
    688      1.76502653275581     1.75699918544662
    689                188670               190260
    690     0.916360483850812    0.839210055478929
    691                270220               270810
    692     0.281648570411144    0.218102594636015
    693               3408152              3554150
    694       4.1314171137949     4.19457258954785
    695              20921743             21737922
    696      3.86709120542471     3.82693398223338
    697              91848722             95817238
    698      4.25799268529955     4.22997085854151
    699             188666931            193495907
    700      2.50703408562558     2.52731691971774
    701               3711507              3778085
    702      1.76501711653341     1.77792738991817
    703               6389235              6480532
    704      1.42874739634574     1.41880628469208
    705              15435425             15602670
    706      1.04321719129238     1.07768610579971
    707              17030314             17131296
    708     0.532178879608489    0.591203366164118
    709               4265348              4320306
    710      1.36567886021458     1.28024610929856
    711               5234519              5276968
    712     0.880969815359919    0.807673181884948
    713               5277466              5449547
    714      2.95793729314515     3.20864276792533
    715              27861186             28183426
    716      0.90447404681918     1.14995372279388
    717                 11437                11682
    718      2.22801188569867     2.11954818190381
    719                 11437                11682
    720      2.22801188569867     2.11954818190381
    721               4072982              4162127
    722      2.31433399944735     2.16508323721803
    723               4714100              4813600
    724      2.24603210015584     2.08872272281936
    725            1074888213           1084145276
    726     0.910771239205687    0.861211695136532
    727            1340532342           1348646085
    728     0.659983315400197    0.605262756129747
    729               3628408              3795173
    730      6.20787607423508     4.49360115215339
    731               4398070              4541854
    732      4.80412978549779     3.21694904054482
    733              18545164             19100755
    734      3.40601168073347     2.99588075899464
    735              30309723             30940039
    736      2.32591876078298      2.0795835052666
    737              77368591             78853074
    738      1.77975713234327     1.90054008619129
    739             213524840            216379655
    740      1.20405567252781     1.32813543357738
    741               2698732              2759313
    742      2.22959618173762     2.21997026314659
    743               4026336              4096063
    744      1.73456002172234     1.71694872170027
    745              24140046             24563784
    746      1.59622337986867     1.74010418538374
    747              31132779             31605486
    748      1.36122543775229     1.50694618507609
    749              48740780             49827667
    750      2.18564513974413     2.20543406602974
    751             104875266            106738501
    752      1.77382446993613     1.76102262001195
    753                 14035                14156
    754     0.908999566823076    0.858435266937116
    755                 17816                17837
    756     0.123560813249084    0.117802162109984
    757               1161342              1194221
    758      2.76028778775784      2.7917856989411
    759               8899169              9114796
    760      2.46859434505795     2.39411255967524
    761              22849639             22824769
    762    -0.209018844680892   -0.108901258043135
    763              37970087             37974826
    764     -0.04298513087435   0.0124800985844923
    765             354766158            369902581
    766      4.28629240419329     4.26659157269449
    767             880891473            905987912
    768      2.85738455089992      2.8489819426371
    769               3188713              3112035
    770     -1.95848440829816    -2.43405340742061
    771               3406672              3325286
    772     -1.93497109034618     -2.4180176217623
    773              15607248             15737956
    774     0.835749842101323    0.833995258817182
    775              25389611             25516321
    776     0.519654351165206    0.497821203562078
    777               6617169              6659350
    778     0.581102100756801    0.635424721292054
    779              10325452             10300300
    780    -0.315459016997883   -0.243889410358812
    781               3824264              3895863
    782       1.8782533083646     1.85491904278946
    783               6266615              6355404
    784      1.42498352233719     1.40691381584073
    785               3302741              3380930
    786      2.59047422456446      2.3398089806951
    787               4367088              4454805
    788      2.24610527580178     1.98868634871278
    789                940354               955660
    790      1.65824337983709     1.62768489313598
    791               2457814              2484263
    792      1.08528904437148     1.07611886009276
    793             891748308            897126460
    794     0.670329861177436    0.603102013399052
    795            1102020063           1106214534
    796     0.451993656161903    0.380616573221133
    797                181215               182541
    798     0.682176965402824    0.729063368543106
    799                293541               295450
    800      0.59932388341049    0.648229492369303
    801               2569604              2686753
    802      7.28354940135209     4.45815998975693
    803               2595166              2711755
    804      7.21280238660749     4.39455430208811
    805              10619522             10565369
    806    -0.549538435633047   -0.511242828196256
    807              19702267             19588715
    808    -0.573660845379617   -0.578007015146885
    809             107050095            107349517
    810     0.324076820521198     0.27931225422418
    811             144342397            144496739
    812     0.170245238698228    0.106870569429971
    813               2034934              2094446
    814      2.74832147024909     2.88256933269116
    815              11930899             12230339
    816      2.44299698959529     2.47880785239257
    817             595910612            610760436
    818      2.47338787799998     2.49195495112278
    819            1796850154           1818868706
    820      1.22081645353067     1.22539722920045
    821              27869503             28592972
    822      2.27979234469454     2.56279448227304
    823              33416270             34193122
    824        2.014458627481     2.29816056079707
    825              13435884             13981657
    826      3.77804939059879     3.98172180574424
    827              39377169             40679828
    828      3.11054531285464     3.25461617065729
    829               6829288              7084752
    830      3.65730985447915     3.67244468263607
    831              14751356             15157793
    832       2.7154429589038      2.7179778377713
    833               5607283              5612253
    834      1.29743609938602   0.0885954699924485
    835               5607283              5612253
    836      1.29743609938602   0.0885954699924263
    837                143352               149877
    838      4.53879138585545     4.45118136142388
    839                628102               643634
    840      2.48924408661005     2.44276679046656
    841               3089590              3196631
    842      3.39198558007286     3.40590466873748
    843               7493913              7677565
    844       2.4195089151865     2.42113485779471
    845               4406672              4466558
    846      1.45421990179833     1.34983335924662
    847               6250510              6266654
    848     0.311563471293443    0.257949946994399
    849                 32789                33059
    850     0.962253073207237    0.820075061278381
    851                 33834                34056
    852     0.783340306115649    0.654001447275102
    853               6262554              6598376
    854      5.08270220064164     5.22354690201429
    855              14292847             14864221
    856      3.77095438271416     3.91978477697357
    857               3939250              3927608
    858    -0.319220860868051    -0.29597606817961
    859               7058322              7020858
    860    -0.523694463062603   -0.532191341024444
    861             405715080            422172204
    862      4.13049023772516     4.05632543902486
    863            1036061038           1063789157
    864      2.72216865021502     2.67630168329909
    865               2112741              2061940
    866     0.113281992844133    -2.43388693742046
    867              11066105             10658226
    868     -1.15177978960304    -3.75548445756917
    869             405767939            422226126
    870      4.13022522257759      4.0560589978007
    871            1036155715           1063885000
    872      2.72204125608293     2.67616967204587
    873              23195834             23795649
    874      2.90635791580749     2.58587382544641
    875              40032809             40727936
    876      1.92483368551753     1.73639326683271
    877                145477               149719
    878      3.02869486591267     2.87422053646305
    879                204632               208036
    880      1.72916107570727     1.64978976233713
    881                383992               388030
    882      1.00926356091477     1.04609371845204
    883                581453               587559
    884      1.03343565089089     1.04465232671753
    885               2922095              2923642
    886   -0.0252184502528786   0.0529274593131155
    887               5430798              5439232
    888     0.128922329956911    0.155178995687406
    889               1115536              1121491
    890     0.516600080108571    0.532404308612749
    891               2065042              2066388
    892    0.0731972072415279   0.0651590392490152
    893               8618398              8764881
    894      1.60131965237834     1.68537210192368
    895               9923085             10057698
    896      1.25645398510305     1.34744505956262
    897                268025               272016
    898      1.43473729255817     1.47806283530696
    899               1142524              1151390
    900     0.754508380699921    0.773005801268303
    901                 39969                40574
    902      2.90397846114339     1.50233137329532
    903                 39969                40574
    904      2.90397846114339      1.5023313732953
    905                 52859                53922
    906      2.11262672564791     1.99105668034239
    907                 94677                95843
    908      1.33763477950025     1.22403375818123
    909              10019763             10156105
    910    0.0080444259956123     1.35155598226393
    911              18964252             18983373
    912      -1.2624196887222    0.100775748776037
    913                 35384                36982
    914      4.91774197898315     4.41715672187687
    915                 38246                39844
    916      4.56861647264044     4.09328496421367
    917               3309161              3448331
    918      3.86559802145116     4.11956629350938
    919              14592585             15085884
    920      3.14864850561675     3.32459491646661
    921            1093427435           1122304700
    922      2.68229455858844      2.6409859562377
    923            2039794828           2055414680
    924     0.754938445837539    0.765756035145699
    925             299939207            301974778
    926     0.785429169309054     0.67866119283299
    927             456167799            458170561
    928     0.543691693877093    0.439040634694152
    929               3112655              3232367
    930      3.79426862325881     3.77386264448345
    931               7661354              7852795
    932      2.48615624048351     2.46807888446754
    933              34207697             34881915
    934      2.01231562414993      1.9517822830395
    935              70607037             70898202
    936     0.443771939293856    0.411525980201064
    937               2342835              2408285
    938      2.74034203782319     2.75531414141805
    939               8725318              8925525
    940      2.33358093103683     2.26862350493592
    941               2977004              3053007
    942      2.56913042668236     2.52095809095721
    943               5868561              5968383
    944      1.75561140344976     1.68665774624937
    945             490376339            496934006
    946      1.37902699653631     1.33727231076702
    947             611668087            617875842
    948      1.04611277822859     1.01488946896784
    949                365532               375606
    950      2.75641960505194     2.71868949903809
    951               1224562              1243235
    952      1.54292008005404     1.51336243484312
    953             230379896            235467333
    954      2.38525759707797     2.20828166360489
    955             380687450            387152617
    956      1.82423120899917     1.69828740085862
    957                 24547                24424
    958    -0.621359645304852   -0.502339174830626
    959                105707               105415
    960    -0.391825983174624   -0.276617487700869
    961             595910612            610760436
    962      2.47338787799998     2.49195495112278
    963            1796850154           1818868706
    964      1.22081645353067     1.22539722920045
    965             405767939            422226126
    966      4.13022522257759      4.0560589978007
    967            1036155715           1063885000
    968      2.72204125608293     2.67616967204587
    969                782418               786693
    970     0.495334042876327    0.544895877661622
    971               1469330              1478607
    972     0.624885332854343    0.629391376030142
    973               7986686              8107611
    974        1.525648852693     1.50273448550641
    975              11685667             11811443
    976      1.10043313328375     1.07057592442396
    977              60062918             61275130
    978      2.41743122457627     1.99814048713847
    979              81019394             82089826
    980      1.70945078412567     1.31255286212126
    981                  6581                 6662
    982      1.28461775755881     1.22330302975058
    983                 10852                10828
    984    -0.230107331502793   -0.221402304463059
    985              17589735             18597942
    986      5.71622362638008     5.57354361783108
    987              54401802             56267032
    988      3.47687650082591     3.37115080503874
    989               8766415              9307879
    990      5.85951722278422     5.99333029060737
    991              38748299             40127085
    992      3.33499474701031     3.49646846553214
    993              31122532             31043768
    994    -0.196760070053521   -0.253397876809837
    995              45004673             44831135
    996    -0.331333796371949   -0.386345310894755
    997            1564963701           1597344399
    998      2.12909288202869     2.06910217657502
    999            2436043943           2453799496
    1000     0.74791350037458    0.728868337987933
    1001              3247994              3259303
    1002    0.425349928465067    0.347579383128451
    1003              3413766              3422200
    1004     0.32121688556848     0.24675383930737
    1005            264473000            266788716
    1006    0.958268060593122    0.871785260117191
    1007            323071755            325122128
    1008    0.724676067451429     0.63264399508256
    1009             16130961             16372437
    1010     1.54160679563816     1.48587813992679
    1011             31847900             32388600
    1012     1.73884926199094     1.68350632535532
    1013                54436                54657
    1014    0.310939231046346     0.40515945535864
    1015               105963               105549
    1016   -0.488598020662577   -0.391467650725961
    1017             27103212             26951752
    1018    0.703663559835989   -0.560394032904996
    1019             30741464             30563433
    1020    0.691185784244756   -0.580806784389088
    1021                13965                14230
    1022     2.06192872027356     1.87982127046208
    1023                29739                30060
    1024     1.26217731781244     1.07360686059582
    1025               102656               102564
    1026  -0.0457734998910416  -0.0896598832113406
    1027               107516               107281
    1028   -0.182132486647949   -0.218811336994337
    1029             32137965             33111857
    1030     3.06143512628087     2.98533941955856
    1031             93126529             94033048
    1032     1.00922663268309    0.968719959338428
    1033                70980                73033
    1034     2.82626575339164     2.85132465785753
    1035               283218               290239
    1036     2.42303547677723     2.44878023745694
    1037           4066384935           4147418821
    1038     2.03755409835287     1.99277459697751
    1039           7491934113           7578157615
    1040     1.17520956388573     1.15088441381759
    1041                38398                38312
    1042   -0.273078543192931   -0.224221186365574
    1043               205544               207630
    1044    0.964528455768062     1.00975265821375
    1045                 <NA>                 <NA>
    1046                 <NA>                 <NA>
    1047              1777557              1791003
    1048   -0.596734073866868    0.753584842739213
    1049             10361240             10817186
    1050     4.38013982335752     4.30642448674733
    1051             29274002             30034389
    1052      2.6215373231585     2.56432067801961
    1053             36866878             37298236
    1054     1.76021405295829     1.16325005336645
    1055             56422274             56641209
    1056     0.97200398206436    0.387278487857363
    1057              7115902              7434012
    1058     4.40652894470669     4.37336881519047
    1059             16767761             17298054
    1060     3.14740749310829     3.11359549302693
    1061              4667645              4755312
    1062     1.80661031506654     1.86076471544313
    1063             14452704             14751101
    1064     2.08180572500247     2.04361989880383

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
