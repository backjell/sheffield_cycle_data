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

    tmap_mode("plot")

    ## tmap mode set to plotting

    cycle_trips = get_pct_rnet(region="south-yorkshire")
    zones= get_pct_zones("south-yorkshire")

    View(cycle_trips)

    cycle_trips %>%
      select(bicycle) %>% 
      plot()

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    zones %>% 
      select(lad_name) %>% 
      plot()

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    zones_sheffield = zones%>% 
      filter(lad_name == "Sheffield") %>%
      select(lad11cd) %>%
      plot(color = "deepskyblue", fill= NA, main= "Sheffield")

    ## Warning in title(...): "color" is not a graphical parameter

    ## Warning in title(...): "fill" is not a graphical parameter

    ## Warning in title(...): "color" is not a graphical parameter

    ## Warning in title(...): "fill" is not a graphical parameter

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    zones_sheffield2 = zones %>% 
      filter(lad_name == "Sheffield")

    rnet_sheffield = cycle_trips[zones_sheffield2, ] %>%
       select(bicycle) %>%
      plot(key.pos=1) 

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    rnet_sheffield = cycle_trips[zones_sheffield2, ] %>%
      select(gendereq_slc) %>%
      plot(key.pos=1)

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    rnet_sheffield = cycle_trips[zones_sheffield2, ] %>%
      select(dutch_slc) %>%
      plot(key.pos=1)

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-11-1.png)

    View(pct_regions_lookup)

    desire = get_pct_lines(region = "south-yorkshire")

    View(desire)

    plot(desire["dutch_slc"])

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    desire_sheffield = desire %>%
       filter(lad_name1 == "Sheffield")

    plot(desire_sheffield)

    ## Warning: plotting the first 9 out of 141 attributes; use max.plot = 141 to plot
    ## all

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-17-1.png)

    View(desire_sheffield)

    plot(desire_sheffield["bicycle"], key.pos=1,)

![](TestCycleData_files/figure-markdown_strict/unnamed-chunk-19-1.png)
