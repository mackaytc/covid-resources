Positive Covid-19 Test Percentages by State
================

Loading Covid Tracking Data
---------------------------

State cumulative testing data called from [Covid Tracking](https://covidtracking.com).

Current Positive Test Percentages
---------------------------------

Per [Covid Tracking](https://covidtracking.com/api), we have the following variable definitions:

-   `positive`: total cumulative positive test results in a given state
-   `totalTestResults`: calculated value (positive + negative) of total test results

Code below calculates the cumulative positive percentage of total tests by state (`pct.positive`) and the number of tests per capita (`tests.per.capita`) and prints table sorted in ascending order of tests per capita. Data is also saved in `.csv` for easier [viewing online](https://github.com/mackaytc/covid-resources/blob/master/code/tests-pct-positive.csv).

``` r
# Subset data + calculate pct.positive = pos. tests / total tests

data <- data %>% mutate(pct.positive = (positive.tests / total.test.results)*100, 
                        pct.positive = round(pct.positive, 1), 
                        tests.per.capita = total.test.results / pop.2019, 
                        tests.per.capita = round(tests.per.capita, 4)) %>% 
  arrange(tests.per.capita)

# Summary stats for both new measures

describe(data$pct.positive)
```

    ##    vars  n mean   sd median trimmed  mad min  max range skew kurtosis   se
    ## X1    1 51 7.38 3.31    7.9    7.45 3.26   1 17.8  16.8 0.17     0.39 0.46

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min  max range skew kurtosis   se
    ## X1    1 51 0.13 0.04   0.12    0.12 0.04 0.07 0.25  0.18 0.93     0.33 0.01

``` r
# Table sorted in descending value of positive percentage

print(arrange(data, -pct.positive))
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 Arizona    04     7278717         131354           735962         17.8
    ##  2 South Car~ 45     5148714          62245           525983         11.8
    ##  3 Massachus~ 25     6892503         112347           988713         11.4
    ##  4 Georgia    13    10617423         127834          1154983         11.1
    ##  5 Maryland   24     6045680          75016           680088         11  
    ##  6 Florida    12    21477737         301810          2735953         11  
    ##  7 Alabama    01     4903185          59067           541049         10.9
    ##  8 Mississip~ 28     2976149          38567           356857         10.8
    ##  9 Texas      48    28995881         282365          2642199         10.7
    ## 10 Pennsylva~ 42    12801989          97665           968649         10.1
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
