This program aims to define formal vs informal style on a sentence level using Logistic Regression Classifier.

В якості даних була взята частина корпусу опублікованого у  "An Empirical Analysis of Formality in Online Communication." (Ellie Pavlick and Joel Tetreault, 2016 TACL). Він складається з речень 4х жанрів( новини, блоги, електронні листи та питання-відповіді з форумів) проанотованих людьми на Amazon Mechanical Turk.
Ми використали речення, які отримали оцінку в діапазоні від -3 до -0.8 та від 0.8 до 3 (де -3 - дуже неформальне речення, а +3 - дуже формальне), щоб відкинути ті, для яких складно визначити точну приналежність до одного чи іншого стилю.
Для того щоб отримати збалансовану тестову вибірку ми залишили 30% від найменш представленої категорії і таку ж кількість з інших. (TotalCount: [3343, 1006, 1733, 1150]; TestCount:[302, 302, 302, 302])
Для оцінки якості моделі було взято метрику f1.
В якості бейзлайну було взято модель "Мішок слів". Для формування мішку слів застосували такі фільтри: відкинули нерелевантні частини мови та деякі стоп-слова, слова що зустрічалися у вибірці менше 3х разів та слова різниця у розподілі яких між двома категоріями склала менши 30%. Випробували 2 варіанти обрахунку стилю речення.
1. В першому для кожного токена в реченні (яке зустрілося у мішку) ми брали більше значення зі словника і рахували суми "формальних" та "неформальних" слів, стиль речення визначався за більшим показником.
     Результати:
         formalFound: 707
         formalTotal: 736
         informalFound: 240
         informalTotal: 476
         Predicted: 947
         Processed: 1212
         Formal recall: 0.9605978260869565
         Informal recall: 0.5042016806722689
         Precision: 0.7813531353135313
         F: 0.‎756084890579551
2. У другому вираховувався середній показник формальності для всіх слів у реченні, якщо він >= 0.5 речення вважалося формальним, у всіх інших випадках - неформальним.
    Результати:
     formalFound: 708
     formalTotal: 736
     informalFound: 218
     informalTotal: 476
     Predicted: 926
     Processed: 1212
     Formal recall: 0.9619565217391305
     Informal recall: 0.4579831932773109
     Precision: 0.‎764026402640264
     F: 0.7360069097602512
Різниця в точності між першим і другим варіантом склала 1.7%.


Наступним етапом будемо додавати правила на основі лінгвістичних закономірностей та алгоритми машинного навчання.





Для того, щоб покращити результат, ми побудували модель на основі таких характеристик (в дужках вказана вага характеристики на основі роботи класифікатора):
	- Середня довжина слова у реченні. (-0.90989133)
	- Кількість слів у реченні. (-0.59958094)
	- Пасивний/активний стан речення. (-0.00656213)
	- Наявність формальних/неформальних особових займенників. (-0.96553641 / -0.16481676)
	- Кількість виразів з неформального словника (скорочень, абревіатур, фразових дієслів та неформальних прикметників). (0.64026971)
	- Кількість виразів з формального словника (скорочень, абревіатур, формальних дієслів та формальних прикметників). (0.64252416)

Тут видно, що алгоритм ставить некоректну вагу для неформальних особових займенників і виразів з формального словника. 
Імовірно, для того щоб виправити це, необхідно більше тренувальних даних і покращити формальний словник.

Нижче наведені результати роботи декількох класифікаторів. Найкраще себе показав метод логістичної регресії.

SVM Classifier
             precision    recall  f1-score   support

     formal       0.89      0.84      0.87       736
   informal       0.77      0.84      0.81       476

avg / total       0.85      0.84      0.84      1212


##################################################

Naive Bayes Classifier
             precision    recall  f1-score   support

     formal       0.88      0.71      0.78       736
   informal       0.65      0.85      0.74       476

avg / total       0.79      0.76      0.77      1212


##################################################

K-Nearest Neighbours Classifier
             precision    recall  f1-score   support

     formal       0.87      0.87      0.87       736
   informal       0.80      0.79      0.80       476

avg / total       0.84      0.84      0.84      1212


##################################################

Descision Tree Classifier
             precision    recall  f1-score   support

     formal       0.83      0.90      0.87       736
   informal       0.83      0.71      0.77       476

avg / total       0.83      0.83      0.83      1212


##################################################

Logistic Regression Classifier
             precision    recall  f1-score   support

     formal      0.884     0.868     0.876       736
   informal      0.802     0.824     0.812       476

avg / total      0.852     0.851     0.851      1212




Тестування моделі на випадкових реченнях:

Nice to see you!
informal

##################################################

Come again!
informal

##################################################

Hot as hell!
informal

##################################################

You could fry an egg on the sidewalk.
informal

##################################################

It is extremely hot today.
informal

##################################################

As the price of five dollars was reasonable, I decided to make the purchase without further thought.
formal

##################################################

This is to inform you that your book has been rejected by our publishing company as it was not up to the required standard.
formal

##################################################

The people at the school seemed to be fairly amused by the mishaps in the playground yesterday.
formal



За даними дослідження 
	"Learning to Classify Documents According to Formal and Informal Style" March 2012
	Fadi Abu Sheikha and Diana Inkpen, School of Electrical Engineering and Computer Science, University of Ottawa, Canada
	http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.364.4671&rep=rep1&type=pdf
для аналізу загальних текстів на рівні речень найкраще себе показав класифікатор на базі дерев рішень.

Machine Learning Algorithm	F-measure (Weighted Avg.)	Accuracy
Decision Trees (J48)		0.865				0.865


Наша модель має наступні результати:

Machine Learning Algorithm	F-measure (Weighted Avg.)	Accuracy

Logistic Regression		0.851				0.851
