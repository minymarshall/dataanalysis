---
title       : Cross validation
subtitle    : 
author      : Jeffrey Leek, Assistant Professor of Biostatistics 
job         : Johns Hopkins Bloomberg School of Public Health
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow  # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---





## Key ideas

* Sub-sampling the training data
* Avoiding overfitting
* Making predictions generalizable

---

## Steps in building a prediction

1. Find the right data
2. Define your error rate
3. Split data into:
  * Training 
  * Testing
  * Validation (optional)
4. <redtext> On the training set pick features </redtext>
5. <redtext>On the training set pick prediction function </redtext>
6. <redtext> On the training set cross-validate </redtext>
7. If no validation - apply 1x to test set
8. If validation - apply to test set and refine
9. If validation - apply 1x to validation 

---

## Study design

<img class=center src=assets/img/studydesign.png height='60%'/>

[http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf](http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf)

---

## Overfitting


```r
set.seed(12345)
x <- rnorm(10); y <- rnorm(10); z <- rbinom(10,size=1,prob=0.5)
plot(x,y,pch=19,col=(z+3))
```

<div class="rimage center"><img src="fig/generateData.png" title="plot of chunk generateData" alt="plot of chunk generateData" class="plot" /></div>


---

## Classifier
If -0.2 < y < 0.6 call blue, otherwise green


```r
par(mfrow=c(1,2))
zhat <- (-0.2 < y) & (y < 0.6)
plot(x,y,pch=19,col=(z+3)); plot(x,y,pch=19,col=(zhat+3))
```

<div class="rimage center"><img src="fig/unnamed-chunk-1.png" title="plot of chunk unnamed-chunk-1" alt="plot of chunk unnamed-chunk-1" class="plot" /></div>


---

## New data

If -0.2 < y < 0.6 call blue, otherwise green


```r
set.seed(1233)
xnew <- rnorm(10); ynew <- rnorm(10); znew <- rbinom(10,size=1,prob=0.5)
par(mfrow=c(1,2)); zhatnew <- (-0.2 < ynew) & (ynew < 0.6)
plot(xnew,ynew,pch=19,col=(z+3)); plot(xnew,ynew,pch=19,col=(zhatnew+3))
```

<div class="rimage center"><img src="fig/unnamed-chunk-2.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" class="plot" /></div>


---

## Key idea

1. Accuracy on the training set (resubstitution accuracy) is optimistic
2. A better estimate comes from an independent set (test set accuracy)
3. But we can't use the test set when building the model or it becomes part of the training set
4. So we estimate the test set accuracy with the training set. 

---

## Cross-validation

_Approach_:

1. Use the training set

2. Split it into training/test sets 

3. Build a model on the training set

4. Evaluate on the test set

5. Repeat and average the estimated errors

_Used for_:

1. Picking variables to include in a model

2. Picking the type of prediction function to use

3. Picking the parameters in the prediction function

4. Comparing different predictors

---

## Random subsampling


<img class=center src=assets/img/random.png height='60%'/>


---

## K-fold


<img class=center src=assets/img/kfold.png height='60%'/>


---

## Leave one out

<img class=center src=assets/img/loocv.png height='60%'/>

---

## Example


```r
y1 <- y[1:5]; x1 <- x[1:5]; z1 <- z[1:5]
y2 <- y[6:10]; x2 <- x[6:10]; z2 <- z[6:10]; 
zhat2 <- (y2 < 1) & (y2 > -0.5)
par(mfrow=c(1,3))
plot(x1,y1,col=(z1+3),pch=19); plot(x2,y2,col=(z2+3),pch=19); plot(x2,y2,col=(zhat2+3),pch=19)
```

<div class="rimage center"><img src="fig/unnamed-chunk-3.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" class="plot" /></div>


---

## Notes and further resources

* The training and test sets must come from the same popluation. 
* Sampling should be designed to mimic real patterns (e.g., sampling time chunks for time series)
* Cross validation estimates have variance - it is difficult to estimate how much 
* [Cross validation in R](http://www.unt.edu/rss/class/Jon/Benchmarks/CrossValidation1_JDS_May2011.pdf)
* [cvTools](http://cran.r-project.org/web/packages/cvTools/cvTools.pdf)
* [boot](http://cran.r-project.org/web/packages/boot/index.html)

