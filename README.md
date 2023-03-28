# Phase 2 Project

This is the phase two project ,in these repository, we shall be analysing the **Kings County house sale dataset**.

### Business Understanding.
Our stake holders are a Real Estate agent or A real estate company. When selling a house, a real estate agent or company, would like to sell a house at the most reasonable price in the market, that would be both good to the customers and to them earning them a profit. 
In order to sell the house at such a price, the house has to have some features that are pleasing to the customers which makes them want to buy the house. We are then going to conduct thorough analysis on a house sale dataset and find out  the features that are related to a good house sale price, then advice the agent or company on the features they should focus on when they want to sell  houses .
### Data Understanding
For this analysis, we shall be using the Kings county house sale dataset which is sourced from canvas. We shall use the Price variable as our target against the various feature's that have been provided, which are 20 feature's in total. Our data also contains 21,597 sales information.The predictors available to us include:<br>*bedrooms<br>,bathrooms,<br>sqft_living,<br>sqft_lot,<br>floors,<br>waterfront,<br>view,<br>condition,<br>grade<br>,yr_built,<br>yr_renovated,<br>sqft_basement',<br>sqft_lot15,<br>sqft_living15,<br>sqft_above.<br>*
Most of our data follow a normal distribution , although some don't we shall deal with this later in Preprocessing.
Below is a heatmap showing a visualisation showing variables that are highly correlated with our target variable.
![correlation](https://user-images.githubusercontent.com/109750154/228082739-4608b9b7-f551-417c-9235-17339fdd1561.png)


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
<img width="602" alt="ols" src="https://user-images.githubusercontent.com/109750154/228085406-5db94325-ff8c-4d8c-90b8-98d8adeaaef4.png">
##### i)Creating an interaction between sqft_year and grade
As you can see, our model has a 45% accuracy which is low, meaning that we need to improve it , we can do this by creating an interaction between variables.We shall exploit the interaction between the variable sqft_living and grade.
Our r2 then improves from 45% accuracy to 65% this show that there is a good relationship btween price and the interaction between sqft_living and grade.We can go ahead and visualize this in a scatter plot as shown below

![grade](https://user-images.githubusercontent.com/109750154/228083478-ebc03d07-5627-4ea5-bcad-804bd27eb172.png)

##### ii)Creating an interaction between sqft_year and yr_built
Wealso create an interaction between sqft_living and yr_built.When we do this, our R2 improves from from 45% accuracy to 65% accuracy.Below is a visualisation between sqft_lving,yr_built and price .
![built](https://user-images.githubusercontent.com/109750154/228086253-e74fa7a4-e6ab-48f3-8d7a-ed364dde6cab.png)
From this we can see that most of the mordern houses sell at high prices than most of the old houses.Some of this old houses do have high prices,this could be purely by chance or as a result to an unknown factor hence more anlysis needs to be done.

##### iii)Creating an interaction between sqft_year and renovations
Lets create an interaction betweeen  sqft_renovations,does a house being renovated have an effect on the sale price of the house ? We investigate this by creating an interaction between sqft_ling and renovations.Here, we set the any value(year) greater than 0 as 1(Meaning :Renovated house) else if it is equal to zero, it remains as a zero(meaning: house not renovated).Our interactions improves the perfomance of the model from 45% accuracy to 65% accuracy.Below is a plot of price against sqft_living based on the renovation category.
![renovations](https://user-images.githubusercontent.com/109750154/228088853-57947b22-27e3-4b53-a798-a2c407798e4e.png)
This visualisation shows that renovations,do not really play a role key in the determination of price, this could be maybe to because some houses are newly built and do not require renovations ,hence more anlysis needs to be done.

##### iv)Creating an interaction between sqft_year and bathrooms.
Here we  are seeking to see if the number of bathrooms has an effect on the sale price of a house , we create an interaction between bathrooms and sqft_living.Our model performance then improves from a 45% accuracy to a  65.3% accuracy , this show great improvement in our model, below is a plot showing my findings.

![bathrooms](https://user-images.githubusercontent.com/109750154/228090042-704da2ba-c3c6-463a-9a32-eae90d499620.png)

From the visualisation above , we see that as the sqft_living and number of bathrooms increase so does the sale price of a house increase.
### Multiple linear regression.
We start with a multiple linear regression between all of our variables as shown below.
```python
##Set out come as price and the predictors as all other variables in our model except price
##fit predictors and outcome into our model
predictors=x
outcome='price'
pred_sum='+'.join(predictors.columns)
formula=outcome+'~'+pred_sum
model=ols(formula=formula,data=df_fin).fit()
model.summary()
```
![0001](https://user-images.githubusercontent.com/109750154/228092448-4a10e579-6534-4cab-a23a-cd8c93b0dd03.jpg)

![0002](https://user-images.githubusercontent.com/109750154/228092518-53687d22-ff43-492c-9fe1-eb4c31528437.jpg)

Our model has a performance accuracy of 65% the performance accuracy increases because we have increased the number of features even though they are irrelevant,this can lead to overfitting, so that we can reduce over fitting , we use RFE to select the most important features, based on our RFE the selected variables include:<br>a)sqft_living,<br>
 b)grade,<br>
 c)yr_built,<br>
 d)waterfront,<br>
 e)view,<br>
f)floors<br>
 g)bathrooms,<br>

 We then fit tis into a model as shown below
 ```python
 linreg6=LinearRegression()
X = df_fin[selected_features]
y = df['price']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1)

linreg6.fit(X_train,y_train)
```
lets check the R2 of the training data (model accuracy)
```python
r2_train=r2_score(y_train,linreg6.predict(X_train))
r2_train
```
0.6277786593656134<br><br>
lets check the R2 of the testing data (model accuracy)
```python
##We are getting the r2_test of the selected features.
r2_test=r2_score(y_test,linreg6.predict(X_test))
r2_test
```
0.622243256725343<br><br>
This shows that there is not much of a difference between the training and testing accuracy, meaning that we are not overfitting nor underfitting.When we use this model to predict the sale price that based on the recommended features , we find that the range of the sale price varies between $**1,316,044.93 to $4,976,256.16**.


### Conclusion.
From my analysis, I concluded that:<br>
1)There is a high correlation between sqft_living and the target price.<br>
2)There is a linear relationship between sqft_living and price.<br>
3)The price of a house increase as the sqft_living and grade increase.<br>
4)The price of a house increase as the sqft_living and the number of bedrooms increase.<br>
5)Renovation does not have much of an effect on the house price.It should be notted that maybe this is because some of this houses were newly built hence more research is needed on this.<br>
6)The best features to be considered when selling a house include:<br>
a)sqft_living<br>
b)yr_built<br>
c)waterfront<br>
d)views<br>
e)grade<br>
f)floors<br>
g)yr_built<br>
h)floors<br>
i)bathrooms<br>
7)The range of sale price for houses with these features is between  $**1,316,044.93**𝑡𝑜 $**4,976,256.16**<br>

### Recomendations
1)I would recommend that more focus on the sale of houses with a good sqft_living as this has quite much of an impact on the house price.<br>
2)I would recommend that the company or agent should look into the following features and seek to satisfy them for a good sale:<br> a)sqft_living<br>
b)yr_built<br>
c)waterfront<br>
d)views<br>
e)grade<br>
f)floors<br>
g)yr_built<br>
h)floors<br>
i)bathrooms<br>
3)I would recommend that more research be done on the renovated houses and their sale price.<br>4)I would recommend that a sales agent should target at selling a house at a price range between **1,316,044.93**𝑡𝑜 **4,976,256.16** .<br>




