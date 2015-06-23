---
layout: post
title: Using Python and AI to predict types of wine
categories: [general, setup, demo]
keywords: "machine learning, artificial intelligence, python, sklearn, programming, engineering, tutorial"
tags: [machine learning, artificial intelligence, python, sklearn, programming, engineering, tutorial]
fullview: true
---

I've been working with AI/Machine Learning at [Jusbrasil](http://www.jusbrasil.com.br/) recently, and it's being pretty challenging due to the _huge_ amount of data that we have to deal with, so cleaning this data and making predictions and classifications in an acceptable time demands a nice AI architecture.

That said I can say that I'm extremely thankful for a few technologies that are helping me go through this challenge _(and the pain of cleaning this amount of data)_: Python, Scikit Learn, Pandas, and the whole stack that the Scikit Learn use, such as NumPy, SciPy, matplotlib and few others.

So, this inspired me to _spread the word_, so I'll be showing here a simple example of Machine Learning using Python, Pandas and Scikit Learn to predict, given a great amount of data/features about wines, if a wine is white or red.

**disclaimer:** _I'm assuming that you already have a small knowledge on the ideas of the machine learning and its mathematical aspects (although not necessary to implement the code that I'll show here), this is just a simple introduction to scikit learn and its power, so the example is pretty simple and straight forward, if you just want the code, here it is: [github link](https://gist.github.com/digorithm/ad742f9314f76e732888)_

## What's Pandas?

Pandas is an amazing library for data manipulation, it makes the process of dealing with data very easy and straight forward, we can work with CSV, JSON and plenty other formats without struggling to manipulate the data, [give it five minutes](https://signalvnoise.com/posts/3124-give-it-five-minutes) and skim their [docs](http://pandas.pydata.org/pandas-docs/dev/), it will definitely worth it!

## Fetching the data

Let's start fetching the data with Pandas, _(you can download the data [here](https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/))_ to do so, just import Pandas and read the CSV file, just like that:

<pre>
<code class="python hljs">
import pandas as pd
reds = pd.read_csv('winequality-red.csv', sep=';')
whites = pd.read_csv('winequality-white.csv', sep=';')
</code>
</pre>

We can see how the data is structured by doing a few commands:

<pre>
<code class="python hljs">
reds.values[0:6]
</code>
</pre>

As output, we have the first 5 rows of the red wine's data

<pre>
<code class="python hljs">
array([[7.4, 0.7, 0.0, 1.9, 0.076, 11.0, 34.0, 0.9978, 3.51, 0.56, 9.4, 5],
       [7.8, 0.88, 0.0, 2.6, 0.098, 25.0, 67.0, 0.9968, 3.2, 0.68, 9.8, 5],
       [7.8, 0.76, 0.04, 2.3, 0.092, 15.0, 54.0, 0.997, 3.26, 0.65, 9.8, 5],
       [11.2, 0.28, 0.56, 1.9, 0.075, 17.0, 60.0, 0.998, 3.16, 0.58, 9.8,6],
       [7.4, 0.7, 0.0, 1.9, 0.076, 11.0, 34.0, 0.9978, 3.51, 0.56, 9.4, 5],
       [7.4, 0.66, 0.0, 1.8, 0.075, 13.0, 40.0, 0.9978, 3.51, 0.56, 9.4, 5]], 
dtype=object)
</code>
</pre>

Or we just use a function from Pandas that describe very well our data

<pre>
<code class="python hljs">
reds.head()
</code>
</pre>

<pre>
<code class="python hljs">

<class 'pandas.core.frame.dataframe'="">Int64Index: 5 entries, 0 to 4
Data columns:
fixed acidity           5  non-null values
volatile acidity        5  non-null values
citric acid             5  non-null values
residual sugar          5  non-null values
chlorides               5  non-null values
free sulfur dioxide     5  non-null values
total sulfur dioxide    5  non-null values
density                 5  non-null values
pH                      5  non-null values
sulphates               5  non-null values
alcohol                 5  non-null values
quality                 5  non-null values
dtypes: float64(11), int64(1), object(1)</class>
</code>
</pre>

## Understanding our data

Matplotlib gives you many ways to plot our data into graphs so we can understand what is going on with the data so we can choose the best model/algorithm for the given scenario,

For instance, let's take a look at the relation between the red wines and its fixed acidity

<pre>
<code class="python hljs">
x = plt.subplots(figsize=(10, 5))
plt.plot(reds.index, reds.get("fixed acidity"), 'ro')
ax.set_title('Wines vs fixed acidity')
ax.set_xlabel('red wine index')
ax.set_ylabel('Fixed Acidity')
plt.show()
</code>
</pre>

![](/content/images/2015/06/mlproblem.png)

## Preparing the Data for classification

Now we're going to add a new feature/variable to our data, which is our target variable, the _Y_, that will be telling if the wine from our dataset is white or red

<pre>
<code class="python hljs">
reds['kind'] = 'red'
whites['kind'] = 'white'
</code>
</pre>

We need to get all of our feature into a vector called X, that will be set into our algorithm, right? And, we need to get all our targets (white or red) and set into a Y variable

<pre>
<code class="python hljs">
wines = reds.append(whites, ignore_index=True)
X = wines.ix[:, 0:-1]
y = wines.kind
</code>
</pre>

Notice that we're merging both datasets together, the one with the red wines and the one with the white wines, so we can send them together to the algorithm. Now we're going to binarize the labels 'white' and 'red', so the mathematical model can use it. It's pretty simple

<pre>
<code class="python hljs">
y = y.apply(lambda val: 0 if val == 'white' else 1)
</code>
</pre>


## The algorithm

Now that we have our data well structured and we do understand it, we can start looking for an algorithm to use. A good algorithm for our scenario is a simple **Logistic Regression**, that will return a model that we'll use to make our predictions/classification. The mathematical linear model that will use is the following:

$$
 
f(w) := \lambda\, R(w) + \frac1n \sum_{i=1}^n L(w;x_i,y_i) \label{eq:regPrimal}\

$$

In addition with the loss function defined by the logistic loss

$$
L(w;x,y) := \log(1+\exp( -y w^T x )) 
$$

The Scikit learn provides an awesome library with an amazingly ease of use, so that we don't have to implement the whole model from scratch. All we have to do is create a object from the model we want to use, understand how it works _(at least understand how to use its interface to do what we want)_. In this case, we will be using the _cross validation_ object, which is another discussion for another time, but in a few words, it will divide our dataset and test it against all parts of the divided dataset, this way we make sure that we're validating the quality of the result.

<pre>
<code class="python hljs">
clf = LogisticRegression()
scores = cross_val_score(clf, X, y, cv=5)
print scores.mean(), scores.std()
</code>
</pre>

And as the result we get:

<pre>
<code class="python hljs">
0.981376321334 0.00638795038332
</code>
</pre>

And that's the exactly the precision of the algorithm over the given dataset, **98% of precision**, which is quite good! Now, for example, we can save this trained classifier and use it for future classifications of incoming data about wines that need to be classified as white or red, to do so, we just call the method _clf.predict(X)_ where this X will be the new wine's data. simplicity at its best!
