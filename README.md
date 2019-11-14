<!DOCTYPE html>



<html xmlns="http://www.w3.org/1999/xhtml">



<head>



<meta charset="utf-8" />

<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

<meta name="generator" content="pandoc" />











<style type="text/css">

h1 {

  font-size: 34px;

}

h1.title {

  font-size: 38px;

}

h2 {

  font-size: 30px;

}

h3 {

  font-size: 24px;

}

h4 {

  font-size: 18px;

}

h5 {

  font-size: 16px;

}

h6 {

  font-size: 12px;

}

.table th:not([align]) {

  text-align: left;

}

</style>





</head>



<body>



<!-- code folding -->











<div class="fluid-row" id="header">







<h1 class="title toc-ignore">Regression Analysis in Motor Trend Car Road Tests</h1>

<h4 class="date"><em>November 1, 2018</em></h4>



</div>





<div id="executive-summary" class="section level2">

<h2>Executive Summary</h2>

<p>In this analysis, the researcher has investigated which is better for transmission - automatic or manual and the difference between the two. This analysis is based from the 1974 the Motor Trend Car Road Tests and comprises of fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973 - 1974 models). The analysis will go through the following: discussion and presentation of descriptive statistics, conducting the initial regression model, diagnostic tests for the initial regression model (tests conducted are: test for multicollinearity, test for heteroskedasticity, and test for overfitting/underfitting), and presentation of the final model along with conclusions. The result of the analysis showed that manual transmission is better than automatic transmission.</p>

</div>

<div id="descriptive-statistics" class="section level2">

<h2>Descriptive Statistics</h2>

<p>Our dataset, mtcars is composed of 32 observations and 11 variables. A summary information about the data is illustrated showing the min values, max values, and values at 1st quarter, 3rd quarter, and median.</p>

<pre class="r"><code>summary(mtcars)</code></pre>

<pre><code>##       mpg             cyl             disp             hp       

##  Min.   :10.40   Min.   :4.000   Min.   : 71.1   Min.   : 52.0  

##  1st Qu.:15.43   1st Qu.:4.000   1st Qu.:120.8   1st Qu.: 96.5  

##  Median :19.20   Median :6.000   Median :196.3   Median :123.0  

##  Mean   :20.09   Mean   :6.188   Mean   :230.7   Mean   :146.7  

##  3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:326.0   3rd Qu.:180.0  

##  Max.   :33.90   Max.   :8.000   Max.   :472.0   Max.   :335.0  

##       drat             wt             qsec             vs        

##  Min.   :2.760   Min.   :1.513   Min.   :14.50   Min.   :0.0000  

##  1st Qu.:3.080   1st Qu.:2.581   1st Qu.:16.89   1st Qu.:0.0000  

##  Median :3.695   Median :3.325   Median :17.71   Median :0.0000  

##  Mean   :3.597   Mean   :3.217   Mean   :17.85   Mean   :0.4375  

##  3rd Qu.:3.920   3rd Qu.:3.610   3rd Qu.:18.90   3rd Qu.:1.0000  

##  Max.   :4.930   Max.   :5.424   Max.   :22.90   Max.   :1.0000  

##        am              gear            carb      

##  Min.   :0.0000   Min.   :3.000   Min.   :1.000  

##  1st Qu.:0.0000   1st Qu.:3.000   1st Qu.:2.000  

##  Median :0.0000   Median :4.000   Median :2.000  

##  Mean   :0.4062   Mean   :3.688   Mean   :2.812  

##  3rd Qu.:1.0000   3rd Qu.:4.000   3rd Qu.:4.000  

##  Max.   :1.0000   Max.   :5.000   Max.   :8.000</code></pre>

<p>It can be observed that variables, vs and am which are discrete has a mean below the 50th percentile. This tells that most of the observations have discrete values 0. Other variables aside from the mentioned earlier are continuous.</p>

</div>

<div id="initial-modelling" class="section level2">

<h2>Initial Modelling</h2>

<p>The initial regression model would include variable, mpg (miles per gallon) as our dependent variable and the rest of other variables as our independent variables. A diagnotic tests will follow after the initial modelling.</p>

<pre class="r"><code>fit0 &lt;- lm(mpg ~., mtcars )

summary(fit0)</code></pre>

<pre><code>## 

## Call:

## lm(formula = mpg ~ ., data = mtcars)

## 

## Residuals:

##     Min      1Q  Median      3Q     Max 

## -3.4506 -1.6044 -0.1196  1.2193  4.6271 

