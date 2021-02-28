# Text Summarization

### II. Dataset Description

● Dataset for fine-tuning the model<br>
The CNN/Daily Mail dataset (Hermann et al., 2015) contains online news articles paired with
multi-sentence summaries. We imported the non-anonymized variant version(See et al., 2017)
‘cnn_dailymail’ from the package datasets. In all, the dataset has 286,113 training pairs, 13,368
validation pairs and 11,490 test pairs. The source documents in the training set have 766 words
spanning 29.74 sentences on an average while the summaries consist of 53 words and 3.72 sentences.<br>
● Dataset for testing<br>
BBC News Summary (Kaggle) originally contains 2,225 pairs of News articles and summary. On
average, the articles have 19.64 sentences and 378.98 tokens while the summaries consist of 8.40
sentences and 165.22 tokens. We have removed the outlier pairs whose summary are up to 2,073 tokens.
The BBC news dataset contains shorter Articles, on average, but longer summaries, so it will be able to
test how well the models can scale to texts of varying length and structure.

### III. Methodology

In this project, we explored a few models based on the Transformer (Vaswani, et al. 2017), a recent
seq2seq architecture relying exclusively on attention. Our objective is to fine-tune two prominent
transformer models (BART and T5) and compare their performance. We use the transformer-based
architecture for Text Summarization since this task involves long input texts.

### IV. Evaluations

For the evaluation, we used BLEU and ROUGE-N as our metrics and developed the evaluation
function based on datasets.load_metric() for our project.
BLEU(Bilingual Evaluation Understudy) evaluates the generated text based on N-gram
precision. As proposed in Penineni’s work in 2002, BLEU measures the closeness to reference
human language. Eisenstein introduced that “The most popular quantitative metric is
BLEU(2019).” Since our prediction is short and there are rare overlapping trigrams between the
inference summary and generated summary, we modified the weights of n-gram precisions and
applied smoothing to the calculation. Meanwhile, we calculated brevity penalty to avoid
misleadingly high precision scores since our generated summaries are shorter than the
reference.

To overcome the potential weakness of BLEU that it only measures precision we also used
ROUGE-1/2 score, which seeks unigram/bigram matches between the generated and the
reference summaries. ROUGE, found to be highly correlated with human evaluations, is a
method based on N-gram statistics. The reasons why we only use ROUGE-N is that : 1.Our task
is a single document summarization and our reference size is 149.5 tokens on average, where
ROUGE-N and ROUGE-L score perform better as suggested in Lin’s research in 2004.

Baseline:<br>
Since news usually provides the most important information at the beginning of the articles, we simply
extract the leading three sentences from each article as our baseline model. The baseline summary
contains 56 tokens on average. The performance of the baseline model is listed below:

|  Models   |rouge1 |rouge2 |rougeL |prec1  |prec2  | precL |
|-----------|-------|-------|-------|-------|-------|-------|
|Baseline-3 | 43.88 | 36.04 | 34.75 | 77.81 | 63.95 | 61.71 |
|Baseline-1 | 22.45 | 19.57 | 21.66 | 87.03 | 77.93 | 83.70 |
|BART-Base  | 5.40  | 0.37  | 4.36  | 29.52 | 22.6  | 24.08 |
|BART-Tuned | 2.58  | 0.18  | 2.11  | 15.23 | 1.18  | 12.56 |
|T5-Small   | 5.30  | 0.38  | 4.384 | 33.69 | 2.73  | 28.35 |
|T5-Tuned   | 1.51  | 0.09  | 1.31  | 11.29 | 0.81  | 9.98  |

Course: CS6120 Natural Language Processing Fall 2020 <br>
Contributors: Mitchell Gauthier, Wei Yu, Mrunmayi Anchawale, Juhner Padia
