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