## 

## Coefficients:

##             Estimate Std. Error t value Pr(&gt;|t|)  

## (Intercept) 12.30337   18.71788   0.657   0.5181  

## cyl         -0.11144    1.04502  -0.107   0.9161  

## disp         0.01334    0.01786   0.747   0.4635  

## hp          -0.02148    0.02177  -0.987   0.3350  

## drat         0.78711    1.63537   0.481   0.6353  

## wt          -3.71530    1.89441  -1.961   0.0633 .

## qsec         0.82104    0.73084   1.123   0.2739  

## vs           0.31776    2.10451   0.151   0.8814  

## am           2.52023    2.05665   1.225   0.2340  

## gear         0.65541    1.49326   0.439   0.6652  

## carb        -0.19942    0.82875  -0.241   0.8122  

## ---

## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

## 

## Residual standard error: 2.65 on 21 degrees of freedom

## Multiple R-squared:  0.869,  Adjusted R-squared:  0.8066 

## F-statistic: 13.93 on 10 and 21 DF,  p-value: 3.793e-07</code></pre>

<p>Based on the initial regression, the variables, cyl (number of cylinder), hp (gross horsepower), wt (weight in 1000 lbs), and carb (number of carburetors) have negative estimated coefficients. This tells a negative relationship to our dependent variable. Our adjusted R-squared is 80.66%. This tells that the variation in our model is explained by 80.66%. This is considered to be a good indicator that our model is good and able to explain our dependent variable.</p>

</div>

<div id="diagnostics" class="section level2">

<h2>Diagnostics</h2>

<p>To get the final model, a series of diagnostic tests is conducted. This tests include: test for heteroskedasticity, variance inflation factor, and test for overfitting/underfitting.</p>

<div id="variance-inflation-factor" class="section level3">

<h3>Variance Inflation Factor</h3>

<p>Under this test, the researcher has tested the model for any potential variance inflation factor. Variance inflation factor tells if the variance of an explanatory variable is inflated due to the high correlation to other explanatory variable(s). The goal in this test is to check our model for multicollinearity and to remove variables which have high variance inflation factor.</p>

<p>In the next figure, the researcher has presented the correlation table of the model.</p>

<pre class="r"><code># Perform correlation

      cor(mtcars)</code></pre>

<pre><code>##             mpg        cyl       disp         hp        drat         wt

## mpg   1.0000000 -0.8521620 -0.8475514 -0.7761684  0.68117191 -0.8676594

## cyl  -0.8521620  1.0000000  0.9020329  0.8324475 -0.69993811  0.7824958

## disp -0.8475514  0.9020329  1.0000000  0.7909486 -0.71021393  0.8879799

## hp   -0.7761684  0.8324475  0.7909486  1.0000000 -0.44875912  0.6587479

## drat  0.6811719 -0.6999381 -0.7102139 -0.4487591  1.00000000 -0.7124406

## wt   -0.8676594  0.7824958  0.8879799  0.6587479 -0.71244065  1.0000000

## qsec  0.4186840 -0.5912421 -0.4336979 -0.7082234  0.09120476 -0.1747159

## vs    0.6640389 -0.8108118 -0.7104159 -0.7230967  0.44027846 -0.5549157

## am    0.5998324 -0.5226070 -0.5912270 -0.2432043  0.71271113 -0.6924953

## gear  0.4802848 -0.4926866 -0.5555692 -0.1257043  0.69961013 -0.5832870

## carb -0.5509251  0.5269883  0.3949769  0.7498125 -0.09078980  0.4276059

##             qsec         vs          am       gear        carb

## mpg   0.41868403  0.6640389  0.59983243  0.4802848 -0.55092507

## cyl  -0.59124207 -0.8108118 -0.52260705 -0.4926866  0.52698829

## disp -0.43369788 -0.7104159 -0.59122704 -0.5555692  0.39497686

## hp   -0.70822339 -0.7230967 -0.24320426 -0.1257043  0.74981247

## drat  0.09120476  0.4402785  0.71271113  0.6996101 -0.09078980

## wt   -0.17471588 -0.5549157 -0.69249526 -0.5832870  0.42760594

## qsec  1.00000000  0.7445354 -0.22986086 -0.2126822 -0.65624923

## vs    0.74453544  1.0000000  0.16834512  0.2060233 -0.56960714

## am   -0.22986086  0.1683451  1.00000000  0.7940588  0.05753435

## gear -0.21268223  0.2060233  0.79405876  1.0000000  0.27407284

