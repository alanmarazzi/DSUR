Regression is one of the most powerful statistical techniques we can
use.

First load libraries

    library(boot)
    library(car)

    ## 
    ## Attaching package: 'car'

    ## The following object is masked from 'package:boot':
    ## 
    ##     logit

    library(QuantPsyc)

    ## Loading required package: MASS

    ## 
    ## Attaching package: 'QuantPsyc'

    ## The following object is masked from 'package:base':
    ## 
    ##     norm

Load data

    album1 <- read.delim("https://studysites.uk.sagepub.com/dsur/study/DSUR%20Data%20Files/Chapter%207/Album%20Sales%202.dat",
                         header = TRUE)
    summary(album1)

    ##     adverts             sales          airplay         attract     
    ##  Min.   :   9.104   Min.   : 10.0   Min.   : 0.00   Min.   : 1.00  
    ##  1st Qu.: 215.918   1st Qu.:137.5   1st Qu.:19.75   1st Qu.: 6.00  
    ##  Median : 531.916   Median :200.0   Median :28.00   Median : 7.00  
    ##  Mean   : 614.412   Mean   :193.2   Mean   :27.50   Mean   : 6.77  
    ##  3rd Qu.: 911.226   3rd Qu.:250.0   3rd Qu.:36.00   3rd Qu.: 8.00  
    ##  Max.   :2271.860   Max.   :360.0   Max.   :63.00   Max.   :10.00

Linear Regression
=================

Now we will perform a linear regression predicting Album sales according
to advertising expenses

    albumSales_1 <- lm(sales ~ adverts, data = album1)
    summary(albumSales_1)

    ## 
    ## Call:
    ## lm(formula = sales ~ adverts, data = album1)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -152.949  -43.796   -0.393   37.040  211.866 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 1.341e+02  7.537e+00  17.799   <2e-16 ***
    ## adverts     9.612e-02  9.632e-03   9.979   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 65.99 on 198 degrees of freedom
    ## Multiple R-squared:  0.3346, Adjusted R-squared:  0.3313 
    ## F-statistic: 99.59 on 1 and 198 DF,  p-value: < 2.2e-16

The output tells us that *R*<sup>2</sup> is .3346 which means that
**advertising** accounts for about 33% of the variability in **sales**.
If we take the square root of *R*<sup>2</sup> we will get the Pearson
correlation:

    sqrt(.3346)

    ## [1] 0.5784462

The **F-value** indicates the *F*-ratio, in this case *F* = 99.59 is
significant at *p* &lt; .001 which means that there is way less than
0.1% chance that an F value so large would happen by chance. We can
conclude that our model predicts album sales significantly better than
using the mean.

Our coefficients are significant as well, so we can now use the model to
predict album sales.

Using the model
---------------

Our model is simply:

$$
album\\ sales = b\_0+b\_1advertising\\ budget = \\\\
       = 134.14 + (0.096 \* advertising\\ budget)
$$

So let's say that an executive wants to spend £100,000 on advertising a
new album. Our units are already in thousands, so we just have to put
100 in our above equation.

$$
album\\ sales = 134.14 + (0.096 \* advertising\\ budget) = \\\\
= 134.14 + (0.096 \* 100) = \\\\
= 143.74
$$

Multiple Regression
===================

Our executive is not happy about our model and wants to extend it by
accounting for other variables, he wants to see what happens to sales
when measuring the number of times an album is played on Radio 1
(*a**i**r**p**l**a**y*) the week prior to release.

In this way our model will become:

*a**l**b**u**m* *s**a**l**e**s* = *b*<sub>0</sub> + *b*<sub>1</sub>*a**d**v**e**r**t**i**s**i**n**g* *b**u**d**g**e**t* + *b*<sub>2</sub>*a**i**r**p**l**a**y*

Before launching a model to the new data it's better to understand what
happens when we use a multiple regression.

*R*<sup>2</sup> is no longer significant, we have to look at Multiple
*R*<sup>2</sup>, but the issue is that this value will always increase
when adding variables to the model. To compare models with multiple
predictors we may use **Akaike information criterion (AIC)**, which is a
measure of fit that penalizes model complexity (aka more variables).

**AIC** is defined as:

$$
AIC = n\\ln{\\frac{SSE}{n}+2k}
$$

*n* is the number of cases in the model, *l**n* is the natural
logarithm, *S**S**E* is the sum of square errors and *k* is the number
of predictors. Of course, because of the 2*k* larger AIC means a worst
fit.
