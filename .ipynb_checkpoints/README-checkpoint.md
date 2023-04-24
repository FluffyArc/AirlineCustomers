# Airline Value Customers Dataset

## Problem Statement:
This dataset stores the information of airline customers' who flew with the airline in the past couple of years. **Due to the large information, the business team needs to know in how many segment can the customers divided into, so it could extract a useful information for the business team** 

## Goals:
To determine the airline's customers segmentation.

# Exploratory Data Analysis (EDA)
Below are the features on the dataset:
<table>
    <tr>
        <td>MEMBER_NO</td>
        <td>Unique ID for members</td>
    </tr>
    <tr>
        <td>FFP_DATE</td>
        <td>Frequent Flyer Program Join Date</td>
    </tr>
    <tr>
        <td>FIRST_ALIGN_DATE</td>
        <td>The date of the customer's first flight</td>
    </tr>
    <tr>
        <td>GENDER</td>
        <td>Gender information of the customer</td>
    </tr>
    <tr>
        <td>FFR_TIER</td>
        <td>Tier from Frequent Flyer Program</td>
    </tr>
    <tr>
        <td>WORK_CITY</td>
        <td>Customer's hometown</td>
    </tr>
    <tr>
        <td>WORK_PROVINCE</td>
        <td>Province of origin</td>
    </tr>
    <tr>
        <td>WORK_CITY</td>
        <td>City of origin</td>
    </tr>
    <tr>
        <td>AGE</td>
        <td>Customer's age</td>
    </tr>
    <tr>
        <td>LOAD_TIME</td>
        <td>Date data was taken</td>
    </tr>
    <tr>
        <td>FLIGHT_COUNT</td>
        <td>Number of flights Customer</td>
    </tr>
    <tr>
        <td>BP_SUM</td>
        <td>Itinerary</td>
    </tr>
    <tr>
        <td>SUM_YR_1</td>
        <td>Fare revenue 1 (revenue)</td>
    </tr>
    <tr>
        <td>SUM_YR_2</td>
        <td>Votes price income</td>
    </tr>
    <tr>
        <td>SEG_KM_SUM</td>
        <td>Total Distance (km) of flights that have been carried</td>
    </tr>
    <tr>
        <td>LAST_FLIGHT_DATE</td>
        <td>Date of last flight</td>
    </tr>
    <tr>
        <td>LAST_TO_END</td>
        <td>Time from the last boarding time to the end of the observation window</td>
    </tr>
    <tr>
        <td>AVG_INTERVAL</td>
        <td>Average time interval</td>
    </tr>
    <tr>
        <td>MAX_INTERVAL</td>
        <td>Maximum time interval</td>
    </tr>
    <tr>
        <td>EXCHANGE_COUNT</td>
        <td>Number of exchanges</td>
    </tr>
    <tr>
        <td>avg_discount</td>
        <td>Average discount obtained</td>
    </tr>
    <tr>
        <td>Points_Sum</td>
        <td>Total points earned by members</td>
    </tr>
    <tr>
        <td>Point_NotFlight</td>
        <td>Points not used by members</td>
    </tr>
</table>

Based on the result of ```df.info()``` command, there are some important notes founded:
1. some features have a missing value on it, like **GENDER, WORK_CITY, WORK_PROVINCE, WORK_COUNTRY, AGE, SUM_YR_1, and SUM_YR_2**
2. The date time features like **FFP_DATE, FIRST_FLIGHT_DATE, LOAD_TIME, and LAST_FLIGHT_DATE** also not on the proper types as well.

The Exploratory process also found that 667 rows from **WORK_CITY and WORK_PROVINCE** features with a single character only (.) instead of the city's name.

**Zero** duplicated value also founded in this step.

## Descriptive Analysis
Based on the code ```df.describe()```, Almost all the features are having a quite major of the gap between min and max OR Q3 and max (outliers indications), except for the following features:
1. FFP_TIER
2. EXCHANGE_COUNT
3. avg_discout

