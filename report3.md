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

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/pair.png)

I am now going to plot contour plots between the various variables and the compressive strength to see if we can find any observations that might prove to be interesting.

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/heat.png)

Again this is not super useful but we see some observations like how the core ingredients (cement, water, coarse and fine) all have a pretty centered heat map covering a good amount of area with an "average" center.

Now lets look at how the different variables correlated with each other. It is a nice visual which shall help us understand which variables have the most influence.

![alt text](https://github.com/ssreekanth2000/project3/blob/master/photos/corr.png)

We see that strength is most greatly correlated with the amount of cement, amount of Superplasticizer and the age since pour. All of this follows common cement logic. Other interesting numbers include, the negative correlation between the quanitity of superplasticizer and the amount of the water. Superplasticizers increase the plasticity of the mixture and reduce the amount of water required. The amount of cement also correlates negatively with the amount of fillers(slag, fly ash) suggesting increased amounts of those would reduce costs.

### Basic linear regression

We shall first  perform basic linear regression to see how the strength of the cement shall vary with the various variables. This may not prove very useful as most of the relationships except for age are highly non linear.


We shall be using all the variables as we don't know yet which have the most influence. We shall use the square of Age as the link between age and the strength of cement is a standardized topic which is non-linear and most tests used to grade cement measure the strength at fixed number of days since casting.  



