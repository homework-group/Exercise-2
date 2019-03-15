Homework 2- Saratoga House Prices
================

Describing the price-modeling strategies for a local taxing authority:

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ───────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(mosaic)
```

    ## Loading required package: lattice

    ## Loading required package: ggformula

    ## 
    ## New to ggformula?  Try the tutorials: 
    ##  learnr::run_tutorial("introduction", package = "ggformula")
    ##  learnr::run_tutorial("refining", package = "ggformula")

    ## Loading required package: mosaicData

    ## Loading required package: Matrix

    ## 
    ## Attaching package: 'Matrix'

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     expand

    ## 
    ## The 'mosaic' package masks several functions from core packages in order to add 
    ## additional features.  The original behavior of these functions should not be affected by this.
    ## 
    ## Note: If you use the Matrix package, be sure to load it BEFORE loading mosaic.

    ## 
    ## Attaching package: 'mosaic'

    ## The following object is masked from 'package:Matrix':
    ## 
    ##     mean

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     count, do, tally

    ## The following object is masked from 'package:purrr':
    ## 
    ##     cross

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     stat

    ## The following objects are masked from 'package:stats':
    ## 
    ##     binom.test, cor, cor.test, cov, fivenum, IQR, median,
    ##     prop.test, quantile, sd, t.test, var

    ## The following objects are masked from 'package:base':
    ## 
    ##     max, mean, min, prod, range, sample, sum

``` r
data(SaratogaHouses)

summary(SaratogaHouses)
```

    ##      price           lotSize             age           landValue     
    ##  Min.   :  5000   Min.   : 0.0000   Min.   :  0.00   Min.   :   200  
    ##  1st Qu.:145000   1st Qu.: 0.1700   1st Qu.: 13.00   1st Qu.: 15100  
    ##  Median :189900   Median : 0.3700   Median : 19.00   Median : 25000  
    ##  Mean   :211967   Mean   : 0.5002   Mean   : 27.92   Mean   : 34557  
    ##  3rd Qu.:259000   3rd Qu.: 0.5400   3rd Qu.: 34.00   3rd Qu.: 40200  
    ##  Max.   :775000   Max.   :12.2000   Max.   :225.00   Max.   :412600  
    ##    livingArea     pctCollege       bedrooms       fireplaces    
    ##  Min.   : 616   Min.   :20.00   Min.   :1.000   Min.   :0.0000  
    ##  1st Qu.:1300   1st Qu.:52.00   1st Qu.:3.000   1st Qu.:0.0000  
    ##  Median :1634   Median :57.00   Median :3.000   Median :1.0000  
    ##  Mean   :1755   Mean   :55.57   Mean   :3.155   Mean   :0.6019  
    ##  3rd Qu.:2138   3rd Qu.:64.00   3rd Qu.:4.000   3rd Qu.:1.0000  
    ##  Max.   :5228   Max.   :82.00   Max.   :7.000   Max.   :4.0000  
    ##    bathrooms       rooms                   heating           fuel     
    ##  Min.   :0.0   Min.   : 2.000   hot air        :1121   gas     :1197  
    ##  1st Qu.:1.5   1st Qu.: 5.000   hot water/steam: 302   electric: 315  
    ##  Median :2.0   Median : 7.000   electric       : 305   oil     : 216  
    ##  Mean   :1.9   Mean   : 7.042                                         
    ##  3rd Qu.:2.5   3rd Qu.: 8.250                                         
    ##  Max.   :4.5   Max.   :12.000                                         
    ##                sewer      waterfront newConstruction centralAir
    ##  septic           : 503   Yes:  15   Yes:  81        Yes: 635  
    ##  public/commercial:1213   No :1713   No :1647        No :1093  
    ##  none             :  12                                        
    ##                                                                
    ##                                                                
    ## 

``` r
# Baseline model
lm_small = lm(price ~ bedrooms + bathrooms + lotSize, data=SaratogaHouses)

# 11 main effects
lm_medium = lm(price ~ lotSize + age + livingArea + pctCollege + bedrooms + 
                 fireplaces + bathrooms + rooms + heating + fuel + centralAir, data=SaratogaHouses)

