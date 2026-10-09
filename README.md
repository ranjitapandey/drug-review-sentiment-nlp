# Drug Review Sentiment Analysis (NLP)

A basic NLP project in Python that reads patient reviews of medicines and predicts whether each review is **positive** or **negative**.

## Data

- **Source:** [Drug Review Dataset (Druglib.com)](https://archive.ics.uci.edu/dataset/461/drug+review+dataset+druglib+com), UCI Machine Learning Repository
- 3,107 patient reviews, each with three text fields (benefits, side effects, overall comment) and a rating from 1 to 10
- The data file is **not included** in this repository, per the dataset's terms. Download it from the link above and place `drugLibTrain_raw.tsv` in the project folder.

## Steps

1. **Load** the data with pandas.
2. **Combine** the three review text fields into one text column.
3. **Label** the reviews: ratings 7–10 = positive (1), ratings 1–4 = negative (0). Ratings 5–6 were removed because they are mixed. This left 2,791 reviews (2,130 positive, 661 negative).
4. **Clean** the text: lowercase, remove numbers and punctuation, and remove common stopwords with NLTK. Negation words like "not," "no," and "didn't" were **kept**, because they change the meaning ("effective" vs "not effective").
5. **Convert** text to numbers with TF-IDF, using single words and word pairs (so "no side" effects is learned as one idea).
6. **Train** a logistic regression model on 80% of the data, with balanced class weights because positive reviews outnumber negative ones about 3 to 1.
7. **Test** on the other 20% and look at which words the model learned.
8. **Final check** on the dataset's separate test file (911 reviews the model never saw), using the same cleaning steps and no retraining.

## Results

| Metric | Validation (20% split, 559 reviews) | Final test file (911 reviews) |
|---|---|---|
| Accuracy | **83%** | **84%** |
| Baseline (always guess "positive") | 76% | 74% |
| F1 score, positive reviews | 0.89 | 0.89 |
| F1 score, negative reviews | 0.64 | 0.70 |

The results on the separate test file are close to the validation results, so the model is stable on new data.

**Top positive words:** no side, worked, able, reduced, mild, years
**Top negative words:** severe, worse, horrible, stopped taking, could not, no benefits, vomiting

## What I learned

- **Accuracy alone can mislead.** Because 76% of reviews are positive, a model that always guesses "positive" scores 76%. Comparing against this baseline, and checking each class separately, gives a truer picture.
- **Check what the cleaning step removes.** The default stopword list removed "didn't," so I added contractions back and re-tested. Accuracy stayed about the same, which showed the bigger limit was the small number of negative reviews and the many mixed reviews ("it helped my cramps, but I hate it").
- **Small word differences matter.** "helped" scored positive, while "help" scored negative, because "help" usually appears in sentences like "nothing seemed to help."

## Next ideas

- Try a transformer model such as BERT, which reads words in context.
- Analyze the side-effects text on its own.

## Tools

Python, pandas, NLTK, scikit-learn, NumPy, Jupyter

## Citation

Gräßer, F., Kallumadi, S., Malberg, H., & Zaunseder, S. (2018). *Aspect-Based Sentiment Analysis of Drug Reviews Applying Cross-Domain and Cross-Data Learning.* Proceedings of the 2018 International Conference on Digital Health.
