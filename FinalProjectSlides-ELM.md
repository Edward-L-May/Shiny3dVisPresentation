---
title: "Developing Data Products: Final Project"
author: "Edward May"
date: "June 18, 2018"
runtime: shiny
output: slidy_presentation
mode: selfcontained
---


This presentation shows the user a 3D visualization of a linear model from the mtcars dataset using the formula: mpg~am+qsec+wt.  This model was chosen by an optimization routine encapsulated in the function:

```r
bestModel <- function(df,dv){
    require(stats)
    require(MuMIn)
    #fits a best model to a multivariable regression problem
    myformula <- paste(dv,"~.",collapse = "")
    full.model <- lm(formula=as.formula(myformula), data=df, na.action = "na.fail")
    result <- dredge(full.model)
    return(get.models(result,subset=1)[[1]])
}
```   
-Inputs: A data frame, and a dependant variable from within the data frame
    
-Outputs: An object of class 'lm' which can then be used in prediction. 

The function uses a function from the R-package 'MuMIn' called dredge() which interates over all the combinations of the independant variables and finds the combination with the best results.

---

###Lets run the code


```r
df <- mtcars
fit <- bestModel(df,"mpg")
```

```
## Fixed term is "(Intercept)"
```

```r
summary(fit)
```

```
## 
## Call:
## lm(formula = mpg ~ am + qsec + wt + 1, data = df, na.action = "na.fail")
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -3.4811 -1.5555 -0.7257  1.4110  4.6610 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   9.6178     6.9596   1.382 0.177915    
## am            2.9358     1.4109   2.081 0.046716 *  
## qsec          1.2259     0.2887   4.247 0.000216 ***
## wt           -3.9165     0.7112  -5.507 6.95e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.459 on 28 degrees of freedom
## Multiple R-squared:  0.8497,	Adjusted R-squared:  0.8336 
## F-statistic: 52.75 on 3 and 28 DF,  p-value: 1.21e-11
```

---

-Results:

We see that the final model used only "am", "qsec", and "wt" in the final model and obtained an 85% R^2 value!

-Visualization

In the next slide, we will demonstrate the shinyapp found here:
<https://edward-l-may.shinyapps.io/DDP-ELM-visualize3dmodel/>
The 3D plot shows the three variables plotted with the z-axis being MPG.  The yellow dot in the center is the predicted value based on the inputs for the variables from the sliders.

It is fun to watch the predicted values stay within the cluster of the other data values.

---



```
## Loading required package: shiny
```

```
## 
## Attaching package: 'shiny'
```

```
## The following object is masked from 'package:git2r':
## 
##     tag
```

```
## Error in appshot.shiny.appobj(structure(list(httpHandler = function (req) : appshot of Shiny app objects is not yet supported.
```
