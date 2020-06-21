Positive Covid-19 Test Percentages by State
================
6/21/2020

## Loading Covid Tracking Data

State cumulative testing data called from [Covid
Tracking](https://covidtracking.com).

## Current Positive Test Percentages

Per [Covid Tracking](https://covidtracking.com/api), we have the
following variable definitions:

  - `positive`: total cumulative positive test results in a given state
  - `totalTestResults`: calculated value (positive + negative) of total
    test results

Code below calculates the cumulative positive percentage of total tests
by state (`pct.positive`) and the number of tests per capita
(`tests.per.capita`) and prints table sorted in ascending order of tests
per capita. Data is also saved in `.csv` for easier [viewing
online](https://github.com/mackaytc/covid-resources/blob/master/code/tests-pct-positive.csv).

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

    ##    vars  n mean   sd median trimmed  mad min max range skew kurtosis   se
    ## X1    1 51 7.35 3.63      7    7.34 4.74 0.9  14  13.1 0.09       -1 0.51

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min max range skew kurtosis se
    ## X1    1 51 0.08 0.03   0.08    0.08 0.03 0.04 0.2  0.16 1.45     2.51  0

``` r
# Table sorted in descending value of positive percentage

print(arrange(data, -pct.positive))
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 Massachus~ 25     6892503         106936           764937         14  
    ##  2 New Jersey 34     8882190         168834          1218873         13.9
    ##  3 Maryland   24     6045680          63956           462280         13.8
    ##  4 District ~ 11      705749           9984            77953         12.8
    ##  5 Pennsylva~ 42    12801989          81266           647727         12.5
    ##  6 Arizona    04     7278717          49798           405280         12.3
    ##  7 Connectic~ 09     3565287          45715           391655         11.7
    ##  8 Nebraska   31     1934408          17591           151854         11.6
    ##  9 Delaware   10      973764          10681            92416         11.6
    ## 10 New York   36    19453561         387272          3327793         11.6
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
