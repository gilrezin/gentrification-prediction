# Gentrification-Prediction
A tool that predicts future gentrification in cities using weighted distance-based metrics across a myriad of datasets.

*This project was created for Dr. Jana Doppa's CptS 545 Class, "*AI in Real World*", at Washington State University.*

## What is this project? ##
Gentrification is the process in which neighborhood investment increases property values and rents, resulting in displacement of existing residents for those of a higher income.
Many in academia have proposed that certain neighborhoods gentrify over others as a result of proximity to urban amenities, such as jobs, parks, and transit stations. This project uses that hypothesis to guide future gentrification predictions.

What this project aims to do is predict, at a census-tract level, what areas of a city are likely to gentrify in the near future, providing a clearer picture to policymakers, local residents, and investors about the future of the area.

## Methodology & Limits ##
### [Read the full technical paper here](https://github.com/gilrezin/gentrification-prediction/blob/main/Bayesian%20Optimization%20Hyperparameter%20Tuning.pdf) ###

This project is divided into 2 sections: Exploratory ([*model-criteria.ipynb*](https://github.com/gilrezin/gentrification-prediction/blob/main/model-criteria.ipynb)) and the Model Tuning ([*model.ipynb*](https://github.com/gilrezin/gentrification-prediction/blob/main/model.ipynb)).

<img width="759" height="385" alt="image" src="https://github.com/user-attachments/assets/5ade8ea3-e038-464d-88b8-5f627b7cd9e7" />

The exploratory phase is used to determine what areas have already gentrified as a ground truth, such that any future predictions can be extrapolated from there. Using Census Data on tracts below the city's median income, socioeconomic factors such as college-educated residents, median household income, and poverty levels between 2010 and 2020, 
a ground truth for what census tracts were gentrified between 2010 and 2020 were formed. The top 10% of census tracts by z-score were determined to be "gentrified" by calendar year 2020.
This data was compiled and normalized into ground-truth labels in [*training_data_t0-t1.csv*](https://github.com/gilrezin/gentrification-prediction/blob/main/training_data_t0-t1.csv). Further detail on this process can be found in the paper.

During the model tuning phase, a distance-based metric is used between all statistically significant datasets, using an inverse Euclidean distance heuristic from a given census tract's centroid to all amenities to calculate their predicted gentrification score. The weighting of each metric is done via hyperparameter tuning to select ideal amenity weights, with Bayesian Optimization and Thompson Sampling
utilized to determine the ideal places to calculate the gravity score. The model is trained after ~200 iterations.

This model has several limitations: ground truth calculations are imperfect, as academia has no unified definition for what metrics should be accounted for in gentrification. Similarly, amenity datasets remain arbitrary and unaccounted for. While the model performed well on Chicago, other cities might struggle depending on the availability and quality of municipal datasets.
Future work will account for this by validating other factors, such as density clustering of points such as building permits, or the usage of urban latent space autoencoders to determine better predictors of gentrification than datasets accounted for in the case study.

## Case Study: Chicago ##
When testing Chicago's gentrification potential, several datasets were gathered across the United States Census Bureau and City of Chicago’s Data Portal:
- US Census LODES data, which consists of census-block granularity job employment and
location data at Census years 2010 and 2020.
- Chicago Park District Facilities, which contains the location data of all park fa-
cilities. Larger parks with more facilities will have more location points and thus greater
weight.
- CTA ’L’ Stops, which contains the location data for all high-frequency rail stations.
- US Census Demographics Data, which consists of median gross rent, median household
income, percentage of population in poverty, median gross rent, median home value, and
number of individuals with a college degree or higher (see Appendix A), at a census-tract
level resolution at Census years 2010 and 2020.
- Chicago Police Department Crime Data, which lists every reported crime between
2001 and present-day, the date of occurrence, and the coordinates at which the crime
occurred.
- Chicago Building Permits, which lists every building permit filed between 2006 and
present-day, the date of occurrence, and the coordinates of the building site.

When run on these datasets, the following predictions are made by the model at $t_2$ (2030). The darker the census tract, the greater probability that the tract will undergo gentrification by $t_2$. The lighter the census tract, the lower the probability. 
Note that any Census tracts above the city's median rent or income are excluded from the dataset, as including already wealthy tracts skews the data towards areas already considered gentrified. 
<img width="386" height="720" alt="image" src="https://github.com/user-attachments/assets/9cedeffd-fced-445d-b227-5472d3440504" />
