---
title: "Integrating multi-omics underlying Down syndrome with OmicsPLS"
subtitle: "Answer key"
author: "Said el Bouhaddani and Jeanine Houwing-Duistermaat"
date: '2022-08-08'
output:
  rmdformats::material:
    highlight: tango
    fig_width: 9
    fig_height: 7
    self_contained: yes
    keep_md: yes
    code_folding: show
---


<style type="text/css">
.Routp {
  background-color: "#D1F2EB";
  border: 2px solid grey;
  font-weight: bold;
}

.bluebox {
  padding: 1em 1em 1em 3.5em;
  background: white;
  border-left: 10px solid green;
}

.blueboxx {
  padding: 0.5em;
  background: white;
  border-left: 10px solid blue;
  color: black;
}

.question {
  background-image: url("https://github.com/selbouhaddani/Contents/raw/main/QuestionTiny.png");
  background-repeat: no-repeat;
  background-size: 3em 3em;
  background-position: left center;
}

</style>





<script type="text/javascript">
    function toggle_visibility(id) {
       var e = document.getElementById(id);
       if(e.style.display == 'block')
          e.style.display = 'none';
       else
          e.style.display = 'block';
    }
</script>


# Introduction and description of the data

These are exercises for the OmicsPLS short course. There are several questions throughout the text, and the corresponding R code to answer the question is given after the question. The answer key contains all output of each code block as well as brief answers to the questions. Note that some questions don't have a unique answer, but the idea should be clear. 

## Load packages and datasets

We need several packages for data handling, fitting and visualizing the results. Run this code to see which are not yet installed. All packages can be installed with `install.packages`, except disgenet2r, which has a separate install command shown below. 


```{.r .Routp}
req_pack <- c("MASS", "parallel", "tidyverse", "magrittr", 
              "plotly", "OmicsPLS", "httr", "disgenet2r")
req_pack[which(!(req_pack %in% installed.packages()[,1]))]
```



```{.r .Routp}
library(MASS)      # statistical tools, such as lda
library(parallel)  # parallel computing
library(tidyverse) # dataset & viz tools
library(magrittr)  # pipe %>% operators
library(plotly)    # interactive plots

library(OmicsPLS)  # data integration toolkit

## Also needed but not loaded
# install.packages("httr")
# devtools::install_bitbucket("ibi_group/disgenet2r")
```

The datasets are found in the `DownSyndrome.RData` file. A simple `load` statement should load them in your workspace. The `str` function can be used to get a first impression of the data objects. 


```{.r .Routp}
load("DownSyndrome.RData")  
## str gives an overview of all kinds of objects
cat("Methylation data:\n")
str(methylation)
cat("\nGlycomics data:\n")
str(glycomics)
cat("\nCpG mapping to gene:\n")
str(CpG_groups)
## Methylation data:
##  num [1:85, 1:3322] 0.0172 0.00452 0.0155 -0.01554 -0.03823 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : chr [1:85] "GSM1272122" "GSM1272123" "GSM1272124" "GSM1272125" ...
##   ..$ : chr [1:3322] "cg00002080" "cg00004533" "cg00026030" "cg00037450" ...
##  - attr(*, "scaled:center")= Named num [1:3322] 6.53e-19 3.30e-19 7.65e-21 -1.37e-19 3.19e-20 ...
##   ..- attr(*, "names")= chr [1:3322] "cg00002080" "cg00004533" "cg00026030" "cg00037450" ...
## 
## Glycomics data:
##  num [1:85, 1:10] 0.159 0.244 0.246 0.366 0.143 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : chr [1:85] "GSM1272122" "GSM1272123" "GSM1272124" "GSM1272125" ...
##   ..$ : chr [1:10] "P1" "P2" "P3" "P4" ...
##  - attr(*, "scaled:center")= Named num [1:10] 1.976 0.407 1.955 1.375 3.711 ...
##   ..- attr(*, "names")= chr [1:10] "P1" "P2" "P3" "P4" ...
## 
## CpG mapping to gene:
##  chr [1:3322] "RWDD2B" "AGPAT3;AGPAT3" "SYNJ1;SYNJ1;SYNJ1;SYNJ1" "PWP2" ...
```

