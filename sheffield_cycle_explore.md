    library(remotes)
    library(rmarkdown)
    library(skimr)
    library(sf)

    ## Linking to GEOS 3.9.1, GDAL 3.3.2, PROJ 7.2.1; sf_use_s2() is TRUE

    library(pct)
    library(stats19)

    ## Data provided under OGL v3.0. Cite the source and link to:
    ## www.nationalarchives.gov.uk/doc/open-government-licence/version/3/

    library(tidyverse)

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.7     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.0
    ## ✔ readr   2.1.2     ✔ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

    library(tmap)
    library(tmaptools)
    library(knitr)
    library(stplanr)
    library(osmdata)

    ## Data (c) OpenStreetMap contributors, ODbL 1.0. https://www.openstreetmap.org/copyright

    library(osmextract)

    ## Data (c) OpenStreetMap contributors, ODbL 1.0. https://www.openstreetmap.org/copyright.
    ## Check the package website, https://docs.ropensci.org/osmextract/, for more details.

    library(ukboundaries)

    ## Using default data cache directory ~/.ukboundaries/cache 
    ## Use cache_dir() to change it.

    ## Contains National Statistics data © Crown copyright and database right2022
    ## Contains OS data © Crown copyright and database right, 2022
    ## See https://www.ons.gov.uk/methodology/geography/licences

    tmap_mode("plot")

    ## tmap mode set to plotting

## Sheffield cycle journeys

    cycle_trips = get_pct_rnet(region="south-yorkshire") %>%
      select(bicycle)

    zones = get_pct_zones("south-yorkshire") %>%
      select(lad_name)

    zones_sheffield = zones %>% ##sheffield lsoa zones
      filter(lad_name == "Sheffield")

    rnet_sheffield = cycle_trips[zones_sheffield, ]

    plot(rnet_sheffield, key.pos = 1)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-7-1.png)
\## Sheffield MSOA Zones

    sy_msoa = get_pct_zones(region = "south-yorkshire", geography = "msoa")

    sheff_msoa = sy_msoa %>%
      filter(lad_name == "Sheffield")

    tm_shape(zones_sheffield) +
      tm_borders()

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    tm_shape(rnet_sheffield) +
      tm_lines()

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-11-1.png)
\## Travel To Work Area

    remotes::install_github("robinlovelace/ukboundaries")

    ## Skipping install of 'ukboundaries' from a github remote, the SHA1 (1d162f0d) has not changed since last install.
    ##   Use `force = TRUE` to force installation

    region_ttwa = ttwa_simple %>% 
      filter(grepl("Sheffield", ttwa11nm)) %>% 
      select(Name = ttwa11nm)

    ## old-style crs object detected; please recreate object with a recent sf::st_crs()

    region_ttwa$Name = "Sheffield (TTWA)"

    plot(region_ttwa)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    tm_shape(zones_sheffield) +
      tm_borders(lwd = 1) +
    tm_shape(rnet_sheffield) +
      tm_lines(lwd = 1, palette = "plasma" , breaks = c(0, 5, 10, 20, 50, 100, 200, 300, 400), col = "bicycle", alpha = 0.8) +
      tm_layout(legend.outside = TRUE)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-15-1.png)
