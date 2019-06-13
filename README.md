t-SNE Clustering Visualization for Causal Forest
================

tSneClstGRF is a R package that generates an interactive shiny interface for easily visualizing and exploring heterogeneous causal effect modeled by causal forest proposed in Athey et al. (2018).

Installation
------------

To install this package in R from the github repository, use:

``` r
if(!require(devtools)) install.packages("devtools") # If not already installed
devtools::install_github("jiongyi-cao/tSneClstGRF")
```

Usage
-----

A basic workflow contains major three steps:

-   read causal forest object
-   run t-SNE
-   generate shiny interface

A sample OHIE dataset analysis can be performed using the following code.

``` r
library(grf)
library(tSneClstGRF)
#load dataset
dt <- data("ohieData")
itt_control <- c('ddddraw_sur_2','ddddraw_sur_3','ddddraw_sur_4','ddddraw_sur_5',
                 'ddddraw_sur_6','ddddraw_sur_7','dddnumhh_li_2','dddnumhh_li_3',
                 'ddddraXnum_2_2','ddddraXnum_2_3','ddddraXnum_3_2','ddddraXnum_3_3',
                 'ddddraXnum_4_2','ddddraXnum_4_3','ddddraXnum_5_2','ddddraXnum_5_3',
                 'ddddraXnum_6_2','ddddraXnum_6_3','ddddraXnum_7_2','ddddraXnum_7_3')
x_htr <- c('age','female_list','race_white_12m','zip_msa_list','smk_ever_12m','first_day_list','edu_1','edu_2','edu_3','edu_4')

#fit causal forest
outcome <- "cost_any_oop_12m"
x <- dt %>% dplyr::select(x_htr,itt_control)
y <- as.numeric(dt[,outcome])
w <- as.numeric(dt$treatment)
c.forest<- causal_forest(x,y,w,clusters = dt$household_id)

#read causal forest object
my_cf <- read.cf(c.forest, x_htr)
#run t-SNE Clustering algorithm
tsne_obj <- run.analsis(my_cf,3000)
#generate shiny application
create.shiny(tsne_obj)
```

References
----------

Athey Susan, Julie Tibshirani, and Stefan Wager. <a href="https://arxiv.org/abs/1610.01271">Generalized Random Forests.</a> <i>Annals of Statistics (forthcoming)</i>, 2018