# Introduction
## Dataset
The [Black Friday dataset](https://www.kaggle.com/mehdidag/black-friday) contains 550,000 observations about Black Friday sales in a particular retail store. The dataset provides some information about the customer and the item being purchased. 

Each entry in the dataset is an instance of a customer making a purchase of a particular item during a particular transaction (a particular time), regardless of quantity. Thus, a customer purchasing different items in the same transaction exists as different entries, a customer purchasing the same item across different transactions (different times) exists as different entries, and a customer purchasing multiple of the same item in the same transaction exists as a single entry.

## Problem Statement
Broadly, our goal is to understand customer purchasing behavior as a function of demographics. To that end, the problem we are trying to solve is: __Given a customer, recommend top 5 product categories the customer is most likely to have interest in by predicting purchase amount per category__. We approach this problem statement from the perspective of the company, by trying __maximize sales by recommending relevant products to new and returning customers__. We will also later expand on other further applications of this problem statement.

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

# Conclusion and Future Steps