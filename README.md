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

Raw text files are parsed into dictionaries, that includes sentence number, label, text, and abstract length.<br>
After pre-processing dataset looks like this: <br>
|index|line\_number|target|text|total\_lines|
|---|---|---|---|---|
|0|0|BACKGROUND|The emergence of HIV as a chronic condition means that people living with HIV are required to take more responsibility for the self-management of their condition , including making physical , emotional and social adjustments \.|11|
|1|1|BACKGROUND|This paper describes the design and evaluation of Positive Outlook , an online program aiming to enhance the self-management skills of gay men living with HIV \.|11|
|2|2|METHODS|This study is designed as a randomised controlled trial in which men living with HIV in Australia will be assigned to either an intervention group or usual care control group \.|11|
|3|3|METHODS|The intervention group will participate in the online group program ` Positive Outlook ' \.|11|
|4|4|METHODS|The program is based on self-efficacy theory and uses a self-management approach to enhance skills , confidence and abilities to manage the psychosocial issues associated with HIV in daily life \.|11|
|5|5|METHODS|Participants will access the program for a minimum of 90 minutes per week over seven weeks \.|11|
|6|6|METHODS|Primary outcomes are domain specific self-efficacy , HIV related quality of life , and outcomes of health education \.|11|
|7|7|METHODS|Secondary outcomes include : depression , anxiety and stress ; general health and quality of life ; adjustment to HIV ; and social support \.|11|
|8|8|METHODS|Data collection will take place at baseline , completion of the intervention \( or eight weeks post randomisation \) and at 12 week follow-up \.|11|
|9|9|CONCLUSIONS|Results of the Positive Outlook study will provide information regarding the effectiveness of online group programs improving health related outcomes for men living with HIV \.|11|
|10|10|BACKGROUND|ACTRN12612000642886 \.|11|
|11|0|BACKGROUND|The aim of this study was to evaluate the efficacy , safety and complications of orbital steroid injection versus oral steroid therapy in the management of thyroid-related ophthalmopathy \.|12|
|12|1|METHODS|A total of 29 patients suffering from thyroid ophthalmopathy were included in this study \.|12|
|13|2|METHODS|Patients were randomized into two groups : group I included 15 patients treated with oral prednisolone and group II included 14 patients treated with peribulbar triamcinolone orbital injection \.|12|
|14|3|METHODS|Only 12 patients in both groups \( 16 female and 8 male \) completed the study \.|12|
|15|4|RESULTS|Both groups showed improvement in symptoms and in clinical evidence of inflammation with improvement of eye movement and proptosis in most cases \.|12|
|16|5|RESULTS|Mean exophthalmometry value before treatment was 22\.6 1\.98 mm that decreased to 18\.6 0\.996 mm in group I , compared with 23 1\.86 mm that decreased to 19\.08 1\.16 mm in group II \.|12|
|17|6|RESULTS|Mean initial clinical activity score was 4\.75 1\.2 and 5 1\.3 for group I and group II before treatment , respectively , which dropped to 0\.83 1\.2 and 0\.83 1\.02 , 6 months after treatment , respectively \.|12|
|18|7|RESULTS|There was no change in the best-corrected visual acuity in both groups \.|12|
|19|8|RESULTS|There was an increase in body weight , blood sugar , blood pressure and gastritis in group I in 66\.7 % , 33\.3 % , 50 % and 75 % , respectively , compared with 0 % , 0 % , 8\.3 % and 8\.3 % in group II \.|12|
|20|9|RESULTS|No adverse local side effects were observed in group II \.|12|
|21|10|CONCLUSIONS|Orbital steroid injection for thyroid-related ophthalmopathy is effective and safe \.|12|
|22|11|CONCLUSIONS|It eliminates the adverse reactions associated with oral corticosteroid use \.|12|
|23|0|OBJECTIVE|The aim of this prospective randomized study was to examine whether active counseling and more liberal oral fluid intake decrease postoperative pain , nausea and vomiting in pediatric ambulatory tonsillectomy \.|17|
|24|1|METHODS|Families , whose child was admitted for ambulatory tonsillectomy or adenotonsillectomy , were randomly assigned to the study groups \( n = 116 ; 58 families in each group \) \.|17|

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
| Mean sentence length (chars)          | 150.522   |
| Median sentence length (words)        | 23.0     |
| Skewness sentence length distribution | 1.971    |
| Min sentence length (words)           | 1        |
| Max sentence length (words)           | 338      |
| 95% sentences are _ or shorter (words) | 54       |
| 95% sentences are _ or shorter (chars) | 291       |

Smallest abstract has 3 sentences, biggest has 51. Average sentence length is between 26-27 words. Around 26.6% of total words are stopwords and around 0.8% are unique words (not lowercased). Smallest sentence contains just a single word, biggest sentence contains 338 words.

## Baseline Models ##

1. Most frequent label:<br>
Model that classifies each sentence as RESULTS has apporox. 35% accuracy. This model was beaten by Multinomial Naïve Bayes Model.

2. Multinomial Naïve Bayes Classifier:<br>
Predicts class of the sentence depending on the distribution of words in each class and probability of the classes. No stopwords were removed, words were lowercased, tf-idf vectorization with unigram (class-word) frequency count was used. Evaluation yielded 76.5% validation accuracy and 75.4% weighted F1-score.

3. Embedding with Conv1D:<br>
First each sentence is tokenized and standardized to have length of 64 tokens. Then sentences in batches of 64 are passed to the Embedding layer that calculates embedding with dimensionality of 128 for each token. Those embedded sentences are then passed to a Text Covolutional layer that extracts common patterns and features, the outputs of Convolutional layer are then globally averaged. Last two layers is a DNN. Model is trained for 10 epochs. Final 85.5% testing accuracy, and 79.6% weighted F1-score.   

Important notice: Since eta on my device for uncut model was 10h+, I took only 10% of the initial batch size. Reason for such long training times is the embedding layer, that has 38M+ parameters. This inefficiency source will be eliminated in the working model by creating pre-trained embeddings on the training sentences and saving them on the disk.

## Working Model - PoseidonLSTM ##
pass


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

## Beyond classification - FastAPI Microservice ##
pass


