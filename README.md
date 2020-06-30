# Project-Group-6
project-group-6 created by GitHub Classroom

#### Group Member Names 
Chuxin Piao, Xiao Wang, Wanyu Zhong 

### Executive Summary
#### Introduction:
This dataset provides two zips: aggregate and deaths. In deaths, the files record every death that occurred within the 720k matches. That is, each row documents an event where a player has died in the match. In aggregate, each match's meta information and player statistics are summarized (as provided by pubg). It includes various aggregate statistics such as player kills, damage, distance walked, etc as well as metadata on the match itself such as queue size, fpp/tpp, date, etc To summarize and explore the dataset, we are interested in the death reason in the PUBG game, whether it is related with location or other metrics.
#### What dataset you used and your initial plan:
Initial Plan: \
Use this datset to develop some useful suggestions for PUBG players to help them improve the placement in the game. 
Stack data together into one csv file, find player placements and positions that related most to death in game. \

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
After discussing and exploring our dataset, we set player placement, which is the players' rank in games as the important dependent variable for our project. 
Aggregate dataset include 15 columns, 11 of the columns are numbers. From these number columns, we picked game_size, party_size, player_kills and player survive_time as the important independent variabls.\
Death dataset include 12 columns, 7 of them are numbers. From these number columns, we picked killer_placement, victim_placement, killer_position_x and y, victim position_x and y and time as our emphasized variables. From other text column, we focus on killed_by, which indicates the killed wheapons for specific players and map.\
Among these chosen variables, we have created different topics, such as the dangerous position of parachuting to explore and visualize our dataset. We will show all the questions in detailed in the next section.\
Besides, after carefully read the dataset, we found there are only two map used in map columns and the map picture are also provided in the Kaggle attachments. Thus, we have a initial plan of combing the position axis in death dataset and the map together to achieve further visualization.

### Methods Section
#### How you sourced in ingested the data
We have read carefully for the PUBG game rules, and in the spark.sql part we decided to separate the data according to different party size. Because if party size equals to 4, then the maximum team_placement will be 25 among an 100-people game. For party size equals to 2, the maximum team_placement would be 50 and for party size equals to 1, the maximum team_placement would be 100. It is not reasonable if we mix all the party_size together and calculate placement ranking during our working process. In the later Machine Learning part, we have add a new column which calculated the placement ranking percent to solve this problem.
#### How you cleaned, prepared the dataset
Find NAs and drop them.
#### How did you model the dataset, what techniques did you use and why?
We first chose to use linear regression. Set Team_placement as our dependent variable and other number variables in the aggregate dataset as the independent variable.\
After that, we would also hope to try some classification method on our dataset. Thus, we set the placement ranking in top 10% as 1 and others as 0 and fit logistic regression model.\
Further using the variable created in the previous steps, we also tested random forest model.
#### Did you just visualize the dataset, and if so, why? 
In Kaggle, it already provided basic visualization for each column. Thus, in spark, we first take(10) to make sure our we have the right data structure and use spark.sql to work on some basic questions:\
a. What location is dangerous for "parachuting"?\
b. Figure out the relationship between player placement and enemies killed(separate by party size).\
c. Figure out relationship of kill_distance and kill_by.

### Results/Conclusions Section
#### What did you find and learn? 

#### How did you validate your results? 
Split the training set into 80% training and 20% testing, and validate our prediction results on the 20% testing data. 
#### Challenges you've had (technical& non-technical) and how you overcame them:
Challenges:\
1.We first mix all the party_size together and get unreasonable results. Our average rank is larger than 25, this is impossible for party size 4.\
2.Try to do grid search on classification models, but it takes more than 4 hours to train only one individual model, so we removed it (Failed to perform parameter tuning. We need more computing power.);\
3.Failed to use xxx.toPandas() method. Everytime we use this command, the session will be died. Alternatively, we use pd.DataFrame(xxx.collect()), but it costs more time.
#### Future work: what would you do differently and what follow-up work would you do? 
We hope to explore more complicated topics in the future on this dataset. Such as finding the last circle before ending the game and so on.
#### Division of labor: which team member was responsible for which part of the project. 
Chuxin Piao: Working on spark.sql and maching learning coding\
Wanyu Zhong: Working on spark.sql and maching learning coding\
Xiao Wang: Working on spark.sql and maching learning coding
#### Takeaways from the course. 
From this course, we have learned how to use spark and rdd techniques. Also, we have learned the way to deal with large dataset without using our own computer execution.


