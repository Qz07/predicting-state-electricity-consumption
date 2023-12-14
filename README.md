# Predicting Electricity Consumption
By: Guoxuan Xu, Qirui Zheng

Our exploratory data analysis on this dataset is found here.[here](https://qz07.github.io/powerOutage-Analysis/)

# Framing the Problem
In our previous analysis, we explored the correlation between the proportion of the utility factor and outage duration, emphasizing the critical role electricity plays in our daily lives. Even a brief power outage can instill uncertainty and fear within communities. Building on this, we seek to further investigate the intricate relationship between electricity and various factors. This study focuses on predicting energy consumption in industrial sectors within a specific state, utilizing relevant information. Accurate predictions are crucial for comprehending how industrial energy demands may impact the power grid, potentially leading to outages. By effectively forecasting industrial electricity consumption, we aim to assist electricity departments in each state in better managing their resources. 

The variable under consideration is `IND.SALES`, representing electricity consumption in the industrial sector measured in megawatt-hours. Our exploratory data analysis in Project 3 uncovered intriguing trends, specifically noting that states with lower utility industry GSP tend to experience shorter outage durations. This led us to formulate a hypothesis: states with a significant proportion of GSP utility contribution are more likely to possess a substantial utility base. In the event of a power outage, it is conceivable that larger utility sectors may require more time to restore service. This observation is just one among several factors contributing to variations in electricity consumption. In our regression model, we aim to investigate the relationship between total consumption and other factors we would know at the time of prediction. Which are the following columns:
- `MONTH`: Month when the outage occurred (1-12)
- `U.S._STATE`: All US States
- `IND.CUSTOMERS`: Annual number of customers served in the industrial electricity sector of the U.S. state
- `POPULATION`: Population in the U.S. state in a year

Given that our problem pertains to regression analysis, we have opted to employ the Mean Absolute Percentage Error (MAPE) as a metric to evaluate our model. This choice is motivated by the inherent characteristics of regression models and the variability present in the data. Predicting exact values with precision can be challenging, and relying solely on the Mean Squared Error might lead to confusion. The Mean Absolute Percentage Error, on the other hand, gauges the average magnitude of errors relative to the scale of the dataset. Expressing the prediction error as a percentage in comparison to the actual value facilitates a more accessible interpretation. This approach provides a general range of error, offering insights into how the model is performing against actual values.

<iframe src="assets/MAPE.png" width=250 height=100 frameBorder=0 ></iframe>

Where:  
n = number of total predictions  
A\_t = actual value  
F\_t = forecast value

We adopted similar steps as in project 3 to clean the dataframe the same way. 

Here is the first 5 rows of the dataframe with features that we will be predicting on: 

|    |   MONTH | U.S._STATE   |   IND.CUSTOMERS |   POPULATION |
|---:|--------:|:-------------|----------------:|-------------:|
|  0 |       7 | Minnesota    |           10673 |      5348119 |
|  1 |       5 | Minnesota    |            9898 |      5457125 |
|  2 |      10 | Minnesota    |           10150 |      5310903 |
|  3 |       6 | Minnesota    |           11010 |      5380443 |
|  4 |       7 | Minnesota    |            9812 |      5489594 |


# Baseline Model
In constructing our baseline model, we opted for a straightforward linear regression approach with two features: month and population. The month feature, initially treated as a discrete quantitative variable, underwent transformation into a ordinal form (turing digits of months 1-12 to literal representation of months) and was encoded using One Hot Encoding. On the other hand, the population, a discrete quantitative variable, was utilized without preprocessing. For our train-test split, we selected a ratio of 30% for training and 70% for testing. Notably, since one of our features involves time series data (month) and predictions were intended for upcoming months, we manually implemented the train-test split by sorting the dataframe based on time series data and allocating the initial 70% for training and the subsequent 30% for testing.

The accuracy metrics produced by the model indicate a training accuracy of 0.47 and a testing accuracy of 0.43. These values, being in proximity to 0.50, suggest a relatively low accuracy, akin to that of a coin flip. We don't think this is a good model therefore, further evaluation and refinement may be necessary to enhance the model's predictive capabilities.

<iframe src="assets/residual-baseline.html" width=800 height=600 frameBorder=0></iframe>

# Final Model 

To improve upon our baseline model, we decided to construct a further investigation into different features options that are avaiable at the time of prediction. 

First, `U.S._STATE`, states can be a geographical information that contribute to the amount of electricity consumption. For example, states with higher indsutrial heavey cities will have a upward sloping relationship with the amount of electricity consumptoin. 

<iframe src="assets/state-relation.html" width=800 height=600 frameBorder=0></iframe>

Moreover, another significant factor that could impact the industrial electricity consumption in an area is the presence of industrial customers. A region with a larger number of industrial customers is likely to experience increased electricity consumption, driven by elevated demand and a greater overall usage of electricity.

<iframe src="assets/ind-customer-relation.html" width=800 height=600 frameBorder=0></iframe>

While the graph reveals three prominent clusters, it is evident that the data points exhibit a significant spread with high variance, indicating the presence of noise. Given the substantial scale of industrial consumers, their influence on the model coefficients can be disproportionately large. To mitigate this impact and achieve more consistent predictions while reducing variance, we have opted to standardize the features. This is a good feature to include in the final model becuase there is a direct trend in the relation of industrial customers and industrial electricity consumption, due to this high corrlation we belive that it will improve our model prediction accuracy. 


Total population is a factor directly related to electricity consumption 

<iframe src="assets/population-relation.html" width=800 height=600 frameBorder=0></iframe>

Suprisingly, there is clustering according to population size. There are four main clusters when the total population is less than 10M, between 10M and 20M, between 20M and 30M, and above 30M. We can generalize the relationship by binning the total population into four corresponding categories(small, medium, large, huge). More specifically, we will transform `POPULATION` two four ordinal categories and OneHot Encode them. This is good for the data prediction task in a sense that we created a observed split within the feature to limit the noise. 


### Model Selection 

With multiple different regression models, we will choose the model base on their performance on the cross validation test. More specifically, we will choose the model with the least average MAPE among all the folds. Since our regression is time sensitve, we can't have normal K-folds cross validation because <u>we need to make sure that the training data only contains the earlier data set while the validation data is later data set</u>. To resolve this issue, we will use `TimeSeriesSplit` from `sklearn.model_selection` to help us create n folds of training and validation data set that aligns with the requirement of time series data.

The graph below illustrates the performance of five distinct models: linear regression, elastic net regression, decision tree regression, random forest regression, and SVR. Notably, decision tree regression and random forest regression exhibit similar accuracy levels; however, owing to the nature of random forest, it is less prone to higher bias compared to a single decision tree regression. Consequently, our final model selection is the implementation of a random forest regressor, based on the result from the grid search for models. 

<iframe src="assets/comparsion-of-diff-model.html" width=800 height=600 frameBorder=0></iframe>

### Finding Hyperparameter


Within a Random Forest Regressor, various hyperparameters play a crucial role in tuning the model for optimal performance:

- **max_features**: This parameter determines the number of features used in each split. Increasing it can enhance the splitting quality across the model, potentially resulting in a lower criterion (measurement). This, in turn, contributes to a more balanced trade-off between variance and bias.

- **n_estimators**: The number of decision trees created to vote on the final result. While having more decision trees can lead to more stable metrics, it's crucial to find a threshold that minimizes the metric while optimizing computational cost.

- **criterion**: This is a measurement of the quality of a split. Similar to the max_feature parameter, a better splitting measurement can reduce the model's bias and variance, leading to improved overall performance.

- **max_depth**: This parameter controls the maximum depth each decision tree can reach. By managing the maximum depth, we can control how pure the leaf nodes will be, subsequently lowering the variance of each decision tree within the model.

The method used to choose the hyperparameter is `GrideSearchCV` with 5 folds, and each fold are determined with `TimeSeriesSplit` that align with the requirement of time series data. We worked with 3 different sizes for max_features (12,20,25), and also 3 different sizes for n_estimators (50,100,150). The two criterion that we choose are absolute error, and squared error. With the max depth of a single tree limited to 10 or None. 

After running `GrideSearchCV` the best hyperparameters are the following: 
- criterion: absolute_error  
- max_depth: None 
- max_features: 20
- n_estimators: 50

From the steps above the final model is built as such: 
1. `MONTH` is transformed to ordianl features and One Hot Encoded
2. `U.S._STATE` is One Hot Encoded
3. `IND.CUSTOMERS` is being standardized 
4. `POPULATION` is categorized and One Hot Encoded 
5. `POPULATION` the orginal value is standardized

With the best hyperparamers found from above our model in the pipeline takes in the parameters of criterion = absolute_error, max_features = 25, and n_estimators = 100.

This model is then fitted on the given input dataframes. 

In comparing to baseline model, we have a lower MAPE on both training and testing data set for final model which shows that our final model has improved in terms of reducing bias and variance.

However, our testing model yields a lower training MAPE when compared to testing MAPE. Based on the nature of how model trainig works, this is resonable but futher works can be done in tuning to yeid a better model in which the training error is closer to the testng error. 

<iframe src="assets/baseline-final.html" width=800 height=600 frameBorder=0></iframe>

# Fairness Analysis

Revisiting the trend observed in states with high GSP per utility experiencing potentially longer durations, we sought to assess the fairness of our model concerning the same criterion of outstanding GSP. We defined the region where PC.REALGSP.REL is greater than or equal to 1 as the outstanding GSP performance area, and conversely, the region where PC.REALGSP.REL is less than 1 as the weak GSP performance area.

**Group A**: The area with outstanding GSP performance

**Group B**: The area with weak (not outstanding) GSP performance

**Null Hypothesis**: Our model is fair. The Root Mean Square Error (RMSE) for the outstanding GSP performance area (Group A) and weak GSP performance area (Group B) is approximately the same, and any differences are attributed to random chance.

**Alternative Hypothesis**: Our model is unfair. The RMSE for outstanding GSP performance area (Group A) differs from that of the weak GSP performance area (Group B).

**Test Statstic**: the absolute difference of the prediction metric(RMSE) of the outstanding GSP perforamnce area(Group A) and the prediction metric(RMSE) of the weak GSP performance area(Group B)

**Significant leve**: 0.05

Utilizing a regression model, we can employ metrics root mean square error to compare the means within the groups and evaluate the fairness of our model with respect to GSP performance.

<iframe src="assets/emp-test-stat.html" width=800 height=600 frameBorder=0></iframe>

From this test it yield a p-value of 0.5308
. Since the p-value is greater than 0.05, we fail to reject our null hypothesis. It is likely that our model is fair toward areas with either outstanding or weak GSP performance. However, further investigation is needed to prove the statement.

# References
Mukherjee, S., Nateghi, R., & Hastak, M. (2018). Data on major power outage events in the continental U.S. Data in Brief, 19, 2079–2083. https://doi.org/10.1016/j.dib.2018.06.067