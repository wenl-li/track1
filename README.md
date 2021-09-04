# track1
This is my first attempt to take part in a Kaggle competition:[industrial recommendation systems](https://www.kaggle.com/c/bigdata2021-rl-recsys/overview)
# Introduction
According to the competition page, We are offering an online item recommendation service in a game. This item recommendation task is characterized by its mid-dimensional action spaces (roughly 400 item candidates after reducing) and special interaction rules. As is shown in figure, for each user request, the recommendation engine will respond with 3 item lists (3 items per list), and the next item list is locked until the items of the current list are sold out. The users' response depends on not only the current item but also the items of the next list. This item recommendation problem can be naturally formalized as a multi-step decision-making problem in which the agent recommends an item or an item list in each step.
![Figure 1](https://cdn.mathpix.com/snip/images/B7jiRX-xCB3DJlnhPM_5nvGni6026PchMu0HCKBpIiM.original.fullsize.png)

# Preprocessing
Take a deep looking into the training set data. The longest information colunm is user's click history we couldn't use directly. I try to add time information into click sequence in the feature engineering step. So I add some columns in the new data training set during preprocess step, they are explained below.
</br>item_event: Dividing the users click history by a time window which length is 600 seconds. 
</br>interval: The interval between purchase actions and the last click timestamp. I weight it by the formla: $$ Interval(i) = 1 - 0.05 * i $$ Where, i is a group number of time groups cutting by one day, two days, and so on. I choose those groups according to the click history distribution. 
</br>time_weights: Calculate the interval time between all the click behavior and the last click using the same interval formla.
# Feature Engineering
#### Feature 1: User-item Scores
Calculating the embedding of each item using Word2vec method. I add all the history clicked item by the weight of time as users' embedding. Then calculate the correlation between item embedding and user embedding as user-item scores. *9 columns for 9 items*
#### Feature 2: Item Protrait
Choose useful features in item_vec in the item_info.csv file. *9×4 columns*
#### Feature 3: User Protrait
User Protrait information in trainset.csv.*10 columns*
#### Feature 4: Price
Item price.*9 columns*
#### Feature 5: User Class Distance
Add up the item class in click history as user's interested class, calculate the cosine distance between it and exposed item class.*9 columns*
# Model
LightGBM 
