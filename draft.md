# Introduction

<div style='page-break-after: always;'></div>

# Method

This study uses data from LSOA atlas downloaded from London Datastore (2014). All the data was for the year of 2011 and was at LSOA (Lower Super Output Area) level. Before analysing the data, a data cleansing was performed, where all LSOAs with 0 value in median house price were removed, as there was no property in those LSOAs. 

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig1.png" style="zoom:36%;" />
<center style="font-size:11px;color:#000000;font-family:sans-serif">Figure 1: Spatial distribution of LSOA median house price across London</center>



The median house price data (`MedianHP`) was from Land Registry. The variable `kMedianHP` was it divided by 1000, and was used in this study as an alternative for the purpose of concise. Figure 1 shows its spatial distribution. In general, it reveals a double-hotspot pattern with exceptionally high median house prices in LSOAs in south Barnet, Camden, Westminster and Kensington and Chelsea. Median house prices are generally higher in the north London than in the south, and higher in the west than in the east.

The socio-economic predictors chosen for this study are listed in **table 1**. After checking their correlations with the dependent variable (**table 1**), `c_per_hhlds` and `Pct_CHDC` were excluded from the predictors. The multicollinearity of the remaining predictors was then checked using VIF. A VIF smaller than 5 indicates the predictor is safe from multicollinearity (==citation on VIF==). **Table 2** shows VIFs for all of the selected factors. All variables had VIFs smaller than 5, suggesting that there was no multicollinearity.

The socio-economic predictors used in this study include:

1. Median annual household income (`MedianIncome`). This factor represents the economic dimension. Figure 2.1 shows how it spreads across London. It has a very similar spatial distribution with the median house price data, where southwest London (*Richmond*, *Wandsworth* and *Merton*), central west London (*Westminster* and *Kensington and Chelsea*) and northwest London (*Camden* and *Barnet*).
2. Percentages of non-white population (`Pct_nonwhite`). It is calculated by dividing non-white population with total population for each LSOA and represents the ethnic dimension. Figure 2.2 shows its spatial distribution. It has four clear spatial clusters in west London (*Ealing* and *Hounslow*), northwest London (*Brent*), east London (*Redbridge* and *Newham*) and south London (*Croydon*) where non-white population percentages are the highest across London.
4. Public Transport Accessibility Level (PTAL) is a measure of public transport accessibility (Shah and Adhvaryu, 2016). The PTAL ranges from 0 (very poor) to 6b (very high). 
   The average PTAL (`PTAL_average`) data was from TfL.  It essentially reflects the density of public transport in the area. Figure 2.3 shows its distribution. It basically follows a radial pattern with higher PTAL average in inner London and lower in outer London, except some peripheral hotspots including *north Croydon*, *north Richmond* and some LSOAs in *Greenwich*, where the average PTAL is relatively high.
5. Percentage of highest level of qualification above level 4 (`Pct_qual_above_l4`). This is the percentage of people who hold a level 4 and above highest qualification. It is a proxy to determine the education backgrounds of residents in the area. Its spatial distribution pattern is demonstrated in figure 2.4, which shows higher overall education levels in inner and southwest London LSOAs while lower in other boroughs'.

<center class='half'><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.1.png" style="zoom: 17%;" /><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.2.png" style="zoom: 17%;" /></center>
<center class='half'><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.3.png" style="zoom:17%;" /><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.4.png" style="zoom:17%;" /></center>

**Table 3** is the descriptive statistics of the dependent and independent variables.

A simple Ordinary Least Square (OLS) regression model was first developed with the listed predictors as its independent variables and the LSOA median house price as its dependent variable. The model results are presented in **table 4**. The global model has an adjusted R^2^ value of ==0.55== which means around 55% of the data can be explained by it.

However, a global model does not address spatial non-stationarity,  which is the variation of the relation across space (==citation on spatial non-stationarity==). In order to capture the potential spatial variations, a two-step approach was carried out. The first step was checking spatial autocorrelation. This was achieved by testing the OLS residuals' global Moran's I, which is a (==citation on moran's I==). The global Moran's I of the OLS residuals was 0.402 (p value < 0.001) which indicated a spatial dependence. Figure 3 shows the distribution of the residuals, from which a clear spatial pattern can be observed.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig3.png" style="zoom:36%;" />

<center style="font-size:11px;color:#000000;font-family:sans-serif">Figure 3: Spatial distribution and global Moran's I of the OLS residuals</center>

Since the existence of spatial autocorrelation, the global model was no longer effective in representing the relationship between the dependent variable and the predictors. As a result, the second step was establishing a local model, namely Geographically Weighted Regression (GWR) model. GWR is a local regression model that captures process's spatial heterogeneity (==citation on GWR==). GWR results contain estimated local parameters of each predictors for every regression point, which makes studying the relationships between different predictors and the dependent variable in different places much easier. 

Building a GWR model requires determining a kernel which defines the 'local'. In this case, an adaptive gaussian kernel was selected. A gaussian kernel was used because the data is  continuous across space, so it makes more sense to give closer data point higher weight.

After the GWR model was built, the model performance was determined based on its corrected Akaike Information Criterion (AICc) and adjusted R^2^. The estimated parameters were then used to study the spatial heterogeneity in the relationship between median house price and the predictors.

All of the data analysis procedures were accomplished by Python. Detailed process can be acquired from the Appendix.

<div style='page-break-after: always;'></div>

# Results

## Model performance comparison

**Table 5** contains information about model performance of the global and local models. 

Two proxies were adopted here - AICc and adjusted R^2^. Adjusted R^2^ indicates the proportion of the variation in the dependent variable that can be explained by the model. Higher R^2^ means more of the dependent variable can be predicted from the independent variables, thus higher performance of the model. The OLS model's adjusted R^2^  is lower than that of the GWR model, which suggests a higher performance of the latter.

AICc reflects how well a model fits the data while also penalises models with more predictors (==citation on AICc==). Lower AICc generally means better model performance. AICc of the GWR model is smaller than that of the OLS model.

As mentioned previously, the residuals of the OLS model showed strong spatial dependence (global Moran's I = 0.402, p <0.001). The GWR's residuals, on the other hand, showed almost no spatial autocorrelation (figure 4).

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig4.png" style="zoom:36%;" />

<center style="font-size:11px;color:#000000;font-family:sans-serif">Figure 4: Spatial distribution and global Moran's I of the GWR residuals</center>

### GWR results

The summary of the GWR result is presented in **table 6**. Four independent variables - median annual household income, percentage of non-white population, average Public Transport Accessibility Level (PTAL) and percentage of level 4 and above highest qualification - were used to predict median house price for each London LSOA. Their relationships with the dependent variable will be discussed below respectively.

##### Median annual household income



<div style='page-break-after: always;'></div>

# Conclusion



<div style='page-break-after: always;'></div>
