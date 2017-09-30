
# Project 5:  Red Wine Analysis
## by Rahul Patra
## Wednesday, June 17, 2017








# Introduction

In this project, we use R and apply exploratory data analysis techniques to explore relationships in one variable to multiple variables and to explore a selected data set for distributions, outliers, and anomalies. This data set is about red wine quality. It contains some chemical properties for each wine. At least 3 wine experts rated the quality of each wine, providing a rating between 0 (very bad) and 10 (very excellent). We want to determine which chemical properties influence the quality of red wines.
The analysis that follows is based on the following dataset:
  P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. 
  Modeling wine preferences by data mining from physicochemical properties.

# Exploratoring the dataset
## An overview of the data
The following variables are included in the Red Wine dataset:

```
##  [1] "X"                    "fixed.acidity"        "volatile.acidity"    
##  [4] "citric.acid"          "residual.sugar"       "chlorides"           
##  [7] "free.sulfur.dioxide"  "total.sulfur.dioxide" "density"             
## [10] "pH"                   "sulphates"            "alcohol"             
## [13] "quality"
```
And the variables are of type:

```
## 'data.frame':	1599 obs. of  13 variables:
##  $ X                   : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ fixed.acidity       : num  7.4 7.8 7.8 11.2 7.4 7.4 7.9 7.3 7.8 7.5 ...
##  $ volatile.acidity    : num  0.7 0.88 0.76 0.28 0.7 0.66 0.6 0.65 0.58 0.5 ...
##  $ citric.acid         : num  0 0 0.04 0.56 0 0 0.06 0 0.02 0.36 ...
##  $ residual.sugar      : num  1.9 2.6 2.3 1.9 1.9 1.8 1.6 1.2 2 6.1 ...
##  $ chlorides           : num  0.076 0.098 0.092 0.075 0.076 0.075 0.069 0.065 0.073 0.071 ...
##  $ free.sulfur.dioxide : num  11 25 15 17 11 13 15 15 9 17 ...
##  $ total.sulfur.dioxide: num  34 67 54 60 34 40 59 21 18 102 ...
##  $ density             : num  0.998 0.997 0.997 0.998 0.998 ...
##  $ pH                  : num  3.51 3.2 3.26 3.16 3.51 3.51 3.3 3.39 3.36 3.35 ...
##  $ sulphates           : num  0.56 0.68 0.65 0.58 0.56 0.56 0.46 0.47 0.57 0.8 ...
##  $ alcohol             : num  9.4 9.8 9.8 9.8 9.4 9.4 9.4 10 9.5 10.5 ...
##  $ quality             : int  5 5 5 6 5 5 5 7 7 5 ...
```

So it looks like the X variable is just an index for each observation in the dataset, and the remaining variables are quantified using numerical data.  The Quality variable is provided as integers.  

Let's look at the variability in the numerical data:

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   3.000   5.000   6.000   5.636   6.000   8.000
```

We can visualize the variability of each variable by plotting each variable using a boxplot:
<img src="useful/figure/unnamed-chunk-14-1.png" title="plot of chunk unnamed-chunk-14" alt="plot of chunk unnamed-chunk-14" style="display: block; margin: auto;" />

The upper and lower whiskers extend to the highest and lowest point whithin 1.5* the inter-quartile range.

We can also plot histograms for each variable to help us understand the distribution of each variable:
<img src="useful/figure/unnamed-chunk-15-1.png" title="plot of chunk unnamed-chunk-15" alt="plot of chunk unnamed-chunk-15" style="display: block; margin: auto;" />

Many of the variables look normally distributed.  Alcohol, sulphates and total sulfur dioxide look like they have lognormal distributions.  The outliers for the residual sugar and chlorides variables make it difficult to see the distribution.  Let's exclude the 95th percentile for residual sugar and chlorides and re-plot their histograms:
<img src="useful/figure/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" style="display: block; margin: auto;" />

The distributions for residual sugar and chlorides also look normal when we exclude the outliers.

Here are the summary statistics for residual sugar:

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.900   1.900   2.200   2.539   2.600  15.500
```

