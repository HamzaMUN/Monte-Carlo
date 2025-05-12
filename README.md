# Monte-Carlo Simulations

## Introduction
Simulating random variables from complex probability distributions is a fundamental task in scientific computing. This project implements two widely used simulation techniques: the **Accept-Reject Method** and the **Markov Chain Monte Carlo** simulation. These methods enable sampling from non-standard or high-dimensional distributions that are challenging to simulate directly.

### Task 1: Accept-Reject Method
We will develop an accept-reject method to simulate the $f(x)$ curve. The given density function is:  
$$f(x) \propto exp(-x^2/2)\{sin(6x)^2 + 3 cos(x)^2 sin(4x)^2 +1\}$$

#### Part (a): Plot $f(x)$ and show that it can be bounded by $M g(x)$, where $g$ is the standard normal density as:
$$g(x) = \frac {exp(-x^2/2)} {\sqrt{2\pi}}$$  
We will try to find an acceptable, if not necessarily optimal, value of $M$ using a built-in Python optimization function **minimize_scalar** from scipy.optimize.