# Sometimes it's easier to name the variables we want to leave out
# The command below yields exactly the same model.
# the dot (.) means "all variables not named"
# the minus (-) means "exclude this variable"
lm_medium2 = lm(price ~ . - sewer - waterfront - landValue - newConstruction, data=SaratogaHouses)

coef(lm_medium)
```

    ##            (Intercept)                lotSize                    age 
    ##            28627.73165             9350.45188               47.54722 
    ##             livingArea             pctCollege               bedrooms 
    ##               91.86974              296.50809           -15630.71950 
    ##             fireplaces              bathrooms                  rooms 
    ##              985.06117            22006.97108             3259.11923 
    ## heatinghot water/steam        heatingelectric           fuelelectric 
    ##            -9429.79463            -3609.98574           -12094.12195 
    ##                fueloil           centralAirNo 
    ##            -8873.13971           -17112.81908

``` r
coef(lm_medium2)
```

    ##            (Intercept)                lotSize                    age 
    ##            28627.73165             9350.45188               47.54722 
    ##             livingArea             pctCollege               bedrooms 
    ##               91.86974              296.50809           -15630.71950 
    ##             fireplaces              bathrooms                  rooms 
    ##              985.06117            22006.97108             3259.11923 
    ## heatinghot water/steam        heatingelectric           fuelelectric 
    ##            -9429.79463            -3609.98574           -12094.12195 
    ##                fueloil           centralAirNo 
    ##            -8873.13971           -17112.81908

``` r
# All interactions
# the ()^2 says "include all pairwise interactions"
lm_big = lm(price ~ (. - sewer - waterfront - landValue - newConstruction)^2, data=SaratogaHouses)


####
# Compare out-of-sample predictive performance
####

# Split into training and testing sets
n = nrow(SaratogaHouses)
n_train = round(0.8*n)  # round to nearest integer
n_test = n - n_train
train_cases = sample.int(n, n_train, replace=FALSE)
test_cases = setdiff(1:n, train_cases)
saratoga_train = SaratogaHouses[train_cases,]
saratoga_test = SaratogaHouses[test_cases,]

# Fit to the training data
lm1 = lm(price ~ lotSize + bedrooms + bathrooms, data=saratoga_train)
lm2 = lm(price ~ . - sewer - waterfront - landValue - newConstruction, data=saratoga_train)
lm3 = lm(price ~ (. - sewer - waterfront - landValue - newConstruction)^2, data=saratoga_train)

# Predictions out of sample
yhat_test1 = predict(lm1, saratoga_test)
yhat_test2 = predict(lm2, saratoga_test)
yhat_test3 = predict(lm3, saratoga_test)
```

    ## Warning in predict.lm(lm3, saratoga_test): prediction from a rank-deficient
    ## fit may be misleading

``` r
rmse = function(y, yhat) {
  sqrt( mean( (y - yhat)^2 ) )
}

# Root mean-squared prediction error
rmse(saratoga_test$price, yhat_test1)
```

    ## [1] 69033.12

``` r
rmse(saratoga_test$price, yhat_test2)
```

    ## [1] 63320.76

``` r
rmse(saratoga_test$price, yhat_test3)
```

    ## [1] 62181.32

``` r
# easy averaging over train/test splits
library(mosaic)

rmse_vals = do(100)*{
  
  # re-split into train and test cases
  n_train = round(0.8*n)  # round to nearest integer
  n_test = n - n_train
  train_cases = sample.int(n, n_train, replace=FALSE)
  test_cases = setdiff(1:n, train_cases)
  saratoga_train = SaratogaHouses[train_cases,]
  saratoga_test = SaratogaHouses[test_cases,]
  
  # fit to this training set
  lm2 = lm(price ~ . - sewer - waterfront - landValue - newConstruction, data=saratoga_train)
  
  lm_boom = lm(price ~ lotSize + age + pctCollege + 
                 fireplaces + rooms + heating + fuel + centralAir +
                 bedrooms*rooms + bathrooms*rooms + 
                 bathrooms*livingArea, data=saratoga_train)
  
  lm_biggerboom = lm(price ~ lotSize + landValue + waterfront + newConstruction + bedrooms*bathrooms + heating + fuel + pctCollege + rooms*bedrooms + rooms*bathrooms + rooms*heating + livingArea, data=saratoga_train)
  
  
  # predict on this testing set
  yhat_test2 = predict(lm2, saratoga_test)
  yhat_testboom = predict(lm_boom, saratoga_test)
  yhat_testbiggerboom = predict(lm_biggerboom, saratoga_test)
  c(rmse(saratoga_test$price, yhat_test2),
    rmse(saratoga_test$price, yhat_testboom),
    rmse(saratoga_test$price, yhat_testbiggerboom))
}

