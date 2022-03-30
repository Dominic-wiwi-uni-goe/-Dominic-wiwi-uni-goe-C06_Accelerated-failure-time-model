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



## Sources:
- Mills, M. (2011). Introducing survival and event history analysis. ISBN: 9781848601024
