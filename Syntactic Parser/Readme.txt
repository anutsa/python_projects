This is a dependency parser for Ukrainian language.

Logistic Regression Classifier
             precision    recall  f1-score   support

       left       0.84      0.86      0.85      7352
     reduce       0.81      0.73      0.77      8370
      right       0.72      0.78      0.75      7182
      shift       0.86      0.86      0.86      7757

avg / total       0.81      0.81      0.81     30661

Total: 14939
Correctly defined: 10235
UAS: 0.69

SVM Classifier
             precision    recall  f1-score   support

       left       0.84      0.86      0.85      7352
     reduce       0.81      0.72      0.76      8370
      right       0.71      0.78      0.75      7182
      shift       0.85      0.86      0.85      7757

avg / total       0.81      0.80      0.80     30661


K-Nearest Neighbours Classifier
             precision    recall  f1-score   support

       left       0.87      0.90      0.88      7352
     reduce       0.58      0.79      0.67      8370
      right       0.77      0.67      0.72      7182
      shift       0.91      0.63      0.75      7757

avg / total       0.78      0.75      0.75     30661


SVM with the following parameters gives same results as Logicstic Regression:

svc = OneVsRestClassifier(svm.LinearSVC(random_state=7, C=0.1, dual=False))


Model improvement:

lrc = LogisticRegression(random_state=0, multi_class='multinomial', solver='saga', C=0.2, n_jobs=-1, max_iter=1500)


             precision    recall  f1-score   support

       left       0.84      0.87      0.86      7352
     reduce       0.82      0.73      0.77      8370
      right       0.74      0.80      0.77      7182
      shift       0.87      0.88      0.87      7757

avg / total       0.82      0.82      0.82     30661

Total: 14939
Correctly defined: 10487
UAS: 0.7

F-score improved 1%. UAS - 1% improved. 252 more actions were predicted correctly.


Dependency tree for new sentences:

def get_tree(sent):
    result = []
    words = tokenize_uk.tokenize_words(sent)
    for i in range(0, len(words)):
        p = morph.parse(words[i])[0]
        feats = OrderedDict()
        if p.tag.animacy:
            feats["Animacy"] = p.tag.animacy.title()
        if p.tag.case:
            feats["Case"] = p.tag.case[0:3].title()
        if p.tag.gender:
            feats["Gender"] = p.tag.gender.title()
        if p.tag.number:
            feats["Number"] = p.tag.number.title()                        
        result.append(OrderedDict([('id', i+1), ('form', words[i]), ('lemma', p.normal_form), ('upostag', normalize_pos(p)),
                                   ('xpostag', None), ('feats', feats), ('head', None), ('deprel', None),
                                   ('deps', None), ('misc', OrderedDict())]))
    return result

['Походження', 'міста', 'Львів', 'оповито', 'пеленою', 'великих', 'таємниць', '.']
[('міста', '<---', 'Походження'), 
('оповито', '<---', 'ROOT'), 
('пеленою', '<---', 'оповито'), 
('великих', '<---', 'таємниць'), 
('таємниць', '<---', 'пеленою')]


['Як', 'видно', 'з', 'обговорення', ',', "львів'яни", 'не', 'цікавляться', 'поглибленням', 'історії', 'свого', 'міста', 'на', 'тисячу', 'років', '!']
[('Як', '<---', 'видно'), 
('з', '<---', 'обговорення'), 
('не', '<---', 'цікавляться'), 
(',', '<---', 'цікавляться'), 
('цікавляться', '<---', 'ROOT'), 
('поглибленням', '<---', 'цікавляться'), 
('історії', '<---', 'поглибленням'), 
('міста', '<---', 'свого'), 
('на', '<---', 'тисячу'), 
('тисячу', '<---', 'цікавляться'), 
('років', '<---', 'цікавляться')]


['За', 'два', 'тижні', 'користування', 'можу', 'впевнено', 'сказати', ',', 'що', 'телефоном', 'задоволена', '.']
[('можу', '<---', 'користування'), 
('впевнено', '<---', 'можу'), 
('що', '<---', 'телефоном'), 
(',', '<---', 'телефоном'), 
('телефоном', '<---', 'сказати'), 
('задоволена', '<---', 'сказати')]


['Сподіваємось', ',', 'ви', 'знайдете', 'ті', 'подіїї', ',', 'які', 'зацікавлять', 'саме', 'вас', '.']
[('ви', '<---', 'знайдете'), 
(',', '<---', 'знайдете'), 
('знайдете', '<---', 'ROOT'), 
('ті', '<---', 'знайдете'), 
('подіїї', '<---', 'ті'), 
(',', '<---', 'зацікавлять'), 
('зацікавлять', '<---', 'ROOT'), 
('саме', '<---', 'зацікавлять'), 
('саме', '<---', 'вас'), 
('вас', '<---', 'зацікавлять')]


['Все', ',', 'що', 'література', 'повинна', 'виховувати', 'у', 'дітей', '–', 'це', 'естетичний', 'смак', ',', 'і', 'більше', 'нічого', '.']
[('повинна', '<---', 'виховувати'), 
('що', '<---', 'виховувати'), 
(',', '<---', 'виховувати'), 
('Все', '<---', 'виховувати'), 
('виховувати', '<---', 'ROOT'), 
('у', '<---', 'дітей'), 
('дітей', '<---', 'виховувати'), 
('естетичний', '<---', 'смак'), 
('–', '<---', 'смак'), 
('смак', '<---', 'ROOT'), 
('більше', '<---', 'нічого'), 
('і', '<---', 'нічого'), 
(',', '<---', 'нічого'), 
('нічого', '<---', 'ROOT')]