STAT433 - HW2
================

1)  How many flights have a missing dep\_time? What other variables are
    missing? What might these rows represent?

<!-- end list -->

``` r
flights_cancled <- flights %>%
  filter(is.na(dep_time))

flights_cancled;nrow(flights_cancled)
```

    ## # A tibble: 8,255 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1       NA           1630        NA       NA           1815
    ##  2  2013     1     1       NA           1935        NA       NA           2240
    ##  3  2013     1     1       NA           1500        NA       NA           1825
    ##  4  2013     1     1       NA            600        NA       NA            901
    ##  5  2013     1     2       NA           1540        NA       NA           1747
    ##  6  2013     1     2       NA           1620        NA       NA           1746
    ##  7  2013     1     2       NA           1355        NA       NA           1459
    ##  8  2013     1     2       NA           1420        NA       NA           1644
    ##  9  2013     1     2       NA           1321        NA       NA           1536
    ## 10  2013     1     2       NA           1545        NA       NA           1910
    ## # ... with 8,245 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

    ## [1] 8255

The number of rows (flights) that are missing dep\_time are 8,255. Since
missing variables suchs as dep\_time, dep\_delay, and arr\_time are
about departure, we can assume that the rows represent the flights whose
departure was cancled.

2)  Currently dep\_time and sched\_dep\_time are convenient to look at,
    but hard to compute with because theyâ€™re not really continuous
    numbers. Convert them to a more convenient representation of number
    of minutes since midnight.

<!-- end list -->

``` r
flights_tempt <- select(flights,
                        dep_time,
                        sched_dep_time)

mutate(flights_tempt,
       dep_time = ( 24 * 60 ) - ( (dep_time %/% 100) * 60 + (dep_time %% 100) ),
       sched_dep_time = ( 24 * 60 ) - ( (sched_dep_time %/% 100) * 60 + (sched_dep_time %% 100) )
)
```

    ## # A tibble: 336,776 x 2
    ##    dep_time sched_dep_time
    ##       <dbl>          <dbl>
    ##  1     1123           1125
    ##  2     1107           1111
    ##  3     1098           1100
    ##  4     1096           1095
    ##  5     1086           1080
    ##  6     1086           1082
    ##  7     1085           1080
    ##  8     1083           1080
    ##  9     1083           1080
    ## 10     1082           1080
    ## # ... with 336,766 more rows

``` r
# This is dummy code
minute <- as.integer(flights[1,5]) %% 100
hour <- as.integer(flights[1,5]) %/% 100

time_remained <- (24*60) - ( (hour * 60) + minute);time_remained
```

    ## [1] 1125

3)  Look at the number of cancelled flights per day. Is there a pattern?
    Is the proportion of cancelled flights related to the average delay?
    Use multiple dyplr operations, all on one line, concluding with
    \`ggplot(aes(x= ,y=)) + geom\_point()’

The number of cancelled flights per day:

``` r
flights_cancled_tempt <- flights_cancled %>%
  group_by(day) %>%
  summarize(count = n())
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
flights_cancled_tempt %>%
  ggplot(mapping = aes(x = day , y= count)) + geom_point(aes(size = count))
```

![](ReadMe_files/figure-gfm/unnamed-chunk-4-1.png)<!-- --> It seems like
the number of missing flights per day is at its peak at around 8.
Therefore, we can assume that at 8th day of a month, the number of
canceled flight is the highest.

github link: <https://github.com/rlawlstjr544/STAT433_HW2>
