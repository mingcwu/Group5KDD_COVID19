# Group5_KDD_Project_COVID19

## Team Members
Osama Almasri; 
Sabda Karkera 
Rishitha Muddana 
Raga Preethi Potu 
Ming Wu

## Project Introduction
COVID-19 is a disease that was identified in Wuhan, China. It is a new strain of coronavirus that has not been previously identified in humans and is now being spread throughout the world. Coronaviruses are a large family of viruses that are known to cause illness ranging from the common cold to more severe diseases such as Severe Acute Respiratory syndrome (SARS) and Middle East Respiratory Syndrome (MERS). It is the cause of an outbreak of respiratory illness.

The people most affected or at the risk of getting very sick from COVID-19 include older adults and people who have serious chronic medical conditions and the ones in the higher-risk population.

Our main goal is to explore the data and predict the total confirmed ratio of COVID-19 cases with respect to the total number of beds considering the shortages in essential staff and Personal Protective Equipment (PPE’s).

## Research Question
What is the total confirmed ratio of COVID-19 cases amongst residents and staff of US nursing homes to the total number of beds considering the shortages in essential staff and Personal Protective Equipment (PPE’s)?

## Relevant Domain Information
https://www.theverge.com/2020/3/5/21166088/coronavirus-COVID-19-protection-doctors-nurses-health-workers-risk 

https://www.latimes.com/california/story/2020-04-10/coronavirus-covid19-hospital-nonclinical-staffers-fear 

https://www.msn.com/en-us/health/medical/doctors-nurses-at-high-risk-of-coronavirus-as-the-front-lines-of-medical-care/ar-BB10PElE

## Data and Source Description
https://data.cms.gov/Special-Programs-Initiatives-COVID-19-Nursing-Home/COVID-19-Nursing-Home-Dataset/s2uc-8wxp

We have chosen The Nursing Home COVID-19 Public data that includes data reported by nursing homes to the CDC’s National Healthcare Safety Network (NHSN) system COVID-19 Long Term Care Facility Module, including Resident Impact, Facility Capacity, Staff & Personnel, and Supplies & Personal Protective Equipment, and Ventilator Capacity and Supplies Data Elements. The file contains an individual record for each certified Medicare skilled nursing facility/Medicaid nursing facility and the ending date for each collection week that has been updated weekly.

This information is used to assist with national surveillance of COVID-19 in nursing homes, and support actions to protect the health and safety of nursing home residents. There could be some limitations in the data set such as the availability of testing may impact the number of confirmed COVID-19 cases facilities reported. For example, facilities that did not have the ability to test all residents a few weeks ago would not be able to report all residents with confirmed cases. Similarly, access to testing can vary by state, region, or facility. This dataset contains 123k rows and 59 columns, where each row is a nursing home.The data collected through the NHSN system directly supports this initiative by helping to prioritize the nursing homes with testing needs and an increasing number of cases.

## Approach
### Data Understanding
This process is one of the very first steps in the CRISP-DM process model that is required to understand the structure of the data well enough and be able to model data successfully. It obtains data and verifies that it is appropriate for one's needs. This phase includes gathering data, describing it, exploring data and verifying the data quality. Data manipulation and basic statistical techniques are used to further check into the data for their range of values and their distributions. This can be done using exploratory data analysis, which is referred to in short as EDA.

In our data, we saw that there were 123,143 records and 59 columns. We had numerical and categorical data such as float, integer, and object.

The data has been collected from certified nursing facilities nationwide, with each data entry being an individual record for each certified nursing facility with an ending date for each collection week starting from May 24. 

The main attributes are about COVID-19 related medical information (such as Personal Protective Equipment, test availability, positive case, and death count for the residents and staff) of the nursing facility.

The data set includes both validated and non-validated data entries. Data quality assurance procedure has been applied to the following variables to validate the data entry by flagging erroneous data. It should be noted that the rest of the columns are not validated in any way:
1) Residents Weekly Admissions COVID-19; 
2) Residents Weekly Confirmed COVID-19;
3) Residents Weekly Suspected COVID-19; 
4) Residents Weekly COVID-19 Deaths; 
5) Residents Weekly All Deaths.
 
### Data Preparation
Data preparation is one of the most important steps where the data is collected and preserved for other purposes and needs some refinement before it is ready to use for modeling. Our dataset contains many missing values. These columns can be imputed using imputation techniques such as mean, median, mode, or advanced imputation methods such as k-nearest imputation, etc. Another important factor to look into a dataset is to find the missing values. The nursing home dataset contains missing values and henceforth can be detected and solved with the help of visualization techniques.

We used box plot visualization to analyze some of the columns and detected the outliers present in them. After they have been detected, they can be removed using other techniques such as histogram, IQR, Z-Score and scatter plots. Finding the relationship between the variables and treating them accordingly in the pre-processing steps is also considered to be an important factor during the process of modeling. To find the relationship or correlation between the attributes in the dataset, we have used correlation matrix. Certain attributes pairs such as:

Residents Weekly Admissions COVID-19 and Residents weekly COVID-19 deaths
Residents Total Admissions COVID-19 and Residents Total COVID-19 deaths
Residents Weekly all Deaths and Residents Weekly COVID-19 deaths
Residents Weekly confirmed and Staff weekly confirmed COVID-19
Residents Total COVID-19 deaths (Vs) Staff Total confirmed COVID-19

