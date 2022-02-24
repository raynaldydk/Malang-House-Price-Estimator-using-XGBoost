# Malang House Price Estimator: Project Overview
* Create a tool to estimate house price in Malang (MAE ~ Rp. 300 jt.) to help people get solid ground price before buying a house in Malang.
* Scraped 3000 house listing from Lamudi.co.id using Python & BeautifulSoup
* Enginereed price/m2 feature for removing outliers
* Optimized XGBoost Regressor using RandomSearchCV & GridSearchCV to reach the best model.

## Resources Used
* Python 
* Packages: pandas, numpy, sklearn, seaborn, matplotlib, bs4, pickle, jcopml

## Web Scraping
Create webscraper for lamudi.co.id to scrape 100 pages of house listing in Malang. With each listing we got:
* Price
* Lot size
* Building size
* Bedroom
* Facility
* Location

## Data Cleaning
After scraping data I clean the data so it can be used to our model later. What I did:
* Parsed numeric values in Price
* Removing nan values in Price
* Removing outliers caused by human error
* Create new column price per m2 to check more human error
* Dropping facility column because it is biased
* Binning location to 5 district in Malang
* Simplify the price column by divide it by 1.000.000

## EDA
* I look at the distribution of each columns and create a visualization for feature vs target
![download](https://user-images.githubusercontent.com/96482347/155471249-ddbb7843-68ef-415b-8f6b-a88f65886cd7.png)
![download (1)](https://user-images.githubusercontent.com/96482347/155471277-44becb10-f861-48e9-b9f6-984d182f2b19.png)
![boxplot](https://user-images.githubusercontent.com/96482347/155470821-18d32106-340f-4d5f-9bdd-b7f9e18e2f8e.png)

## Model Building
* I split the data into train and test. With test size of 20%
* I pick XGBoost for my model and evaluated it with Mean Absolute Error. I use MAE because it's easier to understand and calculate how much error I can take
* Creating pipeline to separate numerical and categorical features to preprocess it
* For numerical features I don't do anything because XGBoost is tree base model and scaling doesn't do anything
* For categorical features I preprocess it using sklearn one hot encoder in the pipeline
* Use RandomizedSearchCV to get the best result

## Model Evaluation
* First model have 96% accuracy on training data ,but only 87.5% on test. The model is overfitted.
* I tune the model hyperparameter using GridSearchCV and do some experiment.
* Tuned model have 89% accuracy on training data and 87% on test data. I think this model is great.
* Tuned model have MAE of 300-350 Million IDR in 3 Fold Cross Validation

![xgboost_tuned](https://user-images.githubusercontent.com/96482347/155473533-495a59ed-39d5-438d-be7d-f42cca17940d.png)

## Conclusion
* The model didn't perform really well with house that have price more than 2 Billion IDR
* We still can improve our model by train it using more data and create more features
* According to XGBoost lot size and house size are important to predict house price in Malang

![feature importance](https://user-images.githubusercontent.com/96482347/155473998-c4abc843-a570-49fa-8797-285b49c0c149.png)