And chlorides:

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
## 0.01200 0.07000 0.07900 0.08747 0.09000 0.61100
```

What we are most interested in is the quality rating and what variables influence the quality rating.  Let's take a look at the red wine quality ratings using a histogram.

<img src="useful/figure/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" style="display: block; margin: auto;" />

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   3.000   5.000   6.000   5.636   6.000   8.000
```

The vast majority of red wines have a quality ranking of 5 and 6.

The alcohol content can be another important consideration when we are purchasing wine:
<img src="useful/figure/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.40    9.50   10.20   10.42   11.10   14.90
```

It looks like the alcohol content of the wine in the dataset follows a lognormal distribution with a high peak at the lower end of the alcohol scale.


## Bivariate relationships
We check the Pearson's correlation between all pairs of variables. We can see 
it in a graphical way using the *psych* package:

<img src="useful/figure/packages1-1.png" title="plot of chunk packages1" alt="plot of chunk packages1" style="display: block; margin: auto;" />

## A correlation table for all variables will help understand the relationships between them

```
## 'data.frame':	1599 obs. of  13 variables:
##  $ X                   : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ fixed.acidity       : num  7.4 7.8 7.8 11.2 7.4 7.4 7.9 7.3 7.8 7.5 ...
##  $ volatile.acidity    : num  0.7 0.88 0.76 0.28 0.7 0.66 0.6 0.65 0.58 0.5 ...
##  $ citric.acid         : num  0 0 0.04 0.56 0 0 0.06 0 0.02 0.36 ...
##  $ residual.sugar      : num  1.9 2.6 2.3 1.9 1.9 1.8 1.6 1.2 2 6.1 ...
##  $ chlorides           : num  0.076 0.098 0.092 0.075 0.076 0.075 0.069 0.065 0.073 0.071 ...
##  $ free.sulfur.dioxide : num  11 25 15 17 11 13 15 15 9 17 ...
##  $ total.sulfur.dioxide: num  34 67 54 60 34 40 59 21 18 102 ...
##  $ density             : num  0.998 0.997 0.997 0.998 0.998 ...
##  $ pH                  : num  3.51 3.2 3.26 3.16 3.51 3.51 3.3 3.39 3.36 3.35 ...
##  $ sulphates           : num  0.56 0.68 0.65 0.58 0.56 0.56 0.46 0.47 0.57 0.8 ...
##  $ alcohol             : num  9.4 9.8 9.8 9.8 9.4 9.4 9.4 10 9.5 10.5 ...
##  $ quality             : int  5 5 5 6 5 5 5 7 7 5 ...
```

<img src="useful/figure/packages2-1.png" title="plot of chunk packages2" alt="plot of chunk packages2" style="display: block; margin: auto;" />

## We can see some correlation in pairs like:

### alcohol vs. density-----negative correlation(-0.5)
### fixed acidity vs. density ------positive correlation(0.67)
### residual sugar vs. density------positive correlation(0.36)
### chlorides vs. sulphates------positive correlation(0.37)
### quality vs. alcohol------positive correlation(0.48)
### sulphate vs. citric acid------------positive correlation(0.31)
### density vs. pH-----negative correlation(-0.34)
### density vs. citric acid------------positive correlation(0.37)
### Total sulphur dioxide vs. free sulphur dioxide----positive correlation(0.67)
### citric acid vs. fixed acidity ----------positive correlation(0.67)
### citric acid vs. volatile acidity--------negative correlation(-0.55)

## BIVARIATE PLOTS TO COMPARE THE CORRELATIONS BETWEEN THE INPUT VARIABLES


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$alcohol and wine$density
## t = -22.838, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.5322547 -0.4583061
## sample estimates:
##        cor 
## -0.4961798
```

<img src="useful/figure/packages3-1.png" title="plot of chunk packages3" alt="plot of chunk packages3" style="display: block; margin: auto;" />

