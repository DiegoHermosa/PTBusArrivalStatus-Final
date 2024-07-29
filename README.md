# Capstone: Bus Arrival Status to Stop 
### Business Understanding: ### 
Can the Public Tranportation Bus Arrival to Stop be predicted as On Time, Early Or Late?

### Data: ### 
The data comes from the Public Trasportation History database that the company I work for has developed for a Public Transportation Company. 


The data can be found in this link

[pt_bus_stop.csv](https://github.com/DiegoHermosa/PTBusArrivalStatus-Final/tree/main/data/pt_bus_stop.csv)

### Notebook Link ###
The following notebook contains all the development of the analysis carried out.

[PTBusStopArrivalStatus.ipynb](https://github.com/DiegoHermosa/PTBusArrivalStatus-Final/tree/main/PTBusStopArrivalStatus.ipynb)

### The techniques and Analysis ###
1. Extract the bus arrival to stop data from the Public Trasportation History database filtering a single Stop and for a date range of 3 months (April-2024 to June-2024).
2. Perform a cleaning of the information in order to eliminate redundant data and outliers.
3. Train KNN, Logistic regression, Decision tree and SVM models in search of the best one.
4. Because of the dataset is imbalanced the SMOTE oversampling techniques will be used.
   
### Dataset Understading ###
#### The dataset contains the next featues:
- ID: record unique ID
- UnitID: Unique ID assigned to the Bus unit.
- StopID: Unique ID assigned to a Bus Stop.
- StopNumber: Friendly name assigned to Stop, this is defined by the Bus Company to name each Route Stop.
- DateIN: Date time when the Bus arrived to the Stop.
- DateOUT: Date time when Bus left the Stop.
- WeekDay: Day name of the arrival date time.
- PrevWeekDay: Day name of the previous arrival date time.
- PeakHour: Flag to mark as Yes/No if traffic is at its worst between 7 a.m. and 9 a.m. and 4 p.m. and 7 p.m.
- StopDuration: time spent by the bus within the Stop.
- PrevDateIn: Date time when the previous bus has arrived to the current stop.
- DifferencePrevBusIn: Time difference between PrevDateIn and DateIN.
- TripDuration: Time spent by the bus to complete a trip.
- Odometer: Bus odometer when it arrived to stop at DateIn.
- PrevStopOdometer: Bus odometer when it arrived to stop at PrevDateIn.
- PrevStopNumber: Previous stop.
- DistanceLastStop: Distance between the last stop visited and the current stop.
- Speed: Bus speed when it arrived to stop.
- OTPMode: Mode of calculation of the Stop status: 2 means Headway
- OnTimeStatus: Status assigned to the bus arrival (ObTime, Early, Late). This status is assigned according to Route Bus Frequency. Each 15 minutes a bus expected to arrive to each stop.
- TripDistance: Distance of the trip.
- TripSpeedAvg: Bus speed average within the trip.

### Data Preparation and Cleaning ###
1. The missing value for some feature were removed. <br/>
![image](https://github.com/DiegoHermosa/PTBusArrivalStatus/assets/160977826/2a21aa20-39b1-4098-90f3-5cea2661c515)
<br/>
3. Features with no relevant information were removed. <br/>
4. Categorical features like PeakHour and WeekDay were transformed to numerical values. <br/>

#### Correlations and Imbalance: ####
The dataset is imbalanced: <br/>
![image](https://github.com/DiegoHermosa/PTBusArrivalStatus/assets/160977826/6fc415a4-eeb5-45a8-9a34-f394a7b47e96)

<br/>
The correlation allowed to identified some featues with a high correlation between which were removed:<br/>

![Screen Shot 2024-07-06 at 11 59 06 PM](https://github.com/DiegoHermosa/PTBusArrivalStatus/assets/160977826/8ff8675d-5d44-422c-8e18-5e420a0eff8e)

<br/>

### Models and tools ### 
1. For the training I used 4 classification models in order to find the most optimal one:<br/>
   - Logistic Regression
   - KNN
   - Decision Tree
   - SVM
2. Cross validation: GridSearchCV with hyperparameters.
3. To balance the dataset I used SMOTE to oversample the minor of the classes.

### Model Evaluation ###
Based on the f1 score, SVC has the best score, followed by KNN which is very close behind. SVC performed well despite it took more time, the KNN was really fast. <br/>

![image](https://github.com/DiegoHermosa/PTBusArrivalStatus/assets/160977826/5f8229b6-6b4c-4754-adaf-4f96e6f19d4f)

<br/>

![image](https://github.com/DiegoHermosa/PTBusArrivalStatus/assets/160977826/26c79dc4-1bcc-405a-8066-de1b68362669)

<br/>

### Feature Importances ###
![image](https://github.com/DiegoHermosa/PTBusArrivalStatus/assets/160977826/2ddabcf4-ca8e-4241-b399-373832a1ede5)

<br/>
 
#### DifferencePrevBusIn #### 
This feature has an important influence on the prediction as it contains the difference between the current arrival time and the previous one. As this value increases the status tends to be late otherwie as it decreases the status will be early.

#### TripSpeedAvg #### 
The speed average of the bus during the trip impacts on the status even though the buses must not exceed the city speed limits, there are times when the speed is reduced due to traffic impacting the bus can meet the frequency of arrivals to the stop.

#### PeakHour #### 
The peak hour has an important influence because the city will have more traffic on those hours, more traffic and more people using the buses causing more stop duration.


### Next Steps and Recomendations ### 
1. Include new features like the weather and the passenger counting.

### References ### 
1. About imbalanced datasets and the oversampling techniques:
   - https://machinelearningmastery.com/multi-class-imbalanced-classification/
   - https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/
   - https://towardsdatascience.com/the-right-way-of-using-smote-with-cross-validation-92a8d09d00c7
