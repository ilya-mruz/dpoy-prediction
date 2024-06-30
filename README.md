# dpoy-prediction
Running Instructions: 
Have the 3 csv files in the same level directory as the .ipynb file
Simply run all then view the file

Libraries Needed:
  pandas
  numpy
  matplotlib
  sklearn
  xgboost
  bayes_opt

**Project Overview**
Data Collection/Generation:

The data was collected from the specified link and is also included in the code submission. The dataset provided all the necessary statistics, which included: "season," "player," "age," "experience," "tm," "g," "mp," "drb_percent," "stl_percent," "blk_percent," "dws," "dbpm," "share," "w," "l," and "d_rtg." The data came from three .csv files: "Player Award Shares.csv," "Advanced.csv," and "Team Summaries.csv." Each .csv file was converted into separate data frames.

Data Transformation/Pre-processing:

The initial step involved filtering out all award votes for non-DPOY awards and deleting all data before the DPOY award was introduced in 1983. Non-essential information related to defense was also removed. The 2023 season was excluded because the voter share of the award had not been announced at the time of this project.

An issue arose regarding how to handle players traded mid-season. As the plan was to use a team’s defensive rating (labeled as “d_rtg”) as a feature, it was challenging to assign a single defensive rating value for players traded mid-season. The solution was to delete all players traded during the season. Notably, Dikembe Mutombo, who won the DPOY in 2001 while being traded mid-season, led to the deletion of the entire 2001 season.

Entries with NaN values in any row were dropped, resulting in the removal of only three entries, none of which had any vote share. All players with a NaN voter share were assigned a voter share of 0.0.

A minimum threshold of 1500 minutes played in a season was set, which eliminated class bias as many players who played a low number of minutes received little to no votes. Finally, the three dataframes were merged into one.

Feature Extraction/Engineering:

Two features were created from the datasets. The first feature, 'W/L_percent,' represented the player’s team record for the season, calculated by dividing the number of wins by the total number of games played. The second feature, “d_rtg_diff,” was the difference between a team’s defensive rating and the league average defensive rating for that season. The team’s raw defensive rating was not used because the pace of the game and average defensive rating per season varies significantly.

Data Split:

The data, unique in its separation into different seasons, was split based on seasons. Out of approximately 40 seasons, a random selection of 4 was used as the testing set. The selected seasons for testing were 2020, 1990, 2009, and 2006, covering different eras of the NBA.

Model Tuning/HPO:

For model tuning, the default parameters of four models (SVM, Random Forest, XGBoost, and MLP Regressor) were initially run. After evaluating the models using R^2, MAE, and MSE, XGBoost was identified as having the best accuracy. Bayesian Optimization was then employed to tune the parameters, resulting in the following optimal values: learning_rate = 0.1929, max_depth = 7, and subsample = 0.982.

Performance Evaluation:

Most of the models performed poorly, with R-Squared values below 0.4: Random Forest had a value of 0.3601, MLP Regressor had a value of 0.3886, and SVM had the lowest value of 0.1018. The only model deemed valid was XGBoost, which had the best R-Squared value of 0.4186. The residuals plot had a random distribution. This means while the model had the best fit out of the rest, it was still not very accurate in prediction while being very discriminating towards the different parameters. Since a player winning DPOY is not a strictly statistical award and has much human bias, it is likely that many more immeasurable parameters affect this award. Outside factors such as player popularity, social media narrative, or voter fatigue could all impact the result on a year to year basis. A possible solution could be to implement sentiment analysis of social media regarding players in conjunction with this statistical outlook. 
