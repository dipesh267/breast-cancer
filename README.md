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

Looking at the results we see that all models have a fairly similar train and test scores. The most differenciating factor is their their training times. Even though the training time for KNN model is the lowest by quite a margin, when predicting cases of cancer, it's better to err on the side of caution and use the most accurate predicting model rather than the quickest one.
For this specifc reason we decided to look further with Decision Tree rather than the other models

When we look at the preliminary confusion matrix with the ideal hyperparameters, cirterion='entropy' and max_depth=3, for the training dataset we see that we have few false negatives and false positives but we're mostly predicting accurately.
But again, when it comes to predicting cancer we're rather predict false positives rather than false negatives. It's always better to get a false positive that can be proven wrong rather than a false negative that can cause the patient to ignore the symptops and cause the cancer to get worse.

![App Screenshot](/images/confusion-3.png)

With the idea that it's better to try to remove all false negatives, I adjusted the hypermeter max_depth slightly at a time to eventually land on max_depth=7 where I got both false readings to 0.

![App Screenshot](/images/confusion-7.png)

### As part of an extensive review I would like to take a look at the confusion matrix for KNN and Logistic Regression as well.

With the test scores being very close to each other in the 95% and above range, let's compare the Decision Tree confusion matrix to that of KNN and Logistic Regression.

With the Logistic Regression Confusion Matrix, we see that we have about 19 misdiagnosis. 11 of those are false negative and 8 are false positives.

![App Screenshot](/images/confusion-lr.png)

The KNN confusion matrix had about 27 misdiagnosis where 20 were false negatives and 7 were false positives.

![App Screenshot](/images/confusion-knn.png)

### Recommendation

The accuracy of the Decision Tree model we picked is fairly high and the fact that KNN and Logistic regression had false positives make the decision easier to go with Desision Tree atleast for now. With the confusion matrix showing that we had 0 false negatives and 0 false positives, I would recommend we take this current model and put it in production and let it work with some new data.

We should keep an eye out on the model and train the model again if we start seeing the failure rates increase.
If the Decision Tree model starts to fail with production data, we should look at tuning the other two models to see if they will perform better with unseen data.
