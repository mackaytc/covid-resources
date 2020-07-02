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

    ##    vars  n mean   sd median trimmed  mad min  max range  skew kurtosis   se
    ## X1    1 51 7.22 3.28    6.9    7.26 3.85 0.9 15.3  14.4 -0.04    -0.53 0.46

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min  max range skew kurtosis   se
    ## X1    1 51  0.1 0.04   0.09     0.1 0.03 0.05 0.23  0.18  1.3     1.73 0.01

``` r
# Table sorted in descending value of positive percentage

print(arrange(data, -pct.positive))
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 Arizona    04     7278717          84092           549596         15.3
    ##  2 Massachus~ 25     6892503         109143           858435         12.7
    ##  3 Maryland   24     6045680          67918           542604         12.5
    ##  4 New Jersey 34     8882190         171928          1442937         11.9
    ##  5 Pennsylva~ 42    12801989          87242           776804         11.2
    ##  6 Nebraska   31     1934408          19177           180608         10.6
    ##  7 District ~ 11      705749          10365           100035         10.4
    ##  8 Delaware   10      973764          11510           111384         10.3
    ##  9 Colorado   08     5758736          32715           327503         10  
    ## 10 Georgia    13    10617423          84237           855038          9.9
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