\## OSM current cycle network

    extra_tags= c("maxspeed", "bicycle")
    osm_data_region = osmextract::oe_get_network("south yorkshire", mode = "cycling", extra_tags = extra_tags)

    ## The input place was matched with: South Yorkshire

    ##   |                                                                              |                                                                      |   0%  |                                                                              |                                                                      |   1%  |                                                                              |=                                                                     |   1%  |                                                                              |=                                                                     |   2%  |                                                                              |==                                                                    |   2%  |                                                                              |==                                                                    |   3%  |                                                                              |===                                                                   |   5%  |                                                                              |====                                                                  |   6%  |                                                                              |=====                                                                 |   6%  |                                                                              |=====                                                                 |   7%  |                                                                              |=====                                                                 |   8%  |                                                                              |======                                                                |   9%  |                                                                              |=======                                                               |   9%  |                                                                              |=======                                                               |  10%  |                                                                              |=======                                                               |  11%  |                                                                              |========                                                              |  11%  |                                                                              |========                                                              |  12%  |                                                                              |=========                                                             |  12%  |                                                                              |=========                                                             |  13%  |                                                                              |=========                                                             |  14%  |                                                                              |==========                                                            |  14%  |                                                                              |==========                                                            |  15%  |                                                                              |===========                                                           |  15%  |                                                                              |===========                                                           |  16%  |                                                                              |============                                                          |  16%  |                                                                              |============                                                          |  17%  |                                                                              |============                                                          |  18%  |                                                                              |=============                                                         |  18%  |                                                                              |=============                                                         |  19%  |                                                                              |==============                                                        |  19%  |                                                                              |==============                                                        |  20%  |                                                                              |==============                                                        |  21%  |                                                                              |===============                                                       |  21%  |                                                                              |===============                                                       |  22%  |                                                                              |================                                                      |  22%  |                                                                              |================                                                      |  23%  |                                                                              |================                                                      |  24%  |                                                                              |=================                                                     |  24%  |                                                                              |=================                                                     |  25%  |                                                                              |==================                                                    |  25%  |                                                                              |==================                                                    |  26%  |                                                                              |===================                                                   |  27%  |                                                                              |====================                                                  |  28%  |                                                                              |====================                                                  |  29%  |                                                                              |=====================                                                 |  29%  |                                                                              |=====================                                                 |  30%  |                                                                              |=====================                                                 |  31%  |                                                                              |======================                                                |  31%  |                                                                              |======================                                                |  32%  |                                                                              |=======================                                               |  32%  |                                                                              |=======================                                               |  33%  |                                                                              |========================                                              |  34%  |                                                                              |========================                                              |  35%  |                                                                              |=========================                                             |  35%  |                                                                              |=========================                                             |  36%  |                                                                              |==========================                                            |  36%  |                                                                              |==========================                                            |  37%  |                                                                              |==========================                                            |  38%  |                                                                              |===========================                                           |  38%  |                                                                              |===========================                                           |  39%  |                                                                              |============================                                          |  39%  |                                                                              |============================                                          |  40%  |                                                                              |============================                                          |  41%  |                                                                              |=============================                                         |  41%  |                                                                              |=============================                                         |  42%  |                                                                              |==============================                                        |  42%  |                                                                              |==============================                                        |  43%  |                                                                              |==============================                                        |  44%  |                                                                              |===============================                                       |  44%  |                                                                              |===============================                                       |  45%  |                                                                              |================================                                      |  45%  |                                                                              |================================                                      |  46%  |                                                                              |=================================                                     |  46%  |                                                                              |=================================                                     |  47%  |                                                                              |=================================                                     |  48%  |                                                                              |==================================                                    |  48%  |                                                                              |==================================                                    |  49%  |                                                                              |===================================                                   |  49%  |                                                                              |===================================                                   |  50%  |                                                                              |===================================                                   |  51%  |                                                                              |====================================                                  |  51%  |                                                                              |====================================                                  |  52%  |                                                                              |=====================================                                 |  52%  |                                                                              |=====================================                                 |  53%  |                                                                              |=====================================                                 |  54%  |                                                                              |======================================                                |  54%  |                                                                              |======================================                                |  55%  |                                                                              |=======================================                               |  55%  |                                                                              |=======================================                               |  56%  |                                                                              |========================================                              |  56%  |                                                                              |========================================                              |  57%  |                                                                              |========================================                              |  58%  |                                                                              |=========================================                             |  58%  |                                                                              |=========================================                             |  59%  |                                                                              |==========================================                            |  59%  |                                                                              |==========================================                            |  60%  |                                                                              |==========================================                            |  61%  |                                                                              |===========================================                           |  61%  |                                                                              |===========================================                           |  62%  |                                                                              |============================================                          |  62%  |                                                                              |============================================                          |  63%  |                                                                              |============================================                          |  64%  |                                                                              |=============================================                         |  64%  |                                                                              |=============================================                         |  65%  |                                                                              |==============================================                        |  65%  |                                                                              |==============================================                        |  66%  |                                                                              |===============================================                       |  66%  |                                                                              |===============================================                       |  67%  |                                                                              |===============================================                       |  68%  |                                                                              |================================================                      |  68%  |                                                                              |================================================                      |  69%  |                                                                              |=================================================                     |  69%  |                                                                              |=================================================                     |  70%  |                                                                              |=================================================                     |  71%  |                                                                              |==================================================                    |  71%  |                                                                              |==================================================                    |  72%  |                                                                              |===================================================                   |  72%  |                                                                              |===================================================                   |  73%  |                                                                              |===================================================                   |  74%  |                                                                              |====================================================                  |  74%  |                                                                              |====================================================                  |  75%  |                                                                              |=====================================================                 |  75%  |                                                                              |=====================================================                 |  76%  |                                                                              |======================================================                |  76%  |                                                                              |======================================================                |  77%  |                                                                              |======================================================                |  78%  |                                                                              |=======================================================               |  78%  |                                                                              |=======================================================               |  79%  |                                                                              |========================================================              |  79%  |                                                                              |========================================================              |  80%  |                                                                              |========================================================              |  81%  |                                                                              |=========================================================             |  81%  |                                                                              |=========================================================             |  82%  |                                                                              |==========================================================            |  82%  |                                                                              |==========================================================            |  83%  |                                                                              |==========================================================            |  84%  |                                                                              |===========================================================           |  84%  |                                                                              |===========================================================           |  85%  |                                                                              |============================================================          |  85%  |                                                                              |============================================================          |  86%  |                                                                              |=============================================================         |  86%  |                                                                              |=============================================================         |  87%  |                                                                              |=============================================================         |  88%  |                                                                              |==============================================================        |  88%  |                                                                              |==============================================================        |  89%  |                                                                              |===============================================================       |  89%  |                                                                              |===============================================================       |  90%  |                                                                              |===============================================================       |  91%  |                                                                              |================================================================      |  91%  |                                                                              |================================================================      |  92%  |                                                                              |=================================================================     |  92%  |                                                                              |=================================================================     |  93%  |                                                                              |=================================================================     |  94%  |                                                                              |==================================================================    |  94%  |                                                                              |==================================================================    |  95%  |                                                                              |===================================================================   |  95%  |                                                                              |===================================================================   |  96%  |                                                                              |====================================================================  |  96%  |                                                                              |====================================================================  |  97%  |                                                                              |====================================================================  |  98%  |                                                                              |===================================================================== |  98%  |                                                                              |===================================================================== |  99%  |                                                                              |======================================================================|  99%  |                                                                              |======================================================================| 100%

    ## File downloaded!

    ## Start with the vectortranslate operations on the input file!

    ## 0...10...20...30...40...50...60...70...80...90...100 - done.

    ## Finished the vectortranslate operations on the input file!

    ## Reading layer `lines' from data source 
    ##   `C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB\geofabrik_south-yorkshire-latest.gpkg' 
    ##   using driver `GPKG'
    ## Simple feature collection with 91514 features and 13 fields
    ## Geometry type: LINESTRING
    ## Dimension:     XY
    ## Bounding box:  xmin: -1.857621 ymin: 53.2878 xmax: -0.8204247 ymax: 53.67845
    ## Geodetic CRS:  WGS 84

    osm_sheffield = osm_data_region[zones_sheffield, ]

    osm_sheffield %>%
      sf::st_drop_geometry() %>%
      skimr::skim()

