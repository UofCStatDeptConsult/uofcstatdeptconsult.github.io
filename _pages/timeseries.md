---
layout: page
title: Time Series Analysis
permalink: /stat/timeseries/
---

*Reference: Time Series Analysis and Its Applications with R Examples* by *Shumway* and *Stoffer*.

# Introduction

Time Series Analysis is an expansive area of modern statistical research, 
comprising of topics far beyond the scope of regular consulting endeavours. 
Here we present some basic concepts and theory that a practicioner might use to analyze temporal data. 

In short, time series data is an experimental data collected over time-points. 
The main statistical handicap in analysing such data is the inherent temporal dependence between the observations; 
usually we might want to assume for a data 
$$X_1, X_2, \cdots, X_n$$ to have $$X_i$$ and $$X_j$$ independent for $$i \neq j$$, 
**but that is no longer a valid assumption when $$X_1, X_2, \cdots, X_n$$ is a time series data**. 
Thus, the first step towards analysing any time series data is by plotting the data. 
It can be one in ```R``` using ```plot.ts``` function. 

# Stationary Time Series

The dependence can be arbitrary in a time-series; but most often we will work with a **Weakly Stationary Time Series**, 
which incorporates a special type of dependence. In a weakly stationary time series, 
the mean of the time series $$\mu_t=\mathbb{E}(X_t)$$ does not depend on $$t$$. 
Further, the covariance between $$X_t$$ and $$X_{t+h}$$ (called the **autocovariance**), 
is a function of only the absolute value of $$h$$. That is, if, <br>

$$
\gamma(h, t)=\mathbb{E}[(X_t-\mu_t)(X_{t+h}-\mu_{t+h})]
$$
<br>

then $$\mu_t$$ and $$\gamma(h,t)$$ are constant as a function of $$t$$. 
One can similarly define the **Autocorrleation Function** (ACF).

### Partial Autocorrelation

The partial autocorrelation of lag $$h$$ is <br>

$$
\phi_{hh}= Corr(X_t- \hat{X}_t, X_{t+h}-\hat{X}_{t+h} )
$$
<br>

where $$\hat{X}_{t+h}$$ and $$\hat{X}_{t}$$ are calculated by regressing them on $$X_{t+1}, \cdots, X_{t+h-1}$$.


# Different types of Stationary Time Series

The most common types of stationary time series one comes across are AR($$p$$) and MA($$q$$) processes. 
Both of them are linear time series. 

## AR(p) Series

AR stands for **A**utoregressive **P**rocess, and $$p$$ denotes its order. 
Mathematically, such a process will be represented as
<br>

$$
X_t= a_1 X_{t-1} + a_2 X_{t-2} + \cdots a_p X_{t-p} + \varepsilon_{t}
$$
<br>

where $$\varepsilon_t$$ are white noise process, i.e IID with mean 0 and some finite variance. 
The name "Autoregressive" is self-explanatory, as the process involves regressing on past $$p$$ values. 
It is important that the polynomial $$\phi(x)=1-a_1 x - \cdots a_p x^p$$ has no roots inside the unit circle. 
Otherwise we run into problems like the time series depending on future values (which we want to avoid), 
called *Non-Causality* in the literature. <br>

The autocorrelation function of AR(1) model is proportional to $$a_1^h$$, where we require $$|a_1|<1$$. 
In general, the ACF of AR models decreases exponentially. 
In contrast, the PACF function for AR(p) model, $$\phi_{hh}=0$$ for $$h>p$$. 

## MA(q) Series

Another popular stationary series is **M**oving **A**verage process of order $$q$$, written as <br>

$$
X_t=\varepsilon_t + b_1 \varepsilon_{t-1} + \cdots + b_q \varepsilon_{t-q}
$$
<br>

For MA(q), ACF function is 0 for lags larger than $q$, but the PACF function tapers off exponentially. 

### ARMA(p,q) Series

