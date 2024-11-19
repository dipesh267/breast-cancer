# Predicting Breast Cancer

The breast cancer dataset was acquired from Kaggle.
Breast cancer is caused when cells in the breast grow out of control and form turmors. Normally, they're suspected when women notice a lump in their breast, change in the shape of the breast etc.
These lumps can either be Benign (non-cancerous) or Malignant (cancerous).
The goal of this predicting model is to be able to predict weather a lump is benign or malignant based on the characteristics of the lump.

![App Screenshot](/images/slightlyunbalanced.png)

We have a dataset that's slightly unbalanced but I think this is ok. We have a fair amount of positive data to work with.

### Preparing the data for modelling

There are 31 features in the dataset and a column for diagnosis, which is either M (malignant) or B (benign).
The features of the dataset all describe the characteristics of the lump of the patient.
Some of the feautres are

1. Area mean
2. Smoothness mean
3. Radius
4. Texture

All the data provided are numerical in value and there aren't any missing values that we need to worry about.
The only column that isn't useful to us is the ID. We will be deleting this column when building our column.

We will encode the target colum to numberical just to keep all the data consistent.

### Modelling

Some of the Models we looked at are

1. Logistic Regression
2. KNN (K Nearest Neighbor)
3. Decision Tree
4. SVC (Support Vector Classifier)

Without knowing which model would work for this dataset we fit each model with the training dataset to see if any one of them performed better than the other. We ran a GridSearchCV for each model with different parameters to get the best possible parameters that worked for each model and did a comparison againt other models and their ideal parameters.

![App Screenshot](/images/gridResults.png)

![App Screenshot](/images/gridResultsChart.png)

Looking at the results we see that all models have a fairly similar train and test scores. What differenciates the models are their training times. Even though test score for KNN is slightly lower that Logistic Regression and SVC, the training time is much lower for KNN.
For this specifc reason we decided to look further with KNN rather than the other models

When we look at the preliminary confusion matrix for the training dataset we see that we have quite a few false negatives and false positives.

![App Screenshot](/images/confusionTraining.png)

Can this be improved upon by trying to balance the dataset?
We did this by using the RandomOverSampler to oversample the positive class records got a dataset of 2657 positive records and 2560 negative records.

After trainging the KNN model with the oversampled training dataset, we were able to see that it improved to reduce the number of false positives and false negatives.

![App Screenshot](/images/oversampledConfusion.png)

We then predicted the classes for the larger unseen dataset to see what our confusion matrix looked like but it seems that the false postives rose by quite a bit.

![App Screenshot](/images/unseenConfusion.png)

So in our case, even though oversampling helped with the train/test data set, with the larger unseen data it performed much worse. Maybe, in this case, oversampling isn't the best approach.

### Quick look at KNN vs. SVC scores to verify our selection

KNN Scores
![App Screenshot](/images/knnScores.png)

SVC Scores
![App Screenshot](/images/svcScores.png)

I think its safe to say even though precision score with SVC for a positive class is 1% more than KNN, the overall F1 score for KNN is higher by 10%. I think KNN is the right choice for this dataset.

### Further Analysis

With KNN being chosen as the model to go with, we can now hand over our model to the subject matter experts to decide the specific around the threshold for the model.

![App Screenshot](/images/thresholdScores.png)

As we see here, our precision goes down as our recall gets higher.

![App Screenshot](/images/roc.png)

With the ROC (Receiver Operator Curve) plot we can see the percentage of True Positive and it's correspoding False Positive rates. Are we ok with 80% True Positive but having 10% False Positive or would we rather go for 100% True Positive with around 38% False Positives?

This is something the subject matter expert would have to weigh to see what a good compromise is.

We also take quick look at the ROC curve taking into consideration the different values for n-neighbor. The GridSearchCV cross validation process had comeup with 28 as the ideal value for the model to give the best results.
We look at a quick comparison at the ROC curves for n-neighbor values of 1, 10, and 28

![App Screenshot](/images/multipleRoc.png)

### Recommendation

I would select the KNN Classifier because it has a much lower training time but very comparable train/test score compared to other classifiers.
I would put the decison to tune the classifier to a specifc threshold upon the subject matter expert to come to an agreeable compromise between recall and precision.
