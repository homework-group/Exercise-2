Question 2
================

Part 1: Which radiologist is much more clinically conservative?

When radiologist gave recall the patient after seeing the mammograms when the patient doesn't need that, we think the radiologist is more clinically conservative. Fisrt step, in order to hold patient risk factors eauql,we use the whole data set to do the logistic regrssion, to set the model which can be uesd to judge the probability that the patient need to recall after her mammogram seen by the radiologist. Second step, we predict the result that whether a patient need to "recall" Third step, we compare the radiologist's judge and model's judge, to determine whether the radiologist is clinically conservative.

    ## , , yhat = 0
    ## 
    ##                
    ## y                 0   1
    ##   radiologist13 165  25
    ##   radiologist34 177  14
    ##   radiologist66 157  33
    ##   radiologist89 157  33
    ##   radiologist95 168  22
    ## 
    ## , , yhat = 1
    ## 
    ##                
    ## y                 0   1
    ##   radiologist13   4   4
    ##   radiologist34   3   3
    ##   radiologist66   4   4
    ##   radiologist89   2   5
    ##   radiologist95   2   5

Last, we calculate which radiologist's probabilty of giving 'recall' to a patient that doesn't need 'recall' is the highest.

``` r
25/(165+25)
```

    ## [1] 0.1315789

``` r
14/(177+14)
```

    ## [1] 0.07329843

``` r
33/(157+33)
```

    ## [1] 0.1736842

``` r
22/(168+22)
```

    ## [1] 0.1157895

Thus, radiologist66 and radiologist89 is more clinically conservative compared to other radiologists.

Part 2 In order to show which model is much suitable, we calculate the RMSE and log likelihood in this question. And split the dataset into train set and test set.

    rmse = function(y, yhat) {
      sqrt( mean( (y - yhat)^2 ) )
    }

    rmse_vals = do(100)*{
      n = nrow(brca)
      n_train = round(0.8*n)  
      n_test = n - n_train
      
      train_cases = sample.int(n, n_train, replace=FALSE)
      test_cases = setdiff(1:n, train_cases)
      train_cases
      test_cases
      
      brca_train = brca[train_cases,]
      brca_test = brca[test_cases,]

      logit2_fit = glm(cancer ~ recall,data = brca_train)
      logit3_fit = glm(cancer ~ recall + history, data=brca_train)
      logit4_fit = glm(cancer ~ recall + history + recall*history, data=brca_train)
      logit5_fit = glm(cancer ~., data = brca_train)

      yhat_test1 = predict(logit2_fit, brca_test)
      yhat_test2 = predict(logit3_fit, brca_test)
      yhat_test3 = predict(logit4_fit, brca_test)
      yhat_test4 = predict(logit5_fit, brca_test)
      
      c(rmse(brca_test$cancer, yhat_test1),
        rmse(brca_test$cancer, yhat_test2),
        rmse(brca_test$cancer, yhat_test3),
        rmse(brca_test$cancer, yhat_test4))
      
     }
    rmse_vals

    colMeans(rmse_vals)

According to the RMSE, we found that the model only link the recall and cancer is the most suitable to predict cancer status.

    rss = function(y, yhat) {
      sum( (y - yhat)^2 ) 
    }
    rss_vals = do(100)*{
     
      n = nrow(brca)
      n_train = round(0.8*n)  # round to nearest integer
      n_test = n - n_train
      
      train_cases = sample.int(n, n_train, replace=FALSE)
      test_cases = setdiff(1:n, train_cases)
      train_cases
      test_cases
      
      brca_train = brca[train_cases,]
      brca_test = brca[test_cases,]
      #set the fit model using the train data set  
      logit2_fit = glm(cancer ~ recall,data = brca_train)
      logit3_fit = glm(cancer ~ recall + history, data=brca_train)
      logit4_fit = glm(cancer ~ recall + history + recall*history, data=brca_train)
      logit5_fit = glm(cancer ~., data = brca_train)
      
      yhat_test1 = predict(logit2_fit, brca_test)
      yhat_test2 = predict(logit3_fit, brca_test)
      yhat_test3 = predict(logit4_fit, brca_test)
      yhat_test4 = predict(logit5_fit, brca_test)
      
      c(rss(brca_test$cancer, yhat_test1),
        rss(brca_test$cancer, yhat_test2),
        rss(brca_test$cancer, yhat_test3),
        rss(brca_test$cancer, yhat_test4))
      
      
    }

    rss_vals*-1/2
    colMeans(rss_vals*-1/2)

According to the RSS, we also found that the model only link the recall and cancer is the most suitable to predict cancer status.

Since the radiologist's opinion to 'recall' or not, is depended on the cancer status, thus if we include more covariates, there will be multicollinearity in the model and cause the result deviance bigger.