### Alcohol has negative correlation with density. This is expected as alcohol is less dense than water


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$fixed.acidity and wine$density
## t = 35.877, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.6399847 0.6943302
## sample estimates:
##       cor 
## 0.6680473
```

<img src="useful/figure/packages4-1.png" title="plot of chunk packages4" alt="plot of chunk packages4" style="display: block; margin: auto;" />

### Density has a very strong correlation with fixed.acidity.


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$residual.sugar and wine$density
## t = 15.189, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.3116908 0.3973835
## sample estimates:
##       cor 
## 0.3552834
```

<img src="useful/figure/packages5-1.png" title="plot of chunk packages5" alt="plot of chunk packages5" style="display: block; margin: auto;" />

### There exists a positive correlation between residual sugar and density


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$pH and wine$density
## t = -14.53, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.3842835 -0.2976642
## sample estimates:
##        cor 
## -0.3416993
```

<img src="useful/figure/packages6-1.png" title="plot of chunk packages6" alt="plot of chunk packages6" style="display: block; margin: auto;" />

### Negative correlation exists between density and pH.


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$citric.acid and wine$density
## t = 15.665, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.3216809 0.4066925
## sample estimates:
##       cor 
## 0.3649472
```

<img src="useful/figure/packages7-1.png" title="plot of chunk packages7" alt="plot of chunk packages7" style="display: block; margin: auto;" />


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$citric.acid and wine$sulphates
## t = 13.159, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.2678558 0.3563278
## sample estimates:
##     cor 
## 0.31277
```

<img src="useful/figure/packages8-1.png" title="plot of chunk packages8" alt="plot of chunk packages8" style="display: block; margin: auto;" />

### Positive correlation between sulpahte and citric acid.


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$citric.acid and wine$fixed.acidity
## t = 36.234, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.6438839 0.6977493
## sample estimates:
##       cor 
## 0.6717034
```

<img src="useful/figure/packages9-1.png" title="plot of chunk packages9" alt="plot of chunk packages9" style="display: block; margin: auto;" />