<table>
<caption>Data summary</caption>
<tbody>
<tr class="odd">
<td style="text-align: left;">Name</td>
<td style="text-align: left;">Piped data</td>
</tr>
<tr class="even">
<td style="text-align: left;">Number of rows</td>
<td style="text-align: left;">37752</td>
</tr>
<tr class="odd">
<td style="text-align: left;">Number of columns</td>
<td style="text-align: left;">13</td>
</tr>
<tr class="even">
<td style="text-align: left;">_______________________</td>
<td style="text-align: left;"></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Column type frequency:</td>
<td style="text-align: left;"></td>
</tr>
<tr class="even">
<td style="text-align: left;">character</td>
<td style="text-align: left;">12</td>
</tr>
<tr class="odd">
<td style="text-align: left;">numeric</td>
<td style="text-align: left;">1</td>
</tr>
<tr class="even">
<td style="text-align: left;">________________________</td>
<td style="text-align: left;"></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Group variables</td>
<td style="text-align: left;">None</td>
</tr>
</tbody>
</table>

Data summary

**Variable type: character**

<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 13%" />
<col style="width: 19%" />
<col style="width: 5%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 12%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">skim_variable</th>
<th style="text-align: right;">n_missing</th>
<th style="text-align: right;">complete_rate</th>
<th style="text-align: right;">min</th>
<th style="text-align: right;">max</th>
<th style="text-align: right;">empty</th>
<th style="text-align: right;">n_unique</th>
<th style="text-align: right;">whitespace</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">osm_id</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">1.00</td>
<td style="text-align: right;">7</td>
<td style="text-align: right;">10</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">37752</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="even">
<td style="text-align: left;">name</td>
<td style="text-align: right;">23442</td>
<td style="text-align: right;">0.38</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">42</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">6527</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="odd">
<td style="text-align: left;">highway</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">1.00</td>
<td style="text-align: right;">4</td>
<td style="text-align: right;">14</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">21</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="even">
<td style="text-align: left;">waterway</td>
<td style="text-align: right;">37752</td>
<td style="text-align: right;">0.00</td>
<td style="text-align: right;">NA</td>
<td style="text-align: right;">NA</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="odd">
<td style="text-align: left;">aerialway</td>
<td style="text-align: right;">37752</td>
<td style="text-align: right;">0.00</td>
<td style="text-align: right;">NA</td>
<td style="text-align: right;">NA</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="even">
<td style="text-align: left;">barrier</td>
<td style="text-align: right;">37752</td>
<td style="text-align: right;">0.00</td>
<td style="text-align: right;">NA</td>
<td style="text-align: right;">NA</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="odd">
<td style="text-align: left;">man_made</td>
<td style="text-align: right;">37747</td>
<td style="text-align: right;">0.00</td>
<td style="text-align: right;">4</td>
<td style="text-align: right;">10</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="even">
<td style="text-align: left;">access</td>
<td style="text-align: right;">37361</td>
<td style="text-align: right;">0.01</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">11</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">9</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="odd">
<td style="text-align: left;">bicycle</td>
<td style="text-align: right;">35626</td>
<td style="text-align: right;">0.06</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">11</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">5</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="even">
<td style="text-align: left;">service</td>
<td style="text-align: right;">35503</td>
<td style="text-align: right;">0.06</td>
<td style="text-align: right;">5</td>
<td style="text-align: right;">16</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">8</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="odd">
<td style="text-align: left;">maxspeed</td>
<td style="text-align: right;">32055</td>
<td style="text-align: right;">0.15</td>
<td style="text-align: right;">1</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">13</td>
<td style="text-align: right;">0</td>
</tr>
<tr class="even">
<td style="text-align: left;">other_tags</td>
<td style="text-align: right;">20757</td>
<td style="text-align: right;">0.45</td>
<td style="text-align: right;">11</td>
<td style="text-align: right;">399</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">4326</td>
<td style="text-align: right;">0</td>
</tr>
</tbody>
</table>

