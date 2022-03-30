# Chapter 6
## Accelerated failure time model

Event history analysis, also known as time-to-event analysis, or survival analysis, summarizes statistical models concerned with the probability and the duration until a given event occurs (Mills, 2011). An event is formally defined as the instantaneous transition from the origin state to the destination state (Oud, 2014), reflecting a broad conceptualization transferable to a large scope of scenarios in production and operations management. In event history analysis, one essential methodological differentiation relates to the assumption regarding the effect of covariates: While proportional hazard models assume that covariates have a constant impact on the hazard function, the AFTM assumes an accelerating or decelerating impact (Greene, 2018). 

In a nutshell, AFTM are regression models with different likelihood estimators than ordinary least-square regressions and use event time or survival time as the dependent variable (Mills, 2011). We transfer this logic to the POM context and propose an AFTM to estimate the impact of independent variables in manual material handling, e.g., travel distance or product weight, on throughput time as the dependent variable. This is inspired by the landmark paper of Batt and Gallino (2019). In AFTM, 𝑇 represents the time-to-event or survival time which we translate to the general LSCM context as throughput time. 𝑇 represents a random variable equal to or greater than zero (𝑇 ≥ 0). In parametric survival models, 𝑇 follows a particular distribution, e.g., exponential, Weibull, logistic, lognormal, or log-logistic. For methodological details, the reader is referred to the comprehensive overview presented by Mills (2011). The choice for the parametric distribution assumed in the AFTM is made by comparing the model fit for a variety of different distributions though, e.g., the Akaike information criterion (AIC) or the Loglikelihood ratio (LL).

In the proposed econometric model, the throughput time is denoted as 𝑇, and it is defined as the elapsed time between the beginning and end of a work process performed by one operator. Because AFTM are log-linear regression models for 𝑇, the basic model is a linear function of the covariate(s) in the form of 𝑌 = log(𝑇) (Mills, 2011). We define 𝑛 independent predictor variables 𝑥𝑛 and their corresponding regression coefficients 𝛽𝑛. Additionally, ε represents the error term assumed to have a particular parametric distribution.

