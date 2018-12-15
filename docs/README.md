![Clothes hanging in store](http://shahidhn.github.io/f18-datascience-final/images/clothes.jpg)

# Abstract
We are trying to understand customer purchasing behavior as a function of demographics, with reference to the [Black Friday dataset](https://www.kaggle.com/mehdidag/black-friday) on Kaggle. We first do some data visualization and pre-processing, primarily in the form of one-hot encoding of categorical data, in order to get a better understanding of the data we are working with. We then performed Principal Component Analysis (PCA) to reduce the dimensionality of the data, followed by K-means clustering to find clusters within our feature set, before running XGBoost on each cluster to predict the purchase amounts for each available product category. Our final XGBoost model had an MAE of 2040.1. Finally we elaborate on some learnings and future improvements of our work.

# Team
- [Chimezie Iwuanyanwu](https://github.com/cpuguy96)
- [Hamza Khatri](https://github.com/hamzacooly)
- [Isabel Li](https://github.com/imabelli)
- [Shahid Nowshad](https://github.com/shahidhn)

# Introduction
## Dataset
The [Black Friday dataset](https://www.kaggle.com/mehdidag/black-friday) contains 550,000 observations about Black Friday sales in a particular retail store. The dataset provides some information about the customer and the item being purchased. 

Each entry in the dataset is an instance of a customer making a purchase of a particular item during a particular transaction (a particular time), regardless of quantity. Thus, a customer purchasing different items in the same transaction exists as different entries, a customer purchasing the same item across different transactions (different times) exists as different entries, and a customer purchasing multiple of the same item in the same transaction exists as a single entry.

## Problem Statement
Broadly, our goal is to understand customer purchasing behavior as a function of demographics. To that end, the problem we are trying to solve is: __Given a customer, recommend top 10 product categories the customer is most likely to have interest in by predicting purchase amount per category__. We approach this problem statement from the perspective of the company, by trying __maximize sales by recommending relevant products to new and returning customers__. We will also later expand on other further applications of this problem statement.

## Assumptions
The dataset provides information about how much a customer has spent on a certain product, but does not specify the price of each product - we know this because the same product ID returns different purchase amounts across different entries. Therefore, for the purposes of this project, we assume that every product has the same cost of $1 - this is clearly a huge assumption, and might lead to a weaker model. Having access to the price of each product at the point of purchase would have helped the accuracy and viability of our model.

Given our problem statement, another assumption we are making is that Black Friday spending habits correlate to the rest of the year for all types of customers. While there is probably a strong correlation, this may not entirely be true. For example, people might purchase more gifts on Black Friday than other times of the year given that it is close to a holiday season. Also, people are possibly more likely to buy cold weather clothing (at least in the US).

# Working with the Dataset
## Information in Dataset
- Customer Information:
	- User ID
	- Gender
	- Age
	- Occupation
	- City Category
	- Length of Stay in Current City
	- Marital Status
- Product Information:
	- Product ID
	- Product Categories 1, 2, 3
- Sales Information:
	- Purchase Amount

For each item, anywhere between 1 - 3 product categories are specified, out of 18 numerical labels. This means that the 'Product Category 2' and 'Product Category 3' fields are empty for many rows.

## Data Pre-processing
The major form of preprocessing we did was one-hot encoding of categorical data. A lot of the information in the dataset took the form of categorical data, so they had to be converted to a form which could be numerically processed. However, this meant that the number of features was increased substantially, and we thus took steps during modeling to reduce the dimensionality of the data.

## Data Visualization
### Statistical Summary of Dataset
![Purchase Values](http://shahidhn.github.io/f18-datascience-final/images/stat_summary.png)

### Range of Purchase Values
![Purchase Values](http://shahidhn.github.io/f18-datascience-final/images/purchase_vals.png)

By visualizing the range of purchase values, we can later better evaluate our models by putting the purchase value errors in context.

### Heatmap of Feature Correlations
![Feature Correlations](http://shahidhn.github.io/f18-datascience-final/images/features_heat_map.png)

We can see that most features are weakly correlated. The customer age correlates with their marital status - this makes sense, as married people tend to be older than unmarried people. 'Product Category 1' (which every entry will have filled in) correlates with the purchase amounts, which suggests that certain product categories cost more. Also, 'Product Category 1' correlates with 'Product Category 3', which suggests that there are certain categories which often show up together.

# Modeling

# Results
These are the MAEs obtained from the various models we used to predict purchase amount per category, following PCA and clustering:

- Linear Regression: __2240.5__
- GB Regression: __2175.5__
- MLP: __2285.4__
- XGBoost: __2040.1__

As you can see, XGBoost performed best among these models. These are the residual plots we obtained from running XGBoost:
![Residuals](http://shahidhn.github.io/f18-datascience-final/images/residuals.png)

This is the feature importance graph returned by XGBoost (we have cropped out the top most important features, as there was a large number of features represented):
![Feature Importance](http://shahidhn.github.io/f18-datascience-final/images/feature_importance.png)

Let us revisit our problem statement of recommending top 10 product categories the customer is most likely to have interest in, given customer demographics, by predicting purchase amount per category. For each cluster, the product categories which are projected to have the highest purchase amounts are as follows:

> Top 10 product categories for cluster 0: 1 8 5 16 2 15 6 14 4 17
> 
> Top 10 product categories for cluster 1: 1 8 5 16 2 15 14 6 4 17
> 
> Top 10 product categories for cluster 2: 8 1 5 16 2 15 14 6 17 4 

# Conclusion and Future Steps
## Insights
### Product Categories per Cluster
As you can see, the top 10 product categories per cluster are in fact very similar. There might be a few reasons for this. 

It is possible that our clusters of customers are not distinct enough, and our PCA and K-means Clustering was not done well. We could repeat clustering with better pre-processing, and see if this leads to a difference in results. Otherwise, it might simply be true that purchasing habits are similar even across distinct clusters; however, we think this is unlikely. Something that might aid this analysis is if we could have an understanding of the demographic similarities that made up each cluster, as well as having a qualitative understanding of each product category.

### Feature Importance
Based on the graph returned by XGBoost, it seems like age, years in current city, and gender are the most important features in predicting the purchase amounts per category.

## Improvements
### Pricing Data
As mentioned before, the dataset provides information about how much a customer has spent on a certain product, but does not specify the price of each product. This makes it tough to build an accurate model, as there are probably differences in price points across product categories. Also, from everday experience, price is certainly a factor in deciding whether or not to buy a certain product. If we had pricing data, we would have been able to regress on product sales rather than purchase amounts, which would have more accurately reflected purchasing behavior.

### Data Granularity
The dataset abstracts out a lot of information, which could have been useful in understanding the data trends and explaining our results. For instance: product categories are denominated 1 - 18, city categories are denominated A-C, etc. Having an understanding of what each product category number corresponds to, for example, would help us better understand trends in purchasing behavior across customer demographic clusters, which would make our work more useful and generalizable.

## Further Applications
### Context-based Recommendation Engine
Right now, our model takes in the demographics of a customer and outputs the top product categories the customer is likely to purchase. The primary application of this comes from the context of a retail store recommending products to a potential customer about whom the store only has demographic information.

We could reconfigure our model to account for the purchase history of a customer, if the customer has shopped at the store before. One way to do this would be to have the model inputs be the demographics of the customer as well as one, possibly more, product category (possibly the category that the customer has purchased most frequently in the past, or the category of the customer's most recent purchase). The output would still be the the top product categories the customer is likely to purchase, but given that there is some variation in purchasing behavior even within customer clusters, the output could quite possibly be different from the base case.

## Store Branch Expansion
Another application of our model could be from the perspective of the retail store opening up a new branch in a new area: Given the demographic distribution of the residents in that area, what products should the store stock up in inventory such that it can maximize sales? This can be done by finding the corresponding cluster proportions of the residents in that area, and stocking up in inventory in those proportions. 

For instance, if it turns out that the residents in this new area fall into the following cluster proportions:

- Cluster 0: 30%
- Cluster 1: 30%
- Cluster 2: 40%

Then the store can stock 30% of their inventory according to the purchasing preferences of Cluster 0, 30% for Cluster 1, and 40% for Cluster 2. This can be adapted for any number of clusters, in the event that we reconfigure our clustering process.

## Predicting Sales Performance of New Products
When a new product hits the shelves, our model allows the store to predict the purchase amounts of that product given the product's category and the demographic distribution of the customers that frequent that store.