## carb -0.65624923 -0.5696071  0.05753435  0.2740728  1.00000000</code></pre>

<p>In the correlation table, it can be seen that most of the correlation values among variables range from 0.50 to 0.90 (or -0.50 to -0.90). High correlation can tell that variance inflation is present in our model. Now, a variance inflation factor test is conducted. If a variable’s VIF is 10 or greater, it would imply the presence of severe multicollinearity and the researcher will exclude those variables in the model</p>

<pre class="r"><code># Load the library for variance inflation factor

      library(car)



# Calculation of variance inflation factor

      vif(lm(mpg ~., mtcars))</code></pre>

<pre><code>##       cyl      disp        hp      drat        wt      qsec        vs 

## 15.373833 21.620241  9.832037  3.374620 15.164887  7.527958  4.965873 

##        am      gear      carb 

##  4.648487  5.357452  7.908747</code></pre>

<p>The result of our test shows that cyl, disp, and wt have the highest variance. We will remove the variable with the highest vif (which in our case, disp) and perform again the VIF to see if the variance inflation has dropped upon the removal of the variable, disp.</p>

<pre class="r"><code># Second run of VIF

      vif(lm(mpg ~. -disp, mtcars))</code></pre>

<pre><code>##       cyl        hp      drat        wt      qsec        vs        am 

## 14.284737  7.123361  3.329298  6.189050  6.914423  4.916053  4.645108 

##      gear      carb 

##  5.324402  4.310597</code></pre>

<p>In our second run of VIF, the variance of the variance has dropped. We will consider removing variable cyl since its VIF is greater than 10.</p>

<pre class="r"><code># Third run of VIF

      vif(lm(mpg ~. -disp -cyl, mtcars))</code></pre>

<pre><code>##       hp     drat       wt     qsec       vs       am     gear     carb 

## 6.015788 3.111501 6.051127 5.918682 4.270956 4.285815 4.690187 4.290468</code></pre>

<p>It can now be observed that the VIF of our variables have dramatically changed due to the removal of variables with high VIF. Our model will now include explanatory variables: hp, drat, wt, qsec, vs, am, gear, and carb.</p>

<div id="test-for-overfittingunderfitting" class="section level4">

<h4>Test for Overfitting/Underfitting</h4>

<p>This diagnostic test will show if inclusion or exclusion of explanatory variables has an impact in the model that would cause it to be overfitted or underfitted. In this test, the researcher will check the estimated coefficients and adjusted R-squared and see the impact of inclusion of explanatory variables. The goal in this test is to find the best fit for the model.</p>

<p>We will based our test for overfitting/underfitting based on their correlation to mpg - in descending order (from the highest positive correlation to the highest negative correlation).</p>

<pre class="r"><code># Perform correlation

      cor(mtcars)[,1]</code></pre>

<pre><code>##        mpg        cyl       disp         hp       drat         wt 

##  1.0000000 -0.8521620 -0.8475514 -0.7761684  0.6811719 -0.8676594 

##       qsec         vs         am       gear       carb 

##  0.4186840  0.6640389  0.5998324  0.4802848 -0.5509251</code></pre>

<pre class="r"><code># Coding the regression models

      fitA &lt;- lm(mpg ~ drat, mtcars)

      fitB &lt;- lm(mpg ~ drat + vs, mtcars)

      fitC &lt;- lm(mpg ~ drat + vs + am, mtcars )

      fitD &lt;- lm(mpg ~ drat + vs + am + gear, mtcars)

      fitE &lt;- lm(mpg ~ drat + vs + am + gear + qsec, mtcars)

      fitF &lt;- lm(mpg ~ drat + vs + am + gear + qsec + carb, mtcars)

      fitG &lt;- lm(mpg ~ drat + vs + am + gear + qsec + carb + hp, mtcars)

      fitH &lt;- lm(mpg ~ drat + vs + am + gear + qsec + carb + hp + wt, mtcars)



# Comparing the estimated coefficients 

      list(coef(fitA), coef(fitB), coef(fitC), coef(fitD), coef(fitE), coef(fitF), coef(fitG), coef(fitH))</code></pre>

<pre><code>## [[1]]

## (Intercept)        drat 

##   -7.524618    7.678233 

## 

## [[2]]

## (Intercept)        drat          vs 

##   -1.825317    5.436549    5.401262 

## 

## [[3]]

## (Intercept)        drat          vs          am 

##    8.326573    1.985086    6.235194    4.668725 

