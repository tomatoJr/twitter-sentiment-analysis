# Sentiment Analysis on Tweets

![Status badge](https://img.shields.io/badge/Status-Archived-important)

**Update**(Apr 6, 2021) This repo is based on [Sentiment Analysis on Tweets](https://github.com/abdulfatir/twitter-sentiment-analysis). This repo is used for CSCE 638 project.

## Dataset Information

We use and compare various different methods for sentiment analysis on tweets (a multi-class classification problem). 

Download the dataset [here](https://www.kaggle.com/kazanova/sentiment140). 

If you want to run models like CNN or LSTM, a pre-trained word embedding file is required. Download [here](https://github.com/stanfordnlp/GloVe). In this project we use `glove.twitter.27B.zip`.

## Requirements

There are some general library requirements for the project and some which are specific to individual methods. The general requirements are as follows.  
* `numpy`
* `scikit-learn`
* `scipy`
* `nltk`

The library requirements specific to some methods are:
* `keras` with `TensorFlow` backend for Logistic Regression, MLP, RNN (LSTM), and CNN.
* `xgboost` for XGBoost.

**Note**: It is recommended to use Anaconda distribution of Python.

## Usage

### Preparation

Make sure the dataset and glove embedding file is in the `twitter-sentiment-analysis/dataset` directory. Rename the dataset file to `training.csv`. We don't need test file here since the training dataset can be split to 10% of validation data.

Make sure create a directory `models` in the directory `twitter-sentiment-analysis/code`.

As for embedding file, use `glove.twitter.27B.200d.txt` since the dimension of embedding defined in `cnn.py` is 200.

### Preprocessing 

1. Run `preprocess.py <raw-csv-path>` on both train and test data. This will generate a preprocessed version of the dataset.
2. Run `stats.py <preprocessed-csv-path>` where `<preprocessed-csv-path>` is the path of csv generated from `preprocess.py`. This gives general statistical information about the dataset and will two pickle files which are the frequency distribution of unigrams and bigrams in the training dataset. 

After the above steps, you should have four files in total: `<preprocessed-train-csv>`, `<preprocessed-test-csv>`, `<freqdist>`, and `<freqdist-bi>` which are preprocessed train dataset, preprocessed test dataset, frequency distribution of unigrams and frequency distribution of bigrams respectively.

For all the methods that follow, change the values of `TRAIN_PROCESSED_FILE`, `TEST_PROCESSED_FILE`, `FREQ_DIST_FILE`, and `BI_FREQ_DIST_FILE` to your own paths in the respective files. Wherever applicable, values of `USE_BIGRAMS` and `FEAT_TYPE` can be changed to obtain results using different types of features as described in report.

### Baseline
3. Run `baseline.py`. With `TRAIN = True` it will show the accuracy results on training dataset.

### Naive Bayes
4. Run `naivebayes.py`. With `TRAIN = True` it will show the accuracy results on 10% validation dataset.

### Maximum Entropy
5. Run `logistic.py` to run logistic regression model OR run `maxent-nltk.py <>` to run MaxEnt model of NLTK. With `TRAIN = True` it will show the accuracy results on 10% validation dataset.

### Decision Tree
6. Run `decisiontree.py`. With `TRAIN = True` it will show the accuracy results on 10% validation dataset.

### Random Forest
7. Run `randomforest.py`. With `TRAIN = True` it will show the accuracy results on 10% validation dataset.

### XGBoost
8. Run `xgboost.py`. With `TRAIN = True` it will show the accuracy results on 10% validation dataset.

### SVM
9. Run `svm.py`. With `TRAIN = True` it will show the accuracy results on 10% validation dataset.

### Multi-Layer Perceptron
10. Run `neuralnet.py`. Will validate using 10% data and save the best model to `best_mlp_model.h5`.

### Reccurent Neural Networks
11. Run `lstm.py`. Will validate using 10% data and save models for each epock in `./models/`. (Please make sure this directory exists before running `lstm.py`).

### Convolutional Neural Networks
12. Run `cnn.py`. This will run the 4-Conv-NN (4 conv layers neural network) model as described in the report. To run other versions of CNN, just comment or remove the lines where Conv layers are added. Will validate using 10% data and save models for each epoch in `./models/`. (Please make sure this directory exists before running `cnn.py`). 

### Majority Vote Ensemble
13. To extract penultimate layer features for the training dataset, run `extract-cnn-feats.py <saved-model>`. This will generate 3 files, `train-feats.npy`, `train-labels.txt` and `test-feats.npy`.
14. Run `cnn-feats-svm.py` which uses files from the previous step to perform SVM classification on features extracted from CNN model.
15. Place all prediction CSV files for which you want to take majority vote in `./results/` and run `majority-voting.py`. This will generate `majority-voting.csv`.

## Information about other files

* `dataset/positive-words.txt`: List of positive words.
* `dataset/negative-words.txt`: List of negative words.
* `dataset/glove-seeds.txt`: GloVe words vectors from StanfordNLP which match our dataset for seeding word embeddings.
* `Plots.ipynb`: IPython notebook used to generate plots present in report.
