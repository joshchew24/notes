---
citation: https://stats.stackexchange.com/questions/107610/what-is-the-reason-the-log-transformation-is-used-with-right-skewed-distribution
---
Economists (like me) love the log transformation. We especially love it in regression models, like this:

$$\ln{Y_i} = \beta_1 + \beta_2 \ln{X_i} + \epsilon_i$$

Why do we love it so much? Here is the list of reasons I give students when I lecture on it:

1. It respects the positivity of Y𝑌. Many times in real-world applications in economics and elsewhere, Y𝑌 is, by nature, a positive number. It might be a price, a tax rate, a quantity produced, a cost of production, spending on some category of goods, etc. The predicted values from an untransformed linear regression may be negative. The predicted values from a log-transformed regression can never be negative. They are $\widehat{Y}_j=\\exp{\left(\beta_1 + \beta_2 \ln{X_j}\right)} \cdot \frac{1}{N} \sum \\exp{\left(e_i\right)}$ (See [an earlier answer of mine](https://stats.stackexchange.com/questions/55692/back-transformation-of-an-mlr-model/58077#58077) for derivation).
2. The log-log functional form is surprisingly flexible. Notice:
$$\ln{Y_i} = \beta_1 + \beta_2 \ln{X_i} + \epsilon_i$$
$$Y_i = \\exp{\left(\beta_1 + \beta_2 \ln{X_i}\right)}\cdot\\exp{\left(\epsilon_i\right)}$$
$$Y_i = \left(X_i\right)^{\beta_2}\\exp{\left(\beta_1\right)}\cdot\\exp{\left(\epsilon_i\right)}$$
Which gives us:![Loving the log-log functional forms](https://i.stack.imgur.com/C63aA.jpg) That's a lot of different shapes. A line (whose slope would be determined by $\exp{\beta 1}$, so which can have any positive slope), a hyperbola, a parabola, and a "square-root-like" shape. I've drawn it with $\beta 1=0$ and  $\epsilon=0$, but in a real application neither of these would be true, so that the slope and the height of the curves at $𝑋=1$ would be controlled by those rather than set at 1.
3. As TrynnaDoStat mentions, the log-log form "draws in" big values which often makes the data easier to look at and sometimes normalizes the variance across observations.
4. The coefficient \beta 2\beta 2 is interpreted as an elasticity. It is the percentage increase in Y𝑌 from a one percent increase in X𝑋.
5. If $X$ is a dummy variable, you include it without logging it. In this case, $\beta 2$ is the percent difference in $Y$ between the $X=1$ category and the $X=0$ category.
6. If $X$ is time, again you include it without logging it, typically. In this case, $\beta 2$ is the growth rate in $Y$---measured in whatever time units $X$ is measured in. If $X$ is years, then the coefficient is annual growth rate in $Y$, for example.
7. The slope coefficient, $\beta 2$, becomes scale-invariant. This means, on the one hand, that it has no units, and, on the other hand, that if you re-scale (i.e. change the units of) $X$ or $Y$, it will have absolutely no effect on the estimated value of $\beta 2$. Well, at least with OLS and other related estimators.
8. If your data are log-normally distributed, then the log transformation makes them normally distributed. Normally distributed data have lots going for them.

Statisticians generally find economists over-enthusiastic about this particular transformation of the data. This, I think, is because they judge my point 8 and the second half of my point 3 to be very important. Thus, in cases where the data are not log-normally distributed or where logging the data does not result in the transformed data having equal variance across observations, a statistician will tend not to like the transformation very much. The economist is likely to plunge ahead anyway since what we really like about the transformation are points 1,2,and 4-7.