### citric acid and fixed acidity are strongly correlated


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$citric.acid and wine$volatile.acidity
## t = -26.489, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.5856550 -0.5174902
## sample estimates:
##        cor 
## -0.5524957
```

<img src="useful/figure/packages10-1.png" title="plot of chunk packages10" alt="plot of chunk packages10" style="display: block; margin: auto;" />

### citric acid and volatile acidity are negatively correlated


```
## 
## 	Pearson's product-moment correlation
## 
## data:  wine$chlorides and wine$sulphates
## t = 15.978, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.3282127 0.4127694
## sample estimates:
##       cor 
## 0.3712605
```

<img src="useful/figure/packages11-1.png" title="plot of chunk packages11" alt="plot of chunk packages11" style="display: block; margin: auto;" />


### Let's use boxplots to further examine the relationship between some varibles and quality.


<img src="useful/figure/packages12-1.png" title="plot of chunk packages12" alt="plot of chunk packages12" style="display: block; margin: auto;" />

### The correlation is clear here. With an increase in alcohol content, we see an increase in the concentration of better graded wines.Based on the R-squared value it seems alcohol alone only explains about 22% of the variance in quality. We're going to need to look at the other variables to generate a better model.

<img src="useful/figure/packages13-1.png" title="plot of chunk packages13" alt="plot of chunk packages13" style="display: block; margin: auto;" />

### As the boxplot showed, fixed.acidity seems to have little to no effect on quality.     

<img src="useful/figure/packages14-1.png" title="plot of chunk packages14" alt="plot of chunk packages14" style="display: block; margin: auto;" />

### Quality seems to go up when volatile.acidity is low.The higher amount of acetic acid in wine seem to produce more average and poor wines.

<img src="useful/figure/packages15-1.png" title="plot of chunk packages15" alt="plot of chunk packages15" style="display: block; margin: auto;" />

### We can see the soft correlation between these two variables. Better wines tend to have higher concentration of citric acid.

<img src="useful/figure/packages16-1.png" title="plot of chunk packages16" alt="plot of chunk packages16" style="display: block; margin: auto;" />

```
## <ScaleContinuousPosition>
##  Range:  
##  Limits:    0 --    5
```

### residual.sugar apparently seems to have little to no effect on perceived quality.Also there seems to have so many outliers.

<img src="useful/figure/packages17-1.png" title="plot of chunk packages17" alt="plot of chunk packages17" style="display: block; margin: auto;" />

### Altough weakly correlated, a lower concentration of chlorides seem to produce better wines.

<img src="useful/figure/packages18-1.png" title="plot of chunk packages18" alt="plot of chunk packages18" style="display: block; margin: auto;" />

### Free sulphur dioxide seems to be an unwanted feature of wine.

<img src="useful/figure/packages19-1.png" title="plot of chunk packages19" alt="plot of chunk packages19" style="display: block; margin: auto;" />

### As a superset of free.sulfur.dioxide there is no surprise to find a very similar distribution here.

<img src="useful/figure/packages20-1.png" title="plot of chunk packages20" alt="plot of chunk packages20" style="display: block; margin: auto;" />

### Better wines tend to have lower densities, but this is probably due to the alcohol concentration. I wonder if density still has an effect if we hold alcohol constant.

<img src="useful/figure/packages21-1.png" title="plot of chunk packages21" alt="plot of chunk packages21" style="display: block; margin: auto;" />

### Altough there is definitely a trend (better wines being more acid) there are some outliers.

<img src="useful/figure/packages22-1.png" title="plot of chunk packages22" alt="plot of chunk packages22" style="display: block; margin: auto;" />

### Interesting. Altough there are many outliers in the medium wines, better wines seem to have a higher concentration of sulphates.

## Multivariate Plots

<img src="useful/figure/packages23-1.png" title="plot of chunk packages23" alt="plot of chunk packages23" style="display: block; margin: auto;" />



<img src="useful/figure/packages24-1.png" title="plot of chunk packages24" alt="plot of chunk packages24" style="display: block; margin: auto;" />

### When we hold alcohol constant, there is no evidence that density affects quality whereas previous bivariate plot showed that density had an impact on the quality of wine.

<img src="useful/figure/packages25-1.png" title="plot of chunk packages25" alt="plot of chunk packages25" style="display: block; margin: auto;" />

### It seems that for wines with high alcohol content, having a higher concentration of sulphates produces better wines.

<img src="useful/figure/packages26-1.png" title="plot of chunk packages26" alt="plot of chunk packages26" style="display: block; margin: auto;" />


Let's try summarizing quality using a contour plot of alcohol and sulphate content:
<img src="useful/figure/packages27-1.png" title="plot of chunk packages27" alt="plot of chunk packages27" style="display: block; margin: auto;" />

This shows that higher quality red wines are generally located near the upper right of the scatter plot (darker contour lines) wheras lower quality red wines are generally located in the bottom right.

Let's make a similar plot but this time quality will be visualized using density plots along the x and y axis and color :
<img src="useful/figure/packages28-1.png" title="plot of chunk packages28" alt="plot of chunk packages28" style="display: block; margin: auto;" />

Again, this clearly illustrates that higher quality wines are found near the top right of the plot.


# Final Plots & Summary

Now let's summarize the main findings with a few refined plots.

## Plot 1 :Alcohol and Quality 
broke into two parts:
### Part 1 (Initial analysis)

```
##       bad   average excellent 
##        63      1319       217
```

<img src="useful/figure/packages29-1.png" title="plot of chunk packages29" alt="plot of chunk packages29" style="display: block; margin: auto;" />

### Part 2 (Main Plot)

<img src="useful/figure/packages30-1.png" title="plot of chunk packages30" alt="plot of chunk packages30" style="display: block; margin: auto;" />

```
## wine$rating: bad
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.40    9.60   10.00   10.22   11.00   13.10 
## -------------------------------------------------------- 
## wine$rating: average
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.40    9.50   10.00   10.25   10.90   14.90 
## -------------------------------------------------------- 
## wine$rating: excellent
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.20   10.80   11.60   11.52   12.20   14.00
```

This graph was interesting because it showed how excellent wines tended to have a higher alcohol content all else equal. By this I mean certain precursors had to exist for alcohol to be the predominant determininant for quality. 

Here are the summary statistics for alcohol content at each quality level:

```
## wine$quality: 3
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   8.400   9.725   9.925   9.955  10.580  11.000 
## -------------------------------------------------------- 
## wine$quality: 4
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.00    9.60   10.00   10.27   11.00   13.10 
## -------------------------------------------------------- 
## wine$quality: 5
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     8.5     9.4     9.7     9.9    10.2    14.9 
## -------------------------------------------------------- 
## wine$quality: 6
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.40    9.80   10.50   10.63   11.30   14.00 
## -------------------------------------------------------- 
## wine$quality: 7
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.20   10.80   11.50   11.47   12.10   14.00 
## -------------------------------------------------------- 
## wine$quality: 8
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.80   11.32   12.15   12.09   12.88   14.00
```


## Plot 2 :Alcohol & Sulphates vs. Quality

<img src="useful/figure/packages32-1.png" title="plot of chunk packages32" alt="plot of chunk packages32" style="display: block; margin: auto;" />


And here is a summary of red wine alcohol content by quality rating:

```
## wine$quality: 3
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   8.400   9.725   9.925   9.955  10.580  11.000 
## -------------------------------------------------------- 
## wine$quality: 4
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.00    9.60   10.00   10.27   11.00   13.10 
## -------------------------------------------------------- 
## wine$quality: 5
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     8.5     9.4     9.7     9.9    10.2    14.9 
## -------------------------------------------------------- 
## wine$quality: 6
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.40    9.80   10.50   10.63   11.30   14.00 
## -------------------------------------------------------- 
## wine$quality: 7
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.20   10.80   11.50   11.47   12.10   14.00 
## -------------------------------------------------------- 
## wine$quality: 8
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.80   11.32   12.15   12.09   12.88   14.00
```

By sulphate content:

```
## wine$quality: 3
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.4000  0.5125  0.5450  0.5700  0.6150  0.8600 
## -------------------------------------------------------- 
## wine$quality: 4
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.3300  0.4900  0.5600  0.5964  0.6000  2.0000 
## -------------------------------------------------------- 
## wine$quality: 5
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.370   0.530   0.580   0.621   0.660   1.980 
## -------------------------------------------------------- 
## wine$quality: 6
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.4000  0.5800  0.6400  0.6753  0.7500  1.9500 
## -------------------------------------------------------- 
## wine$quality: 7
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.3900  0.6500  0.7400  0.7413  0.8300  1.3600 
## -------------------------------------------------------- 
## wine$quality: 8
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.6300  0.6900  0.7400  0.7678  0.8200  1.1000
```

Observe that lower sulphates content typically leads to a bad wine with alcohol varying between 9% and 12%. Average wines have higher concentrations of sulphates, however wines that are rated 6 tend to have higher alcohol content and larger sulphates content. Excellent wines are mostly clustered around higher alcohol contents and higher sulphate contents. 

This graph makes it fairly clear that both sulphates and alcohol content contribute to quality. One thing I found fairly interested was that when sulphates were low, alcohol level still varied by 3%, but the wine was still rated bad. Low sulphate content appears to contribute to bad wines.


## Plot 3 : Volatile Acidity vs Quality ;

### Part 1:

<img src="useful/figure/packages35-1.png" title="plot of chunk packages35" alt="plot of chunk packages35" style="display: block; margin: auto;" />
As we can see, when volatile acidity is greater than 1, the probability of the wine being excellent is zero. When volatile acidity is either 0 or 0.3, there is roughly a 40% probability that the wine is excellent. However, when volatile acidity is between 1 and 1.2 there is an 80% chance that the wine is bad. Moreover, any wine with a volatile acidity greater than 1.4 has a 100% chance of being bad. Therefore, volatile acidity is a good predictor for bad wines.


And by volatile.acidity

```
## wine$quality: 3
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.4400  0.6475  0.8450  0.8845  1.0100  1.5800 
## -------------------------------------------------------- 
## wine$quality: 4
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.230   0.530   0.670   0.694   0.870   1.130 
## -------------------------------------------------------- 
## wine$quality: 5
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.180   0.460   0.580   0.577   0.670   1.330 
## -------------------------------------------------------- 
## wine$quality: 6
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.1600  0.3800  0.4900  0.4975  0.6000  1.0400 
## -------------------------------------------------------- 
## wine$quality: 7
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.1200  0.3000  0.3700  0.4039  0.4850  0.9150 
## -------------------------------------------------------- 
## wine$quality: 8
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.2600  0.3350  0.3700  0.4233  0.4725  0.8500
```


### Part 2: (Another observation)

<img src="useful/figure/packages37-1.png" title="plot of chunk packages37" alt="plot of chunk packages37" style="display: block; margin: auto;" />
This is perhaps the most telling graph. I subsetted the data to remove the 'average' wines, or any wine with a rating of 5 or 6. As the correlation tests show, wine quality was affected most strongly by alcohol and volaticle acidity. While the boundaries are not as clear cut or modal, it's apparent that high volatile acidity--with few exceptions--kept wine quality down. A combination of high alcohol content and low volatile acidity produced better wines.

# Reflection
My biggest challenge in this exercise was attempting to wrangle a dataset in to a tidy datset.  Realizing that this exercise would have taken more time than I had available based on the dataset that I had chosen, I reverted to using one of the recommended datasets for this project.
The above analysis considered the relationship of a number of red wine attributes with the quality rankings of different wines.  Melting the dataframe and using facet grids was really helpful for visualizing the distribution of each of the parameters with the use of boxplots and histograms.  Most of the parameters were found to be normally distributed while citirc acid, free sulfur dioxide and total sulfur dioxide and alcohol had more of a lognormal distribution.

GGally then provides a concise summary of the pairwise relationships in the dataset although the scatter plots can easily become overplotted. Formatting the axis' was also an issue because of the overlapping of the axis labels with the tick mark labels.  I finally settled on the `ggscatmat()` plot which moved the axis labels to the top and right rather than overlapping the tick labels as `ggpairs()` does.  

Using the insights from correlation coefficients provided by the paired plots, it was interesting exploring quality using density plots with a different color for each quality.  Once I had this plotted it was interesting to build up the multivariate scatter plots to visualize the relationship of different variables with quality by also varying the point size, using density plots on the x and y axis, and also using density plots.

Obvious weaknesses in this data are due to biases in the wine tasters' preferences. Since the wine tasters were experts, they tend to look for different things in wine than the average person. For example, many wine experts tend to have certain strategies on which they judge a wine (swish in mouth, dryness, etc). A normal person would likely not know about these methods and as such I'd like to see how normal people would also rate these wines. I'd be curious to see if the factors differ at all. Choosing different populations/levels of wine tasters would further strengthen similarities in the data.

In the future work, we should try to improve our modelling procedures balancing the data and using cross-validation techniques to detect overfitting. Also we could try some algorithm for parameters selection.

Other machine learning algorithms could work better for this problem. Decision trees could be useful to detect a path of rules to determine wine quality. Also classification algorithms could be used since quality is in fact an ordered categorical variable. There are more powerful methods, like random forest or Support Vector Machines (SVM); they could help us to get good predictors, but it would be more complicated to interpret the resulting models. k-Nearest Neighbours algorithm (k-NN) could work very well in this context, but it will not explain anything about the underlying model.
