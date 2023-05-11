# mini-project-III

## Project Goals

The objective of this project was to analyze data about financial clients , and use this to generate clusters or groups of different clients, based on their personal data and their banking behaviour. 

Additionally, the project was to practise skills and gain experience with: 

- Collaboration 
- Data Wrangling
- Data Visualization
- Data Preparation and Feature Engineering
- Dimensionality Reduction
- Unsupervised Learning

## Process
# Data Inspection:

Before the spreadsheets were imported into Pandas DataFrames, they were viewed on Excel, and we inspect details like missing information, duplication, etc. They were then imported into Pandas DataFrames. We used visualization techniques like histograms, boxplots, and correlation matrices to check the distribution of data and observe outliers, etc.

!['age' histogram](images/age_histogram.png "Distribution of age")
!['income' histogram](images/income_histogram.png "Distribution of income")


# Feature Selection:
Based on our inspection, we decided that some columns were unnecessary (eg personal names, unique customer numbers, zip codes and cities) and discarded them. We then selected what we felt were the relevant features for each segmentation. Our choices eventually came to the following:

**Customer Segmentation features**:
1. `income` - (numerical) The assumed montly income of each client.
2. `age` - (numerical) The age of each client 
3. `years_with_bank` - (numerical) The number of years with the bank.
4. `marital_status` - (categorical) These are 4 classes (which we assumed represented single, married, divorced or widowed).
5. `have_children` - (categorical) originally 'number_of_children', this was binarized into 0 or 1 i.e. having any children or not.
6. `state_code_XX` where XX represents each state code - (categorical) Using `pd.get_dummies()`, we split the original `State_Code` into 33 columns.

**Banking Behaviour Segmentation features**:
1. `creditSpendToLimitRatio` - (numerical) average credit transactions each month divided by the credit limit (assuming monthly credit limit)
2. `averageMontlySpending` - (numerical) average checking account transactions per month
3. `numTnxMonthlyAverage` - (numerical) average number of transactions each month
4. `incomeToSavingsRanking` - (categorical) 5 classes showing how much of each customer's incoming went into their savings accounts each month
5. `numAccounts` - number of accounts each customer owned

We applied Pandas aggregate functions to get features that were not directly available e.g. creditSpendingtoLimit ratio.

# Data Wrangling

We explored our features via visualizations, and further fine-tuned the features, using different strategies, including but not limited to:

**Binarization**: We binarized some numerical columns e.g. a column that represented number of children was binarized to just having children - No for 0 children and Yes for anything else.

**Handling Outliers**: We removed some outliers by clipping, or converted numerical columns into cateogrical columns using bins or pd.qcut

Scaling: We applied sklearn scalar functions to the model dataset like `MinMaxScalar` and `StandardScalar` before we applied the Clustering functions.

# Clustering

We applied KMeans, DBSCAN and Hierarchcal Clustering functions to the different datasets.

## Results
### Clustering Outcomes

**Customer dataset**
| Type of Clustering    | n_clusters |
| --------------------- | ------- |
| KMeans                | 3       |
| DBSCAN                | 2       |
| Aggloremative         | 2       |


**Banking Behavior**

| Type of Clustering    | n_clusters |
| --------------------- | ------- |
| KMeans                | 4       |
| DBSCAN                | 5       |
| Aggloremative         | 6       |

### Radar Charts

1. Customers:
[customer_radar_plot](images/radar_plot_customers.jpg  "Customer Radar Plot")


2. Banking Behaviour:

Below are the radar charts showing clusters of Banking Behavior, using the Agglomerative function. Please see the notebooks for the other outcomes. 

![cluster 0](images/cluster0.png "Cluster 0")
![cluster 1](images/cluster1.png "Cluster 1")
![cluster 2](images/cluster2.png "Cluster 2")
![cluster 3](images/cluster3.png "Cluster 3")
![cluster 4](images/cluster4.png "Cluster 4")

**Observations**: It seems that the `numAccounts` and `numnumTnxMonthlyAverage` are the dominant criteria for clustering these datapoints.


### Dimensionality Reduction with PCA

Plot of the expanded variance ratio for the customer features:
![BankingBehavior](images/expanded_variance_ratio_banking_behavior.png "Expanded variance ratio")

(See the notebook for the variance ratio of the customers). In both cases, the first 2 components contribute more than 50% of the total. 

## Challenges:
1. Feature selection: it was a challenge to decide what information was relevant. For example, we wanted to keep as much information from the addresses as possible, but soon realized that would not be practical.
2. Handling categories: We observed that categories tended to bias the clustering i.e. the clustering functions would just group the datapoints by the dominant features like gender. We had to fine tune scaling parameters to offset this tendency.
3. Working with assumptions. We noticed that some customers in the bank did not have any accounts, but we had to assume that their data was still valid i.e. they had other banking accounts and were still active clients. We also had to make assumptions about the meaning of some columns e.g. that income was *montly* income and not *yearly* income. 
4. Understanding PCA



