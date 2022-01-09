# Data Science Salary Estimator: Project Overview 
* Created a tool that estimates data science salaries (MAE ~ ₹ 2k  ) to help data scientists negotiate their income when they get a job.
* Scraped over 1000 job descriptions from glassdoor using python and selenium
* Engineered features from the text of each job description to quantify the value companies put on python, excel, aws, and spark. 
* Optimized Linear, Lasso, and Random Forest Regressors using GridsearchCV to reach the best model. 
* Built a client facing API using flask 

## Code and Resources Used 
**Python Version:** 3.9.7  
**Packages:** pandas, numpy, sklearn, matplotlib, seaborn, selenium, flask, json, pickle  
**For Web Framework Requirements:**  ```pip install -r requirements.txt```  

## Web Scraping
Tweaked the web scraper github repo (above) to scrape 1000 job postings from glassdoor.com. With each job, we got the following:
*	Job title
*	Salary Estimate
*	Job Description
*	Rating
*	Company Name 
*	Location
*	Company Size
*	Company Founded Date
*	Type of Ownership 
*	Industry
*	Sector
*	Revenue

## Data Cleaning
After scraping the data, I needed to clean it up so that it was usable for our model. I made the following changes and created the following variables:

*	Parsed numeric data out of salary 
*	Made columns for employer provided salary and hourly wages and monthly salary 
*	Removed rows without salary 
*	Parsed rating out of company text  
*	Transformed founded date into age of company 
*	Made columns for if different skills were listed in the job description:
    * Python  
    * R  
    * API
    * Machine Learning  
    * Sql  
    * Spark 
*	Column for simplified job title and Seniority 
*	Column for description length 

## EDA
I looked at the distributions of the data and the value counts for the various categorical variables. Below are a few highlights from the pivot tables. 


![image](https://user-images.githubusercontent.com/83781468/148693479-769a1538-4af8-476e-bd2b-c482a8ed605e.png)
![image](https://user-images.githubusercontent.com/83781468/148693503-ec25fb4a-c735-4d93-b303-9e11d7d9d184.png)

## Model Building 

First, I transformed the categorical variables into dummy variables. I also split the data into train and tests sets with a test size of 20%.   

I tried three different models and evaluated them using Mean Absolute Error. I chose MAE because it is relatively easy to interpret and outliers aren’t particularly bad in for this type of model.   

I tried three different models:
*	**Multiple Linear Regression** – Baseline for the model
*	**Lasso Regression** – Because of the sparse data from the many categorical variables, I thought a normalized regression like lasso would be effective.
*	**Random Forest** – Again, with the sparsity associated with the data, I thought that this would be a good fit. 

## Model performance
The Random Forest model far outperformed the other approaches on the test and validation sets. 
*	**Random Forest** : MAE = 2.2
*	**Linear Regression**: MAE = 10.3
*	**Lasso Regression**: MAE = 11.9

## Productionization 
In this step, I built a flask API endpoint that was hosted on a local webserver . The API endpoint takes in a request with a list of values from a job listing and returns an estimated salary. 
