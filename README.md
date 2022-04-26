# Hotel-Booking-Cancellation
Data Analysis And Prediction Supervised Learning to Reduce Number of Booking Cancellation

**Problem Statement**
In the context of the hospitality business, if visitors make room reservations, the hotel will prepare several things to welcome their arrival, including:
>* Contacting visitors regarding when they are expected to arrive at the hotel,
>* Cleaning, tidying and preparing rooms according to visitor orders,
>* Prepare food and drinks to welcome visitors,
>* Refuse other visitors who have booked a room that has been booked (*booked room*), and
>* Provide pick-up service at the airport/station/terminal if needed.

If guests do not stay at the hotel even though they have made a booking, the hotel rooms that have been prepared fail to be rented out and can be detrimental, the operational costs incurred to welcome guests will also be in vain.

**Goals**

So based on this problem, the hotel wants to have the ability to predict the possibility that a guest will cancel a booking or not, so that it can focus on hotel rooms to be rented out again if a guest cancels.

Also, the hotel wants to know what factors cause a guest to cancel or not, so they can make a better plan to reduce the cancellation rate for guests.

**Data Understanding**

This dataset comes from a Scientific Journal paper entitled "Hotel booking demand datasets" written by Nuno Antonio, Ana Almeida, and Luis Nunes for Data in Brief, Volume 22, February 2019. You can access an explanation of each feature/variable from the Journal at https: //www.sciencedirect.com/science/article/pii/S2352340918315191

If you want to know the information in each column, you can access: https://www.kaggle.com/jessemostipak/hotel-booking-demand/data.

# Data Preparation

there are many missing values ​​in the agent and company columns, the missing values ​​here do not appear to be correlated with each other. but for this analysis we do not use these columns because apart from the missing values, there are many and are not useful in this analysis. There are some columns that are not used because they do not function for analysis and modeling. For the column 'stays_in_weekend_nights','stays_in_week_nights','arrival_date_day_of_month' does not match the definition intended, such as arrival date day of month whose maximum value reaches 31 even though in a year it is only 12 months. if indeed the column is needed, it can be made from the available reservation status date column. After that the data is ready to be used for the next process, namely EDA, Business Insight and Modeling. After cleaning the data, which initially consisted of 119,390 rows and 32 columns, it became 87,392 rows and 29 columns

# Modeling

The selected model is a random forest. Based on the tree model, random forest has the advantage of being a more stable model because it performs or makes a decision tree according to the n estimators that we have determined, from repeated decision tree experiments with different features and then the score is taken from the assessment of the entire tree made. Efforts have been made to make the model better, tuning is done by entering the maxdepth params grid to determine how deep our tree is, min samples leaf to determine how many minimum samples are in the leaf of the decision tree, and min sample split to determine the minimum sample before a leaf. split again, but the results do not change significantly because it is enough to use the benchmark model that we have determined.

# Evaluation

The model predicts the user will *cancel booking* (cancelling the order), when in fact/actually the user does not cancel the order. **FP**

the downsides when we ignore **FP** :
* can be detrimental to the hotel because it can be seen from the data analysis above when we predict the user will cancel the booking, even though in reality the user does not cancel, it will make the customer feel disadvantaged and the hotel will lose the customer
* From the barplot of the market segment above, 47.43% of customers book hotel rooms through online travel agents, and what we already know when we book a room at an online travel agent there will be a rating given by the customer to the hotel. When the customer we predict feels disadvantaged gives a bad rating, it will have an impact on the hotel rating in an online travel agent and can reduce the interest of other customers to book a hotel.

The model predicts the user does not cancel the order, when in fact/actually the user *cancel booking* (cancels the order). **FN**

disadvantages of ignoring **FN**:
* Losses on FP can be seen from the benefits and preparations we provide when guests are predicted not to cancel their bookings. starting from the pick-up service if needed, eating and drinking that has been prepared and the most detrimental is when it turns out that the predicted visitor did not cancel and actually cancels, the hotel will lose because it cannot rent out a hotel room

The chosen scoring is f1 score because both classes can provide the same big loss for the hotel

# Conclusion

 from the analysis and visualization of the barplot based on the is_cancelled column, 37.13% of guests canceled their bookings. This means that 44,152 people canceled orders, if using metrics f1 with a score of 74%, then 26% of guests `came/didn't cancel` but were predicted to cancel, with the TA online market segment of around 30,912 people who probably gave a bad rating to the hotel because when they arrived at the hotel even though it was predicted to cancel.

If it is assumed that the rooms in this hotel are 1 million per night, maybe the hotel can be said to have lost 44,152,000,000 because the room failed to be used even though it was already booked, when a room is booked at the hotel, of course it cannot rent out the room to other guests. If using the model from the metrics f1 score, 26% of guests who were predicted not to cancel actually cancel, then we can reduce the loss from the failed room rental to 30,912,000,000. In our opinion, this is quite good because it can reduce hotel losses by as much as 14 billion.

* FP can harm the hotel even though the impact is indirect, but it will have a big impact in the future because customers who are harmed may not book this hotel, and if you ignore FP it will create a bad rating on online travel agents who have become a market segment biggest in this hotel. While FN has a direct impact on hotels, it can be calculated that the loss from FN reaches 30M.

# Recommendation

* I recommend hotels to use this model because it can reduce 14 thousand people who cancel, if it is assumed a 1 night room is 1 million, then this model is able to reduce losses as much as 14 million, but to minimize FN, i.e. guests who are predicted to not cancel but can actually cancel reviewing cooperation agreements with travel agents, whether online or offline, which have become the largest market segment of this hotel to determine the minimum day limit before canceling, so that the hotel can rent out rooms that are not rented.

* And to reduce FP or guests who are predicted to cancel but actually don't, the customer service team can call or whatsapp to confirm arrival so that it can reduce the possibility of disappointing guests who will actually come so as to keep the hotel rating good.
