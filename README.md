# project-group-6
project-group-6 created by GitHub Classroom

#### Group member names 
Chuxin Piao, Xiao Wang, Wanyu Zhong 

### Executive Summary
#### Introduction:
This dataset provides two zips: aggregate and deaths. In deaths, the files record every death that occurred within the 720k matches. That is, each row documents an event where a player has died in the match. In aggregate, each match's meta information and player statistics are summarized (as provided by pubg). It includes various aggregate statistics such as player kills, damage, distance walked, etc as well as metadata on the match itself such as queue size, fpp/tpp, date, etc To summarize and explore the dataset, we are interested in the death reason in the PUBG game, whether it is related with location or other metrics.
#### What dataset you used and your initial plan:
Initial Plan:
Stack data together into one csv file, find player placements and positions that related most to death in game.
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
s3://bigdata2020group6/deaths/kill_match_stats_final_4.csv \

### Exploratory analysis section
#### Any insights you've learned from the data (with charts to illustrate)
After discussing and exploring our dataset, we set player placement, which is the players' rank in games as the important dependent variable for our project. 
Aggregate dataset include 15 columns, 11 of the columns are numbers. From these number columns, we picked game_size, party_size, player_kills and player survive_time as the important independent variabls.
Death dataset include 12 columns, 7 of them are numbers. From these number columns, we picked killer_placement, victim_placement, killer_position_x and y, victim position_x and y and time as our emphasized variables. From other text column, we focus on killed_by, which indicates the killed wheapons for specific players and map.
Among these chosen variables, we have created different topics, such as the dangerous position of parachuting to explore and visualize our dataset. We will show all the questions in detailed in the next section.
Besides, after carefully read the dataset, we found there are only two map used in map columns and the map picture are also provided in the Kaggle attachments. Thus, we have a initial plan of combing the position axis in death dataset and the map together to achieve further visualization.

### Methods section
#### How you sourced in ingested the data
#### How you cleaned, prepared the dataset
Find NAs and drop them.
#### How did you model the dataset, what techniques did you use and why 
#### Did you just visualize the dataset, and if so, why? 

### Results/Conclusions section
#### What did you find and learn?  
#### How did you validate your results? 
#### Challenges you've had (technical & non-technical) and how you overcame them 
#### Future work: what would you do differently and what follow-up work would you do?  
#### Division of labor: which team member was responsible for which part of the project. 
#### Takeaways from the course. 


