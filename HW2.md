HW2 Q3
================

Predicting when articles go viral
---------------------------------

In this question, we use regression and classification to predict when articals go viral.


</br>

### 1. Baseline model

We always predict that the artical will go viral, since the number of viral articals is larger than the number of non-viral articals.

    ## y
    ##     0     1 
    ## 18490 21154


</br> The average confusion matrix for this baseline model is

    ##            y=0     y=1
    ## yhat=0    0.00 3691.12
    ## yhat=1    0.00 4237.88

    ## [1] 0.4655215


</br> And the average overall error rate is

    ## [1] 0.4655215

By construction of the baseline model, true positive and false positive rate are both 0.


 </br>

 </br>

### 2. Regression & threshold


 </br>

#### 1) Linear regression


 </br>
Selecting the variables and applying linear regression with linear effect only, we get overall error rate around 46.0%, which is nearly the same as the baseline model. Selecting the variables and applying regression with quadratic effect, we get overall error rate around 43%, which is slightly better than the baseline model.


</br>

#### 2) KNN


 </br>

We first try a grid of k from 1 to 101, calculate their average error rate in 5 trials, and select an optimal k. We select k=61 in this case.


</br>

![](HW2_Q3_files/figure-markdown_github/unnamed-chunk-10-1.png)


 </br>
Then we get the confusion matrix, overall error rate, TPR and FPR by averaging across 100 rounds. The average overall error rate is around 37.5%, which is better than linear regression model.


</br>

Confusion matrix:

    ##           y=0    y=1
    ## yhat=0 2169.4 1545.9
    ## yhat=1 1445.7 2768.0


</br>

Overall error rate =

    ## [1] 0.3772985


</br> TPR =

    ## [1] 0.6569049


</br> FPR =

    ## [1] 0.4160902


</br>

</br>

### 3. Threshold & classification


</br>

We first categorize the articals into viral and non-viral using 1400 shares as threshold. Then we use logit regression to do the classification. We used similar variable selection as linear regression model. The confusion matrix, overall error rate, TPR and FPR averaging across 100 rounds are as below:


</br>


</br>


</br>

Confusion matrix:

    ##            y=0     y=1
    ## yhat=0 2048.54 1652.16
    ## yhat=1 1236.40 2991.90


</br>

Overall error rate =

    ## [1] 0.3643032


</br> TPR =

    ## [1] 0.7075893


</br> FPR =

    ## [1] 0.4464453


</br> The average overall error rate is around 36%, which is better than regression & threshold method. The reason might be that by first classifying the y using threshold, the model are less prone to outliers with extreme number of shares.