rmse_vals
```

    ##           V1       V2       V3
    ## 1   81277.16 79937.36 68135.71
    ## 2   73247.96 72238.71 65457.97
    ## 3   67508.83 67078.20 57198.85
    ## 4   72012.42 71398.53 58544.51
    ## 5   59790.79 59125.62 54150.41
    ## 6   70137.79 69818.86 61205.65
    ## 7   64269.09 64973.17 56373.21
    ## 8   65835.18 66888.55 56430.74
    ## 9   58258.50 57093.66 55863.66
    ## 10  59936.94 59107.53 52040.96
    ## 11  69534.73 69409.50 59993.41
    ## 12  64494.83 63432.42 54316.41
    ## 13  75705.46 75245.88 65140.59
    ## 14  79706.09 78916.93 64887.24
    ## 15  63847.89 62371.71 58890.77
    ## 16  64867.25 63661.21 57824.83
    ## 17  66338.24 65559.75 54852.74
    ## 18  70078.52 69576.29 61304.57
    ## 19  59697.83 59322.92 51536.83
    ## 20  67655.36 66679.54 57798.47
    ## 21  80942.77 80588.92 68092.66
    ## 22  67096.64 66903.80 61143.00
    ## 23  59333.52 58802.09 54395.28
    ## 24  71070.58 70831.78 64178.84
    ## 25  74483.20 73881.54 60821.93
    ## 26  68504.53 67405.14 57964.20
    ## 27  57802.96 57967.57 50440.86
    ## 28  68966.49 67962.38 57661.74
    ## 29  61037.48 60509.41 59457.19
    ## 30  58945.83 58109.27 51958.57
    ## 31  64729.38 63312.68 57068.99
    ## 32  77219.11 75958.03 67630.65
    ## 33  65796.09 64685.52 58349.18
    ## 34  63319.76 63337.48 58434.69
    ## 35  65449.71 65388.24 58687.98
    ## 36  68046.74 67210.88 59953.73
    ## 37  67647.09 66799.82 57060.52
    ## 38  62955.29 62523.37 51033.87
    ## 39  69871.34 69185.89 60874.67
    ## 40  62451.20 62993.22 56115.12
    ## 41  71965.64 70602.43 65212.40
    ## 42  67506.80 67866.32 60614.72
    ## 43  79992.26 79165.46 68498.16
    ## 44  75574.20 75496.68 61972.28
    ## 45  59620.62 58599.73 49401.08
    ## 46  67783.67 67601.80 60096.84
    ## 47  77055.98 77268.44 66003.14
    ## 48  69956.16 71277.28 59525.86
    ## 49  79096.73 78068.48 69797.04
    ## 50  67607.93 67354.94 59361.80
    ## 51  59063.19 59535.22 54846.28
    ## 52  67298.17 68570.93 62585.70
    ## 53  65929.73 64858.09 55844.52
    ## 54  55294.90 54971.04 52046.34
    ## 55  64021.20 63165.43 55191.14
    ## 56  73590.77 72507.15 64896.66
    ## 57  64386.31 63621.43 54280.17
    ## 58  66777.00 65533.01 59523.18
    ## 59  63984.22 62956.78 55556.65
    ## 60  61187.49 61634.84 56660.30
    ## 61  63529.13 63315.04 53619.26
    ## 62  72787.26 72916.95 62517.61
    ## 63  66087.50 66127.78 56812.74
    ## 64  67276.97 66343.42 61865.46
    ## 65  67019.15 66868.35 59954.75
    ## 66  73502.83 73212.89 62883.66
    ## 67  58017.65 57511.52 52872.39
    ## 68  65029.04 64627.42 58791.20
    ## 69  65055.58 64822.22 54850.02
    ## 70  68241.94 66997.33 59514.92
    ## 71  69603.30 69154.10 56560.57
    ## 72  69484.14 69230.48 58941.06
    ## 73  61093.46 61817.60 59209.55
    ## 74  66512.65 65712.85 56990.10
    ## 75  70856.09 71072.08 60051.84
    ## 76  70544.52 70814.59 55909.24
    ## 77  66135.25 64974.99 55508.69
    ## 78  69149.78 68022.83 57849.00
    ## 79  71165.77 70278.62 60421.29
    ## 80  66315.07 65605.62 54502.51
    ## 81  61761.59 61502.70 54107.08
    ## 82  72989.36 71728.79 60387.44
    ## 83  59783.39 60516.96 57093.11
    ## 84  67344.30 67538.03 58441.13
    ## 85  70770.20 70542.81 63462.97
    ## 86  63777.76 62844.94 58161.26
    ## 87  63798.29 63551.72 55354.23
    ## 88  72114.40 71949.66 63247.07
    ## 89  66965.39 66642.89 59628.06
    ## 90  68817.81 67951.79 58822.29
    ## 91  59194.36 59326.37 54218.49
    ## 92  61200.79 61521.56 55275.72
    ## 93  69839.22 69235.11 60319.89
    ## 94  67810.58 68033.00 60596.45
    ## 95  71781.62 71886.56 59179.19
    ## 96  71575.84 71368.50 61041.72
    ## 97  62594.68 62390.04 57571.63
    ## 98  74835.75 74267.64 63649.82
    ## 99  63940.06 63786.48 57598.81
    ## 100 62077.03 62365.61 55779.21

``` r
colMeans(rmse_vals)
```

    ##       V1       V2       V3 
    ## 67279.43 66852.27 58687.47

Here is a hand-built model (lm\_hw) that outperforms the "medium" model considered in class. The lower RMSE indicates that lm\_hw outperforms lm\_medium and the lm\_boom models that we considered in class. The interaction between bedrooms and bathrooms seems to be an especially strong driver of house prices.

``` r
# re-split into train and test cases
n_train = round(0.8*n)  # round to nearest integer
n_test = n - n_train
train_cases = sample.int(n, n_train, replace=FALSE)
test_cases = setdiff(1:n, train_cases)
saratoga_train = SaratogaHouses[train_cases,]
saratoga_test = SaratogaHouses[test_cases,]

