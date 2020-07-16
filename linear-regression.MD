# Linear Regression

## Using the statsmodel package

```python
import statsmodels.api as sm

df = pd.DataFrame({
    'x':[1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    'y': [10, 20, 35, 40, 52, 66, 77, 81, 99, 101]
})

plt.scatter(df["x"], df["y"])
plt.xlabel("x")
plt.ylabel("y")
plt.show()

df = sm.add_constant(df)

result = sm.OLS(df["y"], df[["x"]]).fit()

print("\nSummary:\n{}\n".format(result.summary()))
print("\nSlope:\n{}".format(result.params[0]))
```

```
Summary:
                                 OLS Regression Results                                
=======================================================================================
Dep. Variable:                      y   R-squared (uncentered):                   0.998
Model:                            OLS   Adj. R-squared (uncentered):              0.998
Method:                 Least Squares   F-statistic:                              4610.
Date:                Thu, 16 Jul 2020   Prob (F-statistic):                    1.65e-13
Time:                        09:57:13   Log-Likelihood:                         -24.803
No. Observations:                  10   AIC:                                      51.61
Df Residuals:                       9   BIC:                                      51.91
Df Model:                           1                                                  
Covariance Type:            nonrobust                                                  
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
x             10.5429      0.155     67.898      0.000      10.192      10.894
==============================================================================
Omnibus:                        2.048   Durbin-Watson:                   2.832
Prob(Omnibus):                  0.359   Jarque-Bera (JB):                0.830
Skew:                          -0.015   Prob(JB):                        0.660
Kurtosis:                       1.589   Cond. No.                         1.00
==============================================================================

Warnings:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


Slope:
10.542857142857143
```