Of course, we can combine the AR and MA models into an ARMA($$p,q$$) model:
<br>

$$
X_t-a_1 X_{t-1} - a_2 X_{t-2} - \cdots a_p X_{t-p} = \varepsilon_t + b_1 \varepsilon_{t-1} + \cdots + b_q \varepsilon_{t-q}
$$
<br>

or in more compact autoregressive operator notation with $$B X_t=X_{t-1}$$, we have
<br>

$$
a(B)X_t=b(B)\varepsilon_t
$$
<br>

where $$a(x)=1-a_1x - \cdots a_p x^p$$, 
and $$b(x)=1+b_1 x + \cdots b_q x^q$$ are two polynomials with no roots inside the unit circles. 


## Non-linear Time Series

Some other standard non-linear process are ARCH, GARCH, Volterra processes etc. 
Standard Volterra process is stationary. 
ARCH and GARCH are used heavily in financial data to explain volatility, 
where often the variance of the data also changes with the time-point. 
These will be added in the future. 

### Sample ACF and PACF plots

A very useful tool to check if there is indeed temporal dependence between the time-stamped observations, 
is to look at the ACF and PACF functions and plot it as a function of lag. 
Due to the difference in behavior of ACF and PACF for AR and MA models, 
such plots can be quite illuminative. The following table, 
taken from **Shumway** and **Stoffer**'s *Time Series Analysis and Its Applications with R Examples*, 
summarizes the usual behaviour. 
<br>

$$
\begin{aligned}
&\text { Table 1. Behavior of the ACF and PACF for ARMA models }\\
&\begin{array}{cccc}
\hline \hline & \operatorname{AR}(p) & \operatorname{MA}(q) & \operatorname{ARMA}(p, q) \\
\hline \text { ACF } & \text { Tails off } & \begin{array}{c}
\text { Cuts off } \\
\text { after lag } q
\end{array} & \text { Tails off } \\
\text { PACF } & \begin{array}{c}
\text { Cuts off } \\
\text { after lag } p
\end{array} & \text { Tails off } & \text { Tails off } \\
\hline
\end{array}
\end{aligned}
$$
<br>

The ```acf1``` and ```acf2``` functions in **astsa** package plots the sample ACF and PACF plots in R. 


# Decomposing a Time Series

Often a time series will have three components. 
The first component is called **Trend** ($$\mu_t$$), 
the second component is called **Seasonality** ($$s_t$$), 
and the third component is called **Remainder** ($$z_t$$). 
Usually we work with an *additive* model , implying our time series $$x_t$$ is decomposable as
<br>

$$
x_t=\mu_t+s_t+z_t
$$
<br>

There can obviously be multiplicative models, 
but those can be converted to an additive model using a log-transformation. 
In the following we briefly discuss how to decompose a time series into each component.

## Detecting Trend

### Moving Average Filter

Most common method of detecting trend is **Moving Average** method. 
In particular, if $$x_t$$ represents the observations, then
<br>

$$
m_t=\sum_{j=-k}^k a_j x_{t-j}
$$
<br>

where $$a_j=a_{-j} \geq 0$$ and $$\sum_{j=-k}^k a_j=1$$ is a symmetric moving average of the data. 
The "moving average'' term comes from the moving window of $$[-k,k]$$ in the definition of the filter. 
```filter``` function in *astsa* package fits moving average smoothing method. 
The *filter=* argument specifies the weights that are used. 


### Kernel Smoothing

Kernel filter is defined as 
<br>

$$
m_t=\sum_{i=1}^n w_i(t) x_i
$$
<br>

where
<br>

$$
w_i(t)=K\left(\frac{t-i}{b}\right) / \sum_{j=1}^n K\left(\frac{t-j}{b}\right)
$$
<br>

where $$K(\cdot)$$ is a kernel function. 
Usually one uses Gaussian kernel defined by $$K(x)=\frac{1}{\sqrt{2\pi}}e^{-x^2/2}$$. 
The ```ksmooth``` function implements kernel smoothing. 

