Executive Summary

This project aimed to use Heston (1993) volatility Model (at different maturity) to price an Asian option on
SM stock data for a client under different approaches. This project consist of (3) step. A total of 250 trading
days was assumed for a year. A constant risk free rate of 15% and constant underlying price of 232.90 was
used throughout the project. Step 1a involved the calibration of the Heston (1993) stochastic volatility
model (without jump component) to market prices of short-term European call options with 15-day maturity
on SM stock using the Lewis (2001) Fourier transform-based pricing approach. This calibration yield the
following parameters values: the mean reversion rate (kappa=0.0100), the long-run variance
(theta=0.0010), the volatility of volatility (sigma=2.000), the correlation between the asset and variance
processes (rho=-0.9990), and the initial variance (v0=0.0620).
In step 1c, the client demanded that we use 20 days to maturity (with the calibrated parameters of Heston)
to obtain a fair price of the Asian option using Monte Carlo simulation. The final amount for the client to
pay (with the 4% bank charges) was found to be $5.80.
Step 2a focuses on calibration of full Bates (1996) model under the Lewis approach. The following
parameter values were obtained for the jump Merton component of Bates model: drift rate (mu = -0.500),
jump intensity (Lambda = 0.000) and the standard deviation of jump size (sigma = 0.000).
In 2b, we employed the Carr-Madan (1999) pricing method, which utilizes the Fast Fourier Transform
(FFT). This technique was found to be more efficient for calibration as it can price a wide range of options
simultaneously. Lambda, mu and delta was found to be 1.0000, -0.0903 and 0.2520 respectively.
Step 3 was for modelling interest rate. In 3a, we calibrate the Cox-Ingersoll-Ross (CIR) interest rate model
to the current Euribor term structure to provide insights on future interest rate movements. The calibrated
parameter values were kappa (k = 0.1706), theta (θ = 0.0100), sigma (σ = 0.0584). In 3b, we performed
Monte Carlo simulation of CIR Euribor rate and obtain a vector of non-central chi-square inverse function
by the use of our computed degree of freedom and the non-central parameters. We used 95% confidence
interval to compute the lower and the upper bound expected rate.

Step 1: Short-Maturity Option Pricing

This step focuses on a client's initial request for a short-term derivative. We calibrate the Heston model to
market data and then use it to price a custom Asian option.

1a. Heston Model Calibration using Lewis (2001) Approach 

In this task, we calibrated the Heston (1993) stochastic volatility model to market prices of short-term
European call options with 15-day maturity on SM stock using the Lewis (2001) Fourier transform-based
pricing approach. The goal was to determine optimal values for the Heston parameters by minimizing the
mean squared error (MSE) between the model-generated and market-observed option prices.
The dataset included strike prices, option prices, time to maturity, and option types. We filtered this data to
isolate call options with exactly 15 days to maturity and converted that maturity into years by assuming 250
trading days per year, resulting in a value of 0.06 years. Using the Lewis (2001) pricing formula, which is
derived via Fourier transform inversion techniques, we calculated theoretical option prices under the Heston
framework.
The model includes five key parameters: the mean reversion rate (denoted as kappa), the long-run variance
(theta), the volatility of volatility (sigma), the correlation between the asset and variance processes (rho),
and the initial variance (v0). Calibration was performed using the L-BFGS-B optimization algorithm with
bounds to ensure numerical stability.

Calibrated Parameters:

● kappa = 0.0100

● theta = 0.0010

● sigma = 2.0000

● rho = -0.9990

● v0 = 0.0620


Although the optimization algorithm converged, the fitted option prices deviated significantly from the
actual market prices. The discrepancy is evident in the figure included below. While the market prices for
options stayed below $15, the model-generated prices were unrealistically high, ranging from around $160
to $220. This suggests that the optimizer may have reached the edge of the parameter bounds, especially
since rho and sigma hit their respective limits. This boundary behavior indicates a need for better-informed
initial parameter estimates or stricter parameter constraints.
This divergence also points to possible numerical issues inherent in the Lewis (2001) method or limitations
of the Heston model itself in accurately capturing the price behavior of very short-term options.
Conclusion: Despite its theoretical strengths, the Lewis (2001) method in this case produced poor
calibration results. The misalignment between market and fitted prices emphasizes the need for better 
calibration strategies.

As a next step, we will explore the Carr-Madan (1999) FFT-based approach in Step
1(b) to assess whether it provides a more accurate and stable fit for short-dated options.

