Load Sci-Hub Data into R
------------------------

To load one file representing one month, simply type:

    my_data <- readr::read_tsv(file = "data/scihub_data/dec2015.tab", col_names = FALSE)

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )

    ## Warning: 2 parsing failures.
    ##     row col  expected    actual
    ## 2498160  -- 6 columns 2 columns
    ## 2498161  -- 6 columns 5 columns

Now let's inspect the data:

    my_data

    ## # A tibble: 3,879,511 x 6
    ##                     X1                               X2            X3
    ##                 <time>                            <chr>         <chr>
    ## 1  2015-12-01 00:00:00        10.1080/00423110701422426 56ed2c3981074
    ## 2  2015-12-01 00:00:03 10.1111/j.1365-2222.2010.03601.x 56ed2b55bf5b4
    ## 3  2015-12-01 00:00:04        10.1007/978-1-4684-0274-2 56ed2b36d7d70
    ## 4  2015-12-01 00:00:04       10.1016/j.ejor.2003.11.032 56ed2c3981124
    ## 5  2015-12-01 00:00:05        10.1049/iet-cdt.2014.0146 56ed9ff1c5403
    ## 6  2015-12-01 00:00:06          10.1073/pnas.1512683112 56ed2c2a0ce6c
    ## 7  2015-12-01 00:00:06       10.1109/MIKON.2002.1017863 56ed2b0ebb3e1
    ## 8  2015-12-01 00:00:07    10.1016/S1368-8375(02)00010-6 56ed2b011ac68
    ## 9  2015-12-01 00:00:08 10.1034/j.1600-048X.2003.03020.x 56ed2b4eb8c73
    ## 10 2015-12-01 00:00:08    10.1016/S0167-7012(00)00219-0 56ed2bffa7e9c
    ## # ... with 3,879,501 more rows, and 3 more variables: X4 <chr>, X5 <chr>,
    ## #   X6 <chr>

Is it clean?

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    my_data %>% 
      dplyr::group_by(X4) %>% 
      dplyr::summarise(Counts = n()) %>%
      dplyr::arrange(desc(Counts))

    ## # A tibble: 179 x 2
    ##               X4 Counts
    ##            <chr>  <int>
    ## 1          China 642602
    ## 2           Iran 423213
    ## 3          India 398111
    ## 4         Russia 220480
    ## 5  United States 214228
    ## 6          Egypt 159465
    ## 7      Indonesia 102955
    ## 8         Brazil  96381
    ## 9            N/A  86057
    ## 10       Morocco  82212
    ## # ... with 169 more rows

It looks clean, great!

Well, the data is huge and you might run out of memory if you try to
load the whole dataset. So, let's subset our data and save it to your
disk. Let's define a function:

    my_helper <- function(file = NULL) {
      tt <- readr::read_tsv(file = file, col_names = FALSE) %>%
      dplyr::filter(X4 == "Iran")
      file_name <- gsub(".tab", "_iran.csv", file)
      write.csv(tt,  file_name, row.names = FALSE)
    }

Get files

    my_files <- list.files("data/scihub_data/", pattern = "tab")
    my_files <- paste0("data/scihub_data/", my_files)

And apply it

    sapply(my_files, my_helper)

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )

    ## Warning: 2 parsing failures.
    ##     row col  expected    actual
    ## 2498160  -- 6 columns 2 columns
    ## 2498161  -- 6 columns 5 columns

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )

    ## $`data/scihub_data/dec2015.tab`
    ## NULL
    ## 
    ## $`data/scihub_data/feb2016.tab`
    ## NULL
    ## 
    ## $`data/scihub_data/jan2016.tab`
    ## NULL
    ## 
    ## $`data/scihub_data/nov2015.tab`
    ## NULL
    ## 
    ## $`data/scihub_data/oct2015.tab`
    ## NULL
    ## 
    ## $`data/scihub_data/sep2015.tab`
    ## NULL

