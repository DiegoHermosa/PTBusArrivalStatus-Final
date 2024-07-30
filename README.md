# Capstone: Bus Arrival Status to Stop Prediction
### Business Understanding ### 
The Public Transportation Companies deal daily with the challenge to ensure their buses be on time in every Stop so passengers can reach their destinations on time too. Sometimes, some factors like the traffic, weather and peak hours avoid meet that goal. 

**Research question:**

Can the Public Transportation Bus Arrival to Stop be predicted as On Time, Early Or Late?

**Why this question is important?**

Be able to predict the Stop Bus Arrival Status with high accuracy will be very helpful to the users who can plan better their trips. For companies this can be very benefitial so they provide this status in the several platform and services they offer to users:
   - App-Based Tracking: Provide real-time bus arrival information, like the Miami-Dade Transit app
   - Digital Displays at Stops: Some bus stops have digital displays that show expected arrival information.
   - Text-Based Alerts: Some transit authorities offer text message alerts that notify users of bus arrival information.

### Data ### 
The data comes from the Public Trasportation History database that the company I work for has developed for a Public Transportation Company. The database is updated daily with the information about:
- City bus routes.
- City bus stops.
- Buses.
- For every bus, a 15 minutes frequency was defined to visit each stop of the assigned route.
- For every stop and bus, a record is stored containing all the data when a Bus arrived and left each stop.

For the scope of this capstone project, I extracted the Bus Arrival to Stop historical data filtering a single Stop and for a date range of 3 months(April-2024 to June-2024).

The data can be found in this link:

