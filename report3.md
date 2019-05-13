## What gives concrete its strength

Sreekanth Reddy Sajjala

Concrete is the world's most widely used material. Its mainly composed of aggregates, fillers, water and cement, which is the main adhesive element. The variation in the quantity of these elements in concrete greatly affects its physical properties, carbon footprint and cost. For maximum strength, it would make sense to use a ratio where as much cement is used along with the appropriate amount of water used but this would make it economically unfeasible and very detrimental to the environment because of cement production's large carbon footprint. Aggregates help to fill volume after a certain strength is reached while fillers help reduce cost of the mixture while also contributing to the strength. In this report, we investigate how the compressive strength of the concrete changes with the quantities of these elements. We also look into the difference between coarse and fine aggregates along with the effect of slag and fly ash as fillers.

The reason this is an interesting analysis is because by being able to predict the strength of concrete we will be able to optimize the amount of filler and aggregate used thereby saving a lot in costs. The relationship between the composition and the strength of the concrete highly non linear due to the variety of chemical compounds present in cement and the physical formation of alite and belite crystals within the concrete.

### Dataset

I am using UCI's concrete compressive strength data repository which has a 1030 instances. The data is very clean with no missing values.


Variable Information:

Given is the variable name, variable type, the measurement unit and a brief description.
The concrete compressive strength is the regression problem. The order of this listing
corresponds to the order of numerals along the rows of the database.

Name -- Data Type -- Measurement -- Description

All the components are quantitative and have the same units of kg in m3 mixture.
They are all input variables.

Cement (component 1)

Blast Furnace Slag (component 2)

Fly Ash (component 3)

Water (component 4)

Superplasticizer (component 5)

Coarse Aggregate (component 6)

Fine Aggregate (component 7)

Age is a quantitative variable which measures, in days, the time since the concrete was cast.

The output variable is the concrete's compressive strength. Its measured in mega pascals and is the only output variable.


### Data visualization

Lets now visualize the dataset in various ways to understand more about the variables we are dealing with. 

First lets understand the range and the spread of the various variables. 

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/var.png)

Some important things that we can observe, all the mixtures contain some amounts of water, cement and both kinds of the aggregates. That is what forms "basic" concrete. The other additions are either to improve strength (superplaticizer, slag) or to act as fillers to reduce cost (slag*, flyash). *Slag contributes to certain aspects of strength but also reduces the cost.

Now lets just plot histograms and PMFs of the data to gain a more visual understanding.

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/hist.png)


We can see that most of the mixes don't use the fillers (slag & flyash) and the superplasticizer and that most of the samples are tested below 50 days, which makes sense as most regulatory standards require strength to be measured on the 28th day.

Lets plot the compressive strength of cement along the amount of slag in the mixture. This shall allow us to identify any trends, if we find this useful we can extend this to the other variables.
![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/plot.png)


This plot seems like it does not contain any valuable information with all the noise and flat lines but it conveys something important about the dataset and concrete. 
It is not built as a test data set where different levels of slag are independently tested. 
Concrete is built to a specific strength in most circumstance and other variables are changed accordingly to achieve the same. 
The only variable which we can measure independently is the age of the pour as strength can be tested for the same mixture at different values of the variable. 
Thereby visualizing the strength of cement with various factors would not be very useful. 
We shall anyway use the Pairplot function to summarize these trends and generate a nice visual


We see that very few of the plots actually show us any meaningful relations, this might be due to the fact the all the variables have some or another effect on others and because that the effects are the highly non-linear.


![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/pair.png)

I am now going to plot contour plots between the various variables and the compressive strength to see if we can find any observations that might prove to be interesting. 


![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/heat.png)

Again this is not super useful but we see some observations like how the core ingredients (cement, water, coarse and fine) all have a pretty centered heat map covering a good amount of area with an "average" center.



Now lets look at how the different variables correlated with each other. It is a nice visual which shall help us understand which variables have the most influence.

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/corr.png)

