# Introduction

<div style='page-break-after: always;'></div>

# Method

This study uses data from Census 2011 downloaded from London Datastore (2014). Before analysing the data, a data cleansing was performed. **Figure 1** shows the spatial distribution of the London LSOA median house prices. 

The socio-economic predictors used in this study are listed in **table 1**. ==Some explanations on the predictors==. **Figure 2** shows their spatial distributions. The multicollinearity of the chosen predictors is checked using VIFs. A VIF smaller than 5 indicates the predictor is safe from multicollinearity (==citation on VIF==). **Table 2** shows VIFs for all of the selected factors. None of the variables showed multicollinearity as there was no VIF greater than 5.

A simple Ordinary Least Square (OLS) regression model was first developed with the listed predictors as the independent variables and the LSOA median house price as the dependent variable. The model results are presented in **figure 3**. The global model has an adjusted R^2^ value of ==0.7== which means 70% of the data can be explained by it.

However, a global model does not address spatial non-stationarity,  which is the variation of the relation across space (==citation on spatial non-stationarity==). In order to capture the potential spatial variations, a two-step approach was carried out. The first step was checking spatial autocorrelation. This was achieved by testing the OLS residuals' global Moran's I, which is a (==citation on moran's I==). The global Moran's I of the OLS residuals was 0.37 (p value < 0.01) which indicated a spatial dependence. **Figure 4** shows the distribution of the residuals, from which a clear spatial pattern can be observed.

Since the existence of spatial autocorrelation, the global model was no longer effective in representing the relationship between the dependent variable and the predictors. As a result, the second step was establishing a local model, namely Geographically Weighted Regression (GWR) model. GWR is a local regression model that captures process's spatial heterogeneity (==citation on GWR==). GWR results contain estimated local parameters of each predictors for every regression point, which makes studying the relationships between different predictors and the dependent variable in different places much easier. 

Building a GWR model requires determining a kernel which defines the 'local'. In this case, an adaptive gaussian kernel was selected with a bandwidth of 57 meters. A gaussian kernal was used because the data is  continuous across space, so it makes more sense to give closer data point higher weight.

<div style='page-break-after: always;'></div>

# Results

<div style='page-break-after: always;'></div>

# Conclusion

<div style='page-break-after: always;'></div>
