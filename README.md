# Project-Group-6
project-group-6 created by GitHub Classroom

#### Group Member Names 
Chuxin Piao, Xiao Wang, Wanyu Zhong 

#### Executive Summary
PUBG is a popular online multiplayer battle royale game. In this projects, we manipulated two csv files (around 20GB) with 1200 million rows of data to analyze game statistics. We look into the relationship between locations, weapons and placements. We also build several models to predict player's placements based on different features. Hope this project can help game developers as well as game players to learn some new insights on this product. 

#### Introduction:
This dataset provides two zips: aggregate and deaths. In deaths, the files record every death that occurred within the 720k matches. That is, each row documents an event where a player has died in the match. In aggregate, each match's meta information and player statistics are summarized (as provided by pubg). It includes various aggregate statistics such as player kills, damage, distance walked, etc as well as metadata on the match itself such as queue size, fpp/tpp, date, etc To summarize and explore the dataset, we are interested in the death reason in the PUBG game, whether it is related with location or other metrics.

#### What dataset you used and your initial plan:
Initial Plan:
Use this datset to develop some useful suggestions for PUBG players to help them improve the placement in the game. 
After stacking all data together into two csv files, we will have sufficient data to find player placements and positions that related most to death in game.

Dataset:
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


### Exploratory Analysis Section
#### Any insights you've learned from the data (with charts to illustrate)
- We set player placement, which is the players' rank in games as the important dependent variable for our project. 
- Aggregate dataset include 15 columns, 11 of the columns are numbers. From these number columns, we picked game_size, party_size, player_kills and player survive_time as the important independent variabls.
- Death dataset include 12 columns, 7 of them are numbers. From these number columns, we picked killer_placement, victim_placement, killer_position_x and y, victim position_x and y and time as our emphasized variables. From other text column, we focus on killed_by, which indicates the killed wheapons for specific players and map.
- We found there are only two map used in map columns and the map picture are also provided in the Kaggle attachments. Thus, we have a initial plan of combing the position axis in death dataset and the map together to achieve further visualization.

### Methods Section
#### How you sourced in ingested the data
- Upload all data files in S3 bucket. 
- Read in 10 csv files (around 2GB each) by creating spark session and save as aggregate file (around 10GB) and death file (around 10GB). 

#### How you cleaned, prepared the dataset
Our dataset is relatively well structured and cleaned. Here are our steps of cleaning and preparing: 
 - Check on variable type of each column. 
 - Find null values in each column. 
 - Drop null values that did not contribute much to our analysis. 
 - Replace values for certain NAs. 
 - We have read carefully for the PUBG game rules, and in the spark.sql part we decided to separate the data according to different party size. Because if party size equals to 4, then the maximum team_placement will be 25 among an 100-people game. For party size equals to 2, the maximum team_placement would be 50 and for party size equals to 1, the maximum team_placement would be 100. It is not reasonable if we mix all the party_size together and calculate placement ranking during our working process. In the later Machine Learning part, we have add a new column which calculated the placement ranking percent to solve this problem.
 
#### How did you model the dataset, what techniques did you use and why?
- We first chose to use linear regression. Set Team_placement as our dependent variable and other number variables in the aggregate dataset as the independent variable.
- After that, we would also hope to try some classification method on our dataset. Thus, we set the placement ranking in top 10% as 1 and others as 0 and fit logistic regression model.
- Further using the variable created in the previous steps, we also tested random forest model.

#### Did you just visualize the dataset, and if so, why? 
Pyspark limited our choice for visualization, since we can only plot histogram on Pyspark. We need to transform the dataset into pandas dataframe for other visualization options. 
First, we plot the distribution of survival time and find out the first 120 seconds are most intense and dangerous time period since many palyers "died" in first 120 seconds. Then we apply this find to find which location is dangerous for parachuting.  
a. What location is dangerous for "parachuting"?\
   In Erangel map, Military Base and Pochinki are the most dangerous places in the first two minuts of the game.  \
   In Miramar map, Water Treatment and San Martin are the most dangerous places in the first two minuts of the game. \
   If players choose to parachute to these locations, they are very likely to meet enemies and battle with them. 

We did not use visualization for following questions since the plot took long time to process, but we still got meaningful findings: \
b. Figure out the relationship between player placement and enemies killed(separate by party size).\
   For party of 1, on average the player needs to kill 6.9 enemies to win the first place. \
   For party of 2, on average the player needs to kill 4.4 enemies to win the first place. \
   For party of 4, on average the player needs to kill 2.9 enemies to win the first place. \
   Hence, playing in larger parties coube be actually less stressful for players. 
   
c. Figure out relationship of kill_distance and kill_by.\
   Redzone, punch,  Motorbike are three shortest kill-distance weapons to be used in the game. \
   M24, AWM, Mk14 are three longest kill-distance weapons to be used in the game. 
   
d. Most popular weapons used in the game. \
   For winner (1st place player) in the game, top3 most popular weapons are M416, SCAR-L, and M16A4. \
   For 2nd - 9th place players, top3 most popluar weapons are also M416, SCAR-L, and M16A4. 

### Results/Conclusions Section
#### What did you find and learn? 
- For regression, we have tested linear regression with elastic net and regularization parameter. R square equals to 0.360794, which indicates linear regression does not work well. Thus, team_placement dependent variable does not follow linear patterns.

- For logistic regression, AUC equals to 0.578535. This indicates that logistic regression does not perform well.

- For random forest, AUC equals to 0.826167 and precision equals to 0.792054. This indicates RF classificatoin model performs well. By comparing the variable importance, we found that survive_time of players related closely to team_placement. Thus, the 'camping' strategy could significantly improved the placement.

- For Gradient Boosted Tree classifier, AUC equals to 0.82522 and precision equals to 0.81939. This indiactes GBT model performs well, too. For feature importance, we got similar results as random forest model. In GBT model, the survive_time is even more important compared to other features.

#### How did you validate your results? 
Split the training set into 80% training and 20% testing, and validate our prediction results on the 20% testing data. 
We checked many metrics such as AUC, F1, accuracy scores on testing dataset.

#### Challenges you've had (technical& non-technical) and how you overcame them:
Challenges:\
1.We first mix all the party_size together and get unreasonable results. Our average rank is larger than 25, this is impossible for party size 4.\
2.Try to do grid search on classification models, but it takes more than 4 hours to train only one individual model, so we removed it (Failed to perform parameter tuning. We need more computing power.);\
3.Failed to use xxx.toPandas() method. Everytime we use this command, the session will be died. Alternatively, we use pd.DataFrame(xxx.collect()), but it costs more time.
#### Future work: what would you do differently and what follow-up work would you do? 
We hope to explore more complicated topics in the future on this dataset. Such as finding the last circle before ending the game and so on.
If we can have more computational power without the limitation on student account, we would love to see the result of grid search with cross-validation. 

#### Division of labor: which team member was responsible for which part of the project. 
The work is evenly divded among group members.\
Chuxin Piao: Working on spark.sql and maching learning coding\
Wanyu Zhong: Working on spark.sql and maching learning coding\
Xiao Wang: Working on spark.sql and maching learning coding

#### Takeaways from the course. 
From this course, we have learned how to use spark and rdd techniques to efficiently manipulate lare datasets. Also, we have practiced different ways to deal with large dataset without using our own computer execution.
One of the most meaningful takeways in this course is the importance of considering computation efficiency. How to obtain the expected results using less computation power and time? When dealing with small datasets, we rarely need to take care of this problem. 