**Variable type: numeric**

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 13%" />
<col style="width: 18%" />
<col style="width: 6%" />
<col style="width: 6%" />
<col style="width: 5%" />
<col style="width: 5%" />
<col style="width: 5%" />
<col style="width: 5%" />
<col style="width: 6%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">skim_variable</th>
<th style="text-align: right;">n_missing</th>
<th style="text-align: right;">complete_rate</th>
<th style="text-align: right;">mean</th>
<th style="text-align: right;">sd</th>
<th style="text-align: right;">p0</th>
<th style="text-align: right;">p25</th>
<th style="text-align: right;">p50</th>
<th style="text-align: right;">p75</th>
<th style="text-align: right;">p100</th>
<th style="text-align: left;">hist</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">z_order</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">1</td>
<td style="text-align: right;">1.67</td>
<td style="text-align: right;">3.33</td>
<td style="text-align: right;">-20</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">64</td>
<td style="text-align: left;">▁▇▁▁▁</td>
</tr>
</tbody>
</table>

    osm_cycleways = osm_sheffield %>%
      filter(highway == "cycleway")

    qtm(osm_cycleways)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-20-1.png)

## Sheffield desire lines

    orig_dest = get_od(region = "south-yorkshire")

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   `Area of residence` = col_character(),
    ##   `Area of workplace` = col_character(),
    ##   `All categories: Method of travel to work` = col_double(),
    ##   `Work mainly at or from home` = col_double(),
    ##   `Underground, metro, light rail, tram` = col_double(),
    ##   Train = col_double(),
    ##   `Bus, minibus or coach` = col_double(),
    ##   Taxi = col_double(),
    ##   `Motorcycle, scooter or moped` = col_double(),
    ##   `Driving a car or van` = col_double(),
    ##   `Passenger in a car or van` = col_double(),
    ##   Bicycle = col_double(),
    ##   `On foot` = col_double(),
    ##   `Other method of travel to work` = col_double()
    ## )

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   MSOA11CD = col_character(),
    ##   MSOA11NM = col_character(),
    ##   BNGEAST = col_double(),
    ##   BNGNORTH = col_double(),
    ##   LONGITUDE = col_double(),
    ##   LATITUDE = col_double()
    ## )

    sheffield_od = orig_dest %>%
      filter(orig_dest$geo_code1 %in% sheff_msoa$geo_code == TRUE)

    zones_attr = sheffield_od %>%
      group_by(geo_code1) %>%
      summarize_if(is.numeric, sum) %>%
      dplyr::rename(geo_code = geo_code1)

    zones_joined = left_join(sheff_msoa, zones_attr, by = "geo_code")
    sum(zones_joined$all.x)

    ## [1] 226477

    names(zones_joined)

    ##   [1] "geo_code"                "geo_name"               
    ##   [3] "lad11cd"                 "lad_name"               
    ##   [5] "all.x"                   "bicycle.x"              
    ##   [7] "foot.x"                  "car_driver.x"           
    ##   [9] "car_passenger.x"         "motorbike.x"            
    ##  [11] "train_tube"              "bus.x"                  
    ##  [13] "taxi_other"              "govtarget_slc"          
    ##  [15] "govtarget_sic"           "govtarget_slw"          
    ##  [17] "govtarget_siw"           "govtarget_sld"          
    ##  [19] "govtarget_sid"           "govtarget_slp"          
    ##  [21] "govtarget_sip"           "govtarget_slm"          
    ##  [23] "govtarget_sim"           "govtarget_slpt"         
    ##  [25] "govtarget_sipt"          "govnearmkt_slc"         
    ##  [27] "govnearmkt_sic"          "govnearmkt_slw"         
    ##  [29] "govnearmkt_siw"          "govnearmkt_sld"         
    ##  [31] "govnearmkt_sid"          "govnearmkt_slp"         
    ##  [33] "govnearmkt_sip"          "govnearmkt_slm"         
    ##  [35] "govnearmkt_sim"          "govnearmkt_slpt"        
    ##  [37] "govnearmkt_sipt"         "gendereq_slc"           
    ##  [39] "gendereq_sic"            "gendereq_slw"           
    ##  [41] "gendereq_siw"            "gendereq_sld"           
    ##  [43] "gendereq_sid"            "gendereq_slp"           
    ##  [45] "gendereq_sip"            "gendereq_slm"           
    ##  [47] "gendereq_sim"            "gendereq_slpt"          
    ##  [49] "gendereq_sipt"           "dutch_slc"              
    ##  [51] "dutch_sic"               "dutch_slw"              
    ##  [53] "dutch_siw"               "dutch_sld"              
    ##  [55] "dutch_sid"               "dutch_slp"              
    ##  [57] "dutch_sip"               "dutch_slm"              
    ##  [59] "dutch_sim"               "dutch_slpt"             
    ##  [61] "dutch_sipt"              "ebike_slc"              
    ##  [63] "ebike_sic"               "ebike_slw"              
    ##  [65] "ebike_siw"               "ebike_sld"              
    ##  [67] "ebike_sid"               "ebike_slp"              
    ##  [69] "ebike_sip"               "ebike_slm"              
    ##  [71] "ebike_sim"               "ebike_slpt"             
    ##  [73] "ebike_sipt"              "base_slcyclehours"      
    ##  [75] "govtarget_sicyclehours"  "govnearmkt_sicyclehours"
    ##  [77] "gendereq_sicyclehours"   "dutch_sicyclehours"     
    ##  [79] "ebike_sicyclehours"      "base_sldeath"           
    ##  [81] "base_slyll"              "base_slvalueyll"        
    ##  [83] "base_slsickdays"         "base_slvaluesick"       
    ##  [85] "base_slvaluecomb"        "govtarget_sideath"      
    ##  [87] "govtarget_siyll"         "govtarget_sivalueyll"   
    ##  [89] "govtarget_sisickdays"    "govtarget_sivaluesick"  
    ##  [91] "govtarget_sivaluecomb"   "govnearmkt_sideath"     
    ##  [93] "govnearmkt_siyll"        "govnearmkt_sivalueyll"  
    ##  [95] "govnearmkt_sisickdays"   "govnearmkt_sivaluesick" 
    ##  [97] "govnearmkt_sivaluecomb"  "gendereq_sideath"       
    ##  [99] "gendereq_siyll"          "gendereq_sivalueyll"    
    ## [101] "gendereq_sisickdays"     "gendereq_sivaluesick"   
    ## [103] "gendereq_sivaluecomb"    "dutch_sideath"          
    ## [105] "dutch_siyll"             "dutch_sivalueyll"       
    ## [107] "dutch_sisickdays"        "dutch_sivaluesick"      
    ## [109] "dutch_sivaluecomb"       "ebike_sideath"          
    ## [111] "ebike_siyll"             "ebike_sivalueyll"       
    ## [113] "ebike_sisickdays"        "ebike_sivaluesick"      
    ## [115] "ebike_sivaluecomb"       "base_slcarkm"           
    ## [117] "base_slco2"              "govtarget_sicarkm"      
    ## [119] "govtarget_sico2"         "govnearmkt_sicarkm"     
    ## [121] "govnearmkt_sico2"        "gendereq_sicarkm"       
    ## [123] "gendereq_sico2"          "dutch_sicarkm"          
    ## [125] "dutch_sico2"             "ebike_sicarkm"          
    ## [127] "ebike_sico2"             "perc_rf_dist_u10km"     
    ## [129] "avslope_perc_u10km"      "geometry"               
    ## [131] "all.y"                   "from_home"              
    ## [133] "light_rail"              "train"                  
    ## [135] "bus.y"                   "taxi"                   
    ## [137] "motorbike.y"             "car_driver.y"           
    ## [139] "car_passenger.y"         "bicycle.y"              
    ## [141] "foot.y"                  "other"

    zones_od = sheffield_od %>%
      group_by(geo_code1) %>%
      summarise_if(is.numeric, sum) %>%
      dplyr::select(geo_code = geo_code1, all_dest = all) %>%
      inner_join(zones_joined, ., by = "geo_code")

    qtm(zones_od, c("all.x", "all.y")) +
      tm_text(text = c("all.x", "all.y"), size = 0.5) +
      tm_layout(panel.labels = c("Origin", "Destination"))

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-26-1.png)

    sheffield_od$Active = (sheffield_od$bicycle + sheffield_od$foot) / sheffield_od$all * 100

    od_intra = filter(sheffield_od, geo_code1 == geo_code2)

    od_inter = filter(sheffield_od, geo_code1 != geo_code2)

    od_inter %>%
      dplyr::rename(geo_code = geo_code1)

    ## # A tibble: 21,734 × 19
    ##    geo_code  geo_code2   all from_home light_rail train   bus  taxi motorbike
    ##    <chr>     <chr>     <dbl>     <dbl>      <dbl> <dbl> <dbl> <dbl>     <dbl>
    ##  1 E02001611 E02000136     1         0          0     0     1     0         0
    ##  2 E02001611 E02000470     1         0          0     0     0     0         0
    ##  3 E02001611 E02000476     1         0          0     0     0     0         0
    ##  4 E02001611 E02000573     1         0          0     1     0     0         0
    ##  5 E02001611 E02000576     1         0          0     0     0     0         0
    ##  6 E02001611 E02000586     1         0          0     0     0     0         0
    ##  7 E02001611 E02000808     1         0          1     0     0     0         0
    ##  8 E02001611 E02000980     1         0          0     1     0     0         0
    ##  9 E02001611 E02000990     1         0          0     0     0     0         0
    ## 10 E02001611 E02001017     1         0          0     0     0     0         0
    ## # … with 21,724 more rows, and 10 more variables: car_driver <dbl>,
    ## #   car_passenger <dbl>, bicycle <dbl>, foot <dbl>, other <dbl>,
    ## #   geo_name1 <chr>, geo_name2 <chr>, la_1 <chr>, la_2 <chr>, Active <dbl>

    od_internew = od_inter %>%
      filter(la_2 == "Sheffield")

    desire_lines = stplanr::od2line(od_internew, zones_od)

    ## Creating centroids representing desire line start and end points.

    od_top5 = sheffield_od %>% 
        arrange(desc(all)) %>% 
        top_n(5, wt = all)

    view(od_top5)

