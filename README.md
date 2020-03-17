# Informing Business Strategy With Hypothesis Testing


## The Objective

Recommend 5 zip codes in the USA (out of a possible c.18,000) in which to invest in property, based in historical property pricing data.

## The Motivation

Property has the potential to be an incredibly lucrative investment. Buying houses in the fastest growing areas in America between 1996-2018 could have given returns of up to 4,000% - massively outperforming pretty much any stock.

However, property investments have the potential to be financially ruinous. Just ask anyone who bought a house in Florida just before the financial crisis. If we're going to invest in property successfully, accurately forecasting house prices by area should allow us to better pick winners, and avoid bear traps.

## The Technologies Used

* Pandas for data munging
* Statsmodels for ARIMA modeling
* Matplotlib and Geopandas for data visualisation


## The Process Overview

1. As can often be the case in data science, we may not have expertise in the subject we're analysing. The first step should therefore be an exploration of the data.

2. Having got to grips with our data, we should then create forecast models for the zip codes. Rather than doing this for every single area, we first narrow our zip codes down to those that seem low-risk, but with the potential to experience high growth.

3. We then use our models to work out which five regions offer the greatest expected returns. We should also consider the confidence intervals for these forecasts, to further mitigate risk.


## The Data

Firstly, a high level view of the data shows how volatile the housing market has been in the last two decades (driven by the financial crisis around 2008-2012).

![MarketTrend](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/PriceTrend.png)

This has significant implications for investors - we can see how different the fortunes of two investors could have been if they'd invested in, respectively, the best and worst zip codes in the 20 years from 1996.

![Investments](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/BestWorstInvestment.jpg)

If we look at the data at a state by state level, we can see how property prices have grown (and fallen) in different areas of the USA:

#### Prices as at April 2018

![PriceByState](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/StatePrices.png)

#### Price growth during financial crisis

![PriceTrend](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/StatePriceGrowth07.png)

#### Price growth since financial crisis

![PriceTrend](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/StatePriceGrowth12.png)


One thing to note here is that whilst some states have grown very strongly in the last few years, many of these states were the ones worst hit by the crash - it may in simply be the case that they've been recovering their value, and don't offer good long-term potential.

![StateGrowth](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/StateGrowth.jpg)

This is all very important context for when we come to fit our forecasting models:

* We should not use the full historical data to train our models. The financial crisis had a material effect on the shape of the time series, but was (we can only hope) a freak exogenous event. If we don’t expect something similar to happen again, then we shouldn’t let our model train itself on that data from that period. We should therefore focus on price data since April 2012…
* However, taking this approach brings its own problems. By using only the last six years of data, a model might assume that regions in states like Nevada and Florida are sure bets, and enthusiastically recommend that we invest there. Of course, we know that growth in these cases was simply recovery from a rough recession. In any case, we should be wary of investing in regions that have the potential to shed half their value in the event of an unforeseen economic shock.


## Selecting Regions

We can get around this by discounting many of our zip codes out of hand straight away (given that we only need to choose 5 anyway).

We plot all the regions on a scatter, with recent price growth on the horizontal and price growth during the financial crisis on the vertical.

![ZipScatter](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/ZipScatter.png)

By looking at regions that are above average in both measures, we can isolate places that both weathered the storm between 2007 and 2012 (demonstrating at least some resilience against economic shocks) and also strong growth potential for future years.

Therefore, these 1,737 ‘Growth Regions’ are all areas that we can, in principle, feel comfortable recommending to investors. Indeed, by indexing April 2007 prices in these regions, we see that they have outperformed other regions since at least 1996.

![GrowthRegions](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/GrowthRegionGrowth.png)


## Modelling Forecasts

Having checked that our data is neither stationary nor seasonal, we can go about producing individual ARIMA models for each zip code.

#### Further detail on ARIMA modelling...

![ARIMA](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/ARIMA.jpg)

Having found an optimal ARIMA model for each zip code, we can see that some regions are forecast to experience very strong growth. However, some forecasts come with a very wide range – and would therefore make for risky investments.

Again, we can focus on the best zip codes - those with strong growth potential, but also with narrow confidence intervals (and thus less intrinsic risk).

![ZipRiskScatter](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/ZipScatter2.png)

Finally, we can pick the five zip codes from these regions that have the highest forecast returns.

![RecommendedRegions](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/Recommendations.jpg)

![ExpectedReturns](https://github.com/calbal91/project-ARIMA-modelling/blob/master/Images/RecommendationReturns.jpg)