## Univariate Analysis
![univariate_analysis](https://user-images.githubusercontent.com/40890491/233973629-956931d1-f293-4e55-a7e7-f5e0ab3c23b7.png)
![univariate_analysis_2](https://user-images.githubusercontent.com/40890491/233973755-c07a367d-ee45-424b-b892-bdd49797dc03.png)

The only feature with normal distribution is AGE

## Multivariate Analysis
![heat_map_analysis](https://user-images.githubusercontent.com/40890491/233973867-253b8aac-99d2-4e10-8c66-22283609b087.png)
Refering to the heatmap plot, the features below have a correlation score **>0.7**, which indicates the redundant value between the features.
1. BP_SUM and FLIGHT_COUNT have a correlation of **0.79**
2. SUM_YR_1 and FLIGHT_COUNT have a correlation of **0.75**
3. SUM_YR_1 and BP_SUM have a correlation of **0.85**
4. SUM_YR_2 and FLIGHT_COUNT have a correlation of **0.79**
5. SUM_YR_2 and BP_SUM have a correlation of **0.88**
6. SEG_KM_SUM and FLIGHT_COUNT have a correlation of **0.85**
7. SEG_KM_SUM and BP_SUM have a correlation of **0.92**
8. SEG_KM_SUM and SUM_YR_1 have a correlation of **0.80**
9. SEG_KM_SUM and SUM_YR_2 have a correlation of **0.85**
10. MAX_INTERVAL and AVG_INTERVAL have a correlation of **0.72**
11. Points_Sum and FLIGHT_COUNT have a correlation of **0.75**
12. Points_Sum and BP_SUM have a correlation of **0.92**
13. Points_Sum and SUM_YR_1 have a correlation of **0.79**
14. Points_Sum and SUM_YR_2 have a correlation of **0.83**
15. Points_Sum and SEG_KM_SUM have a correlation of **0.85**

Moreover, since the date-time feature such as **FFP_DATE, FIRST_FLIGHT_DATE, LOAD_TIME, and LAST_FLIGHT_DATE** are already represented by the **AVG_INTERVAL**, which informed the average flight time, it will be deleted in the next step.

# Data Preprocessing

## Handling Missing and Inccorect Value
On the previous step, some features were founded to have a missing value and incorrect value.
1. The ```mode[0]``` function is used to replace the dot (.) character on both WORK_CITY and WORK_PROVINCE feature
2. The ```mode[0]``` function also used to fill the remaining categorical features with the missing value on it.

## Encode Categorical Features
In order to calculate categorical features, it need to be transform into numerical feature. The encode process is performed by using the ```df_encode[i] = df_encode[i].astype('category').cat.codes``` code

## Removing Redundant Features
Based on the correlation analysis before and the Feature Importance analysis, therefore, the following features will be deleted due to redundant and unnecessary value using the ```drop()``` function:
1. FLIGHT_COUNT
2. BP_SUM
3. SUM_YR_1
4. SUM_YR_2
5. SEG_KM_SUM
6. MAX_INTERVAL
7. LAST_FLIGHT_DATE
8. FFP_DATE 
9. FIRST_FLIGHT_DATE
10. LOAD_TIME

## Removing Outliers
The outliers from the features also need to be removed since they can affect the model's performance. The Outliers were treated by using the **IQR** method. Below is the dataframe info post outlier treatment.

![df_without_outliers](https://user-images.githubusercontent.com/40890491/233977008-f6639ea8-b6c9-4245-b7e8-ed25fed3ee84.png)

## Data Transform
Since there are still too many features left, the dimension of the feature will be reduced. Hence, the standardization process is also required. The used dataframe will be standardized using ```StandardScalar()``` function.

The dimensional reduction process was performed using ```PCA()``` function.

# Identifying Best Number of Clusters
Before performing the clustering model (in this case K-Means), we need to know the optimal number of clusters beforehand. One of the options is by using the **Elbow Method**

![elbow_method](https://user-images.githubusercontent.com/40890491/233978220-2752b1d9-04e6-4457-94e3-0b1851deb9af.png)

Based on the Elbow Graph above, the optimal number of clusters would be 2/3/4 clusters.

# Evaluate Model
To ensure our Elbow Analysis is correct, we need to evaluate every possible cluster number by calculating the **Silhouette Score**.

![silhouette_score](https://user-images.githubusercontent.com/40890491/233978856-874f2a9f-dd64-4751-b9f6-477cb5684f99.png)

Based on the width of every cluster and the average silhouette score, it can be seen that dividing the data into every option (k = 2-6) is a good option. However, based on the silhouette score that calculates the performance of the model, dividing data into **3 clusters** is the most optimal option.

# Fitting Model(K-Means)
The use of the following code resulting the clustering below.
```KMeans(n_clusters=3, init='k-means++', max_iter=300, n_init=10, random_state=42)```

![k-means](https://user-images.githubusercontent.com/40890491/233979512-a190235b-247f-4b7f-b330-4d7db5195eef.png)

# Labels Defition:
Based on the previous clustering analysis, here are the segmentation for every cluster created:
1. Label 0: Male (41++ yo) who are from **Wenzhou, Beijing, China,** with an average flight time of **62 hours** and holds a Tier 4 membership
2. Label 1: Male (44++ yo) who are from **Guangzhou, Beijing, China,** with an average flight time of **38 hours** and holds a Tier 4 membership
3. Label 2: Male (41++ yo) who are from **Haarlem, Sichuan, China,** with an average flight time of **62 hours** and holds a Tier 4 membership

# Business Extraction/Recommendation
Since people from **Beijing, China** are the highest segment of all, the company might be able to apply some bundling promo trips **from/to Beijing, China** to drive more customers.

Morover, company can target customers with **Tier 4** in their membership to apply some additional discounts or other promos.