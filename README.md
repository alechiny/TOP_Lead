# Get The Lead Out
## **Project Scope**

Original text by Rob Herndon, edited by Alessandro Chiari 

This project was completed in conjunction with the Opportunity Project (TOP), a program of the Census Open Innovation Labs in the U.S. Census Bureau. This program helps companies, non-profits, and universities turn federal open data into new technologies that solve real-world problems for people across the country. This year, the program’s problem statement is focused on grant funding available through the recently passed Bipartisan Infrastructure Law.

“The Federal Government awards over $1.5 trillion in Federal contracts and financial assistance each year—and sometimes much more in times of crisis. The Bipartisan Infrastructure Law (BIL), enacted in November 2021, represents a once-in-a-generation investment in our nation’s infrastructure and competitiveness with $1.2 trillion in appropriations funding for over 375 programs, many of which are competitive grant programs that jurisdictions must apply for. Unfortunately, many historic barriers exist to accessing financial assistance, especially for lower-resourced communities … This reinforces existing inequities as higher-resourced communities are more successful in accessing these grant opportunities while lower-resourced communities are left behind.”

The specific challenge issued by the TOP program was to build “Tech tools that customize the delivery of relevant program opportunities to lower-resourced community leaders and help them prepare and plan for upcoming application deadlines and referrals for technical assistance could help mitigate many of these barriers.”
After much discussion and research, our team decided to narrow our focus to a specific issue area addressed in the Bipartisan Infrastructure Law - the prevalence of lead pipes in the drinking water systems of the United States.

- Decision to focus on specific case studies to build product
- Description of product
- Description of Hazel Crest case study ~how we found, about Hazel Crest,

about Hazel Crest study & how performed

- Metropolitan Planning Council for the village of Hazel Crest study.
- Hazel Crest is a small city located in Illinois with a population of

roughly 13,000 residents . Hazel Crest has a predominantly

Black/African-American population (~88%).

## **Data Gathering**

For this project we started with two datasets. The first dataset was from the Cook County Census report, and included demographic and infrastructure data for all of Cook County, Illinois, including the city of Hazel Crest. The second dataset was pulled from a website that reported the findings of a study on lead in water service lines in Hazel Crest, and included some demographic and infrastructure data for all homes in Hazel Crest, along with the predicted likelihood of the presence of lead in the pipes of each home based on the study completed by the city’s Metropolitan Planning Council. When we first obtained it, we believed that it was the original dataset used to formulate predictions in this study. However, upon further examination we discerned that this dataset was auto-generated from the interactive built to display the features considered and the lead probabilities predicted in the study. We used these two datasets because Hazel Crest had conducted a similar analysis to what we want to accomplish in this project, and the census data provided important address-specific information which we hoped to use to generate predictions on the presence of lead in specific homes.

Open the links below to view data sources:

[Hazel Crest Data](https://apps.cnt.org/lsl/hazel_crest/)

[Cook County Data](https://datacatalog.cookcountyil.gov/Property-Taxation/Assessor-Archived-05-11-2022-Residential-Property-/bcnq-qi2z)

## **Data Cleaning Process**

The data from the Cook County Census was relatively clean with only a few missing values. Very little work was needed to clean this data.

The data that was downloaded from the Hazel Crest website was more challenging to process and prepare. There are 5,724 rows in the Hazel Crest dataset. Of those, there were approximately 2,000 rows where the data was corrupted to the point where it was deemed unusable. These included rows where the data was in the wrong columns, the data in the row had been shifted to the right or left so that it didn’t line up with the correct column names, or there was data missing. This problem was difficult to correct because the column names were not clear and there was no data dictionary to help inform what information should be in which column. Due to resource constraints we were unable to resolve the issues in the corrupted data.

We determined that for the purposes of this project, we would remove this data. We made this determination for two main reasons:

1) It seemed unlikely that we would be able to get answers as to why the data was shifted.

2) Short of these answers, we felt that it was highly likely that we would corrupt the data further by attempting to move the data to the columns where we thought it lined up correctly or to impute data where there was no clear understanding on the value of the column.

Once the corrupted data was removed from the Hazel Crest Data set we were able to merge the two data sets.

## **EDA**

