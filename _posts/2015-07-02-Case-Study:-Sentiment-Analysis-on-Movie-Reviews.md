---
layout: default
title: "Case Study: Sentiment Analysis On Movie Reviews"  
categories: [machine-learning, case-study]
keywords: "machine learning, artificial intelligence, engineering, tutorial, sentiment analysis, nlp, python, scikit learn, sklearn"
tags: [machine learning, artificial intelligence, engineering, tutorial, text classification, NLP, Sentiment Analysis]
fullview: true
permalink: post/case-study-sentiment-analysis-movie-reviews
---

Well, this case was a fun one, sentiment analysis is a sub type of NLP *(natural language processing)* in which you have to train a model to analyze a text, and classify it was a negative sentiment or a positive sentiment. In this case, I wanted to apply it on movies reviews, to see if a review is a negative one or a positive one. So, basically, teaching this kind of sentiment to the machine.

There's a lot of challenges in doing this kind of NLP, experience has told me that when the text is big, it is usually hard to predict things about it... and most of the times, movie reviews are big chunk of text.

<!--more-->

That's the case where I needed to learn and use a few *extremely* useful things:

1. Pipeline
2. Grid Search

These two techniques made total difference in my results. Let's go through the concept of these two:

### Pipeline

In a <a href="http://rodrigoaraujo.me/general/setup/demo/2015/06/30/A-Generic-Architecture-for-Text-Classification.html" target="_blank">previous post</a> I talked about a generic Architecture or Flow to achieve basic text classification tasks, so, Pipeline is a way to automate all those tasks, so, instead of explicitly and separately select features, extract features, train the learning algorithms, you can do all of these things inside the pipeline, and, the amazing scikit learn API gives you an awesome Pipeline interface, pretty similar to a basic estimator's interface, so you can call methods such as *fit, fit_transform, predict*, etc...

So you can do even more inside a pipeline, such as new methods for feature extraction/selection, FeatureUnion, use other estimators to select better features for your estimator *(do you even neural netception? LOL).*

Here's an example of a simple pipeline that does PCA to reduce the dimension of the features and creates a SVC:

        from sklearn.pipeline import Pipeline
        from sklearn.svm import SVC
        from sklearn.decomposition import PCA
        estimators = [('reduce_dim', PCA()), ('svm', SVC())]
        clf = Pipeline(estimators)

### Grid Search

This was the answer for many hours of pure pain tweaking gazillions of parameters to improve the algorithm's performance. Grid Search is a way to automate parameters changes, so it can run many times with many different parameters... and choose the best for you, magical, right?

And, once again, the Grid Search object has an interface similar to the basic estimator's interface, so you can call all the same methods. But, when you create the object grid search, you gotta pass a estimator as parameter... and here's when things get interesting:

You can pass a pipeline to the grid search. **so much win!**

<div class="imgcenter">
	<img src="http://cdn.meme.am/instances2/500x/581044.jpg"/>
</div>

&nbsp;

But, the truth is: this operation is *extremely* expensive, and it's recommended to use it only on the few first tries, where you don't know exactly the best parameters values for the task.

Another interesting thing is, after the model inside the grid search is trained, methods like *predict* are computed using the best features. If we look inside the SKlearn's grid search's code, we can see how easily and elegant it's done:

```python
[...]
scores = list()
grid_scores = list()
for grid_start in range(0, n_fits, n_folds):
    n_test_samples = 0
    score = 0
    all_scores = []
    for this_score, this_n_test_samples, _, parameters in \
            out[grid_start:grid_start + n_folds]:
        all_scores.append(this_score)
        if self.iid:
            this_score *= this_n_test_samples
            n_test_samples += this_n_test_samples
        score += this_score
    if self.iid:
        score /= float(n_test_samples)
    else:
        score /= float(n_folds)
    scores.append((score, parameters))
    # TODO: shall we also store the test_fold_sizes?
    grid_scores.append(_CVScoreTuple(
        parameters,
        score,
        np.array(all_scores)))
# Store the computed scores
self.grid_scores_ = grid_scores

# Find the best parameters by comparing on the mean validation score:
# note that `sorted` is deterministic in the way it breaks ties
best = sorted(grid_scores, key=lambda x: x.mean_validation_score,
              reverse=True)[0]
self.best_params_ = best.parameters
self.best_score_ = best.mean_validation_score
```

As you can see, in the end it keeps the best parameters and scores, so it can be used when it needs to predict or output its *predict_proba* or *decision_function*

### Correctly mixing Pipeline, Grid Search and Cross Validation correctly: the hidden wizardry

It's tricky to mix all these things up, the most dangerous mistake that everybody does sometime is: **using the same dataset to the grid search AND the cross validation**