Figure 1 below displays the comparison between market prices and the Heston model-fitted prices using
the Lewis (2001) approach for 15-day European call options. The discrepancy in pricing performance is
clearly visible.

<img width="863" height="547" alt="image" src="https://github.com/user-attachments/assets/617f6b0f-4c4c-4ad4-8d2e-29d332eda856" />

Figure 1: Comparison of Market Prices and Fitted Prices from Lewis (2001) Heston Model
Calibration for 15-Day Maturity Options

1c. Asian Option Pricing 

In this part, the client is interested in the fair price of an ATM Asian option with 20 days to maturity. With
all other parameters being the same as earlier stated in 1a, we used Monte Carlo method in a risk neutral
setting to simulate 25,000 paths that the price of the instrument can possibly follow

<img width="1470" height="547" alt="image" src="https://github.com/user-attachments/assets/481dea36-364e-412d-9f56-39f7861e4aad" />

Figure 2: 200 possible paths that the price and volatility of the 20 days to maturity option could follow
under Monte Carlo.

The price of the instrument is the average payoff of all the generated prices. For each potential path, we
calculated the option's potential payoff by comparing the average stock price to the agreed-upon strike
price.

● Results:

○ The "fair price" of the option was calculated to be $5.58.

○ The 4% bank fee was calculated to be $0.22.

○ The final amount for the client to pay is $5.80.

Step 2a: Mid-Maturity Option Pricing

In this step, the client's changed to a longer maturity of 60 days. This step involves calibrating a more
complex model (Bates model with jumps) for pricing a standard European option.
Calibration process: Using the earlier calibration of Heston (1996) parameters, we proceeded by calibrating
the parameters of jump components of Merton (1976) Model. We employed Tikhonov regularization
technique in implementing the regularization of the error function. With this technique, a penalty term was
imposed to the local optimization error function. We rely on the minimum squared error (MSE) function
for our optimization.

In order to scan the boundaries and the initial parameters of the jump parameters (lambda, mu, sigma) for
sensible region, we run through with the brute force and then dig deeper for convex minimization with
fmin to obtain the values of the parameters.

<img width="790" height="590" alt="image" src="https://github.com/user-attachments/assets/a8d27b15-1712-4681-9d66-e39598299abe" />


Figure 3: A Chart showing the variation of market prices and model prices with their strike prices at 60
days maturity using Bates (1996) model with jumps

The table below shows the parameters of the Bates (1996) model.

 Parameters   =   Values
 
 Kappa_v      =   0.010

 Theta_v      =   0.001

 Sigma_v      =    2.000

 Rho          =   -0.9

 V0          =  0.062

 Lambda  = 0.000

 Mu  =  -0.500

 Delta  =   0.000

Conclusion: 

The calibrated Bates model (with Heston stochastic Volatility and jumps) provides a better fit
to the market data for both call and put options. The model slightly captures the curvature of the volatility
smile implied by the market prices, demonstrating its effectiveness for this dataset over the Heston model
under Lewis (2001) approach earlier demonstrated in step 1a.

2b. Bates Model Calibration - Carr-Madan (Member A)

Objective: 

The primary goal of this task was to calibrate the Bates (1996) stochastic volatility model with
jumps to the observed market prices for SM Energy options with a 60-day maturity.
Methodology:


1. Model Selection: The Bates model was chosen as it extends the Heston model to account for
sudden, large price movements (jumps), which are a known feature of equity markets. This adds
three parameters to the standard Heston model: jump intensity (lambda), mean jump size (mu), and
jump volatility (delta).

3. Pricing Engine: We employed the Carr-Madan (1999) pricing method, which utilizes the Fast
Fourier Transform (FFT). This technique is highly efficient for calibration as it can price a wide
range of options simultaneously, significantly speeding up the optimization process compared to
methods like Monte Carlo simulation.

5. Error Minimization: The calibration was performed by minimizing the Mean Squared Error
(MSE) between the prices generated by our model and the actual prices observed in the market.
The optimization was applied to both call and put options simultaneously to ensure a robust fit
across the entire volatility smile. Put prices were derived from call prices using the put-call parity
relationship.

Results: 

The optimization algorithm successfully converged to a set of stable parameters with a final MSE
of 0.354019. The calibrated parameters are:

Parameter Value

Kappa (κ)  = 5.0000

Theta (θ) =  0.0463

Sigma (σ) = 1.0000 

Rho (ρ) =  -0.9900

V0 = 0.0100

Lambda(λ) = 1.0000

Mu (μ) -0.0903

