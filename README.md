# Black Friday Product Recommendation

## Table of Contents

1. [Overview](#overview)
2. [Preprocessing](#preprocessing)
3. [Analysis](#analysis)
4. [Modeling](#modeling)
5. [Recommender](#recommender)

## 1. Overview <a name = "overview"></a>

**Original Dataset**: [https://www.kaggle.com/mehdidag/black-friday/version/1](https://www.kaggle.com/mehdidag/black-friday/version/1)

**Original problem statement**: A retail company “ABC Private Limited” wants to understand the customer purchase behavior (specifically, purchase amount) against various products of different categories. They have shared purchase summary of various customers for selected high volume products from last month. The data set also contains customer demographics (age, gender, marital status, city_type, stay_in_current_city), product details (product_id and product category) and Total purchase_amount from last month. Now, they want to build a model to predict the purchase amount of customer against various products which will help them to create personalized offer for customers against different products.

**Our problem statement**: Given a customer, recommend top 5 product categories the customer is most likely to have interest in by predicting purchase amount per category.


## 2. Preprocessing <a name = "preprocessing"></a>

The [black_friday_preprocessing](https://github.com/shahidhn/f18-datascience-final/blob/master/black_friday_preprocessing.ipynb) notebook generates three different datasets from the original dataset based on the user specifications. The datasets are saved onto the local disk under the `inputs/` folder. 

The first dataset created, `minimal_preprocess`, is a `.csv` dataset with only basic preprocessing on the features. The dataset only label encodes categorical features, imputes zeros for null values, and normalizes the User_ID and Product_ID features. The second dataset, `some_one_hot`, builds off of the first by one-hot encoding the Occupation, City_Category, and product category features. The final dataset, `all_one_hot`, builds off the second dataset by also one-hot encoding the Age and Stay_In_Current_City_Years features. Both the `some_one_hot` and `all_one_hot` datasets have versions where the product category features are combine named `p_combined_some_one_hot` and `p_combined_all_one_hot`, respectively.

## 3. Analysis <a name = "analysis"></a>

The [black_friday_analysis](https://github.com/shahidhn/f18-datascience-final/blob/master/black_friday_analysis.ipynb) notebook generates charts and figures describing the original dataset, customer clusters, and results from modeling. 

### 3.1 Overview

Descriptions of the the original dataset include correlations and distributions for the Gender, Age, Occupation, City_Category, Stay_In_Current_City_Years, Marital Status, and product category features. Customer distributions for the top 3 products sold are also included.

### 3.2 Clustering

Customer clusters are generated after principal component analysis or factor analysis has been ran on the customer demographics. Graphs of the first four components/factors are shown along with a 3-D plot of the components/factors. After running K-means clustering, the three clusters generated are colored and shown. Distributions of each feature for each cluster are shown to visualize similarities and differences between each cluster.

### 3.3 Modeling Results

Modeling results include the distribution of residuals (differences between predicted purchase amounts and actual purchase amounts) for final pre-trained model. In addition, a graph of the weighted importance of each feature in the final model is shown.

## 4. Modeling <a name = "modeling"></a>

The [black_friday_modeling](https://github.com/shahidhn/f18-datascience-final/blob/master/black_friday_modeling.ipynb) notebook runs predictive models to find the customer purchase amount for a product given the customer demographics and product category. 

### 4.1 Feature Reduction

Principal component analysis and factor analysis are used to reduce the dimensionality of the dataset and allow for visualization after preprocessing. The top 95% of variance explained by each dimensionality reduction method is used as a baseline for feature reduction. Separate datasets are created from each feature reduction method.

### 4.2 Clustering

K-means clustering is used to segment customers based on their demographics. The elbow method is used to determine the optimal number of clusters to create for the normal model, principal component analysis, and factor analysis datasets. After a specified amount of clusters is given, k-means clustering is ran to create additional datasets containing also the user assigned cluster number.

### 4.3 Predictive Modeling

Training and testing splits are created for each dataset produced in the feature reduction and clustering sections. Linear, multi-layer perceptron, and XGBoost regression models are trained and scored using each of produced datasets.

### 4.4 Save Results

The best models and source datasets from XGBoost regressions are saved to the `outputs/` folder. Additionally, the cluster assignments for each user from running k-means clustering on the best source dataset is also saved. 

## 5. Recommender <a name = "recommender"></a>

The [black_friday_recommender](https://github.com/shahidhn/f18-datascience-final/blob/master/black_friday_recommender.ipynb) notebook recommends the top product categories for each cluster given the predicted purchase amount for the cluster.

Using the best models and datasets saved in the modeling portion, the total predicted purchase amounts for each category in each cluster are found and saved in `outputs/`. Additionally, the top specified k categories for each cluster are shown. 
