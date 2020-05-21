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

    ##    vars  n mean   sd median trimmed  mad min  max range skew kurtosis   se
    ## X1    1 51 9.92 6.26    7.9     9.4 6.08 1.1 28.3  27.2 0.77    -0.08 0.88

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean   sd median trimmed  mad  min  max range skew kurtosis se
    ## X1    1 51 0.04 0.02   0.04    0.04 0.01 0.02 0.11  0.09 1.77     3.83  0

``` r
# Table sorted in descending value of positive percentage

print(data)
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 Idaho      16     1787065           2476            38567          6.4
    ##  2 Arizona    04     7278717          14897           165435          9  
    ##  3 Colorado   08     5758736          22482           131837         17.1
    ##  4 Oregon     41     4217737           3801           102149          3.7
    ##  5 Kansas     20     2913314           8539            71203         12  
    ##  6 Ohio       39    11689100          29436           289528         10.2
    ##  7 Virginia   51     8535519          32908           212626         15.5
    ##  8 South Car~ 45     5148714           9056           135063          6.7
    ##  9 Missouri   29     6137428          11232           161984          6.9
    ## 10 North Car~ 37    10488084          20122           277603          7.2
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
