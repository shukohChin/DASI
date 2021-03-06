<!-- Make sure that the knitr package is installed and loaded. -->
<!-- For more info on the package options see http://yihui.name/knitr/options -->

<!-- Replace below with the title of your project -->
### Age and Attitude towards space exploration program

<!-- Enter the code required to load your data in the space below. The data will be loaded but the line of code won't show up in your write up (echo=FALSE) in order to save space-->



### Introduction:
<!-- What is your research question? Why do you care? Why should others care? -->
What I will investigate in this report is the associate between age and people's attitude towards the money we are spending on the Space exploration program.  
Reasons why I am interested in this question, and why others should care are discussed below. 
* First, to a certain extent, I am a fan of manga about space, like "Space brothers". Besides, I believe that we should spend more budget on space exploration because the development of technology during space exploration will benefit human eventually. So I think others should care, too.    
* Second, People's attitude towards the money spending on the Space exploration program, somewhat indicates if people are positive or negative about it. I am interested to find what factors will influence the attitude. Other factors may also affect, like education, satisfaction of current life. But from my personal experience, some people I know, including myself, become conservative as age grows. So maybe it will be interesting if I find the association between age and attitude.
* Third, I care about what the growth of age will bring us. I think others should also care about it, too. Because If we know age growth will make us conservative which we may not want to be, then we may have to pay attention to keep our minds open and accept different things. Space exploration is just one aspect, but I think it somewhat indicates our minds.

### Data:

<!-- Data Collection, Cases, Variables, Study, Scope of inference-generalizability. -->