We see that strength is most greatly correlated with the amount of cement, amount of Superplasticizer and the age since pour. All of this follows common cement logic. Other interesting numbers include, the negative correlation between the quanitity of superplasticizer and the amount of the water. Superplasticizers increase the plasticity of the mixture and reduce the amount of water required. The amount of cement also correlates negatively with the amount of fillers(slag, fly ash) suggesting increased amounts of those would reduce costs.

### Basic linear regression

We shall first  perform basic linear regression to see how the strength of the cement shall vary with the various variables. This may not prove very useful as most of the relationships except for age are highly non linear as seen in the pair plots.


We shall be using all the variables as we don't know yet which have the most influence. We shall use the square of Age as the link between age and the strength of cement is a standardized topic which is non-linear and most tests used to grade cement measure the strength at fixed number of days since casting.  

We can clearly see the positive link between the strength of concrete and the number of days since its pour. Concrete reacts with the water to cure with time. We can see that the largest gain in strength is in the inital stages. The gain in strength then levels off but still continues to grow for years. Cement strength is usually measured by most standards at 28 days since casting

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/lin.png)

### Neural Network

Now lets build a neural network and fit it to our data. This seems useful to do as neural networks are great ways to explore non linear relationships between variables. 

Lets build the Neural net. This is lifted from a template and is appropriate for a data set with < 20 numerical values and 1 numerical output.
`
NN_model = Sequential()

# The Input Layer :
NN_model.add(Dense(256, kernel_initializer='normal',input_dim = train.shape[1], activation='relu'))

# The Hidden Layers :
NN_model.add(Dense(256, kernel_initializer='normal',activation='relu'))
NN_model.add(Dense(256, kernel_initializer='normal',activation='relu'))
NN_model.add(Dense(256, kernel_initializer='normal',activation='relu'))

NN_model.add(Dense(512, kernel_initializer='normal',activation='relu'))


# The Output Layer :
NN_model.add(Dense(1, kernel_initializer='normal',activation='linear'))

# Compile the network :
NN_model.compile(loss='mean_absolute_error', optimizer='adam', metrics=['mean_absolute_error'])
NN_model.summary()

`
For the checkpoint for the neural net. We shall be monitoring the loss value. The target variable is being set to the compressive strength of the concrete.

If we plot the the loss of the nueral net as its trained, this is the graph we get.

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/loss.png)


Here we see that loss varys a lot but consistently keeps coming down.

The testing loss coming down with training loss staying constant may indicate overfitting.

The avg absoulute percentage error using the predictions from the neural net was 24.05425%.

Now lets plot how the strength of the conrete changes with the amount of cement in the mix.

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/nn.png)

We see something that makes sense as the strength of the concrete would be expected to go up as the quantity of the main adhesive went up.


### Testing different machine learning options/models

Now lets use some of the regression methods we learnt in the datacamp course to make models and evaluate those. By testing various kinds of models we will be able to find which fits our data well and can give us the best results.

We shall be using a general function which shall fit the data to the algorithm, give us metrics, use the testing data to make predictions and then plot a graph using the model. It shall also rank the importance of the variables.


#### Linear regression

First we are going to run regular linear regression and see how it performs and then use it to predict how the strength of concrete varies with the amount of cement.


LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None,
         normalize=False)
***************************************************************************
ROOT MEAN SQUARED ERROR : 10.259026562177118
***************************************************************************
CROSS VALIDATION SCORE
************************
cv-mean : -113.26476202982501
cv-std  : 21.485876840663618
cv-max  : -67.5229596210266
cv-min  : -166.69393042000289


The average absoulute percentage error using the predictions from the linear regression was 30.2086%.


![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/linreg.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/linregf.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/linregp.png)


If we look at the comparison between the actual and plotted values, we see that the model is on the right track with the predictions being close to the actual results.

We see that the model considers the amount of superplaticizer used as the most important factor.

Looking at the variation of strength with the amount of cement, this also follows the general theory that more cement equals greater compressive strengths.

Now lets use Lasso regression. This does linear regression coupled with regularization of the parameters.

