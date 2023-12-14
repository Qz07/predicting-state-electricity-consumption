# Predicting State Electricity Consumption
By: Guoxuan Xu, Qirui Zheng

[Power Outage Analysis (Project 3)](https://qz07.github.io/powerOutage-Analysis/)

# Framing the Problem
In our previous analysis, we explored the correlation between the proportion of the utility factor and outage duration, emphasizing the critical role electricity plays in our daily lives. Even a brief power outage can instill uncertainty and fear within communities. Building on this, we seek to further investigate the intricate relationship between electricity and various factors. This study focuses on predicting energy consumption in industrial sectors within a specific state, utilizing relevant information. Accurate predictions are crucial for comprehending how industrial energy demands may impact the power grid, potentially leading to outages. By effectively forecasting industrial electricity consumption, we aim to assist electricity departments in each state in better managing their resources. 

The variable under consideration is `IND.SALES`, representing electricity consumption in the industrial sector measured in megawatt-hours. Our exploratory data analysis in Project 3 uncovered intriguing trends, specifically noting that states with lower utility industry GSP tend to experience shorter outage durations. This led us to formulate a hypothesis: states with a significant proportion of GSP utility contribution are more likely to possess a substantial utility base. In the event of a power outage, it is conceivable that larger utility sectors may require more time to restore service. This observation is just one among several factors contributing to variations in electricity consumption. In our regression model, we aim to investigate the relationship between total consumption and other factors, including land area and population, among others. 
*Which variables are we using in the start?*

Given that our problem pertains to regression analysis, we have opted to employ the Mean Absolute Percentage Error (MAPE) as a metric to evaluate our model. This choice is motivated by the inherent characteristics of regression models and the variability present in the data. Predicting exact values with precision can be challenging, and relying solely on the Mean Squared Error might lead to confusion. The Mean Absolute Percentage Error, on the other hand, gauges the average magnitude of errors relative to the scale of the dataset. Expressing the prediction error as a percentage in comparison to the actual value facilitates a more accessible interpretation. This approach provides a general range of error, offering insights into how the model is performing against actual values.

$$
\begin{gather*}
MAPE = \frac{1}{n} \sum_{t=1}^{n} \left| \frac{A_{t} - F_{t}}{A_{t}} \right|\\
\end{gather*}
$$
Where:  
n = number of total predictions  
A\_t = actual value  
F\_t = forecast value

We adopted similar steps as in project 3 to clean the dataframe the same way. 

*insert head of the dataframe first 5 rows*
# Baseline Model
In constructing our baseline model, we opted for a straightforward linear regression approach with two features: month and population. The month feature, initially treated as a quantitative variable, underwent transformation into a qualitative form and was encoded using One Hot Encoding. On the other hand, the population, a continuous quantitative variable, was utilized without preprocessing. For our train-test split, we selected a ratio of 30% for training and 70% for testing. Notably, since one of our features involves time series data (month) and predictions were intended for upcoming months, we manually implemented the train-test split by sorting the dataframe based on time series data and allocating the initial 70% for training and the subsequent 30% for testing.

The accuracy metrics produced by the model indicate a training accuracy of 0.47 and a testing accuracy of 0.43. These values, being in proximity to 0.50, suggest a relatively low accuracy, akin to that of a coin flip. Further evaluation and refinement may be necessary to enhance the model's predictive capabilities.

# Final Model 

To improve upon our baseline model, we decided to construct a further investigation into different features options that are avaiable at the time of prediction. 

First, `U.S._STATE`, states can be a geographical information that contribute to the amount of electricity consumption. For example, states with higher indsutrial heavey cities will have a upward sloping relationship with the amount of electricity consumptoin. 

*Insert graph after double check*


State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.

# Fairness Analysis

Revisiting the trend observed in states with high GSP per utility experiencing potentially longer durations, we sought to assess the fairness of our model concerning the same criterion of outstanding GSP. We defined the region where PC.REALGSP.REL is greater than or equal to 1 as the outstanding GSP performance area, and conversely, the region where PC.REALGSP.REL is less than 1 as the weak GSP performance area.

Group A: The area with outstanding GSP performance
Group B: The area with weak (not outstanding) GSP performance
Null Hypothesis: Our model is fair. The Root Mean Square Error (RMSE) for the outstanding GSP performance area (Group A) and weak GSP performance area (Group B) is approximately the same, and any differences are attributed to random chance.

Alternative Hypothesis: Our model is unfair. The RMSE for outstanding GSP performance area (Group A) differs from that of the weak GSP performance area (Group B).

Utilizing a regression model, we can employ metrics such as mean square error to compare the means within the groups and evaluate the fairness of our model with respect to GSP performance.

Optional: Embed a visualization related to your permutation test in your website.

Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.