Delta (δ) 0.2520

The resulting values are financially sensible; for instance, the strong negative correlation (rho) of -0.99
reflects the well-known leverage effect where volatility tends to increase as stock prices fall.

Fit Quality:

As shown in the graphs below, the calibrated Bates model provides an excellent fit to the
market data for both call and put options. The model successfully captures the curvature of the volatility
smile implied by the market prices, demonstrating its effectiveness for this dataset.

<img width="1790" height="719" alt="image" src="https://github.com/user-attachments/assets/3e3d320a-1ff4-4d7e-b499-0417ce83e703" />


Step 3: Interest Rate Modeling (Team Task): 
This step addresses the risk of future interest rate changes by modeling the term structure using the CIR model.

3a. CIR Model Calibration

Objective: The goal of this task was to calibrate the Cox-Ingersoll-Ross (CIR) interest rate model to the
current Euribor term structure to provide insights on future interest rate movements.

Methodology:

1. Term Structure Construction: We began with the provided market Euribor rates for maturities
ranging from 1 to 12 months. These discrete points represent the current yield curve.

2. Interpolation:
  
To create a continuous and smooth yield curve, which is necessary for a robust
calibration, we used the cubic spline interpolation method. This technique fits a series of
piecewise cubic polynomials to the market data, ensuring a smooth transition between points. We
interpolated to generate a high-frequency term structure with weekly data points over a one-year
horizon.

3. Model and Error Function:
   
The CIR model is a mean-reverting model for short-term interest
rates. We used its closed-form solution for zero-coupon bond prices to derive a theoretical yield
curve. The calibration was then set up as an optimization problem to find the CIR parameters—
kappa (κ), the speed of mean reversion; theta (θ), the long-run mean rate; and sigma (σ), the
volatility—that minimized the Mean Squared Error (MSE) between the model-generated yields and
the interpolated market yields.

4. Feller Condition:

A crucial constraint, the Feller condition (2κθ ≥ σ²), was enforced during
optimization. This ensures that the interest rate process in the model will not become negative,
which is a key theoretical advantage of the CIR model.

Results:

The optimization algorithm successfully converged with an extremely low final MSE of
0.00000003, yielding the following parameters for the CIR model:

Parameter Value

Kappa (κ) 0.1706

Theta (θ) 0.0100

Sigma (σ) 0.0584

Fit Quality: 

The accompanying graph visually demonstrates the quality of the calibration. The calibrated
CIR model's yield curve provides a very close fit to the interpolated market curve. This indicates that the
CIR model, with the parameters we found, is effective at describing the current term structure of Euribor
rates. These parameters can now be confidently used for the Monte Carlo simulation in the next step.

<img width="1020" height="704" alt="image" src="https://github.com/user-attachments/assets/9eb636b1-a418-4ad9-91d1-0fd988b1ceff" />

3b. Monte Carlo Simulation of Euribor Rates

● Objective:

After the calibration of CIR model in 3a, we hereby aim to simulate the path of the 12-
month Euribor rate over the next year.

● Method: 

Using the calibrated CIR parameters, we run 100,000 Monte Carlo simulation. We used
the initial rate of 0.015 and assumed a 250 trading days for a year. The random numbers were
generated using the rand library in python. Since we needed to transform the previously generated
uniform random variables into a chi-squared variables, we import and initialize the inverse chisquare function. 
We then create a column vector for the initial interest rate.

● Discussion and Results: 

Using the matrix of the normal random variables and the discretized
equation of the geometric Brownian motion as in Glasserman (2003), we generated the simulated
path and plotted the path as shown in the figure below.

<img width="1181" height="534" alt="image" src="https://github.com/user-attachments/assets/06400e3d-a2a5-42be-82e9-a1f61602da1a" />

Figure 4: Showing the paths of simulated interest rate against trading days.

Confidence Interval: 

The min/max range for the 12-month Euribor at a confidence level 95% as well as
expected in one year is seen in the table below.

<img width="854" height="540" alt="image" src="https://github.com/user-attachments/assets/b90b1822-a2e9-40ad-a2f0-f87ddd2b5e13" />


Figure 5: Simulated 12-months Euribor Rate with 95% confidence Level

Impact on Pricing: 

Discuss how this expected future rate would affect option pricing compared to using
today's rate.

Forecasting Accuracy: 

In order to check whether our forecasts are closely matching the data, we use a
measure of the amplitude of the error to quantify the mean squared error (MSE). As we try to minimize this
error, we are improving the forecasting accuracy of the model.