The Data I use is General Social Survey collect data on demographic characteristics and attitudes of residents of the United States, called "GSS". According to [GSS Offical Site]( http://publicdata.norc.org:41000/gssbeta/faqs.html#10), the way of data collection was listed below.
 1. Since 1975 the GSS has used full-probability sampling of households designed to give each household an equal probability of being included in the GSS. Only one adult per household is interviewed.
 2. Majority of GSS data is obtained in face-to-face interviews. Computer-assisted personal interviewing (CAPI) began in the 2002.
 3. From 1972 until 1993, the GSS was administered almost annually. The target sample size for the annual surveys was 1500; actual sample sizes ranged between 1372 (1990) and 1613 (1972). Additionally, there were oversamples of black respondents in 1982 (oversample of 354) and 1987 (oversample of 353). There were no GSSs in 1979, 1981, or 1992.
 4. Not all questions asked of all respondents.
 5. Response rates is not 100%.
 
 
Each case I will focus on is **independently drawn sample of an adult(+18) in this study**.  
As I dicussed in the introduction, I will choose two variables, **age** and **natspac**.  
* age: Age of respondent. Only age is numerical, but I will divide it into different age groups as below to do this study.  
[categorical: (18 - 29), (30 - 44), (45 - 59), (60 - 74), (75+)]
* natspac: attitude towards the money we are spending on SPACE EXPLORATION PROGRAM.  
[categorical: too much money, about right, too little ]


The type of the study is an **observational study**. According to the [codebook of GSS](https://d396qusza40orc.cloudfront.net/statistics%2Fproject%2Fgss1.html#natspac), quote"The GSS aims to gather data on ... in order to ...", and the way of data collection, I think GSS study is an observational study because, different kinds of data are just gathered, and there is not an experimental design including random assignment to groups.  
The population of interested is **residents of the United States of America(age:+18)**. Since the study was conducted by nearly randomly choosing samples from American population, the results **should be generalizable**. **As there is no random assignment, I can not make any established causal links**.  
Although I think the study can be generalized, there are also some bias and other factor we may have to pay attention to.
 1. Response rates is not 100%(always 70% ~ 80%), so **non-response bias** might be considered. As simple random sampling is used in the study, and non-response rate is relatively low, I consider the sample will represent the population of interest.
 2. This is a household based study. It may be not "_exactly_" simple random sampled because persons living in large households have **slightly** lower probabilities of selection. But since it is so slight based on huge population, I believe it is "_nearly_" simple random sampled, so generalizability of the data should be ensured. 

### Exploratory data analysis:

<!-- Calculate and discuss relevant descriptive statistics, including summary statistics and visualizations of the data. Also address what the exploratory data analysis suggests about your research question. -->
* First I will pick up the data of the year 2012(ignore NA) and divide age into different groups. One reason I pick the year 2012 is that it is the newest year, I assume it can represent the recent view as the society changes. The other reason is that the whole years of dataset may create respondents to overlap. As you can see the R code below, pro2012_group will be the data set I use in this study.

```r
project = subset(gss, gss$year == "2012")
pro2012 = na.omit(subset(project, select = c("age", "natspac")))
pro2012$age_group = c("18-29", "30-44", "45-59", "60-74", "75-")[findInterval(pro2012$age, 
    c(-Inf, 29.5, 44.5, 59.5, 74.5, Inf))]
pro2012_group = subset(pro2012, select = c("natspac", "age_group"))
```

* Second I will create the mosaicplot of data to show the relationship of these two variables. X represents age group, Y represents natspac(attitudes towards Space exploration grogram). 

```r
mosaicplot(table(pro2012$age_group, pro2012$natspa), col = c("red", "green", 
    "blue"))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

From the plot above, we may not see the clear relationship between age group and attitudes towards Space exploration program, but we can roughly find except age group(60-74), as age group grows, the number of people who think we spend too much on the space exploration is increasing. So they are maybe associated.

### Inference:

I will set the hypothesis like below.  
**Ho:** Attitude towards Space exploration program and age are independent. Attitude do not vary by age.  
**Ha:** Attitude towards Space exploration program and age are dependent. Attitude do vary by age.

Since these two variables(age group, natspac) are both categorical, and the goal is to find the association of these two variables. I decide to use **chi-square test of independence** if it meet the conditions. 
Here are the conditions I want to check.  
* _Independence_: Sampled observations must be independent
  - random sample  
  **Satisfied** because GSS data was nearly simple random sampled. 
  - if sampling without replacement, n < 10% of population  
  **Satisfied** because the sample size of GSS study every year(about 1500) is definetly < 10% of all American population.
  - each case only contributes to one cell in the table  
  **Satisfied** because each case can only contribute to one cell.
* _Sample size_: Each particular scenario must have at lease 5 expected cases  
I will discuss this condition using contingency table below. The table below is the observed results of each scenario.

```r
addmargins(table(pro2012_group))
```

```
##              age_group
## natspac       18-29 30-44 45-59 60-74 75- Sum
##   Too Little     39    61    50    47   6 203
##   About Right    86   118    95    65  33 397
##   Too Much       44    83    86    57  27 297
##   Sum           169   262   231   169  66 897
```

I calculate the **expected** accounts of each scenario. As you can see in the table below, each scenario have more than 5 cases, so _Sample size_ condition is also satisfied.  

 expected  |(18 - 29) | (30 - 44) | (45 - 59) | (60 - 74) | (75 - )   | sum
:----------|:--------:|:---------:|:---------:|:---------:|:---------:|:---------|
Too Little | 38       | 59        | 53        | 38        | 15        | 203
About Right| 75       | 116       | 102       | 75        | 29        | 397
Too Much   | 56       | 87        | 76        | 56        | 22        | 297
sum        | 169      | 262       | 231       | 169       | 66        | 897

As all conditions are satisfied, I will do the chi-square test of independence by using [inference method provided](http://bit.ly/dasi_inference) to test the hypothesis that age and natspac are associated at the 5% significance level.  
In the inference method, the visualization of the relationship of two variables will be created, X-squared, df and p-value will be calculated.  
I set "_y = pro2012\_group$natspac, x = pro2012\_group$age\_group_" because response variable is natspac and explanatory variable is age.  
I set est "proportion" because essecially proportions is used in this kind of method.  
I set method "theoretical" because I use theoretical rather than simulation as all of the expected cases > 5.  
I set type "ht" because I am doing a hypothesis test.

```r
source("http://bit.ly/dasi_inference")
inference(y = pro2012_group$natspac, x = pro2012_group$age_group, est = "proportion", 
    method = "theoretical", type = "ht", alternative = "greater")
```

```
## Response variable: categorical, Explanatory variable: categorical
## Chi-square test of independence
## 
## Summary statistics:
##              x
## y             18-29 30-44 45-59 60-74 75- Sum
##   Too Little     39    61    50    47   6 203
##   About Right    86   118    95    65  33 397
##   Too Much       44    83    86    57  27 297
##   Sum           169   262   231   169  66 897
```

```
## H_0: Response and explanatory variable are independent.
## H_A: Response and explanatory variable are dependent.
## Check conditions: expected counts
##              x
## y             18-29  30-44  45-59 60-74   75-
##   Too Little  38.25  59.29  52.28 38.25 14.94
##   About Right 74.80 115.96 102.24 74.80 29.21
##   Too Much    55.96  86.75  76.48 55.96 21.85
## 
## 	Pearson's Chi-squared test
## 
## data:  y_table
## X-squared = 16.65, df = 8, p-value = 0.034
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 

As we can see the results of inference above, p value is 0.034. Although p is not a very significant small value, because p < 0.05, I reject the null bypothesis in favor of the alternative, which means that these data provide convincing evidence that age group and attitude towards Space exploration program are associated.  
As a conclusion, the data tells us there is a relationship between age groups and natspac, but that's all it says. Also, since I did a chi-square testing, there are no other methods were applicable and hence there's nothing to compare.

### Conclusion:
<!-- Write a brief summary of your findings without repeating your statements from earlier. Include a discussion of what you have learned about your research question and the data you collected, and include ideas for possible future research. -->

What I want to investigate is the associate between age and people's attitude towards the money we are spending on the Space exploration program. After I doing this study by dividing age into different age groups and perfoming chi-squared test of independence of these two variables, I learnt that attitude about Space exploration program do vary by age group, that also means it do vary by age.  
But that was all the data tells. I still do not know the specific relationship between these two variables, like how attitude changes as age grows, and how other factors(like education, job) will affect. So for the possible future research, I would like to investigate more about the specific relationship and the influence of other factors.

### References:
* The GSS data set I use in this study can be found [here](http://bit.ly/dasi_gss_data).  
* [GSS Offical Site]( http://publicdata.norc.org:41000/gssbeta/faqs.html#10)  
* [codebook of GSS](https://d396qusza40orc.cloudfront.net/statistics%2Fproject%2Fgss1.html#natspac)

### Appendix:

```
##           natspac age_group
## 55090 About Right     30-44
## 55093 About Right     45-59
## 55094  Too Little     30-44
## 55096 About Right     18-29
## 55097 About Right     18-29
## 55099    Too Much     30-44
## 55101    Too Much     45-59
## 55103 About Right     30-44
## 55105    Too Much     45-59
## 55108  Too Little       75-
## 55111    Too Much     45-59
## 55113 About Right     30-44
## 55116    Too Much       75-
## 55117  Too Little     30-44
## 55118 About Right       75-
## 55119 About Right     60-74
## 55121    Too Much     30-44
## 55122 About Right     30-44
## 55128 About Right     18-29
## 55130  Too Little     45-59
## 55132    Too Much     30-44
## 55136 About Right       75-
## 55139 About Right     30-44
## 55140  Too Little     45-59
## 55142  Too Little     45-59
## 55145    Too Much       75-
## 55146    Too Much       75-
## 55150    Too Much     30-44
## 55152    Too Much     30-44
## 55156  Too Little     45-59
## 55157    Too Much     45-59
## 55159 About Right       75-
## 55162  Too Little     45-59
## 55164 About Right     45-59
## 55165  Too Little     30-44
## 55166    Too Much     30-44
## 55169  Too Little     30-44
## 55170 About Right     30-44
## 55171 About Right     45-59
## 55173 About Right     30-44
## 55174    Too Much     45-59
## 55179    Too Much     30-44
## 55180    Too Much     30-44
## 55181 About Right     45-59
## 55187    Too Much     18-29
## 55191 About Right     18-29
## 55192 About Right     18-29
## 55193  Too Little     18-29
## 55197    Too Much     30-44
## 55199    Too Much     60-74
```

