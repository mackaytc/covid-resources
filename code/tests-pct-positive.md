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
    ## X1    1 51 11.92 8.28    9.3   11.02 6.67 1.5 45.3  43.8 1.48     3.24 1.16

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min  max range skew kurtosis se
    ## X1    1 51 0.03 0.01   0.02    0.02 0.01 0.01 0.08  0.07 1.87     3.95  0

``` r
# Table sorted in descending value of positive percentage

print(data)
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 South Car~ 45     5148714           7142            73442          9.7
    ##  2 Virginia   51     8535519          22342           129945         17.2
    ##  3 Ohio       39    11689100          23016           184316         12.5
    ##  4 Colorado   08     5758736          18801            94536         19.9
    ##  5 Kansas     20     2913314           6501            47708         13.6
    ##  6 Arizona    04     7278717          10526           119907          8.8
    ##  7 Texas      48    28995881          36609           477118          7.7
    ##  8 North Car~ 37    10488084          13868           178613          7.8
    ##  9 Oregon     41     4217737           3068            72693          4.2
    ## 10 Nevada     32     3080156           5884            53344         11  
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
