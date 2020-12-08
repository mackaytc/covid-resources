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

    ##    vars  n mean  sd median trimmed  mad min  max range skew kurtosis   se
    ## X1    1 51 8.47 5.2    7.4    7.73 3.71 0.9 25.4  24.5  1.3     1.64 0.73

``` r
describe(data$tests.per.capita)
```

    ##    vars  n mean  sd median trimmed  mad  min  max range skew kurtosis   se
    ## X1    1 51 0.68 0.3   0.64    0.64 0.21 0.26 1.56   1.3 1.19     1.29 0.04

``` r
# Table sorted in descending value of positive percentage

print(arrange(data, -pct.positive))
```

    ## # A tibble: 51 x 7
    ##    state.name fips  pop.2019 positive.tests total.test.resu~ pct.positive
    ##    <chr>      <chr>    <dbl>          <dbl>            <dbl>        <dbl>
    ##  1 South Dak~ 46      884659          86500           341195         25.4
    ##  2 Idaho      16     1787065         110510           492725         22.4
    ##  3 Kansas     20     2913314         174025           861324         20.2
    ##  4 Iowa       19     3155070         214067          1100653         19.4
    ##  5 Alabama    01     4903185         272229          1651573         16.5
    ##  6 Arizona    04     7278717         365843          2388839         15.3
    ##  7 Mississip~ 28     2976149         166194          1192494         13.9
    ##  8 Pennsylva~ 42    12801989         426444          3369727         12.7
    ##  9 Utah       49     3205958         217638          1933554         11.3
    ## 10 Texas      48    28995881        1258214         11455346         11  
    ## # ... with 41 more rows, and 1 more variable: tests.per.capita <dbl>

``` r
# Save .csv version of table 

write_csv(data, "tests-pct-positive.csv")
```