So, here's a nice technique to avoid this: Split the initial dataset into Development Dataset, which will be used to train the algorithm and to execute the grid search, and the Validation Dataset, which will be passed to the cross_val_score and internally splitted again to avoid bias (CV parameter can configure this).

So, in the end, you train the grid search with a subset of the dataset, and validate it with another subset, avoiding many kind of undesired effects.

### The Movie Review Problem

Ok, so now let's head to the real problem. The dataset can be downloaded with:

	wget http://www.cs.cornell.edu/people/pabo/movie-review-data/review_polarity.tar.gz
	tar xzf review_polarity.tar.gz

What I did to solve it was: I used grid search to search a few parameters, such as *TfidfVectorizer's max_features*, that in the end I kept with max_features=10000, also its ngram_range, searching between (1,1) and (1,2) and *the LinearSVC()'s C parameter*.

I used a few other algorithms, but I found the best performance with LinearSVC, though I did not try all the other possibilities *(I could create many pipelines to test many algorithms, but my computer would be unusable during many hours, probably, which I couldn't afford)*.

Why Linear SVC?

The Linear SVC is very similar to the SVC object but using the *kernel = "linear"*. LinearSVC is a type of *Support Vector Machines*, it's known that SVMs are effective in **high dimensional spaces**, which is our case here. Also, it can give weights to classes, helping to go through unbalanced datasets
![](http://scikit-learn.org/stable/_images/plot_separating_hyperplane_unbalanced_0011.png)

And, basically, what the SVMs does is create a hyper-plane (or a set of it) in a high or even infinite dimensional space. And it solves the primal problem:

<div class="imgcenter">
<img src="http://scikit-learn.org/stable/_images/math/396704acdf11cc18d2d02b32275c0ee42d76b95e.png"/>
</div>
&nbsp;

Where Xi are the training vectors and Y is a vector in {1,-1}^n.

So, here's the code where I built the pipeline, the gridsearch, use a subset of the dataset to develop the grid search and another subset to validate using the *cross_val_scores*, then, I call *predict* to get the vector of predictions to build a report *(or, with this, I can start to build a ROC curve, confusion matrix and other statistics debug tools)*

	from sklearn.datasets import load_files
	from sklearn.feature_extraction.text import TfidfVectorizer
	from sklearn.grid_search import GridSearchCV
	from sklearn.pipeline import make_pipeline
	from sklearn.svm import LinearSVC
	from sklearn.cross_validation import train_test_split
	from sklearn.cross_validation import cross_val_score
	from sklearn.metrics import classification_report

	data = load_files('../txt_sentoken/')

	X_train, X_test, y_train, y_test = train_test_split(data.data, data.target, test_size=0.5, random_state=0)

	pipeline = make_pipeline(TfidfVectorizer(sublinear_tf=True, max_features=10000), LinearSVC())

	parameters = {
	        'tfidfvectorizer__ngram_range': [(1,1), (1,2)],
	        'linearsvc__C':(.01,.1,1),
	        }

	grid_search = GridSearchCV(pipeline, parameters, n_jobs=-1) 

	grid_search.fit(X_train, y_train)

	print("Best score: %0.3f" % grid_search.best_score_)
	print("Best parameters iset:")
	best_parameters = grid_search.best_estimator_.get_params()
	for param_name in sorted(parameters.keys()):
	    print("\t%s: %r" % (param_name, best_parameters[param_name]))

	print "The model was trained on the full development set"
	print "the scores are going to be computed with the evaluation set"
	scores = cross_val_score(grid_search, X_test, y_test, cv=5)

	print scores.mean(), scores.std()

	y_pred = grid_search.predict(X_test)

	print classification_report(y_test, y_pred)


and the output:

	Best score: 0.849

	Best parameters iset:
		linearsvc__C: 1
		tfidfvectorizer__ngram_range: (1, 2)

	The model was trained on the full development set
	the scores are going to be computed with the evaluation set
	0.865933623341 0.0281523948704

	Classification report:
	             precision    recall  f1-score   support

	          0       0.88      0.86      0.87       496
	          1       0.87      0.88      0.87       504

	avg / total       0.87      0.87      0.87      1000

So, around 86%~87% of precision/f1-score, not *that* bad.

### Further Improvements

There are many things I could to do elevate this performance, but due the lack of time to play with this dataset, those improvements will be in a next post.

Possible improvements could be: find and extracting new features, to do this, we gotta understand better this data, extract more information about it. Trying new algorithms and techniques such as Ensemble, Classifiers Combination or Fusion. Stemming and Stop Words could improve it too. If you solved this problem and got a better result, share it with me, I'm very curious about how to improve this!
