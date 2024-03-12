# Medical-Abstract-Segmentation
Abstract Segmentation and Compression for Medical Texts

## Dataset ##
Name: PubMed_200k_RCT (_/train.txt, _/test.txt, _/train.txt)
Description: Dataset contains 200k abstracts from medical papers. Each abstract is split into sentences, each sentence is classified into 5 different categories (see below).
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
| Unique words                          | 472016   |
| Sentences                             | 2211861  |
| Min abstract length (sentences)       | 3        |
| Max abstract length (sentences)       | 51       |
| Mean sentence length (words)          | 26.229   |
| Median sentence length (words)        | 23.0     |
| Skewness sentence length distribution | 1.971    |
| Min sentence length (words)           | 1        |
| Max sentence length (words)           | 338      |

Smallest abstract has 3 sentences, biggest has 51. Average sentence length is between 26-27 words. Around 26.6% of total words are stopwords and around 0.8% are unique words. Smallest sentence contains just a single word, biggest sentence contains 338 words.

Full sentence length distribution can be found in sentence_length_distribution.txt.

## Baseline Models ##

1. Most frequent label:<br>
Model that classifies each sentence as RESULTS has apporox. 35% accuracy. This model was beaten by Multinomial Naïve Bayes Model.

2. Multinomial Naïve Bayes Classifier:<br>
Predicts class of the sentence depending on the distribution of words in each class and probability of the classes. No stopwords were removed, tf-idf vectorization with unigram (class-word) frequency count was used. Evaluation yielded 76.5% accuracy and f1-score, precision and recall. Since dataset is balanced multi-class, it is possible for those metrics to be identical.

3. Embedding with Conv1D
pass

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

Model is saved unser medsegnet_model.hash5. <br>
Weights for this model are saved under medsegnet_weights.h5.

## Beyond classification
pass


