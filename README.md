# housing-data

## Research Question
Can we predict housing prices based on characteristics of the houses in King's County?

## Data
Data from Kaggle. Information taken Governement dataset. In total, there are 21 variables and 21,613 observations.
Variables included are: 
  -price
  -date
  -bedrooms
  -bathrooms
  -sqft_living (Sqft of Home)
  -sqft_lot (property size not including home)
  -floors
  -waterfront (yes/no)
  -veiw (how many people have viewed the property)
  -condition (1-5 rating; represents upkeep level)
  -grade (1-13 rating; represents the quality of design and materials)
  -sqft_above (sqft of home excluding basement)
  -sqft_basement (sqft of basement)
  -yr_built
  -yr_renovated
  -zipcode
  -latitude
  -longitude
  -sqft_living15 (takes renovations into account)
  -sqft_lot15 (takes renovations into account)
  
## Feature Engineering
### Removing Outliers
In order to increase predictive power of our model, we got rid of some of the outliers. 
We limited our data with the following constraints:
price < 2,000,000
bathrooms < 5
bedrooms <= 7
floors < 5

### Dependent Variable : Price
We log-transformed price to get a more normal distribution. 
#### Distribution of Prices: 
##### Before:
![Screen Shot 2019-12-03 at 5 17 48 PM](https://user-images.githubusercontent.com/52469561/70094605-df3ef180-15f0-11ea-80bb-3f4d74cb2c6b.png)
![Screen Shot 2019-12-03 at 5 17 40 PM](https://user-images.githubusercontent.com/52469561/70094614-e403a580-15f0-11ea-9374-70ad6ea20d51.png)

##### After:
![Screen Shot 2019-12-03 at 5 18 49 PM](https://user-images.githubusercontent.com/52469561/70094663-07c6eb80-15f1-11ea-9647-724cee3b02fa.png)
![Screen Shot 2019-12-03 at 5 18 41 PM](https://user-images.githubusercontent.com/52469561/70094664-085f8200-15f1-11ea-9edd-597b0515f802.png)

### Independent Variables
We ended up using: 
  -bedrooms
  -bathrooms
  -sqft_living (Sqft of Home)
  -sqft_lot (property size not including home)
  -floors
  **-waterfront (yes/no)***
  **-condition (1-5 rating; represents upkeep level)***
  **-grade (1-13 rating; represents the quality of design and materials)***
  -sqft_above (sqft of home excluding basement)
  -sqft_basement (sqft of basement)
  -yr_built --> age = 2015 - yr_built
  -yr_renovated
  **-zipcode***
  -sqft_living15 (takes renovations into account)
  -sqft_lot15 (takes renovations into account)
  
  * = categorical variable
  
 ### Independent Variable: Correlation Heatmap

![Screen Shot 2019-12-02 at 3 35 56 AM](https://user-images.githubusercontent.com/52469561/70094331-4d36e900-15f0-11ea-9bc7-d91cf5b54151.png)
We noticed high correlation between variables that indicate size. We will drop some of these in our model moving forward to address problem of multicollinearity.

**We created dummy variables for categorical features, and scaled continuous variables using a StandardScalar.**

## Models
### Multiple Linear Regression Using OLS Statsmodel

Our final model included the following features with the associated coefficients. The third column represents the transformed coefficients. These are the values that allow us to interpret the associated change in price for a percent change in x (for continuous variables) or associated premium for belonging to a category (for categorical dummies):

![Screen Shot 2019-12-03 at 4 51 26 PM](https://user-images.githubusercontent.com/52469561/70093020-8752bb80-15ed-11ea-8417-780b9bc45a57.png)

We arrived at this model by eliminating the features with non-significant p-values from the initial regression. We also ran a regression using a forward-backward stepwise function for feature selection. Our final model had a lower AIC than both the initial regression with all features, and the regression using stepwise feature selection.

#### Interpretation of Model
We found that changes in zip code were associated with significant changes in price. The most expensive zipcode was 98039, associated with a price that is about 300% higher than our standard (and close to 300% higher than the lowest). 

### Ridge & Lasso Regression Using SKLearn

We ran Ridge and Lasso Regression models in hopes of controlling for variance in our data. When comparing the test errors against our linear models we found that Ridge Regression produced the best output. However, we noticed that the linear coefficients in SKLearn varied greatly from the ones we produced in Statsmodel. If given more time we would definitely hope to compare and discover the cause of these differences.

    **Train Error Ridge Model 623.3321375142388**
    **Test Error Ridge Model 155.71200692361117**

    **Train Error Lasso Model 797.8057032615068**  
    **Test Error Lasso Model 195.55344911614895**

    **Train Error Unpenalized Linear Model -2682273.3556272555** 
    **Test Error Unpenalized Linear Model -670066.0383374249**

#### Evaluating Model Assumptions
##### Linearity:
We were not to concerned with linearity; there seems to be a general linear trend.
![image](https://user-images.githubusercontent.com/52469561/70093403-50c97080-15ee-11ea-9a13-2e5e313b67df.png)
##### Homoscedasticity
This poses a greater issue for out model. The variance is not evenly distributed as the value of the independent variable changes.
##### Normally Distributed Residuals
The residuals were mostly normally distributed except for the edges of the data. This suggests that our model hs more predictive power within a certain range.
![Screen Shot 2019-12-03 at 5 04 29 PM](https://user-images.githubusercontent.com/52469561/70093728-072d5580-15ef-11ea-9ae7-932daec94803.png)
##### Multicollinearity
Our final model eliminated variables with high correlation. However, we kept some that correlated features as they continued to be significant.




 
 
 


  
