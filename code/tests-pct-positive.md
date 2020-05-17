Positive Covid-19 Test Percentages by State
================
5/7/2020

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

    ##    vars  n  mean   sd median trimmed  mad min  max range skew kurtosis   se
    ## X1    1 51 10.56 6.61    8.3   10.02 6.08 1.2 30.5  29.3  0.8      0.1 0.93

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min max range skew kurtosis se
    ## X1    1 51 0.04 0.02   0.03    0.03 0.01 0.02 0.1  0.08 1.75      3.6  0

``` r
# Table sorted in descending value of positive percentage

print(data)
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 Maine      23     1344212           1648            23740          6.9
    ##  2 Idaho      16     1787065           2389            35688          6.7
    ##  3 Arizona    04     7278717          13631           146788          9.3
    ##  4 Kansas     20     2913314           7886            61592         12.8
    ##  5 Ohio       39    11689100          27474           246702         11.1
    ##  6 Colorado   08     5758736          21232           121840         17.4
    ##  7 South Car~ 45     5148714           8407           109616          7.7
    ##  8 Virginia   51     8535519          29683           185568         16  
    ##  9 Oregon     41     4217737           3612            92199          3.9
    ## 10 Missouri   29     6137428          10675           139340          7.7
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