## Descriptive analysis

Before any analysis can be performed, you should consider calculating some descriptives about the data. 

:::: {.bluebox .question data-latex=""}
**_Exercises._** Plot boxplots of (a subset of) the data columns. Also describe the clinical variables in terms of number of cases and controls, age and sex distributions. Are there any remarkable observations? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo0');">
**_Answers (Click here)_** 
</a>
<div id="foo0" style="display:none">
The variables appear to be symmetric, without large outliers. There are some large variance CpG variables. There are 29 DS patients, 29 mothers, and 27 siblings. You can further inspect age and sex distributions using `hist` and `table`. 
</div>
::::

\ 




```{.r .Routp}
boxplot(glycomics)
```

<img src="Figs/Descriptive plots-1.png" style="display: block; margin: auto;" />

```{.r .Routp}
boxplot(methylation[,1:100])
```

<img src="Figs/Descriptive plots-2.png" style="display: block; margin: auto;" />

```{.r .Routp}
print(ClinicalVars)
table(ClinicalVars$group)
## # A tibble: 85 × 6
##    ID         group group_ds   age sex   family
##    <chr>      <fct> <fct>    <int> <fct>  <int>
##  1 GSM1272122 DS    yes         18 M          1
##  2 GSM1272123 DS    yes         12 F          3
##  3 GSM1272124 DS    yes         13 F          7
##  4 GSM1272125 DS    yes         24 M          9
##  5 GSM1272126 DS    yes         33 M         10
##  6 GSM1272127 DS    yes         31 F         11
##  7 GSM1272128 DS    yes         19 F         13
##  8 GSM1272129 DS    yes         29 M         20
##  9 GSM1272130 DS    yes         22 F         23
## 10 GSM1272131 DS    yes         36 M         28
## # … with 75 more rows
## 
## DS SB MA 
## 29 27 29
```


# Cross-validation to choose number of OmicsPLS components

In this part, we consider a data integration approach to link the information in methylation and glycomics data. This 'joint information' is then related to Down syndrome. 

A flexible data integration approach for two heterogeneous datasets is O2PLS and is available in the OmicsPLS package. We will use OmicsPLS to select the most important genes corresponding with the methylation sites, this mapping can be found in the `CpG_groups` object where each CpG (methylation) site has one or more associated genes. The number of methylation sites is found by running `length(CpG_groups)`, and the number of genes is found with `length(unique(CpG_groups))`.


## Choosing the number of components 

To apply OmicsPLS, we first need to decide on the number of components to retain. A cross-validation is usually performed. In the cross-validation, a grid is specified as well as the number of folds. If desired, you can use multiple cores to speed up the calculations. On a Windows machine, this requires copying the data matrices to each parallel process, so keep an eye on memory usage. 

