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


# Final Model 

# Fairness Analysis