## Investigating potential cycle lane network development

    tmap_mode("plot")

    ## tmap mode set to plotting

    desire_lines_top5 = stplanr::od2line(od_internew, zones_od)

    ## Creating centroids representing desire line start and end points.

    #tmaptools::palette_explorer()
    tm_shape(desire_lines) +
      tm_lines(palette = "plasma", breaks = c(0, 5, 10, 20, 40, 100),
        lwd = "all",
        scale = 9,
        title.lwd = "Number of trips",
        alpha = 0.2,
        col = "Active",
        title = "Active travel (%)"
      ) +
      tm_layout(legend.outside = TRUE)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-35-1.png)

    desire_lines$distance = as.numeric(st_length(desire_lines))

    desire_carshort = dplyr::filter(desire_lines, car_driver > 100 & distance < 5000)

    route_carshort = stplanr::route(desire_carshort, route_fun = stplanr::route_osrm)

    ## Most common output is sf

    plot(route_carshort)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-39-1.png)

    desire_carshort$geom_car = st_geometry(route_carshort)

    centroids = st_geometry(st_centroid(zones_od))

    plot(st_geometry(desire_carshort))
    plot(desire_carshort$geom_car, col = "red", add = TRUE)
    plot(centroids, add = TRUE)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-42-1.png)

    extra_tags = c("railway", "public_transport")
    sy = osmextract::oe_get(place = "South-Yorkshire", layer = "points", extra_tags = extra_tags)

    ## The input place was matched with: South Yorkshire

    ## The chosen file was already detected in the download directory. Skip downloading.

    ## Adding a new layer to the .gpkg file

    ## Start with the vectortranslate operations on the input file!

    ## 0...10...20...30...40...50...60...70...80...90...100 - done.

    ## Finished the vectortranslate operations on the input file!

    ## Reading layer `points' from data source 
    ##   `C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB\geofabrik_south-yorkshire-latest.gpkg' 
    ##   using driver `GPKG'
    ## Simple feature collection with 71381 features and 12 fields
    ## Geometry type: POINT
    ## Dimension:     XY
    ## Bounding box:  xmin: -2.017405 ymin: 53.16238 xmax: -0.7403473 ymax: 53.73468
    ## Geodetic CRS:  WGS 84

    view(sy)

    sy_stations = sy %>%
      filter(railway == "station")

    view(sy_stations)

    sheffield_stations = sy_stations[zones_sheffield, ]

    desire_rail = top_n(desire_lines, n = 3, wt = train)

    ncol(desire_rail)

    ## [1] 21

    desire_rail = line_via(desire_rail, sheffield_stations)

    route_rail = desire_rail %>%
      st_set_geometry("leg_orig") %>%
      route(1, route_fun = route_osrm) %>%
      select(names(route_carshort))

    ## Most common output is sf

    route_cycleway = rbind(route_rail, route_carshort)

    route_cycleway$all = c(desire_rail$all, desire_carshort$all)

    qtm(route_cycleway, lines.lwd = "all")

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-54-1.png)

    tmap_mode("plot")

    ## tmap mode set to plotting

    sy_ways = osmextract::oe_get_network(place = "South-Yorkshire", mode = "driving")

    ## The input place was matched with: South Yorkshire

    ## The chosen file was already detected in the download directory. Skip downloading.

    ## Start with the vectortranslate operations on the input file!

    ## 0...10...20...30...40...50...60...70...80...90...100 - done.

    ## Finished the vectortranslate operations on the input file!

    ## Reading layer `lines' from data source 
    ##   `C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB\geofabrik_south-yorkshire-latest.gpkg' 
    ##   using driver `GPKG'
    ## Simple feature collection with 64240 features and 11 fields
    ## Geometry type: LINESTRING
    ## Dimension:     XY
    ## Bounding box:  xmin: -1.857621 ymin: 53.2878 xmax: -0.8204247 ymax: 53.67845
    ## Geodetic CRS:  WGS 84

    ways_sheffield = sy_ways[zones_sheffield, ] %>%
      select(highway)

    view(ways_sheffield)

    plot(ways_sheffield)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-59-1.png)

    bb = st_bbox(region_ttwa)

    ways_road = opq(bbox = bb) %>% 
      add_osm_feature(key = "highway", value = "motorway|cycle|primary|secondary", value_exact = FALSE) %>% 
      osmdata_sf()

    ways_rail = opq(bbox = bb) %>% 
      add_osm_feature(key = "railway", value = "rail") %>% 
      osmdata_sf()

    res = c(ways_road, ways_rail)
    summary(res)

    ##                   Length Class  Mode     
    ## bbox                1    -none- character
    ## overpass_call       1    -none- character
    ## meta                3    -none- list     
    ## osm_points         91    sf     list     
    ## osm_lines         201    sf     list     
    ## osm_polygons      170    sf     list     
    ## osm_multilines      0    -none- NULL     
    ## osm_multipolygons   0    -none- NULL

    sheff_station_top = sheffield_stations[desire_rail, ,op = st_is_within_distance, dist = 500]

    view(sheffield_stations)

    tm_shape(zones_sheffield) +
      tm_borders(col = "darkblue") +
      tm_shape(ways_sheffield) +
      tm_lines(col = "highway", lwd = 1, palette = c("lightgreen", "grey", "pink"), alpha = 0.8) +
      tm_scale_bar() +
      tm_shape(route_cycleway) +
      tm_lines(col = "blue", lwd = "all", scale = 20, alpha = 0.6) +
      tm_shape(osm_cycleways) +
      tm_lines(col = "red", lwd = 1) +
      tm_shape(sheffield_stations) +
      tm_dots(size = 0.3, col = "red") +
      tm_shape(sheffield_stations) +
      tm_text("name", just = "left", size = 0.5) +
      tm_layout(legend.position = c("LEFT", "TOP"))

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-66-1.png)
\## Plotting cycle accident data

