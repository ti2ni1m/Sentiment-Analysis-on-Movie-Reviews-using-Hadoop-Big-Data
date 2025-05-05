# Sentiment-Analysis-on-Movie-Reviews-using-Hadoop-Big-Data

---

## Data

You will use a large movie review dataset containing a set of 25,000 movie reviews for training, and 25,000 for testing. You can download the data from the vUWS under the assignments folder named aclImdb.zip. You can also visit the following website for more information about the dataset at http://ai.stanford.edu/~amaas/data/sentiment/ or download data directly from there.

Unzip the data to your local directory. Enter the aclImdb/ directory created by the zip file (~500MB), you will find the following three items among others
- train/: feature files and raw text files for the training set
- test/: feature files and raw text files for the testing set
- README: the readme file for more information on the dataset

Read the README file carefully about the descriptions on the text files that contain the reviews and their naming convention. The directories we concern here are
- ./aclImdb/train/pos: raw text files of positive reviews in the training set
- ./aclImdb/train/neg: raw text files of negative reviews in the training set
- ./aclImdb/test/pos: raw text files of positive reviews in the test set
- ./aclImdb/test/neg: raw text files of negative reviews in the test set

A full version of this data set is available at hdfs://hadoop.cdms.westernsydney.edu.au:9000/users/ugbigdata/Hadoop/imdb/fullversion. A tiny cut down version with much less number of files (20 each for training and 10 each for test) is also available at hdfs://hadoop.cdms.westernsydney.edu.au:9000/users/ugbigdata/Hadoop/imdb/tinyversion. The tiny version is for experimenting purpose.

### Task 1 Feature extraction

Use the map reduce model to convert all text data into matrices. Convert ratings to vectors. These will be used for classification in Task 2. Use TF-IDF to vectorise the text files. See previous practical classes and lectures materials for TF-IDF. One step further though is to represent each text file (review) as a very long and sparse vector as the following. Assume wordslist is the final list of distinct words contained in all reviews and its length is *D*. Then each review will be a vector of length , with each position associated with a word in wordlist and the value being either 0, if the corresponding word is absent in the review, or the word’s TF-IDF. For example, if wordlist = [‘word1’, ‘word2’, ‘word3’, ‘word4’] and review 1 contains word1 and word4 , then the vector representation of review 1 is [0.1, 0, 0, 0.4] assuming TF-IDF of word1 and word4 in review 1 is 0.1 and 0.4 respectively. Note that TF is calculated from one single document while IDF is obtained from all documents in the collection.

#### Requirements:
1. Map reduce model is a must. Implement it using Hadoop streaming. All data are available on SCDMS HDFS. The recommendation is to work on the tiny version of the data to make the code work. You may try your code on the full version. However, the application to full version is not required.
2. Generate two matrices: `training_data`, `test_data`, and two vectors, `training_targets`, `test_targets`. training_data should have *N* rows and *D* columns with each row corresponding to each review in the training set, where *N* is the totally number of reviews in training set and *D* is the total number of words. *N* and *D* vary depending on which version of the data you use. `training_targets` should have *N* elements each of which is the rating of the review is for. `test_data` and `test_targets` are similar defined.

---

### Task 2 Classification

Construct a classification model for review sentiment prediction, meaning that given a customer review (taken from test set) about a movie, your program should be able to predict whether it is positive or negative. There is no limitation on how many classifiers and what specific model you should use. You can simply pick one that works for you for this task, either from those covered in lectures and practical classs or any other classifiers from any python packages. A good starting point is the `scikit-learn` (i.e. `sklearn`) package. A few things you need to address in your python program are listed as requirements below.

#### Requirements:
1. Data pre-processing. In task 1, you have extracted the ratings vectors for training and test. They are raw ratings. As we are interested in sentiment prediction, i.e. to predict either the review is positive or negative. You need to convert all ratings>5 as positive class and ratings<=5 as negative class. Choose a coding scheme, e.g. 1 for positive, 0 for negative.
2. Normalisation. Apply at least one normalisation scheme and compare the performance of the classifier(s) with and without normalisation.
3. Training and model selection. Use cross validation to select the best parameters for your classifier. There may be many parameters to tune in some classifiers such as random forest classifier (RFC). You can focus on the most important one(s) such as `max_depth` and `n_estimators` in RFC. Refer to `scikit-learn` package documentation for details. Hint: you can start with a small subset of training set to test a few parameters to get a feel of what range the parameters should be that make the model perform well in terms of prediction accuracy. Then turn on large scale cross validation on the whole training set.
4. Test on test data. After model selection, apply the best model, i.e. model with the parameters that produces the best cross validation scores, to test data and make prediction for each review and record prediction accuracy (ACC).



