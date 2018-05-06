This program splits run-on sentences using Logistic Regression Classifier.
10000 Run-on sentences were formed from data taken from Yahoo Answers Questions https://webscope.sandbox.yahoo.com/catalog.php?datatype=l


Features:

word-1: word, lemma, pos, tag, istitle, isdigit, ngram_W_W+1
word: word, lemma, pos, tag, istitle, isdigit
word+1: word, lemma, pos, tag, istitle, isdigit, ngram_W-1_W


Logistic Regression Classifier:

             precision    recall  f1-score   support

          0       0.98      1.00      0.99      4547
          1       0.76      0.42      0.54       150

avg / total       0.97      0.98      0.97      4697
