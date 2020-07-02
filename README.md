# PUBG Match and Deaths Project - Group 6
![Image of PUBG](https://upload.wikimedia.org/wikipedia/en/3/3d/PlayerUnknown%27s_Battlegrounds_Steam_Logo.jpg)\
project-group-6 created by GitHub Classroom

### Group Member 
Chuxin Piao, Xiao Wang, Wanyu Zhong 

### Executive Summary
PUBG is a popular online multiplayer battle royale game. In this project, we manipulated two CSV files (around 20GB) with 1200 million rows of data to analyze game statistics. We look into the relationship between locations, weapons, and placements. We also build several models to predict a player's placements based on different features. We hope this project can help game developers as well as game players to learn some new insights on this product. 

### Introduction
This dataset provides two zips: aggregate and deaths. In deaths, the files record every death that occurred within the 720k matches. That is, each row documents an event where a player has died in the match. In aggregate, each match's meta information and player statistics are summarized (as provided by PUBG). It includes various aggregate statistics such as player kills, damage, distance walked, etc as well as metadata on the match itself such as queue size, fpp/tpp, date, etc. To summarize and explore the dataset, we are interested in the death reason in the PUBG game, whether it is related with location or other metrics.

#### What dataset you used and your initial plan?
- Initial Plan:
Use this dataset to develop some useful suggestions for PUBG players to help them improve the placement in the game. As PUBG players, we also would like see if our analysis matches our game experience.  
After stacking all data together into two CSV files, we will have sufficient data to find player placements and positions that related most to death in games.

- Dataset:
PUBG Match Death and Statistics from Kaggle.com (https://www.kaggle.com/skihikingkevin/pubg-match-deaths) \
We upload data from S3:
The datasets are stored in ASW S3 bucket as follow: \
s3://bigdata2020group6/aggregate/agg_match_stats_0.csv \
s3://bigdata2020group6/aggregate/agg_match_stats_1.csv \
s3://bigdata2020group6/aggregate/agg_match_stats_2.csv \
s3://bigdata2020group6/aggregate/agg_match_stats_3.csv \
s3://bigdata2020group6/aggregate/agg_match_stats_4.csv \
s3://bigdata2020group6/deaths/kill_match_stats_final_0.csv \
s3://bigdata2020group6/deaths/kill_match_stats_final_1.csv \
s3://bigdata2020group6/deaths/kill_match_stats_final_2.csv \
s3://bigdata2020group6/deaths/kill_match_stats_final_3.csv \
s3://bigdata2020group6/deaths/kill_match_stats_final_4.csv

Note: we also uploaded two images of the game maps, erangel.jpg, and miramar.jpg, to achieve the best visualization results. 


### Exploratory Analysis Section
#### Any insights you've learned from the data (with charts to illustrate):
- We find player placement (int), which is the players' rank in games can be used as an important dependent variable for our project.
The Aggregate dataset includes 15 columns, 11 of the columns are numbers. From these number columns, we picked game_size, party_size, player_kills, and player survive_time as the important independent variable.
- The Death dataset includes 12 columns, 7 of them are numbers. From these number columns, we picked killer_placement, victim_placement, killer_position_x and y, victim position_x, and y and time as our emphasized variables. From another text column, we focus on killed_by, which indicates the killed weapons for specific players and maps.
- We found there are only two maps used in map columns and the map picture are also provided in the Kaggle attachments. Thus, we have an initial plan of combing the position axis in the Death dataset and the map together to achieve further visualization.

Please see the full EDA process in our notebook. 

### Methods Section
#### How you sourced in ingested the data?
- Upload all data files in the S3 bucket. 
- Read in 10 CSV files (around 2GB each) by creating a spark session and save as aggregate file (around 10GB) and death file (around 10GB). 

#### How you cleaned, prepared the dataset?
Our dataset is relatively well structured and cleaned. Here are our steps for cleaning and preparing: 
 - Check on the variable type of each column. 
 - Find null values in each column. 
 - Drop null values that did not contribute much to our analysis. 
 - Replace values for certain NAs. 
 - We have read carefully for the PUBG game rules, and in the spark.sql part, we decided to separate the data according to different party sizes. Because if party size equals 4, then the maximum team_placement will be 25 among a 100-people game. For party size equals 2, the maximum team_placement would be 50 and for party size equals 1, the maximum team_placement would be 100. It is not reasonable if we mix all the party_size together and calculate placement ranking during our working process. In the later Machine Learning part, we have added a new column which calculated the placement ranking percent to solve this problem.
 
#### How did you model the dataset, what techniques did you use and why?
- We first chose to use linear regression. Set Team_placement as our dependent variable and other number variables in the aggregate dataset as the independent variable.
- After that, we would also hope to try some classification methods on our dataset. Thus, we set the placement ranking in the top 10% as 1 and others as 0 and fit a logistic regression model.
- Further using the variable created in the previous steps, we also tested the random forest model.

#### Did you just visualize the dataset, and if so, why? 
Pyspark limited our choice for visualization since we can only plot histograms on Pyspark. We need to transform the dataset into a pandas dataframe for other visualization options. 
First, we plot the distribution of survival time and find out the first 120 seconds are the most intense and dangerous period since the death rate is high in the first 120 seconds. Then we apply this finding to discover which location is dangerous for parachuting.  
- What location is dangerous for "parachuting"? \
   In the Erangel map, Military Base and Pochinki are the most dangerous places in the first two minutes of the game.  \
   In Miramar map, Water Treatment and San Martin are the most dangerous places in the first two minutes of the game. \
   If players choose to parachute to these locations, they are very likely to meet enemies and battle with them.
   ![Image of erangel_death](https://gwu-bigdata-2020-project-group-6.s3.amazonaws.com/erangel_death1.png)
   ![Image of miramar_death](https://gwu-bigdata-2020-project-group-6.s3.amazonaws.com/miramar_death1.png)

We did not use visualization for following questions since the plot took a long time to process, but we still got meaningful findings: 
- Figure out the relationship between player placement and enemies killed(separate by party size). \
   For a party of 1, on average the player needs to kill 6.9 enemies to win the first place. \
   For a party of 2, on average the player needs to kill 4.4 enemies to win the first place. \
   For a party of 4, on average the player needs to kill 2.9 enemies to win the first place. \
   Hence, playing in larger parties could be actually less stressful for players. 
   
- Figure out the relationship between kill_distance and kill_by.\
   Redzone, punch, Motorbike are three shortest kill-distance weapons to be used in the game. \
   M24, AWM, Mk14 are the three longest kill-distance weapons to be used in the game. 
   
- Most popular weapons used in the game. \
   For the winner (1st place player) in the game, top3 most popular weapons are M416, SCAR-L, and M16A4. \
   For 2nd - 9th place players, top3 most popular weapons are also M416, SCAR-L, and M16A4. 

### Results/Conclusions Section
#### What did you find and learn? 
- For regression, we have tested linear regression with an elastic net and regularization parameter. R square equals to 0.360794, which indicates linear regression does not work well. Thus, the dependent variable team_placement does not follow linear patterns.

- For logistic regression, AUC equals to 0.578535. This indicates that logistic regression does not perform well.

- For random forest, AUC equals to 0.826167 and precision equals to 0.792054. This indicates the RF classification model performs well. By comparing the variable importance, we found that survive_time of players related closely to team_placement. Thus, the 'camping' strategy could significantly improve the placement.

- For the Gradient Boosted Tree classifier, AUC equals to 0.82522 and precision equals to 0.81939. This indicates the GBT model performs well, too. For feature importance, we got similar results as the random forest model. In the GBT model, the survive_time is even more important compared to other features.

After attempting several models, the prediction results prove that features selected by us are significant factors for player's placement. (Features selected : 'player_assists', 'player_dbno', 'player_dist_ride', 'player_dist_walk', 'player_dmg',  'player_kills' and 'player_survive_time')

#### How did you validate your results? 
Split the training set into 80% training and 20% testing, and validate our prediction results on the 20% testing data. 
We checked many metrics such as AUC, F1, accuracy scores on the testing dataset.

### Challenges you've had (technical& non-technical) and how you overcame them:

- Plotting in Pyspark with a huge amount of data is very challenging for us. We try to convert spark dataframe to pandas dataframe then plot. 
- Try to do grid search on classification models, but it takes more than 4 hours to train only one individual model, so we removed it (Failed to perform parameter tuning. We need more computing power.); 
- Failed to use xxx.toPandas() method. Every time we use this command, the session will die. Alternatively, we use pd.DataFrame(xxx.collect()), but it costs more time.

### Future work: what would you do differently and what follow-up work would you do? 
We hope to explore more complicated topics in the future on this dataset. Such as finding the last circle before ending the game and so on.
If we can have more computational power without the limitation on the student account, we would love to see the result of the grid search with cross-validation. 

### Division of labor: which team member was responsible for which part of the project? 
The work is evenly divided among group members.\
Chuxin Piao: Working on spark.sql and matching learning coding.\
Wanyu Zhong: Working on spark.sql and matching learning coding.\
Xiao Wang: Working on spark.sql and matching learning coding.

### Takeaways from the course? 
From this course, we have learned how to use spark and rdd techniques to efficiently manipulate large datasets. Also, we have practiced different ways to deal with large datasets without using our own computer execution.
One of the most meaningful takeaways in this course is the importance of considering computation efficiency. How to obtain the expected results using less computation power and time? When dealing with small datasets, we rarely need to take care of this problem. 