Efficient risk management: 

The reversion term in the CIR model help to bring the interest rate back to its
equilibrium value if there is any form of disturbance that put them above or below the equilibrium value in
a short run. If the deviation of the rate from the equilibrium value is negative (that is the rate stays above
the equilibrium), the reversion term turns the negative rate back (down) to the equilibrium level. Likewise,
if the rate is positive (that is the rate is below the equilibrium value), the reversion term brings it up to the
equilibrium level.

Limitations of CIR Model

Implementation Cost: 

While CIR interest rate model are important for financial companies trying to
manage risk and price complicated financial products, actually implementing these models can be quite
difficult.

Sensitivity to initial parameters:

The CIR model, in particular, is very sensitive to the parameters chosen by the analyst. During a period 
of low volatility, the CIR can be an incredibly useful and accurate model. 

However, if the model is used to predict interest rates during a timeframe in which volatility extends
beyond the parameters chosen, the CIR will be limited in scope and reliability.

Conclusion: 

This project cover the calibration of option pricing models (such as Heston volatility model
under Lewis (2003) approach, and Bates (1996) Model with Merton jump components under Carr-Madan
approach) and the calibration of Cox Ingersoll Ross (CIR) interest rate model. These calibration was done
to the real market data (SM Energy Company data. In the calibration of Heston model under Lewis, we
considered 15 days to maturity. The calibration of Bates model under Carr Madan approach was done with
respect to 60 days to maturity while in CIR calibration, we considered 1 year time to maturity. Given a
constant risk free rate of 15% and initial underlying price of $232.90, we obtained the fair price of the Asian
option with 20 days to maturity using Monte Carlo method in a risk neutral setting. With 25,000 simulation,
the price was calculated to be $5.80 (including the 4% bank charges). The client however seem not satisfied
with the 20 days to maturity and requested for a 60 days to maturity price under Bates model with jumps.
The parameters of the Bates was calibrated to include five key parameters of Heston: the mean reversion
rate (denoted as kappa), the long-run variance (theta), the volatility of volatility (sigma), the correlation
between the asset and variance processes (rho), and the initial variance (v0) and three(3) parameters of
Merton jump: drift rate (mu = -0.500), jump intensity (Lambda = 0.000) and the standard deviation of jump
size (sigma = 0.000). The CIR interest rate model was also calibrated to obtain (kappa as 0.1706, theta as
0.0100 and sigma as 0.0584). We finally used 95% confidence level to simulate 12 months Euribor rate for
the CIR model.

References

Glasserman, Paul (2003) Monte Carlo methods in Financial Engineering. Springer, New York.


Hilpisch, Yves. Derivatives Analytics with Python: Data Analysis, Models, Simulation, Calibration and
Hedging. John Wiley & Sons, 2015.

Bates, David S. ‘Jumps and stochastic volatility: Exchange rate processes implicit deutsche mark options’.
The review of financial Studies 9.1 (1996): 69-107.

Cherubini, Umberto, et al. Fourier transform methods in finance. John Wiley & Sons, 2010.

Carr, Peter and Dilip B. Madan (1999): Option valuation using the Fast Fourier Transform, J.
Computational finance, 2, No.4, summer, 61-73.

Lewis, Alan (2001) A Simple Option Formula for General Jump-Diffusion and Other Exponential Levy ´
Processes, Working Paper, OptionCity, www.optioncity.net.

Bakshi, Gurdip, et al. “Empirical Performance of Alternative Option Pricing Models.” *The Journal of
Finance* vol. 52, no. 5, 1997, pp. 2003-2049.

Svoboda, Simona (2002) An Investigation of Various Interest Rate Models and Their Calibration in the
South African Market, Dissertation Thesis, University of the Witwatersrand, Johannesburg,
http://janroman.dhis.org

Schmelzele, Martin (2010) Option Pricing Formulae using Fourier Transform: Theory and Application,
Working Paper, www.pfadintegral.com.

Schoutens,Wim, Erwin Simons and Jurgen Tistaert (2004) A Perfect Calibration! Now What?, WILLMOT
Magazine, March, 66–78.

Protter, Philip (2001) A Partial Introduction to Financial Asset Pricing Theory, Stochastic Processes and
their Applications, 91, 169–203.

Mikhailov, Sergei and Ulrich Nogel (2003) Heston’s Stochastic Volatility Model—Implementation, ¨
Calibration and Some Extensions, WILMOTT Magazine, July, 74–79.
