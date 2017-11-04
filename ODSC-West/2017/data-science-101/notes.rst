She thinks SciKitLearn docs are a good way for learning (or remembering)
data science concepts.

her philosophy: feel free to drop in different algorythms. 
When you find one that appears to work well, stop and read the paper.
"understand the algorithm before you go to production"

Training Set and Testing Set
============================
If it memorizes the training set, it can be perfect at predicting it.

Part of original data is held back, and not used in the training set.
Then use the trained model on the the held-back testing set.

Testing set: "out of sample"

My question: can you try this multiple times,
to maximize the value of of a limited set of data?

Answer: `cross_val_score`. Split data into, eg 3 pieces: train on 2/3, test on 1/3.
Three times.

`cross_val_score` "is not the most sophisticated way, SciKitLearn has better", 
but it is "really fast and 2 lines of code".
Use something "better" in production code.

`classification_report` gives a bit more info. "More canonical".

tranformations
==============
We might turn our strings into ints for efficiency. (But see one-hot encoding)
We probably turn them into

algorithms
==========
How powerful may be a tradeoff of understandability vs power.
(ie neural nets can predict really well, but how do I present it?)

Risk can make a difference is what you choose -
what kinds of errors do we have to avoid?

Neural nets are deliberatley left out of SciKitLearn
tensorflow, theonis

classification vs regression
============================
regression is continuous 
classification is non-ordered
but "in between" (perhaps ordinal, but not continous) you have to make a decision

logistical regression
---------------------
"really a classifier, not a regession"
coefficient * value for each column, sum columns together, to get a number between 0 and 1
Default the threshold is, less than .5 it's 0, .5 or greater it's 1.
But this can be changed.

assumes continuous linear relations

decision tree classifier 
-------------------------
Constructs a binary tree per feature. 
Does the left get enriched by 0s and the right by 1s?
But it can keep regressing down

eg, a maintenance cycle:

:: 
    age:   1 2 3 4 5
    value: 5 3 1 3 5

    <=1: good
    <=2: medium
    <=3: poor
    <=4: medium
    <=5: good

"interaction term" - decision trees handle two vars,
eg branches for different regions with different maintenance cycles.

You can tune how deep, how many examples in an internode before splitting.
"A weakness and a strength"

Can easily overfit.

random forest classifier
------------------------
Build lots of decision tree. 100?

Each tree has some weak spots. "take the majority vote"

One way it might avoid overfit is by having shorter trees.

* Fast
* "They are difficult"
* Her favorite

My questions: is this expensive? What parts arer random?

Optimizing
==========

One-hot encoding
----------------
If we assigned each string or category to an int,
it will look like an ordered value (especially to linear regression).

dummying can be hard, since you might have values present in the testing
set that are not present in the training set.
So dummy with an extra slot called "other".
And when running on the testing set, add logic to say "put in other"

One-hot encoding is a way of "dummying" data. 
instead of integer-encoding, turn each category into a booolean column.
1 US
2 Mexico
3 Canada
4 Us

transformed to

::
    id country_us country_mex country_can
    1  1          0           0
    2  0          1           0
    3  0          0           1
    4  1          0           0

"one-hot" only one colume will be T.

my questions:
Why not tell linear regression that these are, eg chars? or short strings?
BC scikitlearn is built on numpy, and sees everything as numbers. 
And yes, it is expensive.
"We are blowing up the data; next we're going to collapse it down".

Feature selection
-----------------

AKA Dimensionality Reduction

why?
++++
* overfitting
* slow
* not needed

SelectKBest
+++++++++++

orders features based on how well they predict, and choose the top K.

Doesn't necessarily improve decision tree or random forest much,
but it can make things more human-interpretable.

Pipeline
=========

a way of wrapping together a series of transformers,
and optionally an estimator.

(A SciKitLearn feature, not a data science concept.)

"transformers" vs "estimators"
-------------------------------
hot-one, dimensionality reduction change the data- transformers
estimator (classifiers, etc)

Parsing classification report
=============================
these 2 trade off:

* precision: what percentage of my predictions were correct?
* recall: what percentage of things were corectly predicted?
* f1-score is a harmonic mean of the 2 above
* support: what is this?

GridSearchCV
============

Say we want to test tuning several parameters in our pipeline. 
But tuning one variable independantly could alter output of others.

With `GridSearchCV`, we can test a matrix of all combos.

My questions: why does her code always show X_something y_something? 
(capitalization mismatch) - a convention?

Learning
========
udera? machine learning "very famous course"
She taught an online course.

Robustness (going beyond Jupyter)
=================================
