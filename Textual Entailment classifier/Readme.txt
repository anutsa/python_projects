This program implements a classifier that checks hypothesis relation to text using WordNet. 
Train data: 1000 sentences of each hypothesis type (entailment, contradiction, neutral)
Test data: 988 sentences

Features: 
WordNet similiraty metrics http://www.nltk.org/_modules/nltk/corpus/reader/wordnet.html

- path_similarity
- lch_similarity
- wup_similarity
- res_similarity
- jcn_similarity
- lin_similarity

Logistic Regression Classifier
               precision    recall  f1-score   support

contradiction      0.489     0.615     0.545       325
   entailment      0.520     0.687     0.592       339
      neutral      0.397     0.160     0.229       324

  avg / total      0.469     0.491     0.457       988
  
  
One more feature was extracted to improve model performance:

- number of exact matches between hypothesis and text words synsets.

Logistic Regression Classifier
               precision    recall  f1-score   support

contradiction      0.510     0.545     0.527       325
   entailment      0.557     0.617     0.585       339
      neutral      0.470     0.386     0.424       324

  avg / total      0.513     0.517     0.513       988