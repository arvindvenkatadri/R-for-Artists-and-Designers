---
title: "Pronouns and data"
author: Arvind Venkatadri
output: 
  html_document:
    toc: TRUE
    theme: cerulean # try other themes (“default”, “cerulean”, “journal”, “flatly”, “darkly”, “readable”, “spacelab”, “united”, “cosmo”, “lumen”, “paper”, “sandstone”, “simplex”, “yeti”)
  code_download: TRUE
---



# Introduction
In this document, we try to connect story-making questions with two other ideas:

a) a Variable in a dataset
b) A computed Quantity or a Visual based on one or more Variables
c) Add to our story-line, questions and wonderments...

The idea is to get the intuition behind data, and develop the right questions and hypotheses, that lead to the appropriate direction in exploratory data analysis (EDA) using graphs and charts in R.

# References
1. Daniel Kaplan and Randall Prium, [Formula Interface for ggplot2](https://cran.r-project.org/web/packages/ggformula/vignettes/ggformula.html)
- **Short** Note on `ggformula`. Absolutely must see and work through to get jump-started on `ggformula`. 

2. Data Visualization with R, [Robert Kabacoff](https://rkabacoff.github.io/datavis/)
- Good crisp descriptions of many kinds of graphs, no nonsense book. Available free on the web. 

3. Wickham and Grolemund, [R for Data Science](https://r4ds.had.co.nz/)
 - R Bible. Available free on the web.


# Interrogative Pronouns
These are the usually heard *interrogative pronouns* in English.

Pronoun | Answer| English Character | Kind of Data Variable | R Character
--------| ------|------------------ |-----------------------|------------
What  | A Thing | Subject | Nominal| Categorical
Who   | A Person| Subject | Nominal | Categorical
Where | A Place | Subject/Object    | Nominal | Categorical
Whom  | To A Person   | Object| Nominal | Categorical
Which | A Selection or a Type   | Adjective| Qualitative| Qualitative
How   | A Manner/Method| Adverb           | Qualitative | Qualitative
What Kind (1)| Type or Attribute, from a list     | Number (noun/adj) | Qualitative| Factor
What Kind(2) | Type or Attribute, from a list, with order | Affirmative - Comparative - Superlative...| Qualitative with Scale | Factor with Scale
What Kind(3) | Qualitative with Scale, from a list, with order and an absolute value | - | Ordinal | Factor with Scale with "absolute zero"
How Many| A number| Number (noun/adj) | Quantitative| Quantitative
How Often | A number| Number (noun/adj) | Quantitative| Quantitative

Think of :
 - How long? / Since When? : Time
 - How often? / How seldom? : Frequency
 - ..? 

## Look at the dataset `penguins`

1. What are the variable `names()`?
2. What would be the question you might have asked to obtain that variable?
3. How would you process that variable? 
4. What meta questions could you ask of each variable?
5. Where might the answers take your story?


```r
names(penguins) # Column, i.e. Variable names
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```

```r
head(penguins) # first six rows
```

```
## # A tibble: 6 x 8
##   species island bill_length_mm bill_depth_mm flipper_length_~ body_mass_g sex  
##   <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
## 1 Adelie  Torge~           39.1          18.7              181        3750 male 
## 2 Adelie  Torge~           39.5          17.4              186        3800 fema~
## 3 Adelie  Torge~           40.3          18                195        3250 fema~
## 4 Adelie  Torge~           NA            NA                 NA          NA <NA> 
## 5 Adelie  Torge~           36.7          19.3              193        3450 fema~
## 6 Adelie  Torge~           39.3          20.6              190        3650 male 
## # ... with 1 more variable: year <int>
```

```r
tail(penguins) # LAst six rows
```

```
## # A tibble: 6 x 8
##   species island bill_length_mm bill_depth_mm flipper_length_~ body_mass_g sex  
##   <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
## 1 Chinst~ Dream            45.7          17                195        3650 fema~
## 2 Chinst~ Dream            55.8          19.8              207        4000 male 
## 3 Chinst~ Dream            43.5          18.1              202        3400 fema~
## 4 Chinst~ Dream            49.6          18.2              193        3775 male 
## 5 Chinst~ Dream            50.8          19                210        4100 male 
## 6 Chinst~ Dream            50.2          18.7              198        3775 fema~
## # ... with 1 more variable: year <int>
```

```r
dim(penguins) # Size of dataset
```

```
## [1] 344   8
```

```r
# Check for missing data
any(is.na(penguins))
```

```
## [1] TRUE
```

## Interrogations and Graphs
What sort of measures, and visuals charts can we create with different kinds of variables, taken singly or together? Let us write some simple English descriptions of measures and visuals and see what commands they use in R.

### Quick Intro to `ggformula`
All computations and graphs work with the following structure

- Simple Structure 

`goal( y ~ x | z, data = my_data)`

- Bells and Whistles

`goal( y ~ x | z, data = my_data, `

`                 color = ~ some_variable, `

`                 shape = ~ some_variable, `

`                 size = ~ some_variable)`

Usually safe; unused variables are ignored. But keep your eyes open !!
 
- Still more Bells and Whistles !
 add: `gf_refine(.....)`
 
 Look at the `Using Colours in R.Rmd` file for ideas. 


### Single Qualitative/Categorical Variable

1) Questions: How? Which? What Kind?

2) Measures: No of `levels` / Counts for each `level`
  - `counts` / ` tally` (very similar syntax to ggformula)
  
3) Charts: Bar Chart / Pie Chart / Tree Map 
  - `gf_bar` / `gf_bar %>% coord_polar()` / Find out !!


