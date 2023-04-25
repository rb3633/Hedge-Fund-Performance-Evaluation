# Hedge-Fund-Performance-Evaluation
Regression Analysis deep dive on example hedge fund return data

A)	Objective: 

Exercise:  
Run a regression analysis on the returns of each of the two portfolio managers.  
Determine what are the main drivers of the returns based on a regression.

Support:
Several benchmarks are provided for different markets and styles of investing.  
Manager D is a long-only global equity strategy.
Manager L is a hedge fund that primarily invests in convertible bonds, but also will invest long and short equity and credit.  

B)	Findings:

Portfolio Manager D:

Fund D, which follows a long-only global equities strategy, has generally outperformed the benchmarks which best explain its performance - 'MSCI ACWI', 'MSCI Emerging Markets', 'DJ MN Quality' (identified via the regression methodology detailed below). Risk-adjusted returns indicate that the fund is outperforming the three benchmarks. The MSCI ACWI index is the strongest driver of the performance, constituting global equity, while Emerging Markets is focused eponymously. The DJ MN Quality index is negatively correlated and suggests that this fund for the most part follows the Market trend, which is in contrast with, and explained by, the Market Neutral index.


a)	Methodology:
i) Data prep: 
Sourced Benchmark and Manager returns data from excel file. Performed necessary formatting and clean-up steps and combined the two dataframes for the analysis.

ii) Regression Applicability: 
As regression is unreliable when features are highly correlated, removed the features(benchmarks) with correlation > 0.9 . This reduced the feature set from 31 to 12 benchmarks(excluding Cash/Risk-free) which was then checked on the assumptions that need to be satisfied for linear regression to be applicable. The data satisfied the requirements to a good degree to progress with the Regression Analysis.




!(Img/D_Scatter.JPG)

Fig: Scatterplot to visualize distributions of all BMs against manager D


iv) Broad Regression:
As we are interested in relative returns, the cash/risk-free return is subtracted from all benchmarks and manager D. The regression is performed on these relative returns across all the remaining benchmarks(after removing highly correlated benchmarks).

 
Fig: Reg results of all non-correlated benchmarks


While this has a high R-squared of 77.5%, given the intention is to perform the regression to understand the drivers of the fund performance, having 12+ features does not help us make any meaningful inference on the drivers of the fund performance. Importantly, the p-value of most of the benchmarks (aside from MSCI ACWI) are insignificant. 

v) Targeted Regression:
To start with, performing regression on the benchmark MSCI ACWI which is noted as the benchmark for this manager. 

 
Fig: Reg results of MSCI ACWI BM

The R-squared of this regression is 62.2% which is less than that obtained with all benchmarks. However, we see that the Alpha is insignificant, suggesting we cannot say that the manager is performing better than this benchmark. The Beta is positively correlated, indicating better performance in a negative scenario but worse performance in a positive scenario.

vi) Stepwise Regression:
In order to further understand what benchmarks might best explain the manager’s performance, Stepwise regression has been performed to arrive at the 3 benchmarks that best explain the data.
['MSCI ACWI_diff', 'MSCI Emerging Markets_diff', 'DJ MN Quality_diff']

 
Fig: Reg results of top 3 BMs based on stepwise regression


The R-squared of this regression is 72.1%, the Alpha is still not significant meaning the 3 benchmarks are able to explain more of the funds performance themselves. The alpha has increased but by a small margin. The Betas of all 3 benchmarks are positive and per context it makes sense that the ACWI and Emerging Markets index is positively correlated and the Market Neutral index is negatively correlated.

 


As the choice of 3 benchmarks for the stepwise regression was arbitrary, for thoroughness the stepwise regression was repeated for number of features varying from 2 to 10, on observing which the R-Squared consistently improves as we add features(which is expected) but the Adjusted R-Squared drops from 6 benchmarks to 7 benchmarks, which means the performance is best at 6, which has the following benchmarks:
['MSCI ACWI_diff', 'BarCap High Yield_diff', 'MSCI Emerging Markets_diff', 'DJ MN Anti-Beta_diff', 'DJ MN Quality_diff', 'DJ MN Size_diff']

The adjusted R-squared of this regression is the highest thus far at 73.5%. All features are either significant or almost significant. The Alpha, at 0.0062 is small but is the strongest evidence so far that the manager is performing better than these benchmarks that best drive its performance.


vi) Measures:
Having completed the regression analysis, the evaluation of the manager is conducted based on the risk/return metrics, captured below:

 
Fig: The last 3 columns indicate the positive relative returns of manager D

 
Fig: The last 3 columns indicate the relative risk taken on by manager D

 
Fig: The last 3 columns indicate relative risk-adjusted risk returns for manager D