## Detecting Seasonality 

To get seasonal component of the time series, 
first we detrend the time series by estimating trend , 
$$\hat{\mu}_t$$ and subtracting it from the original time series ($$y_t=x_t-\hat{\mu}_t$$). 
A naive method of finding seasonality is simply take some average of the $$y$$ series for that particular series. 
One can adjust the seasonal values so as to make them zero mean. 
More complicated methods which are out of the scope of discussion here, 
like SEATS method, are implemented in ```seas``` function of **seasonal** package.

## Modelling the Remainder Term

If we have successfully detected trend ($$\hat{\mu}_t$$) 
and seasonality ($$\hat{s}_t$$) in our time series $$x_t$$, 
our remainder part is $$z_t=x_t - \hat{\mu}_t - \hat{s}_t$$. 

If $$z_t$$ seems sufficiently stationary, 
one can fit an ARMA$$(p,q)$$ model as we have discussed above. One can also fit ARIMA models.

### ARIMA(p,d,q) Models

Sometimes it can happen that $$z_t$$ is not explained well by an ARMA process, 
however $$(1-B)^d z_t$$ is a good fit for ARMA$$(p,q)$$. 
In that case we say that $$z_t$$ fits an ARIMA$$(p,d,q)$$ model, 
where **I** stands for **I**ntegrated. 
ARIMA (and by extension ARMA) models are implemented 
by functions ```arima``` and ```sarima``` in **astsa** package, 
and **forecasting is done by ```predict.arima``` and ```sarima.for``` function**. 

### Choosing between ARIMA and ARMA

In order to identify if differencing is required, one first plots the time series. 
If the variability increases with data, 
usually first or second order differencing is employed 
before looking at the ACF and PACF plots to decide on the ARMA parameters. 
By default ```arima``` will use a cross-validation approach to identify the parameters for you, 
but it's often instructive to use your own judgement to have better interpretability of the results. 
In particular, often over-differencing can induce spurious dependence between lags. 

### Testing for Sample Autocorrelation

After fitting the remainder terms, one needs to see if the fit is good. 
One way to do that is to test if the errors are indeed independent 
(i.e if all the dependency have been accounted for in our model); 
equivalently, we should test if $$\rho_e(h)=0$$ for all lag $$h$$. 
We do that using **Ljung-Box-Pierce** Test.
<br>

$$
Q=n(n+2) \sum_{h=1}^H \frac{\hat{\rho}_e^2(h)}{n-h}
$$
<br>

Under $$H_0:\rho_e(h)=0$$ for all $$h$$, $$Q$$ 
follows approximately $$\chi^2_{H-p-q}$$ for an ARIMA$$(p,d,q)$$ fit for large $$n$$. 

## SARIMA model

One can fit for seasonality and remainder term together 
using a seasonal ARIMA$$(P,D,Q)_s \times (p,d,q)$$ model, which is given by
<br>

$$
\Phi_P\left(B^s\right) \phi(B) \nabla_s^D \nabla^d x_t=\Theta_Q\left(B^s\right) \theta(B) w_t,
$$
<br>

where $$w_t$$ is the usual Gaussian white noise process. 
The ordinary autoregressive and moving average components are represented by polynomials 
$$\phi(B)$$ and $$\theta(B)$$ of orders $$p$$ and $$q$$, respectively. 

The seasonal period is $$s$$. 
The seasonal autoregressive and moving average components by $$\Phi_P\left(B^s\right)$$ 
and $$\Theta_Q\left(B^s\right)$$ of orders $$P$$ and $$Q$$ 
and ordinary and seasonal difference components by $$\nabla^d=(1-B)^d$$ and $$\nabla_s^D=\left(1-B^s\right)^D$$.

SARIMA model can be fit by ```sarima``` function in **astsa** package. 