## All Cycle accidents in Sheffield since 2017

### Get accident location data for each year

    accidents17= get_stats19(2017, output_format ="sf")

    ## Files identified: dft-road-casualty-statistics-accident-2017.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2017.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2017.csv

    ## Reading in:

    ## C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB/dft-road-casualty-statistics-accident-2017.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character(),
    ##   date = col_character(),
    ##   time = col_time(format = ""),
    ##   local_authority_ons_district = col_character(),
    ##   local_authority_highway = col_character(),
    ##   lsoa_of_accident_location = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

    ## date and time columns present, creating formatted datetime column

    ## 19 rows removed with no coordinates

    accidents18= get_stats19(2018, output_format ="sf")

    ## Files identified: dft-road-casualty-statistics-accident-2018.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2018.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2018.csv

    ## Reading in:

    ## C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB/dft-road-casualty-statistics-accident-2018.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character(),
    ##   date = col_character(),
    ##   time = col_time(format = ""),
    ##   local_authority_ons_district = col_character(),
    ##   local_authority_highway = col_character(),
    ##   lsoa_of_accident_location = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

    ## date and time columns present, creating formatted datetime column

    ## 55 rows removed with no coordinates

    accidents19= get_stats19(2019, output_format ="sf")

    ## Files identified: dft-road-casualty-statistics-accident-2019.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2019.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2019.csv

    ## Reading in:

    ## C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB/dft-road-casualty-statistics-accident-2019.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character(),
    ##   date = col_character(),
    ##   time = col_time(format = ""),
    ##   local_authority_ons_district = col_character(),
    ##   local_authority_highway = col_character(),
    ##   lsoa_of_accident_location = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

    ## date and time columns present, creating formatted datetime column

    ## 28 rows removed with no coordinates

    accidents20= get_stats19(2020, output_format ="sf")

    ## Files identified: dft-road-casualty-statistics-accident-2020.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2020.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-accident-2020.csv

    ## Reading in:

    ## C:\Users\jspbe\AppData\Local\Temp\RtmpCiBIEB/dft-road-casualty-statistics-accident-2020.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character(),
    ##   date = col_character(),
    ##   time = col_time(format = ""),
    ##   local_authority_ons_district = col_character(),
    ##   local_authority_highway = col_character(),
    ##   lsoa_of_accident_location = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

    ## date and time columns present, creating formatted datetime column

    ## 14 rows removed with no coordinates