## 

## [[4]]

## (Intercept)        drat          vs          am        gear 

##   11.042676    2.536344    6.195640    5.904918   -1.405732 

## 

## [[5]]

## (Intercept)        drat          vs          am        gear        qsec 

##  -7.6979547   2.3101970   3.3009058   6.6460260  -0.7583781   1.0158827 

## 

## [[6]]

## (Intercept)        drat          vs          am        gear        qsec 

##   0.5965157   2.1085311   1.5000644   4.0671322   1.7015096   0.4516582 

##        carb 

##  -1.6831137 

## 

## [[7]]

##  (Intercept)         drat           vs           am         gear 

## 14.347876811  1.473189965  1.513483375  3.429537967  1.486429207 

##         qsec         carb           hp 

##  0.002165956 -1.130567978 -0.026936278 

## 

## [[8]]

## (Intercept)        drat          vs          am        gear        qsec 

## 13.80810376  0.88893522  0.08786399  2.42417670  0.69389707  0.63983256 

##        carb          hp          wt 

## -0.61286048 -0.01225158 -2.60967758</code></pre>

<pre class="r"><code># Comparing the adjusted R-squared

      list(summary(fitA)$adj.r.squared, summary(fitB)$adj.r.squared, summary(fitC)$adj.r.squared, summary(fitD)$adj.r.squared, summary(fitE)$adj.r.squared, summary(fitF)$adj.r.squared, summary(fitG)$adj.r.squared, summary(fitH)$adj.r.squared)</code></pre>

<pre><code>## [[1]]

## [1] 0.4461283

## 

## [[2]]

## [1] 0.6028487

## 

## [[3]]

## [1] 0.6657181

## 

## [[4]]

## [1] 0.6646552

## 

## [[5]]

## [1] 0.6834239

## 

## [[6]]

## [1] 0.7734702

## 

## [[7]]

## [1] 0.78793

## 

## [[8]]

## [1] 0.8186912</code></pre>

<p>In the estimated coefficients, the signs and values change as variables are added to the model. Though the signs and values change, the adjusted R-squared increases which means that the variation explained in the model increases as variables are added. The changes in signs and values may prove that as we add variables to the regression model, we are getting closer to finding the true value for our explanatory variables.</p>

</div>

<div id="test-for-heteroskedasticity" class="section level4">

<h4>Test for Heteroskedasticity</h4>

<p>Heteroskedasticity is defined as the nonlinearity behavior of variance in the model. According to statsmakemecry website, heteroskedasticity refers to the circumstance in which the variability of a variable is unequal across the range of values of a second variable that predicts it. The researcher has used Breusch Pagan Test for Heteroskedasticity. Below is the results of the test.</p>

<pre class="r"><code># Load the library

      library(olsrr)</code></pre>

<pre><code>## Warning: package 'olsrr' was built under R version 3.4.4</code></pre>

<pre><code>## 

## Attaching package: 'olsrr'</code></pre>

<pre><code>## The following object is masked from 'package:datasets':

## 

##     rivers</code></pre>

<pre class="r"><code># Perform the test for Heteroskedasticity

      ols_test_breusch_pagan(fitH)</code></pre>

<pre><code>## 

##  Breusch Pagan Test for Heteroskedasticity

##  -----------------------------------------

##  Ho: the variance is constant            

##  Ha: the variance is not constant        

## 

##              Data               

##  -------------------------------

##  Response : mpg 

##  Variables: fitted values of mpg 

## 

##         Test Summary          

##  -----------------------------

##  DF            =    1 

##  Chi2          =    3.001093 

##  Prob &gt; Chi2   =    0.08320837</code></pre>

<p>In this test, the researcher has looked on the p-value. If the p-value is less than 0.05, then we fail to reject the null hypothesis. Based from the results, the p-value is 0.0832 which is more than 0.05. This concludes that our model is not heteroskedastic.</p>

</div>

</div>

</div>

<div id="final-model-conclusion" class="section level2">

<h2>Final Model &amp; Conclusion</h2>

<p>Now that the diagnostic tests for our model is done, this leads to the final model which is fitH. Based from our diagnostic test, our final regression model is not heteroskedastic; variance inflation is eliminated with tolerance VIF level of less than 10; and the fit in our model is the most optimal and best fit based from the test for overfitting/underfitting. The summary of our final regression model is shown below.</p>

<pre class="r"><code>summary(fitH)</code></pre>

<pre><code>## 

## Call:

