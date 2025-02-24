# NPF Event Prediction With Logistic Regression

Study project for the Introduction to Machine Learning course (2022) at the University of Helsinki. 

### Introduction

The aim of the project was to implement a classifier to predict *new particle formation* (NPF) events based on
a set of atmospheric measurements from the Hyytiälä forestry field station (see [Kerminen et al. (2018)](https://doi.org/10.1088/1748-9326/aadf3c) for an explanation of the NPF phenomenon). The main task
was to build a binary classifier to predict whether an NPF event occurs or not on a given day. A
secondary task of multi-class classification was also included, where observations had to be placed in
four classes: no NPF event, or one of three different types of NPF events. A data set with correct
class labels was provided for training the model.

The project was implemented using Python and scikit-learn. The stages of the project included data
analysis, exploration and preprocessing, feature selection, choosing the classification algorithm, choosing
model parameters and estimating the classification accuracy. The accuracy of the classifier was finally tested
in a project challenge, which compared models built by different teams for the same task. For the challenge,
predictions were made on a separate set of test data, for which the correct classes were not known while building
the classifier.

### Data analysis and preprocessing

The training data contained several variables measured on different days at the Hyytiälä forestry field station, mainly at the SMEAR II mast (https://smear.avaa.csc.fi/). The variables were daily means and standard deviations of measurements between sunrise and sunset. The provided data set had been preprocessed to be fairly clean with no missing values.

The training data set included 464 observations, with 104 variables for each observation. The large number
of variables suggested that it might not be feasible to use all variables as features, since this would easily lead
to excessive complexity of the model. There were some variables that didn’t provide any useful information, and were discarded 
right in the early stages of data exploration. 

The data analysis stage also included finding correlations between the different measurements. The analysis
showed measurements for the same variable at different mast heights were very highly correlated, and this
finding was used in feature selection: for each variable, measurements from only one height were used as features. Preprocessing the data also included normalizing the variables, as the data consisted of measurements made
in several different units.

### Choosing the machine learning approach

The project started with choosing a simple model that seemed to perform reasonably well in the task, and then
proceeded to fine-tune the model to achieve a better classification accuracy. A Logistic Regression approach was chosen, as the model is relatively simple, 
and some early experiments indicated that it might perform well on this task. For fine-tuning, models with different parameters were
compared, and their accuracy was estimated by using 10-fold cross-validation. 

As the primary aim of the project was to build a binary classifier, the focus was on this task. However, the
project requirements also asked for multi-class predictions, so these were implemented using a very simple probability-based solution: the most frequent NPF event class in the training data was predicted
for all days that the binary classifier predicted to be event days.

### Feature selection

As discussed in the section describing the data analysis stage, some of the measurements were found to be
very highly correlated with each other, and therefore some of them were left out of the set of features. For example,
a temperature measurement at only one mast height was included for each observation. This resulted in a
feature set consisting of 38 variables.

Further feature selection was performed with L1 regularization, as a model with regularization showed a
better accuracy. Choice of regularization is discussed in more detail in the
next section.

### Model parameter selection

While tuning the model, Logistic Regression models with four different regularization options were compared.
These included no regularization, L1, L2 and Elastic Net. Out of these four, L1 and Elastic Net resulted in
the highest cross-validation accuracies, around 0.893 and 0.888 respectively. The number of coefficients set to
zero was 15 for L1 regularization, and 9 for Elastic Net.

Based on the results stated above, Logistic Regression with L1 regularization was chosen as the final model.
Apart from having slightly better accuracy, the model with L1 regularization resulted in a sparser solution. 
This indicated that the model was less complex and therefore had a lower risk of overfitting, suggesting that it 
would probably generalize better to making predictions for the test data.

Based on the cross-validation score, the accuracy of the model chosen for the predictions was estimated to be
around 0.89.

### Results

The predictions were saved to a ```.csv``` file that was submitted to the course staff for assessment of prediction accuracy.
With the final test data, the model performed well in the binary classification task. In this task, the accuracy
of the model, measured as the fraction of observations classified correctly, was around 0.87. However, the
accuracy was slightly lower than the estimate, so the model did not generalize as well as expected. Nevertheless,
the actual accuracy was close to the cross-validation-based estimate. Also, the binary classification perplexity
was reasonably low at 1.38, which indicates a good performance in the predictions.

In the multi-class predictions, the accuracy was much lower, as was expected based on focusing on binary classification. 
Using a very simple approach, where the most frequent event class was predicted for all
observations classified as events, resulted in an accuracy of around 0.67.

### Insights

Even though the classifier performed well on the test data, the project could have been further developed in
several aspects. For example, feature selection could have been done differently: with more
background knowledge on the NPF phenomenon, it would have been easier to choose the variables that are
highly related to NPF. Even if relatively simple and straightforward approaches to feature selection proved to be sufficiently effective, it would have been interesting to also apply some unsupervised learning methods, such as Principal Component
Analysis. The project could have been extended to make more sophisticated
multi-class predictions, for example by using multinomial logistic regression.
