# Phase Two Project Repo

# King County, Washington: Data Analysis & Strategy Recommendations

**Authors**: Pete VanZandt, Emma Choate, Alex Valencia

## Overview

For this project, we used linear regression to model housing sales in King County, a northwestern county in Washington state that includes Seattle.

## Business Problem

A Seattle real estate firm wants their clients to have a prediction for the sale price of their home based on the information of their home. In order to achieve this, they would like a tool that can be as accurate as possible in their predictions. We have come in to create this tool and use it for this real estate firm’s clients.

## Data

We used data from kc_house_data.csv and column_names.md. This data is based on the numbers between the years 2014 - 2015 and contains the following information:

* **id** - unique ID for a house
* **date** - date house was sold
* **price** -  price house was sold; is prediction target
* **bedrooms** -  number of bedrooms
* **bathrooms** -  number of bathrooms
* **sqft_living** -  square footage of the home
* **sqft_lot** -  square footage of the lot
* **floors** -  floors (levels) in house
* **waterfront** - house does or does not have a waterfront
* **view** - how good the property view is
* **condition** - how good the condition is (1 - 5)
* **grade** - overall grade given to the housing unit building construction and design, based on King County grading system
* **sqft_above** - square footage of house apart from basement
* **sqft_basement** - square footage of the basement
* **yr_built** - year when house was built
* **yr_renovated** - year when house was renovated
* **zipcode** - zipcode area of the house
* **lat** - latitude
* **long** - longitude
* **sqft_living15** - square footage of interior housing living space for the nearest 15 neighbors
* **sqft_lot15** - square footage of the land lots of the nearest 15 neighbors

We also used King_co_zipcodes.csv to OneHotEncode the zipcodes saved as municipalities. The source of the data came from King County Open Data - https://data.kingcounty.gov/.

### Process
We screened the data and looked at the range of total prices to find any patterns. We didn't drop any residual outliers. We saw what features were the most promising by creating correlation plots. We then built a model with the one predictor whose correlation was strongest with price - sqft_living.

We used an iterative process to create more models and come up with the best predicitability on house prices for all house price ranges.

## Methods & Results

First, we pulled the original dataframe and created three separate functions that would run a multiple regression model, a residual model, and a bin-error model. The bin-error model cuts the dataframe into 10 bins based on house prices, shows us how much error there is in predicting house prices in each bin, and shows us the standard deviation of each bin.

Next, we built our first model using sqft_living as our main predictor because its correlation was strongest with price. Our train error was around 262419, our test error was around 259878, and our R-sqaure was around 0.488 -- this told us that sqft_living didn't account for as much variance as we had originally expected, and there were plenty of other predictors we needed to include in our model. Our residual model showed us there were many outliers in the data and our bin-error model showed us the errors for a few bins were high, especially for houses in the top 10% price range. This is our baseline model and you will find our residual model for the baseline below:

![graph1](./images/simple_model.PNG)

We continued modifying and iterating our model by adding a new feature called **area_basement** which was created by subtracting the difference between sqft_living and sqft_above. We added this new feature to 12 other numerical features for this model - bedrooms, bathrooms, sqft_living, sqft_lot, floors, condition, grade, sqft_above, yr_built, zipcode, sqft_living15, and sqft_lot15. Our train error was around 226809, our test error was around 225347, and our R-sqaure was around 0.618 -- our model showed significant improvement compared to the first model based on these results.

We scaled the predictors with our third model using RobustScaler(), we changed our target variable from **price** to **log_price** with our fourth model, and we used fewer features with our fifth model. Unfortunately, none of theses changes brought significant improvement to our model. 

On our sixth model, we added three interaction features:
 - **living_grade** (sqft_living * grade): correlation *0.832*
 - **grade_bath** (grade * bathrooms): correlation *0.824*
 - **living_bath** (sqft_living * bathrooms): correlation *0.819*

These three interactions were added because they had the strongest combined impact on the price of the home.

We saw significant improvement in this model because our train error decreased to 187242, our test error decreased to 188290, and our R-sqaure increased to 0.739. Our residual model showed us less outliers than the first model. Our bin-error model showed us less errors in all 10 bins -- 9 out of 10 bins contained errors that were less than $50,000. We included 20 features to get this output - bedrooms, bathrooms, sqft_living, sqft_lot, floors,
condition, grade, sqft_above, yr_built, zipcode, sqft_living15, sqft_lot15, area_basement, grade * bath, living * grade, living * bath, lat, waterfront, long, and view.

Our best fit model came from our seventh model because we ran a polynomial model. We dropped the features id, date, and yr_renovated. We saw even more significant improvement in this model because our train error decreased to 147269, our test error decreased to 165436, and our R-sqaure increased to 0.798. Our residual model slightly reduced our outliers compared to our sixth model. Our bin-error model showed us even less errors in all 10 bins -- 9 out of 10 bins contained errors that were less than $20,000. Houses in the top bin  - the top 10% price range - had around $100,000 error which was the smallest error this bin received on all our models.

In addition, we made a second polynomial model - our eight model - dropping nine extreme residuals. We did this because there were nine houses that had a stark difference between the selling price and the predictive price. For example, one of the residuals contained a house with a selling price of $2.3 million and a predictive value of $6 million. After dropping the residuals, we saw our train error drop to 144894, our test error drop to 155990, and our R-sqaure increase to 0.823. Our residual model and our bin-error model stayed constant to the previous polynomial model. We chose not to make this our best fit model because we don't have any real justification for dropping these extreme residuals.

For our ninth model, we used data from the King_co_zipcodes.csv file and attempted to OneHotEncode the zipcodes by municipality. Unfortunately, this made our model worse by increasing the train model to 188339, and test model to 190695, and lowering the R-square to 0.736. Therefore, we did not adopt the OneHotEncoder to our best fit model.

On the final model, we removed the poor predictors and simplified our model. We kept 13 features - bedrooms, bathrooms, sqft_living, floors, condition, grade, yr_built, zipcode, grade * bath, lat, waterfront, long, and view - and received a train error around 191327, a test error around 190102, and an R-sqaure around 0.728. Our residual model did not change, our bin-error model showed us our errors for 9 out of 10 bins were less than $50,000, and our Q-Q Plot revealed our data was under-disperesed which means we reduced the number of outliers in our data. This is the model we suggest the Seattle real estate firm uses to predict housing prices. You will find its residual model below:

![graph2](./images/suggested_model.PNG)

## Conclusions

We conclude...

## For More Information

Please review our full analysis in [our Jupyter Notebook](./microsoft_movie_analysis_final.ipynb) or our [presentation](./Microsoft_Movie_Analysis_Presentation.pdf)

For any additional questions, please contact **Pete VanZandt - pevanzandt@gmail.com, Emma Choate - emmachoate11@gmail.com, Alex Valencia - asvalencia1688@gmail.com**

## Repository Structure

Describe the structure of your repository and its contents, for example:

```
├── README.md                           <- The top-level README for reviewers of this project
├── microsoft_movie_analysis_final.ipynb   <- Narrative documentation of analysis in Jupyter notebook
├── Microsoft_Movie_Analysis_Presentation.pdf         <- PDF version of project presentation
├── data                                <- Both sourced externally and generated from code
└── images                              <- Both sourced externally and generated from code