## lm(formula = mpg ~ drat + vs + am + gear + qsec + carb + hp + 

##     wt, data = mtcars)

## 

## Residuals:

##     Min      1Q  Median      3Q     Max 

## -3.8187 -1.3903 -0.3045  1.2269  4.5183 

## 

## Coefficients:

##             Estimate Std. Error t value Pr(&gt;|t|)  

## (Intercept) 13.80810   12.88582   1.072   0.2950  

## drat         0.88894    1.52061   0.585   0.5645  

## vs           0.08786    1.88992   0.046   0.9633  

## am           2.42418    1.91227   1.268   0.2176  

## gear         0.69390    1.35294   0.513   0.6129  

## qsec         0.63983    0.62752   1.020   0.3185  

## carb        -0.61286    0.59109  -1.037   0.3106  

## hp          -0.01225    0.01649  -0.743   0.4650  

## wt          -2.60968    1.15878  -2.252   0.0342 *

## ---

## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

## 

## Residual standard error: 2.566 on 23 degrees of freedom

## Multiple R-squared:  0.8655, Adjusted R-squared:  0.8187 

## F-statistic:  18.5 on 8 and 23 DF,  p-value: 2.627e-08</code></pre>

<p>Now, to answer the research questions:</p>

<ol style="list-style-type: decimal">

<li>Is an automatic or manual transmission better for MPG?</li>

<li>Quantify the MPG difference between automatic and manual transmissions.</li>

</ol>

<p>Since R recognizes categorical or factor variables in ascending order (0 first, then 1) then, we interpret that the value of intercept is the value of the automatic transmission given the other variables held constant. The coefficient value of am refers to the distance between manual and automatic. Thus, to get the value of automatic transmission, add the intercept and coefficient value of am.</p>

<p>The value of automatic transmission is 13.8081038 and the value of manual transmission is 16.2322805. This tells that manual transmission is better performing than automatic transmission when it comes to mpg or gas consumption. This is true because automatic transmission cars consumed more gallons because their torque converter was always slipping and the transmission took a fair amount of energy to run. That is why, manual transmission was preferred because consumers had more fuel savings. But this conclusion may not be anymore applicable in the present because the time gap between the present and the dataset was gathered is huge and that technology has evolved a lot. This further provides that continuous improvement to automatic cars have been developed including fuel consumption reduction. When the time came, automatic cars would be at par with manual transmission in terms of fuel consumption.</p>

<p>Lastly, the researcher presents the various plot of the final model. The first plot is residuals vs fitted. The observations are somewhat near 0 and the red line is nonlinear. Therefore, our conclusion that the model is not heteroskedastic is true.</p>

<p>The second plot is normal Q-Q plot. A Q-Q plot is a graphical technique for determining if two data sets come from populations with a common distribution according to itl website. It can be observed that the standardized residuals lie along the broken lines in our plot. This means to tell that the distribution of the population is normal.</p>

<p>The third plot is scale location plot. Scale location plot, according to data library virginia website, shows if residuals are spread equally along the ranges of predictors. In our plot, it can be seen that the data points are scattered and that the red line is upward. Because there is no pattern, the residuals are randomly spread.</p>

<p>The fourth plot is residuals vs leverage. This helps us identify and look influential cases. Influential cases or data points are those data points which have an unusually large effect on our regression analysis. Our analysis entails to check if the data points are within the cook’s distance or not. If they are within the cook’s distance, then the conclusion is there is no influential cases. In our data, data points are within the cook’s distance hence, no influential cases in our model.</p>

</div>

<div id="appendix" class="section level2">

<h2>Appendix</h2>

<pre class="r"><code># Plot the model

      plot(fitH)</code></pre>

<p><img src="data:image/png;base64,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" width="672" /><img src="data:image/png;base64,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" width="672" /><img src="data:image/png;base64,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" width="672" /><img src="data:image/png;base64,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" width="672" /></p>

</div>









</div>



<script>



// add bootstrap table styles to pandoc tables

function bootstrapStylePandocTables() {

  $('tr.header').parent('thead').parent('table').addClass('table table-condensed');

}

$(document).ready(function () {

  bootstrapStylePandocTables();

});





</script>



<!-- dynamically load mathjax for compatibility with self-contained -->

<script>

  (function () {

    var script = document.createElement("script");

    script.type = "text/javascript";

    script.src  = "https://mathjax.rstudio.com/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML";

    document.getElementsByTagName("head")[0].appendChild(script);

  })();

</script>



</body>

</html>