![image](https://user-images.githubusercontent.com/102478331/160792364-bdc89611-7956-45a4-98c2-706477453cbc.png)

The coefficient in the parametric AFTM can be interpreted as follows: A positive coefficient indicates that the log duration time increases, leading to longer duration
times. A negative coefficient indicates that the log duration time decreases, leading to shorter duration times. The regression coefficients 𝛽𝑛 are parametrized by the following transformation (Mills, 2011):

![image](https://user-images.githubusercontent.com/102478331/160792418-e03da2f3-f2ec-41cf-b95a-5acffca498db.png)

### What we will learn in this chapter

In this chapter we will learn how to apply AFTM to a large order picking dataset. We will stepwise add independent variables to the model in order to explain the time per pick as the dependent variable (the survival object).

### Loading the relevant packages in R

```
library("corrplot")
library("data.table")
library("eha")
library("flexsurv")
library("fs")
library("ggplot2")
library("ggpubr")
library("JM")
library("lattice")
library("plyr")
library("Rcpp")
library("reshape2")
library("readxl")
library("rms")
library("splines")
library("stargazer")
library("survival")
library("survminer")
library("tidyverse")
library("sjPlot")
library("sjmisc")
library("sjstats")
library("stats")
library("lme4")
library("haven")
library("MuMIn")
library("devtools")
library("corrplot")
library("data.table")
library("eha")
library("flexsurv")
library("fs")
library("ggplot2")
library("ggpubr")
library("JM")
library("lattice")
library("PerformanceAnalytics")
library("plyr")
library("Rcpp")
library("readxl")
library("reshape2")
library("rms")
library("splines")
library("stargazer")
library("survival")
library("survminer")
library("tidyverse")
library("mosaicData")
library("dplyr")
library("treemapify")
library("scales")
library("ggcorrplot")
library("factoextra")
library("superheat")
library("texreg")
library("data.table")
library("devtools")
library("plm")
library("corrplot")
library("data.table")
library("eha")
library("flexsurv")
library("fs")
library("ggplot2")
library("ggpubr")
library("JM")
library("lattice")
library("plyr")
library("Rcpp")
library("reshape2")
library("readxl")
library("rms")
library("splines")
library("stargazer")
library("survival")
library("survminer")
library("tidyverse")
library("sjPlot")
library("sjmisc")
library("sjstats")
library("stats")
library("lme4")
library("haven")
library("MuMIn")
library("devtools")
library("corrplot")
library("data.table")
library("eha")
library("flexsurv")
library("fs")
library("ggplot2")
library("ggpubr")
library("JM")
library("lattice")
library("PerformanceAnalytics")
library("plyr")
library("Rcpp")
library("readxl")
library("reshape2")
library("rms")
library("splines")
library("stargazer")
library("survival")
library("survminer")
library("tidyverse")
library("mosaicData")
library("dplyr")
library("treemapify")
library("scales")
library("ggcorrplot")
library("factoextra")
library("superheat")
library("texreg")
library("sjPlot")
library("sjmisc")
library("sjstats")
library("stats")
library("lme4")
library("haven")
library("MuMIn")
library("sjPlot")
library("sjmisc")
library("sjstats")
library("stats")
library("lme4")
library("haven")
library("MuMIn")
library("devtools")
library("plm")
library("readxl")
library("dplyr")
library("vtable")
library("plm")
library("PerformanceAnalytics")
library("tidyverse")
library("corrplot")
library("Hmisc")
library("stargazer")
library("dplyr")
library("psych")
library("tidyverse")
library("corrplot")
library("data.table")
library("eha")
library("flexsurv")
library("fs")
library("ggplot2")
library("ggpubr")
library("JM")
library("lattice")
library("PerformanceAnalytics")
library("tidyverse")
library("Rcpp")
library("readxl")
library("reshape2")
library("rms")
library("splines")
library("stargazer")
library("survival")
library("survminer")
library("tidyverse")
library("multtest")
```

### Application of AFTM for order picking

(1) We load the dataset `BON1` into R and then we have a look at the first lines. 

```
view(BON1)
```

(2) We define the time per pick in seconds as the dependent variable and survival model.

```
PICKINGTIME <- as.matrix(BON1['picktime']) 
```

(3) We define the independent variable of interest. In our case, this is the transport unit used in a warehouse for deep-freeze and perishable items.

```
unit <- as.matrix(BON1['unit']) 
```

(4) Finally, we define several control variables in our model.

```
PICKER       <- as.matrix(BON1['MDENR'])                #7
DIST         <- as.matrix(BON1['distance'])             #32
WEIGHT       <- as.matrix(BON1['weight'])               #28
VOL          <- as.matrix(BON1['volume'])               #29
CUM_PICKS    <- as.matrix(BON1['AUFTRAGSPOS'])          #6
PICKS        <- as.matrix(BON1['ANZ_PICK'])             #12
LEV          <- as.matrix(BON1['level'])                #22
ART_PACKAGE  <- as.matrix(BON1['packes_SKU'])           #25
Experience   <- as.matrix(BON1['picker_experience'])    #31
```

- `PICKER`: The anonymous identification number of the order picker.
- `DIST`: Distance travelled from pick location to pick location.
- `WEIGHT`: Weight in kg per SKU picked.
- `VOL`: Volume in liters per SKU picked.
- `CUM_PICKS`: Number of cumulative picks within a batch at the point of a pick.
- `PICKS`: Number of picks from the respective pick location.
- `LEV`: Level of pick location which can be the ground level (level 0=ground level) or the chest level (level 1=chest level).
- `ART_PACKAGE`: Number of primary packages in one stock keepting unit.
- `Experience`: The cumulative number of picks for one order picker in the respective dataset.

(5) We formulate the AFTM. ´AFTM01´ is the null model where we only integrate the control variables while we integrate the independent variable of interest in ´AFTM02´. Herein, we only allow one regression line for the entire model.

```
AFTM01<-survreg(
  formula = Surv(PICKINGTIME) ~  VOL + WEIGHT + DIST + CUM_PICKS + PICKS + LEV + ART_PACKAGE + Experience, 
  data = BON1, 
  dist = "loglogistic")

AFTM02<-survreg(
  formula = Surv(PICKINGTIME) ~  unit + VOL + WEIGHT + DIST + CUM_PICKS + PICKS + LEV + ART_PACKAGE + Experience, 
  data = BON1, 
  dist = "loglogistic")
```

(6) We formulate the AFTM. ´AFTM10´ is the null model where we only integrate the control variables while we integrate the independent variable of interest in ´AFTM11´. Herein, we only allow one regression line per order picker identification number.

```
AFTM10<-survreg(
  formula = Surv(PICKINGTIME) ~  VOL + WEIGHT + DIST + CUM_PICKS + PICKS + LEV + ART_PACKAGE + Experience + strata(PICKER), 
  data = BON1, 
  dist = "loglogistic")

AFTM11<-survreg(
  formula = Surv(PICKINGTIME) ~  unit + VOL + WEIGHT + DIST + CUM_PICKS + PICKS + LEV + ART_PACKAGE + Experience + strata(PICKER), 
  data = BON1, 
  dist = "loglogistic")
```

(7) We create an output table for the AFTM. 

```
stargazer(AFTM01, AFTM02, AFTM10, AFTM11,
          type = "html", 
          digits=4,
          order=c(""),
          dep.var.labels=c("Survival object of picking time (sec.)"),
          no.space=TRUE,
          summary=TRUE,
          single.row=TRUE,
          rownames=TRUE,
          align=TRUE,
          out="D:/Dokumente/203_Transport unit deep frozen_IJLRA/02_R Results/with and without subsets_randompicker.html",
          flip = FALSE)
```

![image](https://user-images.githubusercontent.com/102478331/160859649-64780352-0de8-4791-849b-a53f92da50a2.png)


### Interpretation of estimates

![image](https://user-images.githubusercontent.com/102478331/160800944-7c47428e-6fc9-48df-8666-924f1ae74621.png)

Source: https://methods.sagepub.com/images/virtual/introducing-survival-and-event-history-analysis/p131-1.jpg

### Distribution of dependent variable and parameterization

![image](https://user-images.githubusercontent.com/102478331/160793098-6e3c0564-41fb-40bc-afe2-54a0f369e56c.png)

Source: https://methods.sagepub.com/images/virtual/introducing-survival-and-event-history-analysis/p118-1.jpg

## Sources:
- Mills, M. (2011). Introducing survival and event history analysis. ISBN: 9781848601024
- https://methods.sagepub.com/images/virtual/introducing-survival-and-event-history-analysis/p131-1.jpg
- https://methods.sagepub.com/images/virtual/introducing-survival-and-event-history-analysis/p118-1.jpg