We now have the subset on our local disk. Let's load in the whole
Iranian usage events.

    my_files <- list.files("data/scihub_data/", pattern = "iran")
    my_files <- paste0("data/scihub_data/", my_files)

    sci_hub_ir <- NULL

    for (i in my_files) {
      my_data <- readr::read_csv(file = i)
      sci_hub_ir <- rbind(sci_hub_ir, my_data)
    }

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_datetime(format = ""),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character()
    ## )

So, let's inspect the data frame and save a local copy

    sci_hub_ir

    ## # A tibble: 2,631,035 x 6
    ##                     X1                             X2            X3    X4
    ## *               <time>                          <chr>         <chr> <chr>
    ## 1  2015-12-01 00:00:05      10.1049/iet-cdt.2014.0146 56ed9ff1c5403  Iran
    ## 2  2015-12-01 00:00:08   10.1016/j.apsusc.2015.05.185 56ed2c38b0cbc  Iran
    ## 3  2015-12-01 00:00:09              10.1021/jp404207x 56ed2c396a16c  Iran
    ## 4  2015-12-01 00:00:13   10.1016/0957-4158(91)90024-5 56ed2c397970f  Iran
    ## 5  2015-12-01 00:00:15  10.1016/S0167-6105(97)00220-1 56ed2c381399b  Iran
    ## 6  2015-12-01 00:00:24     10.1016/j.ejor.2007.03.011 56ed2c3731b6d  Iran
    ## 7  2015-12-01 00:00:35      10.1007/s11418-015-0931-7 56ed2b4d23a81  Iran
    ## 8  2015-12-01 00:00:44 10.1007/978-3-642-36197-5_60-1 56ed2c3974529  Iran
    ## 9  2015-12-01 00:00:46     10.1016/j.ejps.2005.04.010 56ed2c3968813  Iran
    ## 10 2015-12-01 00:00:49              10.1021/ja208256u 56ed2b4017fd3  Iran
    ## # ... with 2,631,025 more rows, and 2 more variables: X5 <chr>, X6 <chr>

    write.csv(sci_hub_ir, "data/iran.csv")

Analysis
--------

Possible questions:

What is the usage by: - month - city - subject (defined by publishers at
the journal level, might be a bit messy) - journal - publisher

To get subject, journal and publisher information, we need to use data
from Crossref based on the DOI.

Fetching data from Crossref with the `rcrossref`package
-------------------------------------------------------

-   Link: <https://github.com/ropensci/rcrossref>

Make sure to install `rcrossref` from CRAN

    install.packages("rcrossref")
    library(rcrossref)

And now get metadata for a sample of 10 publications

    my_dois <- sample(sci_hub_ir$X2, 10)

And now fetch the metadate

    my_md <- rcrossref::cr_works(my_dois)

And let's inspect it:

    my_md$data

    ## # A tibble: 10 x 32
    ##               alternative.id
    ##                        <chr>
    ## 1          S0376738814007686
    ## 2                           
    ## 3  10.1108/03090560610670016
    ## 4          10.1021/bi900756y
    ## 5  10.1108/14637150810876634
    ## 6          10.1021/ja973437o
    ## 7                           
    ## 8                        189
    ## 9                           
    ## 10                          
    ## # ... with 31 more variables: container.title <chr>, created <chr>,
    ## #   deposited <chr>, DOI <chr>, funder <list>, indexed <chr>, ISBN <chr>,
    ## #   ISSN <chr>, issued <chr>, license_date <chr>,
    ## #   license_content.version <chr>, license_delay.in.days <chr>,
    ## #   license_URL <chr>, link <list>, member <chr>, page <chr>,
    ## #   prefix <chr>, publisher <chr>, reference.count <chr>, score <chr>,
    ## #   source <chr>, subject <chr>, title <chr>, type <chr>,
    ## #   update.policy <chr>, URL <chr>, volume <chr>, assertion <list>,
    ## #   author <list>, issue <chr>, subtitle <chr>

Wow, very comprehensive!
