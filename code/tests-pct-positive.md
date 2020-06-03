Positive Covid-19 Test Percentages by State
================
6/3/2020

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

    ##    vars  n mean   sd median trimmed  mad min  max range skew kurtosis  se
    ## X1    1 51 8.61 4.99    6.9    8.33 4.89 0.9 19.8  18.9 0.47    -0.85 0.7

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min  max range skew kurtosis se
    ## X1    1 51 0.06 0.02   0.05    0.05 0.02 0.03 0.15  0.12 1.67     3.42  0

``` r
# Table sorted in descending value of positive percentage

print(data)
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 Idaho      16     1787065           2906            48133          6  
    ##  2 Oregon     41     4217737           4335           134209          3.2
    ##  3 Arizona    04     7278717          21250           237833          8.9
    ##  4 Colorado   08     5758736          26577           190537         13.9
    ##  5 Missouri   29     6137428          13575           205430          6.6
    ##  6 Texas      48    28995881          66568           986224          6.7
    ##  7 Hawaii     15     1415872            652            48921          1.3
    ##  8 Ohio       39    11689100          36350           409908          8.9
    ##  9 Kansas     20     2913314          10011           103312          9.7
    ## 10 Pennsylva~ 42    12801989          73510           472871         15.5
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
