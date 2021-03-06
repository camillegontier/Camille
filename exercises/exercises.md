Exercises
------------------------------



Data sets used in the following exercises can be found on [Github](https://github.com/cogmaster-stats/r-cogstats/tree/master/data).

### R language and descriptive statistics

**1.** Consider the following series of data collected on 8 subjects:

    5.4 6.1 6.2 NA 6.2 5.6 19.0 6.3
      
Store these values in a variable named `x`. It turns out that the 7th observation is inexact and should be considered as a missing value. It is also suggested to replace the two missing values (observations 4 and 7) by the mean computed on the whole sample.
   
**2.** Create a treatment facteur, `tx`, of size 60, with balanced levels (30 `std` and 30 `new`) organized as follows:

    std std std new new new ... std std std new new new
      
Replace label `std` with `old`. Then, update this vector so that the sequence of labels starts with `new`, instead of `old` (i.e., swap the two labels of the factor). Finally, intermix the two labels randomly.
   
**3.** Load the `birthwt` data set from the `MASS` package (`data(birthwt, package="MASS")`), and compute the following quantities:

   1. The relative frequency of history of hypertension (`ht`).  
   2. The average weight of newborns whose mother was smoking (`smoke=1`) during pregnancy but was free of hypertension (`ht=0`).  
   3. The five lowest baby weights for mothers with a weight below the first quartile of maternal weights.     
   4. Draw an histogram of baby weights by taking into account the number of previous premature labours. You will consider the following recoding of the `ptl` variable: `ptl2=0` if `ptl=0`, `ptl2=1` when `ptl>0`.  
   5. Finally, show the distribution of all *numerical* variables as box and whiskers plots.
   
**4.** Based on the following artificial data of students' heights in two classrooms, indicate if the largest student comes from classroom A or B. Data were generated as follows:


```r
d <- data.frame(height = rnorm(40, 170, 10), class = sample(LETTERS[1:2], 40, rep = TRUE))
d$height[sample(1:40, 1)] <- 220
```


**5.** Load the data included in `lungcancer.txt`. This data file features several coding problems (variables type, missing values, etc.). Inspect these data (e.g., using `summary()`) and suggest appropriate corrections.

### Data exploration and two-group comparisons

**6.** Import the `reading2.csv` [data](http://lib.stat.cmu.edu/DASL/Datafiles/DRPScores.html) file using `read.csv()` or `read.csv2()` depending on the type of field delimiter (refer the on-line documentation). Details about this study are given in <a href="">Schmitt (1987)</a>. Note that the original data set was modified for the purpose of this exercise.

   1. Report the number of missing values for each treatment group (`Treated` vs. `Control`).  
   2. Report sample size for each group in a Table, while reporting all missing observations.    
   3. Plot the distribution of responses in each group with a density plot.  
   4. Do a t-test without Welch correction, and compare its results with a Wilcoxon test (see `wilcox.test`).

**7.** The `fusion.dat` file includes [data](http://lib.stat.cmu.edu/DASL/Datafiles/FusionTime.html) from an experiment on visual perception relying on the use of random-dots stereograms (<a href="">Frisby & Clatworthy, 1975</a>). The aim of this study was to verify that prior knowledge of visual shape to detect enhances processing time for fusing the two images. One group of subjects (`NV`) received either no information or verbal vue only, while the other group (`VV`) received verbal and visual cues (drawing of the target object).

   1. Display subjects' performance in each condition with a convenient graphic, and compute their means and standard deviations.  
   2. Perform a t-test assuming equality of variances and conclude with respect to the null hypothesis (no effect of the manipulated factor). Compare with a t-test that does not assume equality of variances.  
   3. Run the same 'classical' t-test on log-transformed response values, and verify if the assumption of equal variances holds (use `var.test()`).

**8.** The `IQ_Brain_Size.txt` data file contains data from a brain imaging study on brain volume and anthropometric characteristics of Monozygotic twins (<a href="">Tramo et al. 1998</a>). More details are available in the file header. It can be loaded as follows:   
   

```r
brain <- read.table("IQ_Brain_Size.txt", header = FALSE, skip = 27, nrows = 20)
```

   
This assumes that the data file sits in the current working directory (otherwise, you can change the pathname, or update the current working directory with `setwd()` or RStudio's facilities). Do read carefully the on-line help for `read.table()` to understand what the above command is actually doing.
   
   1. Provide meaningful variable names based on the available information in the source data file.  
   2. Recode variables as factors when appropriate.  
   3. How many boys and girls are there in this sample (counts and frequencies)?  
   4. What is the average IQ level, what about median IQ, and how many children do have an IQ level below (strictly) 90?  
   5. Compute the inter-quartile range for the following variables: `CCMIDSA`, `HC`, `TOTSA`, `TOTVOL`.  
   6. Draw an histogram of children weights, separately for each twin (you can consider odd and even rows to perform stratification, or directly use birth order).  
   7. For each tercile of children weights, compute the mean and standard deviation of `TOTVOL`.  
   8. Compare IQ levels of monozygotic twins with a t-test.  
   9. Compare head circumference of males vs. females with a t-test.
   
For questions (8) and (9), don't just give the p-values; give informative summary of existing differences (e.g., means ± standard deviation, standardized means difference), if any.

### One-way and two-way ANOVA

**9.** Here is an artificial data set to study the one-way ANOVA model.


```r
set.seed(101)
k <- 3  # number of groups 
ni <- 10  # number of observations per group
mi <- c(10, 12, 8)  # group means
si <- c(1.2, 1.1, 1.1)  # group standard deviations
grp <- gl(k, ni, k * ni, labels = c("A", "B", "C"))
resp <- c(rnorm(ni, mi[1], si[1]), rnorm(ni, mi[2], si[2]), rnorm(ni, mi[3], si[3]))
```


Note that the last command is not necessarily the most elegant way to generate individual responses, and such operations can easily be vectorized like this:

```r
resp <- as.vector(mapply(rnorm, ni, mi, si))
```


All variables are then grouped into a data frame called `dfrm`:

```r
dfrm <- data.frame(grp, resp)
rm(grp, resp)  # delete original variables
```


   1. Compute average response for condition A, B, and C.
   2. Compute difference between these group means and the overall mean.
   3. Show the distribution of individual responses in each condition using a box and whiskers chart.
   4. Compute the value of the F-statistic testing the null hypothesis of equality of the three means. Based on the ANOVA table returned by R, verify the value of the test statistic manually.
   5. Apply the one-way ANOVA to the same data, but ignoring the last level of the factor (`C`). Compare the results with those obtained from a t-test.

**10.** The `taste.dat` file includes results from a study on appetite induced by different foods (<a href="http://lib.stat.cmu.edu/DASL/Datafiles/tastedat.html">Street & Carroll, 1972</a>). Subjects assessed food flavor on a 7-point ordinal scale (-3 = awful to +3 = excellent). There were a total of 50 participants randomized to one of 4 conditions defined by the following factors: screen type (`SCR`, 0=coarse or 1=fine) and concentration of a liquid component (`LIQ`, 0=low or 1=high). The experiment was repeated 4 times, so that there are 16 groups of 50 participants in total. The response variable was the total score (`SCORE`) recorded for each treatment.

   1. Compute means and standard deviations of scores for each treatment.
   2. Use a Cleveland dot chart to display average scores in each condition, for each factor separately. Draw box and wiskers charts of scores for the 4 treatments.
   3. Test the hull hypothesis of no effect of the experimental factors using the appropriate model. Is the interaction between the two factors significant at 5%? Is it possible to simplify the full model (`SCR + LIQ + SCR:LIQ`)?
   4. Look in the online help for the Bartlett test (see `help.search()`), and apply the command to the current data. What conclusion can be drawn from its results?
   5. How would you compute an effect size for the `SCR` factor? Compare with what would be obtained using a simple t-test.


### Correlation and linear regression

**11.** Data in the file `brain_size.dat` were collected by authors interested in the relationship between [brain size and weight and intelligence](http://lib.stat.cmu.edu/DASL/Stories/BrainSizeandIntelligence.html) (<a href="">Willerman et al. 1991</a>).

*Hint:* Be careful with missing data when loading the data.

1. Compute the linear correlation between `MRI_count` and `Weight`, with its 95% confidence interval.
2. Estimate Pearson's r between these two variables in men and in females. Can they be considered different from a statistical point of view? *Hint:* You can use the `r.test()` command from the [psych](http://cran.r-project.org/web/packages/psych/) package (`install.packages("psych")`) or `corXY()` from `epibasix`.
3. Show in a scatterplot the relationship between `MRI_count` and `Weight` , highlighted by gender levels.
4. Compute the slope of the regression line for the model given by the following formula `fm <- MRI_count ~ Weight`. Does its 95% confidence interval include the value 0?
5. Show the distribution of residuals for this model using an histogram or a density plot.

**12.** Load data included in the R file `sim.rda`. Once loaded using `data()`, you will have access to a data frame named `sim`. This is a rectangular data set with 80 observations and 44 variables. The response variable is `y`, and explanatory variables are named `x1` to `x43`. We seek to isolate among them predictors showing interesting relationship with `y`, in terms of positive or negative linear correlation. To keep a variable, a criteria based on a significant correlation test (unadjusted 5% level) will be considered.

1. Write R code that will select all variables fulfilling the above criteria.
2. How many variables would be kept if we were simply correcting the type I error rate by the Bonferroni method (i.e., divide observed p-values by the number of tests, or, equivalently, consider a nominal risk of $0.05k$, where $k$ is the number of tests)?
3. Are there potential issues with such univariate screening? 
4. Using the selected variables in (1) (unadjusted 5% error rate criteria), fit a regression model with `y` as the response variable and all candidates predictors. Comment the results.

### ANOVA and regression

**13.** Consider the `ToothGrowth` dataset, available in R with the following command:

```r
data(ToothGrowth)
```

We are interested in studying the effect of `dose` alone (i.e., don't pay attention to the `supp` factor). Compare and discuss the results of the following models:

* A one-way ANOVA with `dose` treated as a factor with unordered levels
* A one-way ANOVA with `dose` treated as a factor with ordered levels (`ToothGrowth$dose <- factor(ToothGrowth$dose, ordered=TRUE)`)
* A linear regression with `dose` treated as a numeric variable.
* A linear regression including `dose` and `I(dose^2)` (which means the square of the dose) as numeric variables.

**14.** Using data from Exercise 10, we will be interested in modeling the relationship between score and screen type, `SCORE ~ SCR`, without treating `SCR` as a factor.

1. Compute the intercept and slope of the regression line for the model `SCORE ~ SCR`, and show the regression line in a scatterplot display, together with individual data points.
2. Assuming the above regression model has been saved in a variable `m`,  what represent the following values: `taste$SCORE - fitted(m)`?
3. Compare results from a test of significance of the slope to a simple t-test with `SCR` treated as a grouping variable?
4. The levels for the `SCR` variable are currently coded as {0,1}. Update them to {-1,+1}. Does this affect the previous comparison? If so, in what sense?

**15.** Authors studied the effect of different dieting treatment on 60 rats, and they considered two factors of interest: type of proteins used to feed the rats (beef, pork, cereals) and their concentration level (high or low) (<a href="">Snedecor & Cochran, 1980</a>). The response variable was the weight gain (in grams) after treatment. Data are available in the `rats.rda` data file.

```r
load("rats.rda")
rat <- within(rat, {
    Diet.Amount <- factor(Diet.Amount, levels = 1:2, labels = c("High", "Low"))
    Diet.Type <- factor(Diet.Type, levels = 1:3, labels = c("Beef", "Pork", "Cereal"))
})
```


1. Show average responses for the 60 rats in an interaction plot.
2. Consider the following 6 treatments: `Beef/High`, `Beef/Low`, `Pork/High`, `Pork/Low`, `Cereal/High`, et `Cereal/Low`. What is the result of the F-test for a one-way ANOVA?
3. Use `pairwise.t.test()` with Bonferroni correction to identify pairs of treatments that differ significantly one from the other.
4. Based on these 6 treatments, build a matrix of 5 contrasts allowing to test the following conditions: beef vs. cereal and beef vs. porc (2 DF); high vs. low dose (1 DF); and interaction type/amount (2 DF). Partition the SS associated to treatment computed in (2) according to these contrasts, and test each contrast at a 5% level (you can consider that there's no need to correct the multiple tests if contrasts were defined *a priori*).
5. Compare those results with a two-way ANOVA including the interaction between type and amount.
6. Test the following *post-hoc* contrast: beef and pork at high dose (i.e., `Beef/High` and `Pork/High`) vs. the average of all other treatments. Is the result significant at the 5% level? What does this result suggest?

### References   

Frisby J and Clatworthy J (1975). "Learning to see complex random-dot
steregrams." _Perception_, *4*, pp. 173-178.

Schmitt MC (1987). _The Effects on an Elaborated Directed Reading
Activity on the Metacomprehension Skills of Third Graders_. PhD thesis,
Purdue University.

Snedecor G and Cochran W (1980). _Statistical Methods_. Iowa State
Press.

Street E and Carroll M (1972). "Preliminary Evaluation of a Food
Product." In Tanur J (ed.), pp. 220-238. Holden-Day, San Francisco.
<URL: http://lib.stat.cmu.edu/DASL/Datafiles/tastedat.html>.

Tramo M, Loftus W, Green R, Stukel T, Weaver J and Gazzaniga M (1998).
"Brain Size, Head Size, and IQ in Monozygotic Twins." _Neurology_,
*50*, pp. 1246-1252.

Willerman L, Schultz R, Rutledge J and Bigler E (1991). "In Vivo Brain
Size and Intelligence." _Intelligence_, *15*, pp. 223-228.