```r
mosaic::counts( ~ island, data = penguins)
```

```
##   response n_Biscoe n_Dream n_Torgersen
## 1   island      168     124          52
```

```r
mosaic::counts(~ species, data = penguins)
```

```
##   response n_Adelie n_Chinstrap n_Gentoo
## 1  species      152          68      124
```

```r
counts(~ sex, data = penguins)
```

```
## Warning: Excluding 11 rows due to missing data [df_stats()].
```

```
##   response n_female n_male
## 1      sex      165    168
```

```r
mosaic::tally( ~ island, data = penguins)
```

```
## island
##    Biscoe     Dream Torgersen 
##       168       124        52
```

```r
# Curious: what's the difference in output between tally and counts?
# 
gf_bar( ~ island, fill = ~ island, data = penguins)
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Single Cat Var-1-1.png" width="672" />

```r
gf_bar(~ island, fill = ~ sex, data = penguins)
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Single Cat Var-1-2.png" width="672" />



```r
# YOUR TURN
# Run names(penguins) is you have forgotten
counts(~ species, data  = penguins)
```

```
##   response n_Adelie n_Chinstrap n_Gentoo
## 1  species      152          68      124
```

```r
tally( ~ species, data = penguins)
```

```
## species
##    Adelie Chinstrap    Gentoo 
##       152        68       124
```

```r
gf_bar( ~ species, data = penguins)
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Single Cat Var-2-1.png" width="672" />
Storyline: Where do most of the penguins live? On Treasure Island....?
Most of the Penguins live on Biscoe island. And Adelie penguins are most frequent in this dataset.


### Single Quantitative Variable

1) Questions: How many? How few? How often? How much?

2) Measures: max / min / mean / mode / (units)
  - `max()`, `min()`, `range()`, `mean()`, `favstats`
  - `mode()` # non-formula ;-(
  
3) Charts: Bar Chart / Histogram / Density  
    - `gf_bar()` / `gf_histogram()` / `gf_density()` 


```r
range(~ bill_length_mm, na.rm =TRUE, data = penguins) 
```

```
## [1] 32.1 59.6
```

```r
# Note this. Try deleting this option!
favstats(~ bill_length_mm, data = penguins, groups = ~island)
```

```
##      island  min    Q1 median    Q3  max     mean       sd   n missing
## 1    Biscoe 34.5 42.00  45.80 48.70 59.6 45.25749 4.772731 167       1
## 2     Dream 32.1 39.15  44.65 49.85 58.0 44.16774 5.953527 124       0
## 3 Torgersen 33.5 36.65  38.90 41.10 46.0 38.95098 3.025318  51       1
```

```r
gf_histogram(~ bill_length_mm, data = penguins)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_bin).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Single Quant-1-1.png" width="672" />


```r
# YOUR TURN BELOW
gf_density(~ bill_length_mm | island, data = penguins)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_density).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Single Quant-2-1.png" width="672" />

```r
gf_density(~body_mass_g | island, data  = penguins)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_density).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Single Quant-2-2.png" width="672" />

Storyline: Some of those penguins have really big bills. I mean beaks!! 59 cms !!!
Are those the ones with a lot of body mass as well? Bite more, eat more, weigh more...



### Two Variables: Categorical vs Categorical

1) Questions: What Kind(x) vs What Kind(y)? 

2) Measures: Counts and Tallies sliced by Category
    - `counts` , `tally`(very similar syntax to ggformula)
    
3) Charts: Stacked Bar Charts / Grouped Bar Charts / Segmented Bar Chart / Mosaic Chart
    - `gf_bar()` 
    - Use with second Categorical variables to modify `fill`, `color`. 
    - Also try to vary the parameter `position` of the bars. 


```r
counts(~ species | sex , data = penguins)
```

```
## Warning: Excluding 11 rows due to missing data [df_stats()].
```

```
##   response    sex n_Adelie n_Chinstrap n_Gentoo
## 1  species female       73          34       58
## 2  species   male       73          34       61
```

```r
counts(~ species + sex , data = penguins)
```

```
## Warning: Excluding 11 rows due to missing data [df_stats()].
```