### Filter accident location data to Sheffield

    sheffaccs17 = accidents17 %>%
      filter(local_authority_district == "Sheffield")

    sheffaccs18 = accidents18 %>%
      filter(local_authority_district == "Sheffield")

    sheffaccs19 = accidents19 %>%
      filter(local_authority_district == "Sheffield")

    sheffaccs20 = accidents20 %>%
      filter(local_authority_district == "Sheffield")

### Join Sheffield accident data together for all accidents across Sheffield between 2017 & 2020

    sheffaccsyears = rbind(sheffaccs17 , sheffaccs18, sheffaccs19)

### Get accident vehicle data for each year

    accs_type17 = get_stats19(year = 2017, type = "Vehicle")

    ## Files identified: dft-road-casualty-statistics-vehicle-2017.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-vehicle-2017.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-vehicle-2017.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

    accs_type18 = get_stats19(year = 2018, type = "Vehicle")

    ## Files identified: dft-road-casualty-statistics-vehicle-2018.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-vehicle-2018.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-vehicle-2018.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

    accs_type19 = get_stats19(year = 2019, type = "Vehicle")

    ## Files identified: dft-road-casualty-statistics-vehicle-2019.csv

    ##    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-vehicle-2019.csv

    ## Attempt downloading from: https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-vehicle-2019.csv

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   accident_reference = col_character()
    ## )
    ## ℹ Use `spec()` for the full column specifications.

