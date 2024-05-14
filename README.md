# Optimal-Hedging
 
In this project we implement optimal delta hedging on SPX 500 options data using the industry-famous stochastic volatility model, the SABR model. We calibrate the SABR model on one-month SPX option time series and evaluated the heding performance of different deltas.

## Reference
Most implementations in this project are based on Bartlett's delta in the SABR model by Hagan and Andrew (2020) and Optimal Delta Hedging by Hull and White (2016).

## Scripts
- `data`: SPX 500 options data from 2023-02-01 to 2023-02-28 from WRDS
- `papers`: a list of papers used for this project
- `presentation`: presentation slides for the project
- `sabr_calibration`: an example of SABR model implementation and calibration 
- `optimal_hedging`: main notebook that implements SABR model and optimal delta hedging

## Overview

### SABR Model
SABR model is a stochastic volatility model given by
$$
\begin{align*}
dF(t) &= \sigma(t)(F(t)+\theta)^{\beta}dW_1(t), F(0)=f\\
d\sigma(t) &= \nu\sigma(t)dW_2(t), \sigma(0)=\sigma_0\\
\end{align*}
$$

where $W_1(t)$ and $W_2(t)$ are two correlated Wiener processes with correlation $\rho$, namely, $dW_1(t)dW_2(t) = \rho dt$.

- F: forward rate
- $\sigma$: volatility of forward rate
- $\nu$: volatility of volatility (volvol)
- $\theta$: shift parameter to avoid negative rates

### Optimal Delta Hedging

The SABR delta is given by
$$\Delta^{\text{SABR}} = \frac{\partial B}{\partial F} + \frac{\partial B}{\partial \sigma}  \frac{\partial \sigma_{\text{imp}}}{\partial F}  $$

The Bartlett's delta further incorporates the adjustment for the implied volatility skew

$$\Delta^{\text{Bartlett}} = \frac{\partial B}{\partial F} + \frac{\partial B}{\partial \sigma} \left( \frac{\partial \sigma_{\text{imp}}}{\partial F} + \frac{\partial \sigma_{\text{imp}}}{\partial \sigma} \frac{\rho \alpha}{C(F_t)}\right) $$

It is shown in Hagan (2019) that the Bartlett's delta is the optimal delta for hedging in the SABR model, which can be approximated by
$$\Delta^{mod}\approx \Delta^{BS}+\text{Vega}^{BS}\times \eta$$

### Hedging Performance Evaluation
In Hull, J., and White (2016), the effectiveness of a hedge is measured by the $Gain$ metric, defined as the percentage reduction in the sum of squared residuals resulting from the hedge, i.e.
$$\text{Gain} = 1- \cfrac{\sum(\Delta f  - \delta_{\text{SABR}}\Delta S)^2}{\sum(\Delta f  - \delta_{\text{BS}}\Delta S)^2}$$

## Results
#### Implied volatility Smile Calibration

<img src="./presentation/imgs/impliedvol.png" width="750">


#### BS Delta (Blue), SABR Delta (Red), Bartlett's Delta (Green)
<img src="./presentation/imgs/comparison.png" width="750">

#### Bartlett's Delta for Different Maturities
<img src="./presentation/imgs/bartlett.png" width="750">


#### Hedging Parameters Evolution
<img src="./presentation/imgs/param.png" width="750">


#### Hedging Gain for Bartlett's Delta
<img src="./presentation/imgs/gain_sse.png" width="750">



#### SABR Delta vs. Bartlett's Delta

<img src="./presentation/imgs/gain_relative.png" width="750">



## Short Conclusion
- SABR model calibrates the implied volatility smile of SPX 500 options data extremely well.
- Both SABR delta and Bartlett’s delta are effective in hedging the options, much better than Black-Scholes delta.
- Bartlett’s delta performs slightly but consistently better than SABR delta. 