Staff weekly confirmed COVID-19 (Vs) Residents total COVID-19 deaths have shown a correlation value between 0.5 to 0.7, which is considered to be appropriate.
The dataset also contains many categorical attributes and there are some libraries that do not take categorical variables as input. Thus, we convert them into numerical variables. Therefore, in our dataset we used dummy encoding by converting a categorical input variable into a continuous variable or a binary variable. Presence of a level is represented by 1 and absence is represented by 0. Some of the other important things that can also be done to the data before we model it are:
1. Look for high cardinality features and drop them.
2. Use Label Encoder to transform non-numerical labels to numerical labels that are always between 0 and n_classes-1. However this may sometimes decrease the performance of the model.

The followings are what we did for the data cleaning, followed by outlier handling and data imputation:
 
- Remove the records in which the majority of variables are empty;
- Remove the records marked as Not “SubmittedData” and the records fail to pass Quality Assurance Check;
- For the remaining data after the above two steps, drop “SubmittedData” and “PassedQualityAssuranceCheck” since they only have one value (i.e., “Y”);
- Remove the variables that are empty for more than two thirds records;
- Because the maximum capacity of all nursing homes in the dataset is 815, it is impossible for the number of occupied beds of any nursing home in the dataset to exceed 815. Therefore, for outlier handling, the number of occupied beds at a given nursing home is capped at the capacity of that nursing home (i.e., 100% occupancy);

 
- The number of deaths per 1000 residents should not exceed 1000. Therefore, as outlier handling, the records contrary to that are removed;
      
- For data imputation, the mode is used to impute categorical variables. The mean is used to impute numerical valued variables.

### Machine Learning
We are dealing with a supervised learning model as our nursing home dataset contains both input and output variables. A supervised learning contains two types of learning problems out of which the regression model could possibly be related to the dataset as our target variable is a real value. Hence, it is used to predict a class label.

Since the number of COVID-19 cases can be highly influenced by the number of residents or number of beds in the facility, we chose to create a new variable; CovidtoOccupancyRatio. This ratio has been calculated by dividing the number of resident confirmed cases by the total number of occupied beds.

By setting this as a target variable, we are looking to predict a numerical variable which is the total number of resident cases in the particular week (CovidtoOccupancyRatio). We took a subset of the dataset that was mainly focused on the shortages of Personal Protective Equipment (PPE’s) as independent variables to predict the number of resident confirmed cases. A linear regression model was used but unfortunately, a lot of noise in the data made this target variable very difficult to predict and an R-squared of only 4% was achieved. With these results, we moved to another choice which is creating another target variable which takes the ratio of staff and residents confirmed with COVID-19 to the total number of available beds.

From there, we examined this variable for outliers and removed them. To avoid dealing with numerical target variables, we chose to discretize this new target variable to Low, Medium and High. We started with random bins and ran a random forest model that achieved only 47% accuracy. To improve the results, we plotted a histogram of the target variable to help us choose the right cut off points for the bins and got the following results:

With this histogram, it became easier for us to determine the cut off points for Low, Medium and High. We chose cut bins 0, 0.1, 0.4 and 1. The corresponding classes were saved in a new target variable; ConfirmedCases. The output shows a slightly unbalanced count for each class but it is nothing that should skew the model interpretation.

Once the new target variable was cleaned and preprocessed, we trained Random Forest and Decision Tree models. 

### Evaluation
Since we are not dealing with unbalanced data, accuracy makes a good measure. When the target variable is unbalanced, we can use alternative measures like Area Under Curve (AUC). Both Random Forest and Decision Tree models achieved 61% accuracy (14 percentage points higher than the previous model with random bins). With these results, we can get some prediction of the ratio of COVID-19 cases among residents and staff of nursing homes. 

### Known Issues
The nursing home COVID-19 dataset is said to have many missing values. This was found by using the n_miss () function and by just looking into the dataset. Other factors related to the dataset were of the outliers that were detected using box plot visualization. Some essential variables were said to have outliers, which could be removed by using other visualization techniques as well. There were also some variables that were highly correlated that would be difficult for us to change one variable without having to change the other. There were also variables that were less or not correlated at all. This could lead to unpredictability of the target variable. Our dataset has many categorical variables that could be a problem in evaluating our model. This can be solved by simply assigning numbers to categorical variables by performing dummy encoding.

We faced a challenge creating an appropriate target variable since the size of the nursing home can have an impact on the total number of COVID-19 cases. A trial and error with the different derived target variables was necessary to find a good metric. 

### Conclusion
Our dataset had many missing values so we used imputation techniques such as mean, median, mode, or advanced imputation methods such as k-nearest imputation. We also used box plot visualization to analyze some of the columns and detected the outliers present in them. We found the relationship between the variables and treated them accordingly.

The outcome of the first trial in the Linear Regression was 4% of R-squared and outcome of the last model was an accuracy of 61%. This is a good starting point to predicting COVID-19 severity of low, medium or high in nursing homes. This can definitely improve in future work and as we learn more about this virus.
A future work to improve the results is to collect newer data as the testing capacity is increasing in the United States and the CDC is setting better standard operating procedures to collect and report data. At the beginning of this pandemic, both testing capacity and the lack of standardization in data collection have caused the dataset to be inaccurate which can cause any predictive model to be inaccurate as a result of that.
 