We see that the manager has positive relative return measures against all the benchmarks, with a risk profile comparable to the MSCI ACWI index, while it does take much greater risk than the MN index (8% more). The risk-adjusted returns(Sharpe Ratio) indicate that manager D does provide superior returns for a comparable level of risk when compared with MSCI ACWI index. It is interesting to note that in the case of the Emerging Markets index, the manager provides superior returns while taking on less risk, significantly outperforming it. The returns compared to MN while good come with much higher risk measure, as previously noted.



Portfolio Manager L:

Fund L, which primarily invest in convertible bonds but also in long/short equity, due to its poor performance in one single month(Nov 2018) of -80%, underperforms the benchmarks against all metrics. The regression analysis(with this outlier removed) suggests that the BarCap High Yield (bonds), CSI 300, and DJ MN Quality best explain the data to the extent possible and are the strongest indicators that such investments drive the performance of the manager L.

a)	Methodology:
i) Data prep: 
Sourced Benchmark and Manager returns data from excel file. Performed necessary formatting and clean-up steps and combined the two dataframes for the analysis.

ii) Regression Applicability: 
As regression is unreliable when features are highly correlated, removed the features(benchmarks) with correlation > 0.9 . This reduced the feature set from 31 to 12 benchmarks(excluding Cash/Risk-free) which was then checked on the assumptions that need to be satisfied for linear regression to be applicable. 

 
Fig: Scatterplot of distribution with the outlier, suggesting problematic regression results


Fund L’s returns are interesting in that there is one month, Nov 2018, when the fund had a huge negative return of -80%, which impacts the regression analysis due to its large variance from the remaining monthly returns. The adjusted R-squared value of the resulting regression is actually negative, meaning the bulk of the issue is arising from this outlier point. This single point is hindering our ability to draw meaningful inference from the data via a regression, so the following regression has been performed with this outlier removed.

 
Fig: Scatterplot of distribution after removing outlier
 
Fig: Regression on non-correlated benchmarks after removing outlier

However, the data still does not satisfy all the requirements for a powerful regression analysis to a good degree, so we don’t expect much to be explained by the regression, as seen above. The R-Squared is indeed better(compared with outlier) but still quite small compared to manager D.

iii) Targeted Regression
To start with, performing regression on the benchmark Barclays US Covertible Bond Index based on the context of the type of portfolio managed by L as provided in the case. 

 
Fig: Reg results on Barclays Convertible Index

The R-squared of this regression is 34.4% which is lesser than that obtained with all benchmarks. However here we see that the Alpha, though positive, is insignificant suggesting we cannot really say that the manager is performing better than this benchmark. The Beta/systematic risk is positively correlated.

iv) Stepwise Regression:
In order to further understand what benchmarks might best explain the manager’s performance, Stepwise regression has been performed to arrive at the 3 benchmarks that best explain the data.

['CSI 300 TR USD_diff', 'DJ MN Momentum_diff', 'DJ MN Quality_diff']

 
Fig: Reg results on the top 3 BMs as identified by stepwise regression

The R-squared of this regression is 40%, the Alpha, though positive is still not significant meaning the 3 benchmarks are able to explain more of the funds performance themselves. 

vi) Context-based BM Selection:
As the choice of 3 benchmarks for the stepwise regression was arbitrary, for thoroughness the stepwise regression was repeated for number of features varying from 2 to 10. However the results do not seem to jive with the contextual knowledge of the portfolio that L manages. So I chose 3 benchmarks based on the contextual knowledge, and after trying a few combinations, arrived at the following combination that best explained the data:

['BarCap High Yield', 'CSI 300 TR USD', 'DJ MN Quality']

 
Fig: Reg results of the 3 selected BMs

 
Fig: Plotting all returns against the regression estimated results

The adjusted R-squared of this regression is the highest thus far at 40%. There is still a significance issue where the Alpha and the CSI index are less significant.




vii) Measures:
Having completed the regression analysis, the evaluation of the manager is conducted based on the risk/return metrics, captured below:
For this part, the outlier negative return of -80% from Nov 2018 is retained because so long as it is not a data entry issue, this is a real and relevant datapoint that impacts the manager’s performance with compared to the benchmark.

 
Fig: The last 3 columns indicate the relative returns of manager L

 
Fig: The last 3 columns indicate the relative risk taken on by manager L

 
Fig: The last 3 columns indicate the relative risk-adjusted returns of manager L

The single negative performance of the fund has negatively impacted the fund, as the calculated return measures are negative, despite taking a lot of risk(High annual Std Dev). Correspondingly, the risk-adjusted returns are all also negative in comparison with the benchmarks. The manager has drastically underperformed compared to the market.

(The same analysis has been repeated with the outlier negative return removed for comparison purpose, and while that reveals that the manager has outperformed the benchmarks, the issue is that if -80% is a real datapoint it should be utilized to reflect the performance of the manager.)