Note that there are other ways besides cross-validation, such as the scree plot ([click here for more info](https://selbouhaddani.eu/2020-10-11-Introduction-OmicsPLS-scree/)).

:::: {.bluebox .question data-latex=""}
Perform a cross-validation for the number of joint and specific components. What is the optimal number of components for each part? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo1');">
**_Answers (Click here)_** 
</a>
<div id="foo1" style="display:none">
The optimal number of components changes a bit between runs, but 2, 4 and 6 (or 4,2,6) seems fine. One can also run the commented out lines to make a scree plot, as mentioned above. 
</div>
::::

\ 


```{.r .Routp}
set.seed(83654)
crossval_o2m_adjR2(X = methylation, Y = glycomics, 
                   a = 1:5, ax = 0:10, ay = 0:9, nr_folds = 10, nr_cores = 1)
## Some combinations of # components exceed data dimensions, these combinations are not considered
## Minimum is at n = 1
## Elapsed time: 71.69 sec
## -> 2 4 6

# par(mfrow=c(1,3))
# plot(svd(crossprod(methylation,glycomics),0,0)$d^2 %>% (function(e) e/sum(e)), main='Joint Scree plot')
# plot(svd(tcrossprod(methylation),0,0)$d %>% (function(e) e/sum(e)), main="Methylation Scree plot") 
# plot(svd(crossprod(glycomics),0,0)$d %>% (function(e) e/sum(e)), main="Glycomics Scree plot")
# par(mfrow=c(1,1))
# ## -> 3 5 1

r <- 4; rx <- 2; ry <- 6
##         MSE n nx ny
## 1 0.3298761 1  5  7
## 2 0.3306665 2  4  6
## 3 0.3382926 3  4  5
## 4 0.3304293 4  2  6
## 5 0.3328180 5  1  5
```

<!---
### Choosing the number of methylation groups

After choosing the number of components, we need to specify how many methylation groups to select (a methylation group is a set of CpG sites that were grouped because the have the same target gene). Also here, a cross-validation is possible. There are many criteria to optimize in a cross-validation. We will go through some of them. 

The first one is the covariance between the joint components in the methylation and glycomics data, $\mathrm{Cov}(T,U)$ in the left out fold. This is implemented in the function `crossval_sparsity`. The advantage is that this criterion is independent of the case-control status and can be used in all kinds of study designs. On the other hand, the resulting number of groups to keep can be too small or large with respect to the number of groups that are relevant to the case-control status. 

:::: {.bluebox .question data-latex=""}
How many unique groups do we have? Use `length` and `unique` on `CpG_groups` for this.
Using the function `crossval_sparsity`, how many groups of CpG sites should we keep?
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo2');">
**_Answers (Click here)_** 
</a>
<div id="foo2" style="display:none">
There are 483 groups. Based on the CV, we should keep 200 groups in the first component, and 40 groups in the second. 
</div>
::::

\ 



```{.r .Routp}
set.seed(7545)
crossval_sparsity(methylation, glycomics, r,rx,ry, 10, 
                  groupx = CpG_groups, keepx_seq = 1:10*40, keepy_seq = 10)
## Group information provided, CV for number of groups to keep
## $Best
## x_1sd1 x_1sd2 x_1sd3 x_1sd4 y_1sd1 y_1sd2 y_1sd3 y_1sd4     x1     x2     x3 
##     40     40     40     40     10     10     10     10    400     40    400 
##     x4     y1     y2     y3     y4 
##     80     10     10     10     10 
## 
## $Covs
##             40          80         120         160         200         240
## 10 0.001890742 0.001935982 0.001884394 0.001825501 0.001760104 0.001698085
##            280         320         360         400
## 10 0.001635634 0.001546551 0.001482363 0.001424144
## 
## $SEcov
##              40           80         120          160          200          240
## 10 0.0004748762 0.0005008425 0.000495813 0.0004872649 0.0004644634 0.0004502913
##             280          320          360          400
## 10 0.0004389727 0.0004247667 0.0004117842 0.0003966472
```

To explicitly run a cross-validation that optimizes for predicting Down syndrome, we need to write a piece of code ourselves. We divide the subjects in each outcome class in ten folds, such that each fold consists of around three DS cases, three siblings and three mothers. We further define a grid of ten values for the number of groups to keep, from 1 to 483. 


```{.r .Routp}
cv_folds <- 10 # number of folds
cv_cores <- 1 # number of cores
cv_intervals <- 10 # a grid with keepx values (groups to keep)
cv_set <- c(DS=cut(1:29, cv_folds) %>% as.numeric, # define folds in each outcome class
              SB=cut(1:27, cv_folds) %>% as.numeric, 
              MA=cut(1:29, cv_folds) %>% as.numeric)
X <- methylation
Y <- glycomics
group <- ClinicalVars$group_ds
cv_grid <- round(seq(1,length(unique(CpG_groups)),length.out=cv_intervals))

cvoutp <- mclapply(mc.cores=cv_cores,cv_grid, # compute errors across the grid, over the folds
function(jj){
  jj <- as.numeric(jj)
  cat(jj, "/", length(unique(CpG_groups)), "; ",sep="")
  outp <- sapply(1:cv_folds, function(ii){
    iii <- which(ii == cv_set)
    Xtst <- X[iii,]
    Ytst <- Y[iii,]
    Xtrn <- X[-iii,]
    Ytrn <- Y[-iii,]
    fit <- try(suppressMessages(o2m(Xtrn,Ytrn,r,rx,ry,sparse=T,keepx=jj,groupx = CpG_groups)), silent = TRUE)
    if(inherits(fit, "try-error")) fit <- suppressMessages(o2m(Xtrn,Ytrn,r,rx,ry,sparse=T,keepx=jj-1,groupx = CpG_groups))
    err1 <- mse(Y,predict(fit, X))/sqrt(ssq(Y))
    err2 <- mse(X,predict(fit, Y, "Y"))/sqrt(ssq(X))
    fit_outc <- lda(x=fit$Tt,group[-iii])
    err3 <- mse(as.numeric(group[iii])-1, 
                as.numeric(predict(
                  fit_outc,(Xtst-Xtst%*%fit$W_Y%*%t(fit$P_Y))%*%fit$W.)$class
                )-1)/sqrt(ssq(as.numeric(group[iii])-1))
    c(nr = jj, Yhat = err1, Xhat = err2, outc = err3)
  })
  outp
})
names(cvoutp) <- as.character(cv_grid)
## 1/483; 55/483; 108/483; 162/483; 215/483; 269/483; 322/483; 376/483; 429/483; 483/483;
```

An interactive plot is shown with three error types. The error when predicting $Y$ with $X$, $X$ with $Y$, and the outcome with $T$. 

:::: {.bluebox .question data-latex=""}
Based on this plot, how many groups should we keep? Based on which of the three error measures? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo3');">
**_Answers (Click here)_** 
</a>
<div id="foo3" style="display:none">
Based on the MSE of predicting the outcome, we keep around 100 groups. 
</div>
::::

\ 



```{.r .Routp}
sapply(cvoutp, function(e) rowMeans(e)) %>% t %>% 
  as.data.frame %>% mutate(across(-nr, scale)) %>% 
  gather(key = "Type", value = "Error", -nr) %>% 
  ggplot(aes(x=nr, y=Error, col=Type)) + geom_line() #-> p1
## Warning: attributes are not identical across measure variables;
## they will be dropped
```

<img src="Figs/Plot CV results for number of groups-1.png" style="display: block; margin: auto;" />

```{.r .Routp}
#ggplotly(p1)
```

We redo the cross-validation for a finer grid around potential choices for the sparsity level. This should allow us to choose the definitive number of genes to retain. 


```{.r .Routp}
cv_gridbest <- round(c(25, 50, 75, 100, 150, 175, 200))
cvoutp_best <- mclapply(mc.cores = cv_cores, cv_gridbest, 
function(jj){
  jj <- as.numeric(jj)
  cat(jj,";  ")
  outp <- sapply(1:cv_folds, function(ii){
    iii <- which(ii == cv_set)
    Xtst <- X[iii,]
    Ytst <- Y[iii,]
    Xtrn <- X[-iii,]
    Ytrn <- Y[-iii,]
    fit <- try(suppressMessages(o2m(Xtrn,Ytrn,r,rx,ry,sparse=T,keepx=jj,groupx = CpG_groups)), silent = TRUE)
    if(inherits(fit, "try-error")) fit <- suppressMessages(o2m(Xtrn,Ytrn,r,rx,ry,sparse=T,keepx=jj-1,groupx = CpG_groups))
    err1 <- mse(Y,predict(fit, X))/sqrt(ssq(Y))
    err2 <- mse(X,predict(fit, Y, "Y"))/sqrt(ssq(X))
    fit_outc <- lda(x=fit$Tt,group[-iii])
    err3 <- mse(as.numeric(group[iii])-1, 
                as.numeric(predict(
                  fit_outc,(Xtst-Xtst%*%fit$W_Y%*%t(fit$P_Y))%*%fit$W.)$class
                )-1)/sqrt(ssq(as.numeric(group[iii])-1))
    c(nr = jj, Yhat = err1, Xhat = err2, outc = err3)
  })
  outp
}); 
names(cvoutp_best) <- as.character(cv_gridbest)
## 25 ;  50 ;  75 ;  100 ;  150 ;  175 ;  200 ;
```

We plot the error for each choice, using boxplots. In this way, the variation across cross-validation runs is shown. 

:::: {.bluebox .question data-latex=""}
What would you choose as definitive number of groups to keep? Why?
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo4');">
**_Answers (Click here)_** 
</a>
<div id="foo4" style="display:none">
Somewhere between 100 and 200 is the optimal number. Taken into account that the other two error measures decrease, 175 probably the best. 
</div>
::::

\ 



```{.r .Routp}
# sapply(cvoutp_best, function(e) rowMeans(e)) %>% t %>% 
#   as.data.frame %>% mutate(across(-nr, scale)) %>% 
#   gather(key = "Type", value = "Error", -nr) %>% 
#   ggplot(aes(x=nr, y=Error, col=Type)) + geom_line() -> p1
# ggplotly(p1)

lapply(cvoutp_best, function(e) t(e) %>%
         as.data.frame %>% gather(key = "Type", value = "Error",-nr)) %>%
  Reduce(f=bind_rows) %>%
  ggplot(aes(x=as.factor(nr), y=Error)) + geom_boxplot() +
  facet_grid(Type~.,scales = "free")
```

<img src="Figs/Plot again CV results-1.png" style="display: block; margin: auto;" />

--->

# Apply OmicsPLS to methylation and glycomics

## Fit O2PLS

We fit O2PLS to the methylation and glycomics data, and calculate the variance explained by the joint and specific parts. 


```{.r .Routp}
fit <- o2m(methylation, glycomics, r, rx, ry)
summary(fit)
## 
## *** Summary of the O2PLS fit *** 
## 
## -  Call: o2m(X = methylation, Y = glycomics, n = r, nx = rx, ny = ry) 
## 
## -  Modeled variation
## -- Total variation:
## in X: 377.2632 
## in Y: 73.11544 
## 
## -- Joint, Orthogonal and Noise as proportions:
## 
##            data X data Y
## Joint       0.108  0.084
## Orthogonal  0.135  0.916
## Noise       0.757  0.000
## 
## -- Predictable variation in Y-joint part by X-joint part:
## Variation in T*B_T relative to U: 0.514 
## -- Predictable variation in X-joint part by Y-joint part:
## Variation in U*B_U relative to T: 0.5 
## 
## -- Variances per component:
## 
##         Comp 1 Comp 2 Comp 3 Comp 4
## X joint 14.955  7.679 10.218  7.890
## Y joint  3.983  1.286  0.761  0.117
## 
##        Comp 1 Comp 2
## X Orth 30.302 20.611
## 
##        Comp 1 Comp 2 Comp 3 Comp 4 Comp 5 Comp 6
## Y Orth 18.318 13.758 19.502  5.324  2.926  5.448
## 
## 
## -  Coefficient in 'U = T B_T + H_U' model:
## -- Diagonal elements of B_T =
##  0.38 0.302 0.209 0.078
```

## Visualize the results: loadings

Next, we inspect the loadings. Each of the 3322 loading values in the methylation parts represents a CpG site indicated by a cg ID. For the glycomics parts, we have 10 glycan peaks/IDs.

Each label is a cg ID or glycan ID, and the axes represent the respective components. 

:::: {.bluebox .question data-latex=""}
Give an interpretation of these results. Which features have highest loadings, in which components? Which glycan and methylation features have the highest covariance according to the plot? Click "zoom" in RStudio if the labels don't fit on the screen. 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo5');">
**_Answers (Click here)_** 
</a>
<div id="foo5" style="display:none">
For the glycan data: in component 1, P1 and P7 have a high loading value. In component 2, P4, P10 and P8 are relatively high. For the methylation data component 1: cg11866463, component 2: cg02464073. 
</div>
::::

\ 



```{.r .Routp}
plot(fit,loading_name = "Yj",i=1,j=2,label = "col") + theme_bw()
```

<img src="Figs/Plot the loadings-1.png" style="display: block; margin: auto;" />

```{.r .Routp}
plot(fit,loading_name = "Xj",i=1,j=2,label = "col") + theme_bw()
```

<img src="Figs/Plot the loadings-2.png" style="display: block; margin: auto;" />


## Visualize the results: scores

Next, we investigate if these joint components are associated with Down syndrome. We first look at the scatterplot of the scores, colored by DS. 

:::: {.bluebox .question data-latex=""}
Give an interpretation. Are the joint scores able to separate Down syndrome? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo6');">
**_Answers (Click here)_** 
</a>
<div id="foo6" style="display:none">
There is no perfect separation, but in the second component there seems to be a difference in means between Down syndrome and the other two groups. 
</div>
::::

\ 



```{.r .Routp}
plot(data.frame(Tt=fit$Tt,U=fit$U), col = ClinicalVars$group, pch=20,
     main="Joint X and Y components against each other")
```

<img src="Figs/Plot the scores-1.png" style="display: block; margin: auto;" />

```{.r .Routp}
data.frame(Group = ClinicalVars$group, JPC = scores(fit, "Xjoint")) %>% 
  pivot_longer(-Group,
               names_to = "Comp", 
               values_to = "Scores") %>% 
  ggplot(aes(x=Comp, y=Scores, col=Group)) + 
  geom_boxplot() + xlab("Component") + ylab("X scores") +
  theme_bw()
```

<img src="Figs/Plot the scores-2.png" style="display: block; margin: auto;" />

We perform a logistic regression with the Down syndrome status as outcome, and the joint methylation scores as covariates. We exclude the mothers for now. 

:::: {.bluebox .question data-latex=""}
Which group category is the reference? Are there joint scores that are significantly associated with Down syndrome? Are the p-values correctly interpretable in this case? Why (not)? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo7');">
**_Answers (Click here)_** 
</a>
<div id="foo7" style="display:none">
DS is the reference (the first level when running `levels(glm_datmat$outc)`). The first joint component is significant. However, the components themselves are estimated, this extra uncertainty is not incorporated in the p-value. 
</div>
::::

\ 



```{.r .Routp}
glm_datmat <- data.frame(JPC=scores(fit, "Xjoint"), 
           outc = ClinicalVars$group) 
glm(outc ~ ., data = glm_datmat%>% filter(outc != "MA"), family = "binomial") %>% summary()

summary(lm(JPC.1 ~ outc, data = glm_datmat))
## 
## Call:
## glm(formula = outc ~ ., family = "binomial", data = glm_datmat %>% 
##     filter(outc != "MA"))
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.8549  -0.8082  -0.2482   0.8247   1.9569  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)   
## (Intercept)  -0.2565     0.3406  -0.753  0.45130   
## JPC.1         3.3245     1.1260   2.952  0.00315 **
## JPC.2        -1.2129     1.1936  -1.016  0.30954   
## JPC.3        -0.8280     1.1664  -0.710  0.47776   
## JPC.4         1.6215     1.1565   1.402  0.16090   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 77.561  on 55  degrees of freedom
## Residual deviance: 54.914  on 51  degrees of freedom
## AIC: 64.914
## 
## Number of Fisher Scoring iterations: 5
## 
## 
## Call:
## lm(formula = JPC.1 ~ outc, data = glm_datmat)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.79148 -0.26034 -0.03155  0.22301  1.11569 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.17510    0.07133  -2.455   0.0162 *  
## outcSB       0.43569    0.10273   4.241 5.81e-05 ***
## outcMA       0.10759    0.10088   1.067   0.2893    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.3841 on 82 degrees of freedom
## Multiple R-squared:  0.1909,	Adjusted R-squared:  0.1712 
## F-statistic: 9.673 on 2 and 82 DF,  p-value: 0.0001691
```


# Interpretation of the top genes

We saw that joint methylation component one seemed to be significantly associated with Down syndrome: the mean scores differed significantly between DS and SB. Of interest is the genes corresponding with the top CpG sites, are their target genes representing some biological pathway? To this end, we use String-DB to cluster the selected genes. Although there is an R package for String-DB, we are going to use [the String-DB website](https://string-db.org/). On the website, click "multiple proteins". The input there is the list of selected genes. 

Although determining a threshold to select the number of 'top' CpG sites is not straightforward, we are going to select 200 based on earlier analysis of these data. We also need to map from cg ID to gene ID. 



```{.r .Routp}
top_cg <- order(loadings(fit, subset=1)^2,decreasing = TRUE)
gene_list <- CpG_groups[top_cg[1:200]]
gene_list %<>% paste0(collapse = ";") %>% 
  str_split(";") %>% unlist %>% unique 
```


## Using string-DB

Copy-paste the selected genes in the String-DB website. 

:::: {.bluebox .question data-latex=""}
Is there any remarkable clustering visible? Go to the analysis tab, is there any significant enrichment? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo8');">
**_Answers (Click here)_** 
</a>
<div id="foo8" style="display:none">
From the [https://string-db.org/](https://string-db.org/) website, choose homo sapiens as organism and paste the gene list. There is one clear cluster with many KRTAP genes, the other genes seem less clustered. Note that KRTAP (keratine associated proteins) are all located on chromosome 21. Enrichment analysis shows the complexity of the genetic architecture of DS, it is somewhat dominated by keratin related terms. The PubMed enrichment shows multiple terms investigating Down syndrome. 
</div>
::::

\ 



```{.r .Routp}
# gene_list %>% cat(sep="\n")
```


## Using DisGeNet 

You can also use the DisGeNet R package to perform Disease-gene enrichment. If you cannot install the package, the output is given below. 

:::: {.bluebox .question data-latex=""}
Which disease clusters are most significant? What does this say about the top genes based on integrating methylation and glycomics data? 
::::

\ 

:::: {.blueboxx data-latex=""}
<a href="javascript:void()" onclick="toggle_visibility('foo9');">
**_Answers (Click here)_** 
</a>
<div id="foo9" style="display:none">
The top hit is Down syndrome. The top genes apparently contain more genes that are associated with Down syndrome than you would expect by chance. 
</div>
::::

\ 



```{.r .Routp}
### Bonus, run this code to perform DisGeNet enrichment
# disgenet_api_key <- "271e054761763b144a97872b059fd573186bdd9f"
options(timeout=4e9)
httr::timeout(4e9) # if needed to give curl more time
DGN_DE <- disgenet2r::disease_enrichment(gene_list, database = "ALL")
DGN_DE@qresult[1:10,
          c("Description", "FDR", "Ratio",  "BgRatio")]
## <request>
## Options:
## * timeout_ms: 4e+12
##                        Description          FDR Ratio   BgRatio
## 236                  Down Syndrome 4.983930e-35 38/65 766/21666
## 2736  Complete Trisomy 21 Syndrome 2.709284e-34 36/65 669/21666
## 2320 DOWN SYNDROME CRITICAL REGION 9.293346e-19 13/65  57/21666
## 1853        Chromosome 21 monosomy 1.849628e-07  5/65  13/21666
## 2468      Alzheimer disease type 1 1.452115e-05  3/65   3/21666
## 169            Cognition Disorders 8.819130e-05 12/65 607/21666
## 375           Hirschsprung Disease 8.819130e-05 10/65 384/21666
## 548             Mental Retardation 1.006005e-04 11/65 505/21666
## 207             Presenile dementia 2.703029e-03 11/65 718/21666
## 303             Fragile X Syndrome 4.152165e-03  6/65 194/21666
```

```
                       Description          FDR Ratio   BgRatio
236                  Down Syndrome 4.983930e-35 38/65 766/21666
2736  Complete Trisomy 21 Syndrome 2.709284e-34 36/65 669/21666
2320 DOWN SYNDROME CRITICAL REGION 9.293346e-19 13/65  57/21666
1853        Chromosome 21 monosomy 1.849628e-07  5/65  13/21666
2468      Alzheimer disease type 1 1.452115e-05  3/65   3/21666
169            Cognition Disorders 8.819130e-05 12/65 607/21666
375           Hirschsprung Disease 8.819130e-05 10/65 384/21666
548             Mental Retardation 1.006005e-04 11/65 505/21666
207             Presenile dementia 2.703029e-03 11/65 718/21666
303             Fragile X Syndrome 4.152165e-03  6/65 194/21666
```


BONUS: You can also combine the String-DB and DisGeNet analyses by making an interaction netwerk of the genes in a particular disease term. Below is the code to print the genes that are in the Down Syndrome disease term. This list can be copy-pasted into String-DB and analyzed.



```{.r .Routp}
DGN_DownS <- DGN_DE@qresult %>%
    filter(Description == "Down Syndrome") %>%
    pull(shared_symbol) %>% str_split(";") %>% unlist
# cat(DGN_DownS, sep="\n")
```