#### Lasso


Lasso(alpha=1.0, copy_X=True, fit_intercept=True, max_iter=1000,
   normalize=False, positive=False, precompute=False, random_state=None,
   selection='cyclic', tol=0.0001, warm_start=False)
***************************************************************************
ROOT MEAN SQUARED ERROR : 10.222289461079958
***************************************************************************
CROSS VALIDATION SCORE
************************
cv-mean : -113.33064732894589
cv-std  : 21.135184436354397
cv-max  : -67.93694309398683
cv-min  : -164.92280242787803


![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/lasso.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/lassof.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/lassop.png)


The average absoulute percentage error using the predictions from the Lasso regression was 30.2206%.


#### Ridge

We see very similar CV scores and results. The feature identified as the most important also stays the same. This indicates that regularization might not have been much help. The average absolute error was also within a tenth of a percentage.

I shall now use Ridge regression and see if that method helps.


Ridge(alpha=1.0, copy_X=True, fit_intercept=True, max_iter=None,
   normalize=False, random_state=None, solver='auto', tol=0.001)
***************************************************************************
ROOT MEAN SQUARED ERROR : 10.258998220231287
***************************************************************************
CROSS VALIDATION SCORE
************************
cv-mean : -113.26468230948262
cv-std  : 21.48565085432168
cv-max  : -67.52314595848341
cv-min  : -166.69259552822317



![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/ridge.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/ridgef.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/ridgep.png)


The average absoulute percentage error using the predictions from the ridge regression was 30.206%.

We see very similar CV scores and results. The feature identified as the most important also stays the same. This indicates that the change in the kind of regularization might not have been much help.#

#### KNN

Now let's try a different type of model, K Nearest Neighbours and look at its results.


KNeighborsRegressor(algorithm='auto', leaf_size=30, metric='minkowski',
          metric_params=None, n_jobs=None, n_neighbors=5, p=2,
          weights='uniform')
************************************************************************
ROOT MEAN SQUARED ERROR :  8.368062025239324
************************************************************************
CROSS VALIDATION SCORE
************************
cv-mean : -94.18435302297297
cv-std  : 24.67626497553364
cv-max  : -53.25858833333333
cv-min  : -148.8523550000001


![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/knn.png)

The avg absoulute percentage error using the predictions from the K Nearest Neighbours was 24.054%.

Looking at the CV scores and absolute mean error, it looks like this model has served us better than the linear regression ones.

#### Decision trees

DecisionTreeRegressor(criterion='mse', max_depth=None, max_features=None,
           max_leaf_nodes=None, min_impurity_decrease=0.0,
           min_impurity_split=None, min_samples_leaf=1,
           min_samples_split=2, min_weight_fraction_leaf=0.0,
           presort=False, random_state=None, splitter='best')
***************************************************************************
ROOT MEAN SQUARED ERROR : 6.727918373067839
***************************************************************************
CROSS VALIDATION SCORE
************************
cv-mean : -46.18396763795045
cv-std  : 22.329441277507076
cv-max  : -22.837075694444447
cv-min  : -104.78438333333332


The average absoulute percentage error using the predictions from the decision tree regression was 14.451%.



![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/dt.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/dtf.png)

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/dtp.png)


The CV scores and the performance chart indicates that this might be the best model to use with our data. The fact that cement and age are the most important variables also agrees with traditional cement logic. The graph showing the variation between the amount of cement used and the compressive strength of the concrete looks weird for some reason. The effects below 200Kg can be explained by the fact that those points are outliers and very few cases of that exist.

All the errors comapared


The average absoulute percentage error using the predictions from the ridge regression was 30.206%.

The average absoulute percentage error using the predictions from the Lasso regression was 30.2206%.

The average absoulute percentage error using the predictions from the linear regression was 30.2086%.

The avg absoulute percentage error using the predictions from the K Nearest Neighbours was 24.054%.

The average absoulute percentage error using the predictions from the decision tree regression was 14.451%.