[pt_bus_stop.csv](https://github.com/DiegoHermosa/PTBusArrivalStatus-Final/tree/main/data/pt_bus_stop.csv)

### Notebook Link ###
The following notebook contains all the development of the analysis carried out.

[PTBusStopArrivalStatus.ipynb](https://github.com/DiegoHermosa/PTBusArrivalStatus-Final/tree/main/PTBusStopArrivalStatus.ipynb)

### The techniques and Analysis ###
1. Extract the bus arrival to stop data from the Public Trasportation History database filtering a single Stop and for a date range of 3 months (April-2024 to June-2024).
2. Perform a cleaning of the information in order to eliminate redundant data and outliers.
3. Train KNN, Logistic regression, Decision tree, SVM and Ensemble models in search of the best one.
4. Because of the dataset is imbalanced the SMOTE oversampling technique will be used.
   
### Dataset Understading ###
#### The dataset contains the next featues:
- **ID**: record unique ID
- **UnitID**: Unique ID assigned to the Bus unit.
- **StopID**: Unique ID assigned to a Bus Stop.
- **StopNumbe**r: Friendly name assigned to Stop, this is defined by the Bus Company to name each Route Stop.
- **DateIN**: Date time when the Bus arrived to the Stop.
- **DateOUT**: Date time when Bus left the Stop.
- **WeekDay**: Day name of the arrival date time.
- **PrevWeekDay**: Day name of the previous arrival date time.
- **PeakHour**: Flag to mark as Yes/No if traffic is at its worst between 7 a.m. and 9 a.m. and 4 p.m. and 7 p.m.
- **StopDuration**: time spent by the bus within the Stop.
- **PrevDateIn**: Date time when the previous bus has arrived to the current stop.
- **DifferencePrevBusIn**: Time difference between PrevDateIn and DateIN.
- **TripDuration**: Time spent by the bus to complete a trip.
- **Odometer**: Bus odometer when it arrived to stop at DateIn.
- **PrevStopOdometer**: Bus odometer when it arrived to stop at PrevDateIn.
- **PrevStopNumber**: Previous stop.
- **DistanceLastStop**: Distance between the last stop visited and the current stop.
- **Speed**: Bus speed when it arrived to stop.
- **OTPMode**: Mode of calculation of the Stop status: 2 means Headway
- **OnTimeStatus**: Status assigned to the bus arrival (ObTime, Early, Late). This status is assigned according to Route Bus Frequency. Each 15 minutes a bus expected to arrive to each stop.
- **TripDistance**: Distance of the trip.
- **TripSpeedAvg**: Bus speed average within the trip.

### Data Preparation and Cleaning ###

![image](https://github.com/user-attachments/assets/3d5fa951-fd50-45b7-910b-fb2713aba84e)
<br/>
1. The missing values for some features were removed because there were few records without impacting the overall distribution. <br/>
2. Features with no relevant information were removed. <br/>
3. Categorical features like PeakHour and WeekDay were transformed to numerical values. <br/>

#### Correlations and Imbalance: ####
The dataset is imbalanced: <br/>
![image](https://github.com/user-attachments/assets/ce997df9-2dff-431a-acbb-ac5951f46462)
<br/>
For handling this imbalanced dataset we are going to use the SMOTE oversampling techique, it will help us to get better prediction of the Ontime Status.
<br/>
<br/>
The correlation matrix allowed us to identify some features with a high correlation between them which were removed:
<br/>
![image](https://github.com/user-attachments/assets/f3a15828-b5dd-410e-8817-15ce18cfa9c6)
<br/>
The new correlation matrix shows us the most relevant features.
<br/>
![image](https://github.com/user-attachments/assets/d5deae74-b7c4-4a1f-af09-0252f4a4bcc1)
<br/>

### Models and tools ### 
1. In the modeling stage we're going to use the next Machine Learning algotithms:
    - KNN
    - Decistion Tree
    - SVM
    - Logistic Regression
    - Random Forest
    - Gradient Boosting Classifier
    - AdaBoost Classifier
2. For each classification algorithm we will use the cross validation using the Gridsearch and hyperparameters.
3. With the aim to find the best model since the dataset is imbalanced we're going to use SMOTE tecnique for oversampling
4. The macro-averaged F1 score is the most suitable measure for multiclass classification and imbalanced dataset.

### Model Evaluation ###
After training and running the predictions with the proposed algorithms and techniques, this is the summary of the model performance scores:<br/>
![image](https://github.com/user-attachments/assets/b810b616-366b-4acf-b163-a41e677ebee0)
<br/>
![image](https://github.com/user-attachments/assets/a3992aea-18c3-4cec-a5c7-b7e59cf98cef)
<br/>
Based on the f1 score and training time, we found that:
- AdaBoostClassifier and GradientBoostingClassifier are overfitted so they are not going to be used.
- SVC has the best score, followed by KNN which is very close behind.
- SVC performed well but it took at lot time in comparison to the others models
- KNN has a good score and it was really fast so we are going to use it as our final model.
<br/>

### Feature Importances ###
![image](https://github.com/user-attachments/assets/8ab20978-2c50-4c26-a33b-917380f9dd56)
<br/>
Running the Feature Importance analysis with the KNN best model we found the next features which are the most relevant features for the OnTime Status prediction:
<br/>
- **DifferencePrevBusIn**: This feature has an important influence on the prediction as it contains the difference between the current arrival time and the previous one. As this value increases the status tends to be Late otherwie as it decreases to much the status will be Early. Late status means the travel time between stops could be affected by factor like the number of passengers, traffic and weather by ,identificating these factor the company can take decision about modify the bus frequency and design new routes. 
- **TripDuration**: The time needed by the bus to complete a route loop is also related to the number of passengers, traffic and weather, also there can be route detours, as the TripDuration increases the more Late statuses will be detected.
- **StopDuration**: This duration can be related with the number of passengers which were boarding and alighting the bus, unfortunately we were not able to include that feature in the dataset for the scope of this capstone. 
- **PeakHour**: The peak hour has an important influence because the city will have more traffic on those hours, more traffic and more people using the buses causing the travel time increases between stops so more Late statuses.  

### Findings
1. The results show us that with the help of Machine Learning techniques and the Exploratory Data Analysis it's possible to predict Bus Arrival Status.
2. According to the performance metrics we used the best model for the On Time Status prediction is KNN, it got a good score and due to the low training time it will be more suitable for implemetation that other models.
3. To meet the expected bus frequency (each 15 minute a bus visiting a stop), the company needs to monitor periodically the bus performance by checking each stop arrival status metric along with Time difference between Stop Visits, Trip Duration. Stop Duration and the Peak Hours.

### Next Steps and Recommendations
1. Include new features to datatset to help with the predictions:
    - Passenger Counter: the number of passengers which board and alight the bus could have an impact on the travel times so we recommend to the Public Transportation company give access to that info so we can extract it and include within the dataset.
    - Weather: the weather conditions also impact on the travel times, we recommend the weather be tracked by integrating weather API with Public Transportations System then we can include in the dataset.
2. Create new datasets for Stops from different Routes, that new data will help for tuning the features and Machine Learning Models

### References
1. About imbalanced datasets and the oversampling techniques:
    - https://machinelearningmastery.com/multi-class-imbalanced-classification/
    - https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/
    - https://towardsdatascience.com/the-right-way-of-using-smote-with-cross-validation-92a8d09d00c7
2. About the ROC Curve for multiclass:
    - https://github.com/vinyluis/Articles/blob/main/ROC%20Curve%20and%20ROC%20AUC/ROC%20Curve%20-%20Multiclass.ipynb  
3. About the Public Transportations Bus frequency
    - https://www.transitwiki.org/TransitWiki/index.php/Headway 