Our next step was to complete some exploratory analysis of the data we collected, cleaned, and merged. Using Tableau, we created visualizations to explore demographic factors such as median household income, property value, and property age to create a profile on the likelihood of the presence of lead pipes. ([Tableau](https://public.tableau.com/views/HazelCrestLeadAnalysis_16614458537570/DASHBOARD?:language=en-US&:display_count=n&:origin=viz_share_link))

We then created a heatmap to explore the relationship all of the independent variables have with the likelihood of there being lead in a given service line in Hazel Crest.

The variables with the strongest negative correlations were the estimated year of construction, median household income, size of the home (square feet), and the market value of the home.  That means as each of these variables decreases, the probability of lead increases. For example, as the year of construction gets smaller (meaning older) there is a higher likelihood of lead. Or, as median household income decreases the likelihood of lead increases.

The variables with the highest positive correlations were the percent single parent homes along with the latitude and longitude coordinates. This suggests that as the percentage of single-parent homes increases, there is a corresponding increase in the likelihood of a lead service line. The interesting part about longitude and latitude increasing is that we did find that homes in the Northeastern part of Hazel Crest were more likely to have lead, which follows an increase in longitude and latitude.

Once we were able to determine the variables with the highest correlations, we were able to move into the modeling phase to see if we could replicate the findings from the Metropolitan Planning Council for the village of Hazel Crest study.****

## **Modeling**

For the first models that we built, we wanted to use the same variables that had been used in the model that Blue Conduit built for Hazel Crest, which were listed in the report on the Hazel Crest study. These 5 independent variables were estimated year of construction, the value of the house, the ratio of building value to land value, the type of house (i.e., single story, split level, two-story, etc.), and the year that the fire hydrant was stamped (installed). The reason that fire hydrant installation variable was chosen in Hazel Crest is that it could be an indicator of when service lines were installed.

The original study used a random forest classifier, but we wanted to test 3 different models to see which produced the best results. We did this to improve the model that we would use for final predictions. We began with a Logistic Regression model, then tried both a Random Forest and Gradient Boosted Random Forest. We separated the data into a training set for the model and a testing set to validate the accuracy of the models. Each model was trained on training data from the dataset and then scored on the testing data. The Logistic Regressor scored the worst with an accuracy score of 65%, while both the Random Forest and Gradient Booster scored around 90% on the testing data.

We then wanted to build a second group of models that would include more variables to see if we could increase the accuracy scores. First, we wanted to add all of the variables that had high correlations to the dependent variable, the likelihood of lead. We also referenced an article written about the Hazel Crest study that highlighted some other variables that the author believed should be included. (include link to article)

The main variables that the article suggests to include to improve accuracy are the longitude and latitude coordinates of the houses. However, after consultation with our data consultant Jacob, we agreed that including longitude and latitude in the logistic regression model could prejudice the model because the computer would not read the numbers as coordinates, but as simply numbers, leading to the model drawing potentially inaccurate correlations in the data.  However, we still wanted to use location data in this model, so to get around this issue we decided to cluster the longitude and latitude points and assign numbers to the different clusters. We used a KMeans clustering algorithm to detect patterns in the longitude and latitude data.
We chose 4 clusters because that most accurately shows the distinction between the different areas of Hazel Crest. We also then felt comfortable using the clusters instead of longitude and latitude.

The variables that we added to the original list of variables were the percentage of single-family homes in the specific neighborhood, the median household income for the neighborhood, the square footage of the house, and the numbered clusters from the KMeans cluster analysis.

We used the same hyper parameters as the previous models to keep a constant for the testing. The new variables increased the accuracy of each model: Logistic Regression was up to 76%, Random Forest was up to 93%, and Gradient Booster was up to 94%.

The high accuracy scores achieved of our models indicate that we were able to very closely reproduce the predictions generated by the study done in Hazel Crest. While the practical usefulness of this is somewhat limited by the accuracy of the original study (which is not entirely known due to the lack of visual confirmation of the presence of lead pipes in a large number of areas), this does indicate that the same level of predictive power achieved by the Hazel test study can be reproduced with a relatively small amount of difficulty and resources.

## **Flint**

An admitted problem that we have with using the Hazel Crest dataset was that it contained predictions on where lead service lines were most likely to be in the city. That meant that, in essence, the best that we could do as a data team was see if we could accurately get the same probabilities of lead generated by the previous Hazel Crest study. We therefore wanted to test on a dataset that included a definitive categorization of yes, this house is known to have a lead service line or no, it does not have a lead service line. We were able to find that dataset by looking at publicly-available data from the voluntary residential water testing program in Flint, Michigan. ([Flint Data](https://www.kaggle.com/competitions/mdst-flint/overview))

The group that put together this dataset for Flint was able to go house by house in Flint and determine if each house had a lead service line or not. For data science purposes, this means that we can use a model to test whether independent variables can be used to predict an known outcome. The dataset also contained many of the same variables that were used in the prediction model for Hazel Crest. That made this dataset an ideal choice for the next step in this project.

In order to preserve as .. we followed the same process we used in building the Hazel Crest models. This means that we first wanted to change the latitude and longitude coordinates into clusters using the K-Means clustering algorithm. In order to determine how many different clusters to use, we created an elbow graph, showing that 3 clusters is the most optimal.
We then created a graph to show what the Flint latitude/longitude coordinates  would look like with the clustering.
With the 3 clusters seemingly offering a good division of the coordinates, we decided to use the clusters instead of the latitude and longitude in the same way that we did for the Hazel Crest dataset.

The next step we took was to build the model. We also used a grid search algorithm to determine the most optimal hyper parameters for our new model. To keep continuity from the previous tests we decided to test both a random forest and gradient booster algorithm. These were the results of the two tests:

Since the results were almost identical, we decided to go with a gradient booster since the amount of features we had for the Flint model are more similar to the first model we built for the Hazel Crest data set.
Based on the confusion matrix we can see that there are more than double the amount of false negatives (bottom left corner: 156) than false positives (top right corner: 64). This means that our model was more attuned for specificity, with a score 0f .99 than for sensitivity, with a score of .92. In the case of this project, a false positive means that the model predicted the existence of a lead service line when it doesn’t exist in real life, while a false negative means that the model predicts the absence of a lead service line when there is one in real life. We would like to recalibrate the model to be more sensitive, meaning that there would be fewer or no false negatives but potentially a higher amount of false positives. We believe that a false positive would do less harm in the real-world application of the model than a false negative because the impact of a false positive would be that someone would be more inclined to check for a lead pipe and discover that it’s not, whereas a false negative would give a false sense of security that there isn’t a lead pipe when in actuality there is one.

## **Next Steps**

Find a new dataset that we can test the model on to see if the model performs as well on new data as it did on Flint.

Change the thresholds on the model in order to reduce false negatives (when the model predicts that the service line is not lead but it actually is) in the system. This might lead to more false positives (when the model predicts a service line has lead, but it actually does not).