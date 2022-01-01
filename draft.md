# Introduction

<div style='page-break-after: always;'></div>

# Method

This study uses data from LSOA atlas downloaded from London Datastore (2014). All the data was for the year of 2011 and was at LSOA (Lower Super Output Area) level. Before analysing the data, a data cleansing was performed, where all LSOAs with 0 value in median house price were removed, as there was no property in those LSOAs. 

The meidan house price data (`MedianHP`) was from Land Registry. The variable `kMedianHP` was it divided by 1000, and was used in this study as an alternative for the purpose of concise. **Figure 1** shows its spatial distribution. In general, it reveals a double-hotspot pattern with exceptionally high median house prices in LSOAs in south Barnet, Camden, Westminster and Kensington and Chelsea. Median house prices are generally higher in the north London than in the south, and higher in the west than in the east.

The socio-economic predictors used in this study include:

1. Median annual household income (`MedianIncome`). **Figure 2a** shows how it spreads.
2. Percentages of nonwhite population (`Pct_nonwhite`) and working age (16-64) population (`Pct_workingage`). The two percentages were calculated by dividing nonwhite population/working age population with total population. **Figure 2b and 2c** show their spatial distributions respectively.
3. Unemployment rate data (`UnRate`) came from Census 2011. **Figure 2d** 
4. Public Transport Accessibility Level (PTAL) is a measure of public transport accessibility (Shah and Adhvaryu, 2016). It essentially reflects the density of public transport in the area. The PTAL ranges from 0 (very poor) to 6b (very high). 
   The average PTAL (`PTAL_average`) data was from TfL. **Figure 2e** shows its distribution.
5. Percentage of highest level of qualification above level 4 (`Pct_qual_above_l4`). However, it was decided to be removed from the predictors due to multicollinearity.

The multicollinearity of the chosen predictors is checked using VIFs. A VIF smaller than 5 indicates the predictor is safe from multicollinearity (==citation on VIF==). **Table 1** shows VIFs for all of the selected factors. Two of them (`MedianIncome` and `Pct_qual_above_l4`) have VIF that is close to or above 5.

A simple Ordinary Least Square (OLS) regression model was first developed with the listed predictors as its independent variables and the LSOA median house price as its dependent variable. The model results are presented in **figure 3**. The global model has an adjusted R^2^ value of ==0.7== which means 70% of the data can be explained by it.

However, a global model does not address spatial non-stationarity,  which is the variation of the relation across space (==citation on spatial non-stationarity==). In order to capture the potential spatial variations, a two-step approach was carried out. The first step was checking spatial autocorrelation. This was achieved by testing the OLS residuals' global Moran's I, which is a (==citation on moran's I==). The global Moran's I of the OLS residuals was 0.37 (p value < 0.01) which indicated a spatial dependence. **Figure 4** shows the distribution of the residuals, from which a clear spatial pattern can be observed.

Since the existence of spatial autocorrelation, the global model was no longer effective in representing the relationship between the dependent variable and the predictors. As a result, the second step was establishing a local model, namely Geographically Weighted Regression (GWR) model. GWR is a local regression model that captures process's spatial heterogeneity (==citation on GWR==). GWR results contain estimated local parameters of each predictors for every regression point, which makes studying the relationships between different predictors and the dependent variable in different places much easier. 

Building a GWR model requires determining a kernel which defines the 'local'. In this case, an adaptive gaussian kernel was selected. A gaussian kernal was used because the data is  continuous across space, so it makes more sense to give closer data point higher weight.

<div style='page-break-after: always;'></div>

# Results

<div style='page-break-after: always;'></div>

# Conclusion

<div style='page-break-after: always;'></div>
