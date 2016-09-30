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
