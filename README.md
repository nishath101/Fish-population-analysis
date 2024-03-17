
### 1.1 Project- FISH POPULATION TIME SERIES

This time series represents the number of new fish for a period ranging from 1950 to 1987. The data collected is used to monitor the population of different fish species within an area. 
Rec refers to 'recruitment', which suggests the population of the number of young fish that grow up enough to have a good chance of living as long as adult fish.
Recruitment data is vital to tell us if the fish population is healthy and whether the increasing population of young fish becoming adult fish shows that the fish population is thriving. It helps us learn more about environmental changes, i.e., pollution that may lead to habitat loss. This will directly affect the population of fish in the sea. We can observe and predict how fish populations might change over time by looking at recruitment trends. Observing these trends will be useful for finding solutions to help protect the fish population.

```{r, echo=FALSE}
knitr::include_graphics("fish-swimming.GIF")
```


### 1.2 Time Series Analysis
```{r, echo=FALSE}
library(prophet)
library(astsa)
rec.df = data.frame(
  ds=zoo::as.yearmon(time(rec)), 
  y=rec)
model1=prophet::prophet(rec.df,weekly.seasonality=TRUE,daily.seasonality = TRUE)
future=prophet::make_future_dataframe(model1, periods=1, freq="quarter")
prediction = predict(model1,future)
plot(model1,prediction)
```

The time series plot illustrates data points represented by black dots. The black dots are scattered, which suggests there is variability. Over time, we have seen a slight increase in the spread of the data points. The blue line outlines the trend/pattern of this time series period over 37 years. The trend suggests a seasonal pattern where we can see consistent fluctuations and a slight increase in recruitment throughout the years. Finally, we can see blue shading around the trend line, indicating uncertainty in estimating the trend. Since the shaded area is wide, this shows larger uncertainty.




### 1.3 Decomposition of additive time series
```{r, echo=FALSE}
library(astsa)
plot(decompose(rec)) 
```


This plot shows a decomposition of additive time series. The first subplot outlines the observed recruitment index of new fish; the graph shows fluctuations over a period, which means the number of fish entering the new population varied over time. This may be due to environmental factors i.e. pollution or water temperatures. 
The trend 
The seasonal plot illustrates a regular yearly pattern with consistency in the height of peaks and depths of troughs which shows that the seasonal factors are regular and predictable. The seasonal component may be related to the amount of food available in different seasons, habitat conditions or other biological factors.
Finally, the residual plot shows a part of the data that seasonal patterns or trends can not explain. It highlights other anomalies or outliers related to other external random events.


### 1.4 Prophet seasonality (weekly, daily, yearly)
```{r, echo=FALSE}
prophet_plot_components(model1,prediction) 
```


Over the years, more young fish have survived and joined the fish population. This increase could be due to various positive changes such as better protection of the fish environment, cleaner water, fewer fish being caught before they can reproduce, or overall conditions underwater that help young fish thrive. However, we can not predict the trend will continue to increase because this depends on the circumstances and external environmental factors that may affect the fish population.
The weekly component shows a sharp peak on Tuesdays. If fish are being counted every week, we might notice more young fish at certain times. For example, if fishing is not allowed on certain days, or if predators like birds or bigger fish have weekly patterns that affect the baby fish, we might see weekly changes.
Fish often have specific times of the year when they have their babies, known as the spawning season. We may see more young fish at certain times of the year and fewer at other times. The weather, food availability, and fish migration to different places can influence these patterns.


```{r, echo=FALSE}
fish=lm(y~ds, data=rec.df) 
summary(fish)
```

The linear regression output B0 hat is -1035.5774 which may suggest 1035 less fishes are dying every year. The r squared value is 4.728% which is very low. This model explains a very small portion of the variance in the recruitment. This suggests that other factors not included in the model might be influencing recruitment. Overall, the model suggests that there is a significant but small relationship between time and the recruitment index. It's possible that other variables not included in the model, for example environmental factors, predator and prey relationship, could be critical in explaining changes in the recruitment.


### 1.5 Dyplot of the forecast
```{r, echo=FALSE}
dyplot.prophet(model1,prediction) #interactive plot of the forecast

```
The dyplot is a good visualisation of the forecast and displays the actual and predicted data over the years.

### 1.6 References

https://edis.ifas.ufl.edu/publication/FA222


https://www.nature.com/articles/s41598-020-73025-z
