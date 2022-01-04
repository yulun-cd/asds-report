# Introduction

House price has always been one of the central topics of the public concern. It is a matter to everyone, not only to those who want to buy a property for their own, but also to those who seek housing investment. The relations between house price and other demographic and socio-economic factors has been widely studied and practiced (Algieri, 2013; McQuinn and O'Reilly, 2007; Hashim, 2010), but few of them considered spatial non-stationarity, which is the variation of the relation across space (Brunsdon *et al.*, 1996). This study aims to use Geographically Weighted Regression (GWR) model to explore the relations between house price and a series of socio-economic factors. The result should contribute to the rich contexts of the study on house price estimators.

<div style='page-break-after: always;'></div>

# Method

This study uses data from LSOA atlas downloaded from London Datastore (2014). All the data was for the year of 2011 and was at LSOA (Lower Super Output Area) level. Before analysing the data, a data cleansing was performed, where all LSOAs with 0 value in median house price were removed, as there was no property in those LSOAs. 

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig1.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 1: Spatial distribution of LSOA median house price across London</center>

The median house price data (`MedianHP`) was from Land Registry. The variable `kMedianHP` was it divided by 1000, and was used in this study as an alternative for the purpose of concise. Figure 1 shows its spatial distribution. In general, it reveals a double-hotspot pattern with exceptionally high median house prices in LSOAs in south *Barnet*, *Westminster* and *Kensington and Chelsea*. Median house prices are generally higher in the north London than in the south, and higher in the west than in the east.

The socio-economic predictors chosen for this study are listed in **table 1**. After checking their correlations with the dependent variable (**table 1**), `c_per_hhlds` and `Pct_CHDC` were excluded from the predictors. The multicollinearity of the remaining predictors was then checked using VIF. A VIF smaller than 5 indicates the predictor is safe from multicollinearity (Akinwande *et al.*, 2015). **Table 2** shows VIFs for all of the selected factors. All variables had VIFs smaller than 5, suggesting that there was no multicollinearity.

The socio-economic predictors used in this study include:

1. Median annual household income (`MedianIncome`). This factor represents the economic dimension. Figure 2.1 shows how it spreads across London. It has a very similar spatial distribution with the median house price data, where southwest London (*Richmond*, *Wandsworth* and *Merton*), central west London (*Westminster* and *K&C*) and northwest London (*Camden* and *Barnet*).
2. Percentages of non-white population (`Pct_nonwhite`). It is calculated by dividing non-white population with total population for each LSOA and represents the ethnic dimension. Figure 2.2 shows its spatial distribution. It has four clear spatial clusters in west London (*Ealing* and *Hounslow*), northwest London (*Brent*), east London (*Redbridge* and *Newham*) and south London (*Croydon*) respectively, where non-white population percentages are the highest across London.
4. Public Transport Accessibility Level (PTAL) is a measure of public transport accessibility (Shah and Adhvaryu, 2016). The PTAL ranges from 0 (very poor) to 6b (very high). 
   The average PTAL (`PTAL_average`) data was from TfL.  It essentially reflects the density of public transport in the area. Figure 2.3 shows its distribution. It basically follows a radial pattern with higher PTAL average in Inner London and lower in Outer London, except some peripheral hotspots including *north Croydon*, *north Richmond* and some LSOAs in *Greenwich*, where the average PTAL is relatively high.
5. Percentage of highest level of qualification above level 4 (`Pct_qual_above_l4`). This is the percentage of people who hold a level 4 and above highest qualification. It is a proxy to determine the education backgrounds of residents in the area. Its spatial distribution pattern is demonstrated in figure 2.4, which shows higher overall education levels in Inner and southwest London LSOAs while lower in other boroughs'.

<center class='half'><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.1.png" style="zoom: 17%;" /><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.2.png" style="zoom: 17%;" /></center>
<center class='half'><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.3.png" style="zoom:17%;" /><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.4.png" style="zoom:17%;" /></center>

**Table 3** is the descriptive statistics of the dependent and independent variables.

A simple Ordinary Least Square (OLS) regression model was first developed with the listed predictors as its independent variables and the LSOA median house price as its dependent variable. The model results are presented in **table 4**. The global model has an adjusted R^2^ value of ==0.55== which means around 55% of the data can be explained by it.

However, a global model does not address spatial non-stationarity. In order to capture the potential spatial variations, a two-step approach was carried out. The first step was checking spatial autocorrelation. This was achieved by testing the OLS residuals' global Moran's I, which is a (Li *et al.*, 2007). The global Moran's I of the OLS residuals was 0.402 (p value < 0.001) which indicated a spatial dependence. Figure 3 shows the distribution of the residuals, from which a clear spatial pattern can be observed.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig3.png" style="zoom:25%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 3: Spatial distribution and global Moran's I of the OLS residuals</center>

Since the existence of spatial autocorrelation, the global model was no longer effective in representing the relationship between the dependent variable and the predictors. As a result, the second step was establishing a local model, namely Geographically Weighted Regression (GWR) model. GWR is a local regression model that captures process's spatial heterogeneity (Brunsdon *et al.*, 1998). GWR results contain estimated local parameters of each predictors for every regression point, which makes studying the relationships between different predictors and the dependent variable in different places much easier.

Building a GWR model requires determining a kernel which defines the 'local'. In this case, an adaptive gaussian kernel was selected. A gaussian kernel was used because the data is  continuous across space, so it makes more sense to give closer data point higher weight.

Apart from the GWR model, a spatial lag model is also built for performance comparison purpose. It is a spatial model that address the problem of spatial autocorrelation by taking neighbours into account. However, it does not address the spatial non-stationarity.

After the GWR model was built, the model performance was determined based on its corrected Akaike Information Criterion (AICc), adjusted R^2^ and mean squared error (MSE). The estimated parameters were then filtered at 95% confidence level and used to study the spatial heterogeneity in the relationship between median house price and the predictors.

All of the data analysis procedures were accomplished by Python. Detailed process can be acquired from the Appendix.

<div style='page-break-after: always;'></div>

# Results and discussion

### Model performance comparison

**Table 5** contains information about model performance of the global and local models. 

Three proxies were adopted here - AICc, MSE and adjusted R^2^. Adjusted R^2^ indicates the proportion of the variation in the dependent variable that can be explained by the model. Higher R^2^ means more of the dependent variable can be predicted from the independent variables, thus higher performance of the model. The GWR model's adjusted R^2^  is the highest among the three, which suggests its performance is the highest.

AICc reflects how well a model fits the data while also penalises models with more predictors (Cavanaugh, 1997; Akaike, 1998). Lower AICc generally means better model performance. AICc of the GWR model is the smallest. The MSE results also show that GWR performed the best out of the three models.

### GWR results

The summary of the GWR result is presented in **table 6**. Four independent variables - median annual household income, percentage of non-white population, average Public Transport Accessibility Level (PTAL) and percentage of level 4 and above highest qualification - were used to predict median house price for each London LSOA. An adaptive gaussian kernel with a bandwidth of 51 (51 nearest neighbours) was adopted. Their relationships with the dependent variable will be discussed below respectively.

#### Median annual household income

The median annual household income data has a very similar spatial distribution to the median house price (figure 1 and figure 2.1). This is possibly due to the fact that higher household income means higher purchase and repayment affordability (Gan and Hill, 2009), which further means higher mortgage the household can get on the market. House price to income ratio (PIR) is a very important indicator of affordability in house market (Chen and Cheng, 2017; Leung and Tang, 2021) for the very similar reason.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig5.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 5: statistically significant (95%) GWR estimates for median annual household income</center>

The GWR model parameter estimates for the median annual household income show a clear spatial variation in the relation, as shown in figure 5. Almost all LSOAs in London detects a positive effect. The size of this effect is between 1.98 to 7.79 in most areas, which means a one unit increase in median income is related to a 1.98 to 7.79 units increase in median house price. In some areas (mainly southwest to north London) the estimated effect size increases to higher ranges. *Westminster*, *K&C* and southern part of *Barnet* have the highest parameter estimates (above 37 and up to 52.17), which coincides with the areas with the highest median house prices and median annual household incomes. Other parameter estimates ranges are also roughly consistent with the corresponding median house price data and median income data ranges. This explains the extraordinarily high median house prices in these areas. Average house price level (`kMedianHP` as an indicator) will rise more quickly with average income level (`MedianIncome` as an indicator) in places where the income level is already higher compared to others.

#### Percentage of non-white population

Ethnicity is a huge factor in mortgage lending (Haughwout *et al.*, 2009; Bocian *et al.*, 2008), especially in high-risk mortgage (Bayer *et al.*, 2018). Figure 6 shows the spatial pattern of the GWR estimates for percentage of non-white population. 

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig6.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 6: statistically significant (95%) GWR estimates for percentage of non-white population</center>

The estimates have very huge spatial variations across London. On one hand, the estimates are negative for most LSOAs in Inner London, including *the City of London*, northern parts of *Southwark* and *Lambeth*, much of *Westminster*, *Hackney* and *Tower Hamlets*, and northern parts of *K&C* and *Hammersmith and Fulham*. The negative estimates area also reaches *Brent*, eastern parts of *Ealing* and *Hounslow*, north-eastern part of *Richmond* and western part of *Haringey*. In these areas, the percentage of non-white population is predicted to have a negative effect on house prices, with the size of the effect ranging from -4.45 to -2.05 in most areas, and up to -8.49 in some regions (areas around *the City of London* and from east *Ealing* to *Westminster*).

On the other hand, there are places in London where the parameter estimates are positive. Such places are mainly in Outer London, including areas in the north (*Enfield*, *Barnet*, *Haringey* and *Camden*), the southwest (from north *Kingston* and *Merton* all the way to south *Croydon*), the northwest and southeast ends and some parts in east London. Several areas in Inner London also detect positive relations, including a small area in *H&F* and southern part of *Southwark*. The effect size in most areas is between 0 to 3.65, while some areas in north and southwest London have an effect size up to 7.35.

The reasons behind the spatial heterogeneity of the percentage of non-whites in relation to median house price need to be further investigated, but the presence of it indicates that the percentage of non-white population might not be the determining factor, but it has influence on the house price, and there are likely to be other variables closely associated with it that have huge impact on the dependent variable.

#### Average Public Transport Accessibility Level

Public transport links an area with others. It is a very important dimension in social connectivity and allows people, especially those without private vehicles, to participate in activities that are not in their areas (Fransen *et al.*, 2015). The spatial distribution of GWR parameter estimates for average PTAL is shown in figure 7. Most significant estimates are positive, which mainly spread along the Thames, from *Richmond* and *Hounslow* in the west to *Greenwich* and *Newham* in the east, covering most areas in Inner London. They also extend further north to eastern part of *Ealing*, *Brent* and southern part of *Barnet*. The effect size rises gradually from the border in this area, reaching 6.55 to 10.4 in the centre parts, including *the City of London*, *H&F*, most parts of *K&C*, and a small area in south *Barnet*. What's worth mentioning is that most parts of *Westminster* and *Camden* are excluded from this area. In fact, a small area of the latter, together with a part of *Enfield*, have has negative parameter estimates ranging from -2.69 to -1.95.

Although very strong estimated relations in some areas, the parameter estimates in the majority parts of London are not statistically significant. This suggests that the average PTAL is only a great estimator in certain parts and beyond that it cannot be considered as an indicator for house price.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig7.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 7: statistically significant (95%) GWR estimates for average PTAL</center>

#### Percentage of level 4 and above highest qualification

Figure 8 shows the variations of the percentage of level 4 and above highest qualification in relation to the median house price. It is a negative estimator in all significant areas, with an effect size between -24.62 and -16.6 in river bank areas in *H&F*, *Wandsworth*, *K&C* and *Westminster* along the Thames, as well as in neighbouring areas between *Camden*, *Haringey* and *Barnet*. This means that a one unit increase in `Pct_qual_above_l4` is associated with a 16.6 to 24.62 units drop in median house price.

However, such negative relation is quite localised. The effect size drops quickly to close to zero (-4.61 to -1.96) or becomes insignificant in areas like *the City of London*, *Islington*, *Harrow*, *Brent*, *Hounslow* and most boroughs south of the Thames.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig8.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 8: statistically significant (95%) GWR estimates for percentage of level 4 and above highest qualification</center>

<div style='page-break-after: always;'></div>

# Conclusion and limitation

The relations between house price and socio-economic factors are closely associated with their spatial locations. The GWR model not only performs better than a traditional OLS model and a global spatial lag model, but also has more concrete outcomes that explain the relationships under a geographical context. Further studies can be developed based on this results regarding the reasons behind the spatial heterogeneity of their relations, as well as exploring more significant factors affecting house prices.

The biggest flaw of this study is the failure of using MGWR (multiscale GWR) due to limitation of computational power. There is a high probability that MGWR would have a better performance than GWR because the former uses optimum bandwidths in each iteration so it eliminates the assumption that the variations of different variables occur within the same scale (Shabrina *et al.*, 2021). 

<div style='page-break-after: always;'></div>