#### Unable to download data for 2020?

\###`{r} accs_type20 = get_stats19(year = 2020, type = "Vehicle")`

### Filter vehicle type to bike for each year

    accs_bike17 = accs_type17 %>%
      filter(vehicle_type == "Pedal cycle")

    accs_bike18 = accs_type18 %>%
      filter(vehicle_type == "Pedal cycle")

    accs_bike19 = accs_type19 %>%
      filter(vehicle_type == "Pedal cycle")

\##`{r} accs_bike20 = accs_type20 %>%   filter(vehicle_type == "Pedal cycle")`

### Join bike accident data together

    accs_bikeyears = rbind(accs_bike17 , accs_bike18, accs_bike19, by = "accident_year")

### Filter the location data using the vehicle type data based on matching values in accident reference column

    accs_sheffbike = sheffaccsyears %>%
      filter(sheffaccsyears$accident_reference %in% accs_bikeyears$accident_reference)

    tm_shape(accs_sheffbike["accident_year"]) +
      tm_dots()

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-84-1.png)

    tm_shape(accs_sheffbike["accident_year"]) +
      tm_dots(col = "accident_year", size = "accident_year") +
      tm_shape(zones_sheffield) +
      tm_borders(lwd = 1) +
      tm_shape(rnet_sheffield)+
      tm_lines(lwd = 1.5, palette = "plasma", breaks = c(0, 5, 10, 20, 50, 100, 200, 300, 400), col = "bicycle", alpha = 0.6) +
    tm_layout(legend.outside = TRUE)

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-85-1.png)
\## Represent which roads currently used by cyclists

    tm_shape(ways_sheffield) +
      tm_lines(col = "grey", lwd = 1) +
      tm_shape(rnet_sheffield) +
      tm_lines(col = "bicycle", lwd = "bicycle", palette = "magma")

![](sheffield_cycle_explore_files/figure-markdown_strict/unnamed-chunk-86-1.png)
\## Developing further ideas
