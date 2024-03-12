# Medical-Abstract-Segmentation
Abstract Segmentation and Compression for Medical Texts

## Dataset ##
Name: PubMed_200k_RCT (_/train.txt, _/test.txt, _/train.txt) <br>
Description: Dataset contains 200k abstracts from medical papers. Each abstract is split into sentences, each sentence is classified into 5 different categories (see below). Words are split by whitespaces and sentences are split by lines.

Split:
|             | # Sentences|
|-------------|------------|
| TRAINING    | 2211861    |
| TEST        | 29493      |
| DEVELOPMENT | 28932      |

## Statistical information regarding training set (PubMed_200k_RCT/train.txt) ##
| Label       | #      |
|-------------|--------|
| RESULTS     | 766271 |
| METHODS     | 722586 |
| CONCLUSIONS | 339714 |
| BACKGROUND  | 196689 |
| OBJECTIVE   | 186601 |

As we can see most common label is RESULTS with 766271 appearances.


|             |                                    |
|-------------|------------------------------------|
| Words                                 | 58015688 |
| Stopwords                             | 15986680 |
| Unique words (n. lowercased, incl. stopwords)         | 472016   |
| Sentences                             | 2211861  |
| Min abstract length (sentences)       | 3        |
| Max abstract length (sentences)       | 51       |
| Mean sentence length (words)          | 26.229   |
| Median sentence length (words)        | 23.0     |
| Skewness sentence length distribution | 1.971    |
| Min sentence length (words)           | 1        |
| Max sentence length (words)           | 338      |
| 95% sentences are _ or longer (words) | 54       |

Smallest abstract has 3 sentences, biggest has 51. Average sentence length is between 26-27 words. Around 26.6% of total words are stopwords and around 0.8% are unique words (not lowercased). Smallest sentence contains just a single word, biggest sentence contains 338 words.

Full sentence length distribution can be found in sentence_length_distribution.txt.

## Baseline Models ##

1. Most frequent label:<br>
Model that classifies each sentence as RESULTS has apporox. 35% accuracy. This model was beaten by Multinomial Naïve Bayes Model.

2. Multinomial Naïve Bayes Classifier:<br>
Predicts class of the sentence depending on the distribution of words in each class and probability of the classes. No stopwords were removed, words were lowercased, tf-idf vectorization with unigram (class-word) frequency count was used. Evaluation yielded 76.5% accuracy and f1-score, precision and recall. Since dataset is balanced multi-class, it is possible for those metrics to be identical.

3. Embedding with Conv1D:<br>
First each sentence is tokenized and standardized to have length of 64 tokens. Then sentences in batches of 64 are passed to the Embedding layer that calculates embedding with dimensionality of 128 for each token. Those embedded sentences are then passed to a Text Covolutional layer that extracts common patterns and features, the outputs of Convolutional layer are then globally averaged. Last two layers is a DNN. Model is trained for 10 epochs. Final accuracy 85%, and final F1-score 80%. Architecture:

|Layer (type) | Output Shape                          |Param # |
|-------------|---------------------------------------|----------
| embedding (Embedding)|       (None, 64, 128)|           38400000|
| conv1d (Conv1D)|             (None, 64, 32)|            24608|                         
| global_average_pooling1d | (None, 32) |               0      |                                                  
| dense (Dense)|               (None, 64) |               2112 |                                                   
| dense_1 (Dense)  |           (None, 5)  |               325 |        

Total params: 38427045 (146.59 MB) <br>
Trainable params: 38427045 (146.59 MB) <br>
Non-trainable params: 0 (0.00 Byte) <br>

Important notice: Since eta on my device for uncut model was 10h+, I took only 10% of the initial batch size.

## Working Model ##

Architecture:
pass

Evaluation:
| Metric      |            |
|-------------|------------|
| Accuracy    | 90%        |
| F1          | 87%        |
| Precision   | 87%        |
| Recall      | 87%        |

Model is saved under medsegnet_model.hash5. <br>
Weights for this model are saved under medsegnet_weights.h5.

## Beyond classification
pass


