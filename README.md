# WTI-futures-rolling-regression-war-sentiment
In this project we use a carry strategy to trade WTI futures, utilising a rolling regression and a Middle-Eastern war sentiment index.

NOTE: PLEASE VISIT final project Arjun Arthur.ipynb FOR FULL JUPYTER NOTEBOOK AND RESULTS

Project summary - Arjun Kalsi & Arthur Boissavy

Investment Idea :

We chose to predict the price of oil using three types of data : the inflation rate, USD value, and inventory levels. Our first supposition is that the 
price of WTI oil is a function of these three components:
price of oil = F(inflation, USD, inventory)

It was noted in the lectures that inflation and oil prices are highly correlated, and in our final lecture we covered the interconnectedness of inflation
rates, exchange rates and inventory levels. Thus we decided to use these 3 factors in order to predict the WTI price as we believed they would form an 
accurate representation of it. To do this we decided to do a rolling regression where we regress on the previous n days in order to predict the oil price
today. The window of the regression would be the parameter we are looking to optimize. Thus, the idea is simple: we buy when the market price is below the
model price (undervalued) and we sell when the market price is over the model price (overvalued). In this way it is more or less a carry strategy where we
are looking at the spread of two prices. We will optimize a threshold epsilon above and below which we will sell or buy. We will optimize the threshold in
order to maximize the P&L and minimize the cost of transaction.
π = { 1 if predict ≥ price (underpriced) −1 if predict < price (overerpriced)

We believe this strategy may work if our model is good enough. For that we will optimize all the parameters involved such as the window of the rolling 
regression. Also we decided to add an epsilon parameter just like the carry strategy that we will also optimize :
1 if predict − price > ε (underpriced)
π = −1 if predict − price < − ε (overerpriced)
0 if|predict−price|≤ε

We use epsilon to avoid whipsaw and thus avoid a too important transaction cost. We believe this strategy may work because for us it makes sense the that 
the three biggest factors which significantly change oil price are: inflation, USD and inventory.

Bonus idea: geopolitical risk reduction

We then tried to ameliorate our model by minimizing the maximum drawdown and reduce downside risk as much as possible. To do so we realized that the oil
market was extremely sensitive to geopolitical issues in some regions of the world such. For example, major conflicts in the Middle East in 2014-2015 
affected oil prices as well as volatility, and this was our main motivation for our secondary idea. In order to reduce downside risk we investigated some
databases regarding conflict and terrorism in the world. For instance we worked on the global terrorism database from Kaggle 
(https://www.kaggle.com/START-UMD/gtd) and we identified all the attacks which targeted utilities (oil pipeline etc...) but we had some difficulty to use 
and integrate in the data in an effective way. We finally decided to use the rate of death due to terrorism in the middle east as an indicator of violence.
This is because despite its simplicity, it is incredibly easy to understand and is probably a decent representation of conflict within a region. Note that
this data was annual, thus we had to use interpolation in order to make the data daily. Looking at the data we set a threshold death rate for which we
halt trading because the situation is too risky according to the data. By doing so we were able to slightly ameliorate our Sharpe ratio and to reduce our
maximum drawdown.

Data used 

We used the three data sets :
- Inflation rate (https://fred.stlouisfed.org/series/T10YIE)
- USD (https://fred.stlouisfed.org/series/DTWEXBGS)
- Inventory (https://www.eia.gov/dnav/pet/pet_stoc_wstk_dcu_nus_w.htm)

Inflation rate data and USD data are coming from the same website FRED (https:// fred.stlouisfed.org/). These are daily data.
For the inventory, as we are working on WTI we used the US energy data from (https:// www.eia.gov/). We decided to focus on the US because this dataset 
is made of weekly data. Thus to get daily data we decided to proceed to an interpolation.

Before to use the USD which is an index we used the exchange rate between USD and Euro because we believed that this exchange rate is a good indictor 
however it is not fully representative of the value of the American currency. Thus we used the ‘nominal broad USD index’ which measures the value of the
US dollar relative to other world currencies (not only euro). By doing so our model went from a Sharpe ratio of ≈0.20 to a Sharpe ratio of 0.42.
For our geopolitical risk reduction we first started to look at the global terrorism database from Kaggle (https://www.kaggle.com/START-UMD/gtd) and we
identified all the attacks which targeted utilities (oil pipeline etc...) But we had some difficulty to use the data.
We decided to use the data of the deaths from conflict and terrorism per 100,000 in the Middle East & North Africa: 
(https://ourworldindata.org/terrorism). We believe that the conflicts in this region may have a strong impact on the price oil. As the data is yearly we 
interpolated daily.
