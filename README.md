# Phase 2 Project

This is the phase two project ,in these repository, we shall be analysing the **Kings County house sale dataset**.

### Business Understanding.
Our stake holders are a Real Estate agent or A real estate company. When selling a house, a real estate agent or company, would like to sell a house at the most reasonable price in the market, that would be both good to the customers and to them earning them a profit. 
In order to sell the house at such a price, the house has to have some features that are pleasing to the customers which makes them want to buy the house. We are then going to conduct thorough analysis on a house sale dataset and find out  the features that are related to a good house sale price, then advice the agent or company on the features they should focus on when they want to sell  houses .
### Data Understanding
For this analysis, we shall be using the Kings county house sale dataset which is sourced from canvas. We shall use the Price variable as our target against the various feature's that have been provided, which are 20 feature's in total. Our data also contains 21,597 sales information.The predictors available to us include: *bedrooms,bathrooms,sqft_living,sqft_lot,floors,waterfront,view,condition,grade,yr_built,yr_renovated,sqft_basement',sqft_lot15,sqft_living15,sqft_above.*
Most of our data follow a normal distribution , although some don't we shall deal with this later in Preprocessing.

### Data Preprocessing.
Here , we are prepearing the data for analysis, the steps I took include:<br>
1) Detecting and cleaning of missing values, extraneous values and outliers.<br>
2) Checking and removing multicolinearity.<br>
3) Removing the unwanted columns.ie the columns that you do not find relevant to your analysis.<br>
4) Transformation of categorical values: This is where you encode categorical values into values their various categories so that you can use the for modelling.<br>
5) Log Transformation and scaling :You perform log transformation because you want your data to follow a normal distribution, hence transforming variables is great for your model and improves its perormance, for scaling, you basically want your data values to be in the same scale.<br>

### Modelling 

Here you are now fitting your already preprocessed data into a model . We will start by a simple linear regression then a  multiple linear regression.<br>
#### a)Simple linear regression.
Our simple linear regression is between two variables **Price** and **sqft_living**.We create this model as shown below

```python 
## In our formula, lets use the two variables price(target variable) and sqft_living(indipendent variable)
##Lets initiate a new DataFrame
df_new=pd.DataFrame([])
## lets add the column sqft_livivng and price to our dataframe and create a model
df_new['sqft_living']=df_fin['sqft_living']
df_new['price']=df_fin['price']
formula='price ~ sqft_living'
model=ols(formula=formula, data=df_new).fit()
model.summary()
```
As you can see, our R2 is 0.245 which is low, meaning that we need to improve it , we can do this by creating an interaction between variables.We shall exploit the interaction between the variable sqft_living and grade as shown below
```python 
x_interact=x.copy()
linreg=LinearRegression()
##Performing an interaction between grade and sqft_living
x_interact['sqft_living_grade']=x['sqft_living']*x['grade_9']
linreg.fit(x_interact,y)
```
Our r2 then improves from 0.245 to 0.5030 this show that there is a good relationship btween price and the interaction between sqft_living and grade.We can go ahead and visualize this in a scatter plot as shown below
```python 
sns.scatterplot(data=df,x='sqft_living',y='price',hue='grade')
plt.title('A visialisation of the sqft_living against the price based on grade.')
plt.show();
```