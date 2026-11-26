# Global Power Plant Capstone: Capacity, Output and the Saudi Data Gap

**Author:** Husnain Zahoor | Riyadh, Saudi Arabia | Lumen Data Analytics Program, capstone project

**Live storyboard (Tableau Public):** https://tinyurl.com/y5hbb52n

## What this project asks

How does installed power plant capacity vary across countries and fuel types worldwide, and, in the plants that report actual 2017 generation (mostly US), which plant characteristics best explain differences in output? The answer is built from four analyses: a country map, plant clustering, a regression and a fuel-level change over time.

The project also shows a gap that matters for Saudi Arabia. It ranks #12 of 164 countries for installed capacity (84,219 MW, 86 plants), but no Saudi plant reports generation in this database. All generation-based results therefore describe mainly US plants and are a benchmark, not a Saudi or global result.

## Key findings

|Question|Method|Result|
|-|-|-|
|1. Where is the capacity?|Choropleth map (Folium)|The top 15 countries hold 76.8% of the database's capacity (5,572,078 MW). China holds 24.5% and the USA 21.4%. Saudi Arabia is #12 with 84,219 MW (1.5%).|
|2. What plant types exist?|K-means, k = 3, on log capacity, log 2017 generation and commissioning year (8,778 plants, 95% US)|Large Mixed-Fuel plants are 33% of plants but hold 93.6% of capacity and produce 97.3% of 2017 output. The two small groups are newer solar-heavy plants (54% of plants) and older hydro-heavy plants (13%).|
|3. What explains output?|Linear regression, 70/30 split, random\_state = 42, log scale|Test R-squared: capacity only 0.693, fuel only 0.423, build year only 0.005, capacity + fuel 0.791, adding build year 0.793. Capacity matters most, fuel second, build year adds nothing.|
|4. How did output change, 2013 to 2017?|Year-over-year and % change by fuel, using the 6,654 plants that report all five years (about 91% US)|Gas +14.9% (US only +16.3%), nuclear +2.1%, coal +3.9% overall but US -3.5% and India +17.1%.|

## Next step for Saudi Arabia

Obtain plant-level generation from official Saudi sources (for example ECRA or KAPSARC; availability to be confirmed), calculate capacity factor per plant, and compare it with the US benchmark from this project. This is a plan, not a result.

## Data and citations

1. **Global Power Plant Database, v1.2.0 (released 2019-06-12).** Global Energy Observatory, Google, KTH Royal Institute of Technology in Stockholm, Enipedia, World Resources Institute. 2019. Source: https://datasets.wri.org/dataset/globalpowerplantdatabase. License: CC BY 4.0. Obtained from a Kaggle copy of the WRI release: https://www.kaggle.com/datasets/mathurinache/global-power-plant-database. A newer version (v1.3.0, 2021) exists; this project uses v1.2.0 throughout.
2. **Country outlines (shapefile):** Datahub Country Polygons as GeoJSON, from Natural Earth, public domain. https://r2.datahub.io/clvyjaryy0000la0cxieg4o8o/main/raw/data/countries.geojson. Used only to draw country borders and joined on the 3-letter country code.
3. **Time series practice data:** U.S. Energy Information Administration (EIA), Electric Power Monthly, Appendix C, "Net Generation by State by Type of Producer by Energy Source" (generation\_monthly.xlsx). https://www.eia.gov/electricity/monthly/. U.S. Government public domain; credit EIA. US monthly natural gas generation, 2010 to 2025, used only to practise decomposition, the Dickey-Fuller test, differencing and the ACF. It is not part of the storyboard and says nothing about Saudi Arabia.
4. **Cross-check for Saudi capacity:** KAPSARC (King Abdullah Petroleum Studies and Research Center) open data, national capacity by producer, 2014. https://datasource.kapsarc.org. KAPSARC's 2014 total (65,506.5 MW) is 28.6% below the database figure, which is explained by capacity added after 2014.

## Repository structure

```
notebooks/   Python notebooks (Google Colab), one per technique
data/
  cleaned/       cleaned plant table, consistency checks, clustering files
  tableau/       small files prepared for the Tableau storyboard
  substitute\_eia/  EIA monthly series used for the time series practice
outputs/     charts and the interactive capacity map (HTML)
docs/        running findings document and research brief
```

|Notebook|What it does|
|-|-|
|01\_data\_cleaning.ipynb|Loads the 29,910 plants, runs 15 consistency checks, flags unusual rows (nothing imputed or deleted)|
|02\_geospatial\_analysis.ipynb|Capacity by country, top 15 ranking, choropleth map|
|03\_clustering.ipynb|Log transform, standardise, elbow method, k-means (k = 3), cluster profiles|
|04\_regression.ipynb|Capacity-only regression, then fuel and build year compared as an extension|
|05\_time\_series.ipynb|Generation change by fuel 2013 to 2017; decomposition, Dickey-Fuller test, differencing and ACF on the EIA series|

## Limitations

* Generation data exists for only six countries, and 95% of the modelling set is US plants. Results are a US benchmark.
* Saudi Arabia has capacity data but no generation data, so no Saudi plant is in the clusters, regression or fuel comparison.
* The data is the 2019 release. Solar and wind are under-counted (21% and 49% of capacity captured), and reporting quality differs by country.
* About 3.6% of modelling plants report exactly 0 GWh in 2017. They are kept; the reason (idle or unreported) is not known.
* Regression scores are on a log scale, so every plant counts equally. Errors in GWh for the largest plants were not measured.
* The 2013 to 2017 comparison uses two end years and five yearly points. It shows a change, not a statistical trend.

## How to reproduce

The notebooks were written in Google Colab with Python (pandas, numpy, scikit-learn, seaborn, matplotlib, plotly, folium, statsmodels). Download the original data from the sources above, or use the cleaned files in `data/`. Run the notebooks in order, 01 to 05. The storyboard was built in Tableau Public.

## Credit

Power plant data: World Resources Institute and data contributors, CC BY 4.0. Country outlines: Natural Earth. Monthly US generation: U.S. Energy Information Administration.

