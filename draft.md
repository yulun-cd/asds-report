<p style="text-align:right" size=80><font size='+10'><br><br><br>House Price Modelling using GWR</font></p>

<p style='text-align:right'><font size='-1'>Module Name: 6SSG3077 Application of Spatial Data Science</font></p>
<p style='text-align:right'><font size='-1'>Due Date: 14 Jan 2022</font></p>
<p style='text-align:right'><font size='-1'>Student Number: 19040824</font></p>
<p style='text-align:right'><font size='-1'>Word Count: 2411 </font></p>

<div style='page-break-after: always;'></div>

# Introduction

House price has always been one of the central topics of public concern. It is a matter to everyone, not only to those who want to buy a property for their own, but also to those who seek housing investment. The relations between house price and other demographic and socio-economic factors have been widely studied and practised (Algieri, 2013; McQuinn and O'Reilly, 2007; Hashim, 2010), but few of them considered spatial non-stationarity, which is the variation of the relationship across space (Brunsdon *et al.*, 1996). This study aims to use the Geographically Weighted Regression (GWR) model to explore the relations between house prices and a series of socio-economic factors. The result should contribute to the rich contexts of the study on house price estimators.

<div style='page-break-after: always;'></div>

# Method

This study uses data from the LSOA atlas downloaded from London Datastore (2014). All the data was for the year 2011 and was at LSOA (Lower Super Output Area) level. Before analysing the data, a data cleansing was performed, where all LSOAs with 0 value in median house price were removed, as there was no property in those LSOAs. 

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig1.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 1: Spatial distribution of LSOA median house price across London</center>

The median house price data (`MedianHP`) was from Land Registry. The variable `kMedianHP` was it divided by 1000, and was used in this study as an alternative for the purpose of concise. Figure 1 shows its spatial distribution. In general, it reveals a double-hotspot pattern with exceptionally high median house prices in LSOAs in south *Barnet*, *Westminster* and *Kensington and Chelsea*. Median house prices are generally higher in north London than in the south, and higher in the west than in the east.

The socio-economic predictors chosen for this study are listed in Table 1. After checking their correlations with the dependent variable (Table 1), `c_per_hhlds` and `Pct_CHDC` were excluded from the predictors. The multicollinearity of the remaining predictors was then checked using VIF. A VIF smaller than 5 indicates the predictor is safe from multicollinearity (Akinwande *et al.*, 2015). Table 1 shows VIFs for all of the selected factors. All variables had VIFs smaller than 5, suggesting that there was no multicollinearity.

| Variable            | Detail                                                 | Correlation coefficient with the dependent variable | VIF   |
| :------------------ | :----------------------------------------------------- | :-------------------------------------------------- | :---- |
| `MedianIncome`      | Median annual household income                         | 0.542                                               | 3.778 |
| `Pct_nonwhite`      | Percentage of non-white population                     | -0.261                                              | 1.561 |
| `Pct_CHDC`          | Percentage of couple household with dependent child(s) | -0.023                                              | \     |
| `c_per_hhlds`       | Cars per household                                     | 0.067                                               | \     |
| `PTAL_average`      | Average Public Transport Accessibility Level           | 0.122                                               | 1.497 |
| `Pct_qual_above_l4` | Percentage of level 4 and above highest qualification  | 0.507                                               | 3.586 |

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Table 1: Selected independent variables and their correlation coefficient with the dependent variable</center>

The socio-economic predictors used in this study include:

1. Median annual household income (`MedianIncome`). This factor represents the economic dimension. Figure 2.1 shows how it spreads across London. It has a very similar spatial distribution with the median house price data, where southwest London (*Richmond*, *Wandsworth* and *Merton*), central west London (*Westminster* and *K&C*) and northwest London (*Camden* and *Barnet*).
2. Percentages of the non-white population (`Pct_nonwhite`). It is calculated by dividing the non-white population with the total population for each LSOA and represents the ethnic dimension. Figure 2.2 shows its spatial distribution. It has four clear spatial clusters in west London (*Ealing* and *Hounslow*), northwest London (*Brent*), east London (*Redbridge* and *Newham*) and south London (*Croydon*) respectively, where non-white population percentages are the highest across London.
4. Public Transport Accessibility Level (PTAL) is a measure of public transport accessibility (Shah and Adhvaryu, 2016). The PTAL ranges from 0 (very poor) to 6b (very high). 
   The average PTAL (`PTAL_average`) data was from TfL.  It essentially reflects the density of public transport in the area. Figure 2.3 shows its distribution. It follows a radial pattern with a higher PTAL average in Inner London and lower in Outer London, except for some peripheral hotspots including northern parts of *Croydon* and *Richmond* and some LSOAs in *Greenwich*, where the average PTAL is relatively high.
5. Percentage of level 4 and above highest qualification (`Pct_qual_above_l4`). This is the percentage of people who hold a level 4 and above highest qualification. It is a proxy to determine the educational backgrounds of residents in the area. Its spatial distribution pattern is demonstrated in Figure 2.4, which shows higher overall education levels in Inner and southwest London LSOAs while lower in other boroughs'.

<center class='half'><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.1.png" style="zoom: 18.5%;" /><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.2.png" style="zoom: 18.5%;" /></center>
<center class='half'><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.3.png" style="zoom:18.5%;" /><img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig2.4.png" style="zoom:18.5%;" /></center>

|            Variable | Count |      Mean |       Min |    Median |       Max |
| ------------------: | ----: | --------: | --------: | --------: | --------: |
|      `MedianIncome` |  4822 | 35796.332 | 16167.000 | 32652.000 | 92431.000 |
|      `Pct_nonwhite` |  4822 |     0.392 |     0.018 |     0.368 |     0.965 |
|      `PTAL_average` |  4822 |     3.746 |     0.300 |     3.342 |     8.000 |
| `Pct_qual_above_l4` |  4822 |    37.337 |     8.300 |    34.500 |    83.800 |
|         `kMedianHP` |  4822 |   330.354 |    58.000 |   268.000 |  3377.000 |

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Table 2: Descriptive statistics of the dependent and independent variables</center>

Table 2 is the descriptive statistics of the dependent and independent variables.

A simple Ordinary Least Square (OLS) regression model was first developed with the listed predictors as its independent variables and the LSOA median house price as its dependent variable. The model results are presented in Table 3. The global model has an adjusted R^2^ value of 0.55 which means around 55% of the data can be explained by it.

| Variable            | Coefficient | Std.Error | t-Statistic | Probability |
| ------------------- | :---------- | :-------- | :---------- | :---------- |
| Intercept           | -329.656    | 12.372    | -26.645     | 0           |
| `MedianIncome`      | 0.017       | 0.000     | 47.881      | 0           |
| `Pct_nonwhite`      | 112.230     | 12.689    | 8.845       | 0           |
| `PTAL_average`      | 29.236      | 1.577     | 18.539      | 0           |
| `Pct_qual_above_l4` | -2.535      | 0.270     | -9.421      | 0           |

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Table 3: OLS model parameters</center>

However, a global model does not address spatial non-stationarity. In order to capture the potential spatial variations, a two-step approach was carried out. The first step was checking spatial autocorrelation. This was achieved by testing the OLS residuals' global Moran's I, which is a (Li *et al.*, 2007). The global Moran's I of the OLS residuals was 0.402 (p-value < 0.001) which indicated a spatial dependence. Figure 3 shows the distribution of the residuals, from which a clear spatial pattern can be observed.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig3.png" style="zoom:25%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 3: Spatial distribution and global Moran's I of the OLS residuals</center>

Since the existence of spatial autocorrelation, the global model was no longer effective in representing the relationship between the dependent variable and the predictors. As a result, the second step was establishing a local model, namely Geographically Weighted Regression (GWR) model. GWR is a local regression model that captures the process's spatial heterogeneity (Brunsdon *et al.*, 1998). GWR results contain estimated local parameters of each predictor for every regression point, which makes studying the relationships between different predictors and the dependent variable in different places much easier.

Building a GWR model requires determining a kernel that defines the 'local'. In this case, an adaptive Gaussian kernel was selected. A Gaussian kernel was used because the data is continuous across space, so it makes more sense to give closer data points a higher weight.

Apart from the GWR model, a spatial lag model is also built for performance comparison purposes. It is a spatial model that addresses the problem of spatial autocorrelation by taking neighbours into account. However, it does not address spatial non-stationarity. Table 4 shows its result summary.

|Variable|Coefficient|Std.Error|z-Statistic|Probability|
|---|:--|:--|:--|:--|
|Intercept|0.0146925|0.0077473|1.8964647|0.0578986|
|`MedianIncome`|0.6866607|0.0166259|41.3007156|0.0000000|
|`Pct_nonwhite`|0.1269163|0.0096814|13.1093227|0.0000000|
|`PTAL_average`|0.1091396|0.0096403|11.3211226|0.0000000|
|`Pct_qual_above_l4`|-0.2320701|0.0147032|-15.7836786|0.0000000|
|`W_kMedianHP`|0.5935792|0.0117161|50.6634736|0.0000000|

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Table 4: Spatial lag model parameters, the W_kMedianHP is the spatial term. The data used has been standardised.</center>

After the GWR model was built, the model performance was determined based on its corrected Akaike Information Criterion (AICc), adjusted R^2^ and mean squared error (MSE). The estimated parameters were then filtered at 95% confidence level and used to study the spatial heterogeneity in the relationship between median house price and the predictors.

All of the data analysis procedures were accomplished by Python. Detailed process can be acquired from the Appendix.

<div style='page-break-after: always;'></div>

# Results and discussion

## Model performance comparison

Table 5 contains information about model performance of the global and local models. 

| Model       | AICc     | Adjusted R^2^        | MSE      |
| ----------- | -------- | -------------------- | -------- |
| OLS         | 9800.413 | 0.5537               | 0.445966 |
| Spatial Lag | 8028.491 | 0.7114 (Pseudo R^2^) | 0.289345 |
| GWR         | 6269.827 | 0.794                | 0.197463 |

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Table 5: Information about the model comparison. The data used has been standardised.</center>

Three proxies were adopted here - AICc, MSE and adjusted R^2^. Adjusted R^2^ indicates the proportion of the variation in the dependent variable that can be explained by the model. Higher R^2^ means more of the dependent variable can be predicted from the independent variables, thus higher performance of the model. The GWR model's adjusted R^2^  is the highest among the three, which suggests its performance is the highest.

AICc reflects how well a model fits the data while also penalises models with more predictors (Cavanaugh, 1997; Akaike, 1998). Lower AICc generally means better model performance. AICc of the GWR model is the smallest. The MSE results also show that GWR performed the best out of the three models.

## GWR results

|Variable|Mean|STD|Min|Median|Max|
|---|---|---|---|---|---|
|Intercept|0.028  |    0.465   |  -0.671 |    -0.104   |   1.436|
|`MedianIncome`|0.707 |     0.408  |    0.224   |   0.546    |  1.962|
|`Pct_nonwhite`|-0.003   |   0.189  |   -0.938   |   0.022    |  0.525|
|`PTAL_average`|0.040   |   0.101  |   -0.153    |  0.016    |  0.426|
|`Pct_qual_above_l4`|-0.279    |  0.361   |  -1.583 |-0.152  |    0.210|

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Table 6: Summary statistics for GWR parameter estimates. The data used has been standardised.</center>

The summary statistics for GWR parameter estimates are presented in table 6. Four independent variables - median annual household income, percentage of non-white population, average Public Transport Accessibility Level (PTAL) and percentage of level 4 and above highest qualification - were used to predict median house price for each London LSOA. An adaptive gaussian kernel with a bandwidth of 51 (51 nearest neighbours) was adopted. Their relationships with the dependent variable will be discussed below respectively.

### Median annual household income

The median annual household income data has a very similar spatial distribution to the median house price (figure 1 and figure 2.1). This is possibly due to the fact that higher household income means higher purchase and repayment affordability (Gan and Hill, 2009), which further means higher mortgage the household can get on the market. House price to income ratio (PIR) is a very important indicator of affordability in the housing market (Chen and Cheng, 2017; Leung and Tang, 2021) for a very similar reason.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig4.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 4: statistically significant (95%) GWR estimates for median annual household income</center>

The GWR model parameter estimates for the median annual household income show a clear spatial variation in the relation, as shown in figure 4. Almost all LSOAs in London detects a positive effect. The size of this effect is between 1.98 to 7.79 in most areas, which means a one-unit increase in median income is related to a 1.98- to 7.79- unit increase in median house price. In some areas (mainly southwest to north London) the estimated effect size increases to higher ranges. *Westminster*, *K&C* and southern part of *Barnet* have the highest parameter estimates (above 37 and up to 52.17), which coincides with the areas with the highest median house prices and median annual household incomes. Other parameter estimates ranges are also roughly consistent with the corresponding median house price data and median income data ranges. This explains the extraordinarily high median house prices in these areas. Average house price level (`kMedianHP` as an indicator) will rise more quickly with average income level (`MedianIncome` as an indicator) in places where the income level is already higher compared to others.

### Percentage of non-white population

Ethnicity is a huge factor in mortgage lending (Haughwout *et al.*, 2009; Bocian *et al.*, 2008), especially in high-risk mortgages (Bayer *et al.*, 2018). Figure 5 shows the spatial pattern of the GWR estimates for the percentage of the non-white population. 

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig5.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 5: statistically significant (95%) GWR estimates for the percentage of the non-white population</center>

The estimates have very huge spatial variations across London. On one hand, the estimates are negative for most LSOAs in Inner London, including *the City of London*, northern parts of *Southwark* and *Lambeth*, much of *Westminster*, *Hackney* and *Tower Hamlets*, and northern parts of *K&C* and *Hammersmith and Fulham*. The negative estimates area also reaches *Brent*, eastern parts of *Ealing* and *Hounslow*, north-eastern part of *Richmond* and western part of *Haringey*. In these areas, the percentage of the non-white population is predicted to have a negative effect on house prices, with the size of the effect ranging from -4.45 to -2.05 in most areas, and up to -8.49 in some regions (areas around *the City of London* and from east *Ealing* to *Westminster*).

On the other hand, there are places in London where the parameter estimates are positive. Such places are mainly in Outer London, including areas in the north (*Enfield*, *Barnet*, *Haringey* and *Camden*), the southwest (from north *Kingston* and *Merton* all the way to south *Croydon*), the northwest and southeast ends and some parts in east London. Several areas in Inner London also detect positive relations, including a small area in *H&F* and southern part of *Southwark*. The effect size in most areas is between 0 to 3.65, while some areas in north and southwest London have an effect size up to 7.35.

The reasons behind the spatial heterogeneity of the percentage of non-whites in relation to median house price need to be further investigated, but the presence of it indicates that the percentage of the non-white population might not be the determining factor, but it has an influence on the house price, and there are likely to be other variables closely associated with it that have a huge impact on the dependent variable.

### Average Public Transport Accessibility Level

Public transport links an area with others. It is a very important dimension in social connectivity and allows people, especially those without private vehicles, to participate in activities that are not in their areas (Fransen *et al.*, 2015). The spatial distribution of GWR parameter estimates for average PTAL is shown in figure 6. Most significant estimates are positive, which mainly spread along the Thames, from *Richmond* and *Hounslow* in the west to *Greenwich* and *Newham* in the east, covering most areas in Inner London. They also extend further north to eastern part of *Ealing*, *Brent* and southern part of *Barnet*. The effect size rises gradually from the border in this area, reaching 6.55 to 10.4 in the centre parts, including *the City of London*, *H&F*, most parts of *K&C*, and a small area in south *Barnet*. What's worth mentioning is that most parts of *Westminster* and *Camden* are excluded from this area. In fact, a small area of the latter, together with a part of *Enfield*, have negative parameter estimates ranging from -2.69 to -1.95.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig6.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 6: statistically significant (95%) GWR estimates for average PTAL</center>

Although very strong estimated relations in some areas, the parameter estimates in the majority parts of London are not statistically significant. This suggests that the average PTAL is only a great estimator in certain parts and beyond that, it cannot be considered as an indicator for house price.

### Percentage of level 4 and above highest qualification

Figure 7 shows the variations of the percentage of level 4 and above highest qualification in relation to the median house price. It is a negative estimator in all significant areas, with an effect size between -24.62 and -16.6 in riverbank areas in *H&F*, *Wandsworth*, *K&C* and *Westminster* along the Thames, as well as in neighbouring areas between *Camden*, *Haringey* and *Barnet*. This means that a one-unit increase in `Pct_qual_above_l4` is associated with a 16.6 to 24.62 units drop in median house price.

<img src="D:\OneDrive - King's College London\Study\Year 3\6SSG3077 Applications of Spatial Data Science\Report\graph\fig7.png" style="zoom:30%;" />

<center style="font-size:11px;color:#c0c0c0;font-family:sans-serif">Figure 7: statistically significant (95%) GWR estimates for the percentage of level 4 and above highest qualification</center>

However, such negative relation is quite localised. The effect size drops quickly to close to zero (-4.61 to -1.96) or becomes insignificant in areas like *the City of London*, *Islington*, *Harrow*, *Brent*, *Hounslow* and most boroughs south of the Thames.

<div style='page-break-after: always;'></div>

# Conclusion and limitation

The relations between house prices and socio-economic factors are closely associated with their spatial locations. The GWR model not only performs better than a traditional OLS model and a global spatial lag model, but also has more concrete outcomes that explain the relationships under a geographical context. Further studies can be developed based on this result regarding the reasons behind the spatial heterogeneity of their relations, as well as exploring more significant factors affecting house prices.

The biggest flaw of this study is the failure of using MGWR (multiscale GWR) due to the limitation of computational power. There is a high probability that MGWR would have a better performance than GWR because the former uses optimum bandwidths in each iteration so it eliminates the assumption that the variations of different variables occur within the same scale (Shabrina *et al.*, 2021). 

<div style='page-break-after: always;'></div>

# Reference List

- Akaike, H. (1998) Information theory and an extension of the maximum likelihood principle. In: *Selected papers of hirotugu akaike.* New York: Springer, 1998, 199-213.
- Akinwande, M.O., Dikko, H.G. and Samson, A. (2015) Variance Inflation Factor: As a Condition for the Inclusion of Suppressor Variable(s) in Regression Analysis. *Open Journal of Statistics, 05*, 754-767.
- Algieri, B. (2013) House price determinants: Fundamentals and underlying factors. *Comparative Economic Studies*, *55*(2), 315-341.
- Bayer, P., Ferreira, F. and Ross, S. L. (2018) What drives racial and ethnic differences in high-cost mortgages? The role of high-risk lenders. *The Review of Financial Studies*, *31*(1), 175-205.
- Bocian, D. G., Ernst, K. S. and Li, W. (2008) Race, ethnicity and subprime home loan pricing. *Journal of Economics and Business*, *60*(1-2), 110-124.
- Brunsdon, C., Fotheringham, A. S. and Charlton, M. E. (1996) Geographically weighted regression: a method for exploring spatial nonstationarity. *Geographical analysis*, *28*(4), 281-298.
- Brunsdon, C., Fotheringham, S. and Charlton, M. (1998) Geographically weighted regression. *Journal of the Royal Statistical Society: Series D (The Statistician)*, *47*(3), 431-443.
- Cavanaugh, J. E. (1997) Unifying the derivations for the Akaike and corrected Akaike information criteria. *Statistics & Probability Letters*, *33*(2), 201-208.
- Chen, N. K. and Cheng, H. L. (2017) House price to income ratio and fundamentals: Evidence on long‐horizon forecastability. *Pacific Economic Review*, *22*(3), 293-311.
- Fransen, K., Neutens, T., Farber, S., De Maeyer, P., Deruyter, G. and Witlox, F. (2015) Identifying public transport gaps using time-dependent accessibility levels. *Journal of Transport Geography*, *48*, 176-187.
- Gan, Q. and Hill, R. J. (2009) Measuring housing affordability: Looking beyond the median. *Journal of Housing economics*, *18*(2), 115-125.
- Hashim, Z.A (2010) House Price and Affordability in Housing in Malaysia. *Akademika, 78*.
- Haughwout, A., Mayer, C., Tracy, J., Jaffee, D.M. and Piskorski, T. (2009) Subprime mortgage pricing: the impact of race, ethnicity, and gender on the cost of borrowing. *Brookings-Wharton Papers on Urban Affairs*, 33-63.
- Leung, C. K. Y. and Tang, E. C. H. (2021) The dynamics of the house price‐to‐income ratio: Theory and evidence. *Contemporary Economic Policy*.
- Li, H., Calder, C. A. and Cressie, N. (2007) Beyond Moran's I: testing for spatial dependence based on the spatial autoregressive model. *Geographical Analysis*, *39*(4), 357-375.
- London Datastore. (2014) LSOA Atlas. [Data file] Available from: https://data.london.gov.uk/download/lsoa-atlas/b8e01c3a-f5e3-4417-82b3-02ad271e6ee8/lsoa-data.xls [Accessed 29 Dec 2021]
- McQuinn, K. and O’Reilly, G. (2007) A model of cross-country house prices. *Research Technical Paper*, *5*.
- Shabrina, Z., Buyuklieva, B. and Ng, M. K. M. (2021) Short‐Term Rental Platform in the Urban Tourism Context: A Geographically Weighted Regression (GWR) and a Multiscale GWR (MGWR) Approaches. *Geographical Analysis*, *53*(4), 686-707.
- Shah, J. S. and Adhvaryu, B. (2016) Public transport accessibility levels for Ahmedabad, India. *Journal of Public Transportation*, *19*(3), 2.

<div style='page-break-after: always;'></div>

<p style="text-align:right"><font size='+10'><br><br><br>Appendix</font></p>
