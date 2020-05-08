Positive Covid-19 Test Percentages by State
================
Taylor
5/7/2020

## Loading Covid Tracking Data

State cumulative testing data called from [Covid
Tracking](https://covidtracking.com) using using `covid19us` API
wrapper.

``` r
states.current <- as_tibble(get_states_current())

head(states.current)
```

    ## # A tibble: 6 x 30
    ##   state positive positive_score negative_score negative_regula~ commercial_score
    ##   <chr>    <int>          <int>          <int>            <int>            <int>
    ## 1 AK         374              1              1                1                1
    ## 2 AL        8898              1              1                0                1
    ## 3 AR        3611              1              1                1                1
    ## 4 AZ        9945              1              1                0                1
    ## 5 CA       60614              1              1                0                1
    ## 6 CO       17830              1              1                1                1
    ## # ... with 24 more variables: grade <chr>, score <int>, notes <chr>,
    ## #   data_quality_grade <chr>, negative <int>, pending <int>,
    ## #   hospitalized_currently <int>, hospitalized_cumulative <int>,
    ## #   in_icu_currently <int>, in_icu_cumulative <int>,
    ## #   on_ventilator_currently <int>, on_ventilator_cumulative <int>,
    ## #   recovered <int>, last_update <dttm>, check_time <dttm>, death <int>,
    ## #   hospitalized <int>, total <int>, total_test_results <int>, fips <chr>,
    ## #   date_modified <dttm>, date_checked <dttm>, hash <chr>,
    ## #   request_datetime <dttm>

## Current Positive Test Percentages

Per [Covid Tracking](https://covidtracking.com/api), we have the
following variable definitions:

  - `positive`: total cumulative positive test results in a given state
  - `totalTestResults`: calculated value (positive + negative) of total
    test results

Code below calculates the cumulative positive percentage of total tests
by state and prints table sorted in descending order of positive
percentage.

``` r
# Subset data + calculate pct.positive = pos. tests / total tests

states.current <- select(states.current, state, positive, total_test_results) %>% 
  mutate(pct.positive = positive / total_test_results, 
         pct.positive = round(pct.positive, 3)) %>% 
  arrange(-pct.positive)

# Summary stats

describe(states.current$pct.positive)
```

    ##    vars  n mean   sd median trimmed  mad min max range skew kurtosis   se
    ## X1    1 56 0.13 0.15   0.09    0.11 0.07   0   1     1 4.04     20.8 0.02

``` r
# Table sorted in descending value of positive percentage

print(states.current, n = 56)
```

    ## # A tibble: 56 x 4
    ##    state positive total_test_results pct.positive
    ##    <chr>    <int>              <int>        <dbl>
    ##  1 PR        2031               2031        1    
    ##  2 NJ      133635             292658        0.457
    ##  3 NY      327649            1089916        0.301
    ##  4 CT       31784             116174        0.274
    ##  5 DC        5654              25856        0.219
    ##  6 DE        5939              27326        0.217
    ##  7 MA       73721             351632        0.21 
    ##  8 PA       52915             262788        0.201
    ##  9 MD       29374             148600        0.198
    ## 10 CO       17830              91347        0.195
    ## 11 IL       70873             379043        0.187
    ## 12 MI       45646             247062        0.185
    ## 13 IN       22503             124782        0.18 
    ## 14 NE        6771              37758        0.179
    ## 15 VA       21570             123152        0.175
    ## 16 IA       11059              66427        0.166
    ## 17 LA       30652             200767        0.153
    ## 18 GA       31439             217303        0.145
    ## 19 SD        2905              20114        0.144
    ## 20 KS        6144              44822        0.137
    ## 21 RI       10530              82318        0.128
    ## 22 OH       22131             176059        0.126
    ## 23 NV        5766              51357        0.112
    ## 24 MS        8686              80787        0.108
    ## 25 MN        9365              97421        0.096
    ## 26 NH        2740              28806        0.095
    ## 27 AZ        9945             111086        0.09 
    ## 28 MO        9341             103622        0.09 
    ## 29 SC        6936              77482        0.09 
    ## 30 WI        9215             102250        0.09 
    ## 31 FL       38828             492950        0.079
    ## 32 NC       13397             171328        0.078
    ## 33 TX       35390             455162        0.078
    ## 34 AL        8898             115173        0.077
    ## 35 KY        5934              78606        0.075
    ## 36 CA       60614             842874        0.072
    ## 37 WA       15905             224813        0.071
    ## 38 ID        2158              30890        0.07 
    ## 39 VI          66               1056        0.062
    ## 40 AR        3611              59995        0.06 
    ## 41 TN       14096             236328        0.06 
    ## 42 ME        1330              23422        0.057
    ## 43 WY         631              12034        0.052
    ## 44 NM        4291              85684        0.05 
    ## 45 OK        4330              86887        0.05 
    ## 46 VT         916              18451        0.05 
    ## 47 UT        5724             134543        0.043
    ## 48 OR        2989              70458        0.042
    ## 49 GU         151               3715        0.041
    ## 50 ND        1371              40867        0.034
    ## 51 MT         456              20247        0.023
    ## 52 WV        1287              57521        0.022
    ## 53 HI         626              35541        0.018
    ## 54 AK         374              24341        0.015
    ## 55 MP          15               1798        0.008
    ## 56 AS           0                 83        0