# fit to this training set
lm2 = lm(price ~ . - sewer - waterfront - landValue - newConstruction, data=saratoga_train)

lm_hw = lm(price ~ lotSize + landValue + waterfront + newConstruction + bedrooms*bathrooms + heating + fuel + pctCollege + rooms*bathrooms + livingArea, data=saratoga_train)


# predict on this testing set
yhat_test2 = predict(lm2, saratoga_test)
yhat_testhw = predict(lm_hw, saratoga_test)
c(rmse(saratoga_test$price, yhat_test2),rmse(saratoga_test$price, yhat_testhw))
```

    ## [1] 67929.69 60499.79

Here is our KNN model using the same variables and standardized. Some are necessarily dropped because they are string variables.

    ## 
    ## Attaching package: 'foreach'

    ## The following objects are masked from 'package:purrr':
    ## 
    ##     accumulate, when

![](data_mining_files/figure-markdown_github/Rplot01.png)

    ## [1] 65917.35

    ## [1] 77

Above is a plot of RMSE versus K. K is on the x axis and RMSE is on the y axis. We see that the RMSE is minimized when k about equal to 77. The RMSE in our linear model is lower than that of our KNN model likely due to dropping the string varaibles. The linear model seems to do the best looking at out of sample (we split into train and test to see this). The KNN model, if we would like to use it, performs best when we have k=77.