```
##   response n_Adelie n_Chinstrap n_Gentoo n_female n_male
## 1  species      152          68      124       NA     NA
## 2      sex       NA          NA       NA      165    168
```

```r
tally(~ species + sex  , data = penguins)
```

```
##            sex
## species     female male <NA>
##   Adelie        73   73    6
##   Chinstrap     34   34    0
##   Gentoo        58   61    5
```

```r
# try other pairs of Categorical variables. Interpret. 
```
Storyline: We have larger numbers of Adelie penguins...



```r
gf_bar(~ species, fill = ~ sex, data = penguins, position = "fill")
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Cat vs Cat-2-1.png" width="672" />

```r
# Try position = "dodge" and "fill"
```

Storyline: तीन पेनगीन। और तुम भी तीन

(Oh never mind!


```r
## YOUR TURN BELOW
gf_bar(~ species, fill = ~ sex, data = penguins, 
       position = "fill") %>% 
  gf_refine(scale_fill_brewer(palette = "Set3"))
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Cat vs Cat-3-1.png" width="672" />



### Two Variables: Quantitative vs Quantitative

1) Questions: How many vs How many? Does this depend upon that?

2) Measures: Correlation / Covariance / T-test / Chi-Square Test for Two Means etc. We won't go too deep into this !!

3) Charts: Scatter Plot / Line Plot / Regression i.e. best fit lines (Remember $ y = mx + c $ and friends?)


```r
#cor(penguins$body, penguins$flipper_length_mm) # Error! Why?
penguins %>% 
  drop_na() %>% # remove entries that are missing (NA)
  mosaic::cor(body_mass_g ~ bill_length_mm, data = .)
```

```
## [1] 0.5894511
```
Storyline: More weight, bigger the flipper, at least 59% of the time. They need to swim...but then there must be some overweight penguins with small flippers? I mean, 59%...



```r
gf_point(body_mass_g ~ flipper_length_mm, data = penguins) # Why do we get a warning?
```

```
## Warning: Removed 2 rows containing missing values (geom_point).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Quant-2-1.png" width="672" />

```r
gf_point(flipper_length_mm ~ body_mass_g, data = penguins) %>% 
  gf_smooth(method = "lm") # Try other methods
```

```
## Warning: Removed 2 rows containing non-finite values (stat_smooth).

## Warning: Removed 2 rows containing missing values (geom_point).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Quant-2-2.png" width="672" />

Storyline: More weight, bigger the flipper...looks like a fairly predictable trend. Is this what Darwin meant?


```r
# YOUR TURN BELOW
```





### Two Variables: Quantitative vs Qualitative

1) Questions: How many vs What Kind? How many vs Which? How many vs How?

We usually do stuff on the Quant Variable, after `grouping` using the Qual variable. (See `Cat vs Cat` above)

2) Measures: Quant Variable summaries, grouped by Categorical variable. 
 - Think!!

3) Charts: Bar Chart using group / density plots by group / violin plots by group / box plots by group
 - `gf_bar` / `gf_density` / `gf_violin` / `gf_boxplot` using Categorical Variable for grouping


```r
penguins %>% 
  group_by(island) %>% 
    gf_density(~ body_mass_g, color = ~island, fill = ~ island, alpha = 0.3, data = .)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_density).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Qual-1-1.png" width="672" />

```r
penguins %>% 
  group_by(species) %>% 
  gf_histogram(~ flipper_length_mm, data = .)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_bin).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Qual-1-2.png" width="672" />

```r
penguins %>% 
  group_by(species) %>% 
  gf_violin(flipper_length_mm ~ species, data = .)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_ydensity).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Qual-1-3.png" width="672" />

Storyline: Hmm....there are significantly different weight distributions among Antarctic penguins...based on the island they live on!
What do they eat?

What else lives there?


```r
#library(dplyr) # this package is part of the tidyverse.
penguins %>% 
  group_by(island) %>% 
  summarise(mean_weight = median(body_mass_g, na.rm = TRUE)) %>% 
# sd, var, median

  gf_col(mean_weight ~ island, color = ~island, fill = ~ island, data = .)
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Qual-2-1.png" width="672" />

Storyline: Why are all heavier penguins are on one island....???

 Let's try one last plot: the Boxplot. ![boxplot](https://rkabacoff.github.io/datavis/img/boxplot.png)



```r
#library(dplyr) # this package is part of the tidyverse.
penguins %>% 
    gf_boxplot(body_mass_g ~ island, color = ~island, data = .,notch = FALSE)
```

```
## Warning: Removed 2 rows containing non-finite values (stat_boxplot).
```

<img src="/labs/02-Pronouns-and-Data_files/figure-html/Quant vs Qual-3-1.png" width="672" />
Storyline: Clearly there is a relationship between Species, Island and BodyWeight. 
How do we discover that?

Ah, welcome to **Predictive Modelling** and **Machine Learning**....



```r
# YOUR TURN HERE
```


