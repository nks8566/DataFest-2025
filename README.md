# DataFest-2025

**One Page Report**

**Primary questions/goal**: How can they leverage market growth trends and rent forecasts to strategically advise clients on the optimal locations for office spaces?

**Methods**: Of the feature engineering endeavors, creating a growth index provided us with the most information in a very simple and interpretable manner. We define a “growth index” as the weighted summation of features we believe to be good predictors: w1 * go-proportion + w2 * percent change of leases - w3 * remote workers rate, where wi = 0.5, 0.3, 0.2 respectively. Here is a quick breakdown of the features we used for this calculation (remote work and population information are from US Census data):
go_stay =’go’ if transaction_type is ‘New’ or ‘Relocation’, else ‘stay’ 
go_proportion = group by 'market', 'internal_industry', 'quarter' → take the mean of ‘go’
percent_change_leases = group by 'market' → use percent_change function on leasing * 100
remote_workers_rate = work_at_home / employed_population
Then, we normalized using z-score to allow for better comparison. Next, to improve interpretability, we stretched the normalized growth index to fix [-1, 1] range using in-max normalization = 2 * frac{x - min(x))}{max(x) - min(x)} - 1. If the growth index is closer to 1, the market is receiving a lot of leasing business from new companies. We added these values to the map for a full insight into annual changes. This map also contains information on the market level for every quarter from 2018-2024. It shows the average rent of a class A building and a class Other building for the market within a given quarter. 

For the forecasting, we wanted to help they stay informed about average rent prices by internal class for each market. A very simple ARIMA(1, 0, 0) model was used due to limited data even though we considered SARIMA fits. We understand that this is not the best model for all 50+ time series we are forecasting, but it provided the best overall results when quickly observing the AIC and p-values. We also explored possible forecasting for the growth index, but with the even more limited data, the ARIMA model did not perform well and we decided that simply using a constant ARIMA(0, 0, 0) was sufficient for the purposes of our demonstration and could be improved with additional data. The appendix shows the models and their results for the market in Manhattan. With more time, we could add this information to the interactive map for they to stay informed about prospective changes in rent and growth index.

**Findings**: This analysis provides valuable insights into the commercial real estate market by offering a comprehensive view of market growth, rental costs, and industry-specific trends over time. By leveraging the growth index, we can identify markets that are experiencing significant lease activity, with the growth rate for each industry (tech, legal, finance) providing a clear picture of sectoral demand. The interactive map allows users to explore these trends across different markets and quarters, helping they identify high-growth areas and industries poised for expansion. Additionally, the predictive time series model provides forward-looking rent forecasts for Class A and Class Other buildings, allowing they to strategically plan office locations by anticipating cost trends. This enables the consulting firm to advise clients on optimal office locations based on future growth potential and cost considerations, empowering them to make data-driven decisions about when and where to expand their operations.
Appendix






