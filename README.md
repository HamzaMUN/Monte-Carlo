# Monte-Carlo Simulations

## Introduction
Simulating random variables from complex probability distributions is a fundamental task in scientific computing. This project implements two widely used simulation techniques: the **Accept-Reject Method** and the **Markov Chain Monte Carlo** simulation. These methods enable sampling from non-standard or high-dimensional distributions that are challenging to simulate directly.

### Task 1: Accept-Reject Method
We will develop an accept-reject method to simulate the $f(x)$ curve. The given density function is:  
$$f(x) \propto exp(-x^2/2)\{sin(6x)^2 + 3 cos(x)^2 sin(4x)^2 +1\}$$

#### Part (a): Plot $f(x)$ and show that it can be bounded by $M g(x)$, where $g$ is the standard normal density as:
$$g(x) = \frac {exp(-x^2/2)} {\sqrt{2\pi}}$$  
We will try to find an acceptable, if not necessarily optimal, value of $M$ using a built-in Python optimization function **minimize_scalar** from scipy.optimize.

#### Comments on Task 1 (a) output:
From the above plot we can see that the bounding constant $M = 10.94$ ensures $f(x) \leq M.g(x)$, where $g(x)$ is the standard normal density. This value is derived by optimization function minimize_scalar from scipy.optimize with bounds $(-5, 5)$. In the given example, the function $f(x)$ is unnormalized and it can be normalized by calculating normalizaion constant ($K$) by using integration and then dividing the function $f(x)$ with it. However, this part is not implemented in this code for simpilicity as the value of $M$ is still acceptable and satisfies the density function $f(x)$ and its bounds $M g(x)$.

#### Part (b): Generate $n = 2500$ random variables from $f$ using the Accept-reject algorithm

#### Comments on Task 1 (b) output:
In above histogram, we have plotted $n = 2500$ random variables from $f$ those were generated using accept-reject algorithm. In the following part, we will compare the generated data with the $f$ density function.

#### Part (c): Plot the histogram of the generated data and compare it with the $f$ density

#### Comments on Task 1 (c) output:
From the above plot, we can see that the histogram of the generated samples closely aligns with the $f$ density, particularly around $x \in (-3, 3)$. Also, both the histogram and $f$ density decay rapidly beyond $x$ values of $-3$ and $3$, which is consistent with the $exp(-x^2/2)$. So, the generated sample and $f$ density follow the same distribution because the accept-reject algorithm ensures that accepted samples are distributed according to $f$. i.e., $f \leq M g(x)$ for all $x$. Also, the acceptance rate in the accept-reject algorithm is $54.03 \%$, which is significant.

### Task 2: Markov Chain Monte Carlo
We will develop a Markov Chain Monte Carlo to simulate dependent data and estimate the $f(x)$ curve. The given density function is:  
$$f(x) = \frac {1} {\pi (1 + x^2)}, x \in \R$$

#### Part (a): Apply the random walk Metropolis-Hastings method and generate $n = 1000$ observations from the desnity $f(x)$.

#### Comments on Task 2 (a) output:
In part (a), we have used the same code snippet we wrote in the class and changed the density function $f(x) = \frac {1}{\pi (1 + x^2)}, x \in \R$ as per the given example. Then we executed this function with some parameters, which we will plot in part (d) to do a comparisson between different approaches.

#### Part (b): Apply the standard form of Metropolis-Hastings method using instrumental distribution $g(x)$ and generate $n = 1000$ observations from the desnity $f(x)$.

#### Comments on Task 2 (b) output:
In part (b), we have introduced an instrumental distribution function as $g(x) = \frac {1}{\sqrt {2 \pi}} exp(-x^2/2), x \in \R$ and the probability of accepting the candidate $x^*$ is calculated by:  
$$\rho(x^{(t)}, x^*) = min (\frac {f(x^*)g(x^{(t)})}{f(x^{(t)})g(x^*)}, 1)$$
Then we executed this function with some parameters, which we will plot in part (d) to do a comparisson between different approaches.

#### Part (c): Apply the standard form of Metropolis-Hastings method using uniform distribution (Box-Muller) and generate $n = 1000$ observations from the desnity $f(x)$.

#### Comments on Task 2 (c) output:
In part (c), we have introduced Box-Muller transofmation and the function box_muller takes two input parameters as $U_1$ and $U_2$ and returns $Z_1$ and $Z_2$. Then we chose $U_1$ and $U_2$ as independent samples on the unit interval $(0, 1)$, and we pass these $U_1$ and $U_2$ to the box_muller function which computes $Z_1$ and $Z_2$ as $Z_1 = \sqrt {-2 ln U_1 cos(2 \pi U_2)}$ and $Z_2 = \sqrt {-2 ln U_1 sin (2 \pi U_2)}$. The results of this function call ($Z_1$ and $Z_2$) are independent and we can use either of these in $\rho$ calculation. However, in our code implementation, we computed the modulus 2 of the loop counter and used an alternate between $Z_1$ and $Z_2$ throughout iterations. Then we executed this function with some parameters, which we will plot in part (d) to do a comparisson between different approaches.

#### Part (d): Plot the histograms of full chains, accepted chains and trace plots for part a-c and compare.

#### Comments on Task 2 (d) output:
From the above plots, we can observe that the Random-Walk method has a low acceptance rate but the accuracy of histogram is high and the tail exploration is also good. In contrast to Random-Walk, Instrumental and Box-Muller have very high acceptance rates and the tail exploration is poor and the accuracy of the histogram is also not that high. However, the results keep changing on each trial and parameters of these approaches have to be tuned to get a better output.

### Task 2: Conclusion
In above code, we executed 10 trials with a burn_in value of 100 which will skip the initial 100 samples. After that, we calculated the Effective Sample Size (ESS) for each trial and chose the trial based on best ESS value and plotted it. We also calculated the average values of ESS.  

From the above plots, we can see that the histogram for Box-Muller method aligns more closely to the given desnity function in contrast to other methods. In the trace plot for last 200 samples, we can see that Random-Walk is hitting the tail values but most of the time it is staying in the center which means that it is not converging effectively as compared to Instrumental and Box-Muller.  

Other than plots, we have printed some metrics which we can compare. Firstly, we have Best ESS which indicates the Effective Sample Size for the best trial. From the printed output, we can see that the Best ESS value is for Random-Walk is the lowest and Instrumental has a significantly higher Best ESS value. While Box-Muller has a balanced Best ESS value. Best Accept Rate for Random-Walk is the lowest but optimal while for Instrumental and Box-Muller method it is very high. If we have to chose the best method based on Accept Rate, Random-Walk should be the best method as it has the most optimal acceptance rate. If we compare the Best Tail Coverage, Random-Walk has the best tail coverage while Instrumental and Box-Muller have poor tail coverage. Avg ESS for Random Walk is the lowest and Instrumental has the highest Avg ESS.  

After comparing all of these metrices, there are some scenraios where Random-Walk looks like the best method while for other metrics the Instrumental method looks the best. However, Box-Muller lies between these two methods and is more consistent and reliable as compared to other two methods. The method values might change from trial to trial but Box-Muller method seems the most optimal for the given density function. Hence, based on my trials and results, I will suggest Box-Muller over other two methods for this specific problem statement.

## References
1. **Gelman, A., Carlin, J. B., Stern, H. S., Dunson, D. B., Vehtari, A., & Rubin, D. B.** (2013) *Bayesian Data Analysis* (3rd ed.). Chapman and Hall/CRC.