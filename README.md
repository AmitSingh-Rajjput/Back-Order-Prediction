# Back-Order-Prediction

## Problem Statement 

To build a model which will be able to predict whether an order for a given product can go on backorder or not.  

A backorder is the order which could not be fulfilled by the company. Due to high demand of a product, the company was not able to keep up with the delivery of the order. 

### Language Used - Python
### Deployed on - Heroku
### Framework - Flask
### Tools - PyCharm
### Algorihtms - K-Mean Clustring, XG Boost Classifer and Random Forest Classifer
### Accuracy Metric - AUC Score

## Architecture

![Architeture](https://user-images.githubusercontent.com/66250589/116420100-4e0e9180-a85b-11eb-97cb-6f9f40c6b4d8.PNG)


## Data Description 

Data Description: This dataset describes the differet parmaeters to judge whether a website is a phising website. 

### Features: 

The client will send data in multiple sets of files in batches at a given location. Data will contain 24 columns:  

sku – 		 	Random ID for the product 

national_inv –   	Current inventory level for the part 

lead_time – 	 	Transit time for product (if available) 

in_transit_qty – 	Amount of product in transit from source 

forecast_3_month – 	Forecast sales for the next 3 months 

forecast_6_month – 	Forecast sales for the next 6 months 

forecast_9_month – 	Forecast sales for the next 9 months 

sales_1_month – 	Sales quantity for the prior 1 month time period 

sales_3_month – 	Sales quantity for the prior 3 month time period 

sales_6_month – 	Sales quantity for the prior 6 month time period 

sales_9_month – 	Sales quantity for the prior 9 month time period 

min_bank – 		Minimum recommend amount to stock 

potential_issue – 	Source issue for part identified 

pieces_past_due – 	Parts overdue from source 

perf_6_month_avg – 	Source performance for prior 6 month period 

perf_12_month_avg – 	Source performance for prior 12 month period 

local_bo_qty – 		Amount of stock orders overdue 

deck_risk – 		Part risk flag 

oe_constraint – 	Part risk flag 

ppap_risk – 		Part risk flag 

stop_auto_buy – 	Part risk flag 

rev_stop – 		Part risk flag 

went_on_backorder – 	Product actually went on backorder. This is the target value.

Apart from training files, we also require a "schema" file from the client, which contains all the relevant information about the training files such as: 

Name of the files, Length of Date value in FileName, Length of Time value in FileName, Number of Columns, Name of the Columns, and their datatype. 


## Data Validation

In this step, we perform different sets of validation on the given set of training files.

### Name Validation-

We validate the name of the files based on the given name in the schema file. We have created a regex pattern as per the name given in the schema file to use for validation.

After validating the pattern in the name, we check for the length of date in the file name as well as the length of time in the file name. If all the values are as per

requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."

### Number of Columns -

We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."

### Name of Columns -

The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

### The datatype of columns -

The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to

"Bad_Data_Folder".

### Null values in columns -

If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".

### Data Insertion in Database

Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database.

### Table creation in the database -

Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the

table is already present, then the new table is not created and new files are inserted in the already present table as we want training to be done on new as well as old training

files.

### Insertion of files in the table -

All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and

is moved to "Bad_Data_Folder".

## Model Training

### Data Export from Db - 

The data in a stored database is exported as a CSV file to be used for model training.

### Data Preprocessing

1: Drop the columns not required for prediction.

2: For this dataset, the null values were replaced with ‘?’ in the client data. Those ‘?’ have been replaced with NaN values.

3: Check for null values in the columns. If present, impute the null values using the categorical imputer.

4: Replace and encode the categorical values with numeric values.

5: Scale the numeric values using the standard scaler.

### Clustering -

KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the

number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms To train data in different clusters. The Kmeans model is

trained over preprocessed data and the model is saved for further use in prediction.

### Model Selection -

After clusters are created, we find the best model for each cluster. We are using two algorithms, "Random Forest Classifier" and "XGBoost Classifer". For each

cluster, both the algorithms are passed with the best parameters derived from GridSearch. We calculate the AUC scores for both models and select the model with the best score.

Similarly, the model is selected for each cluster. All the models for every cluster are saved for use in prediction.

## Prediction

### Data Export from Db - 

The data in the stored database is exported as a CSV file to be used for prediction.

### Data Preprocessing

a) Check for null values in the columns. If present, impute the null values using the Categorical imputer.

b) Check if any column has zero standard deviation, remove such columns as we did in training.

### Clustering -

KMeans model created during training is loaded, and clusters for the preprocessed prediction data is predicted.

### Prediction -

Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.

Once the prediction is made for all the clusters, the predictions are saved in a CSV file at a given location and the location is returned to the client.

## Deployment

We will be deploying the model to the Heroku Cloud platform.

### Advantage of Heroku:

1: Beginner and start-up-friendly

2: It allows you to create a new server in just 10 seconds by using the interface of Heroku Command Line.

3: This cloud computing platform takes care of patching systems and keeping everything healthy.

4: A range of automated functionalities including the scaling, configuration, setup, and others

5: Easy integration with other AWS products.

## Output:

After compilation you can see this type of interface. Where you can give Custom files path for prediction or Default File prediction.

![Back order pd](https://user-images.githubusercontent.com/66250589/120898093-11e30380-c647-11eb-9a22-af85a8601ef2.JPG)






