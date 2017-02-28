---
layout: default
title: "A Generic Architecture for Text Classification with Machine Learning"  
categories: [general, setup, demo]
keywords: "machine learning, artificial intelligence, engineering, tips, architecture, text classification"
tags: [machine learning, artificial intelligence, engineering, tips, architecture, text classification]
fullview: true
permalink: post/generic-architecture-text-classification
---

One of the most commons tasks in Machine Learning is text classification, which is simply teaching your machine how to read and interpret a text and predict what kind of text it is. 

The purpose of this essay is to talk about a simple and generic enough Architecture to a supervised learning text classification. The interesting point of this Architecture is that you can use it as a basic/initial model for many classifications tasks. 

## Supervised Learning

If you're already familiar with this concept, just jump this step, but I feel it's important to beginners to know. 

Supervised Learning is when you have to first train your model with already existing labeled dataset, just like teaching a kid how to differentiate between a car and a motorcycle, you have to expose its differences, similarities and such. Whereas unsupervised learning is about learning and predicting without a pre-labeled dataset.

## Starting to sketch the Architecture

With the dataset in hands, we start to think about how is going to be our architecture to achieve the given goal, we can resume the steps in:

1. Cleaning the dataset
2. Partitioning the dataset
3. Feature Engineering
4. Chosing the right Algorithms, Mathematical Models and Methods
5. Wrapping everything up

## Cleaning the dataset

Cleaning the dataset is a crucial initial step in Machine Learning, many Toy Datasets don't need to be cleaned, because it's already clean, peer-reviewed and published in a way you can use it exactly to work on the learning algorithms.

The problem is: 

#### The real world is full of painful and noisy datasets

If there's one thing I learned while working with Machine Learning is, there's no such thing as shiny and perfect dataset in the real world, so we have to deal with this beforehand. Situations where there are many empty fields, wrong and non-homogeneous formats, broken characters, is very common. I won't talk about such techniques now, but I will write something about it in another post.

## Partitioning the Dataset

We always need to partition the dataset in, at least, 2 partitions: the training dataset and the test/validation dataset. Why?

Suppose we fed the learning algorithm with a training data X and it already known the output Y (because it's a training data pair (X,Y)), which is, for given text X, Y is its classification, the algorithm will learn it. 


Great, the algorithm learned this. But now, we're going to validate the learning, so we use the same data X, I mean, we pass X to the model and ask what's its classification... 

Do you see the problem here?

Of course the algorithm will output Y, the same Y we passed to its training. So, if we pass the complete dataset D in the training phase, then we validate the model using the SAME D dataset, we will be steping onto this very same situation. It's like cheating, it's like we point to a car and say "this is a car", then, at the same time and with the same car, we ask to the kid "is this a car?", it will be highly probable that the kid will answer correctly, though it may not learned correctly. What we must do is, point to a car and say "this is a car", and then, point to a different car and ask "is this a car?".

Stepping out of the metaphor, we must check if the machine learned correctly by using a diferent portion of the dataset.

So, at the end of this step, we'll have the training dataset and the test dataset, both are subset of the same initial dataset. 

With Python's Scikit Learn you can do this easily using <a href="http://scikit-learn.org/stable/modules/generated/sklearn.cross_validation.train_test_split.html" target="_blank">Train Test Split</a> <em>(read the docs, it's very simple to use it.)</em>.

&nbsp;

<div class="imgcenter">
<img src="/content/images/images/ml1.png"/>
</div>
&nbsp;

##Feature Engineering

This is one of the most important steps when doing Machine Learning. Briefly, Features are the data that the learning algorithm will use as the "X", which will be used to compute and understand the patterns. A simpler example would be a non-text classification or a regression, e.g: house pricing predictions, where our Features could be: number of rooms, size in squared meters, location and more. As you can see, these feature will describe the patterns of each data point and will affect how the house will be priced. 

The crucial point is, features can vary, you can identify new features that can level up your model's predictions in huge proportions, a simple example would be, in a text classification you count the frequency of each word, this is one feature... you feed your learning algorithm with it... and the result isn't enough: 50% of precision. But then you see that, in this case, the length of the whole data point (the text) plays a huge role determining a pattern for each class. Now that you **identified** this feature, you do something to **extract** that feature and then add this new feature and feed your learning algorithm with it, then, the result: 95% of precision. 

Ok, so you know the importance of the feature engineering phase, now it's time to understand the most common technique to extract feature in texts: **TF-IDF**

&nbsp;

#### TF-IDF

Though the most intuitive way to look for patterns in texts is to count each word in the text (and use it as a Feature), it may not be the best way to do it. A few reasons for it is, larger texts will have higher averages than the shorter texts, these discrepancies can hurt the learning algorithm, and this Feature **doesn't say much about the importance of the words**, which is very important to find patterns.

So, instead of computing the occurrence, it's better to compute the importance of the words. to accomplish this we can use 2 statistic's techniques:

**TF** - which is basically the raw frequency of the word given the document, raw frequency of t by f(t,d), then the simple tf scheme is tf(t,d) = f(t,d).

**IDF** - Inverse Document Frequency, which is a technique to give emphasis to words that *don't appear with high frequency*, because these are the words that can differentiate the texts, which means, in this case, these are the most important words, so we inverse its frequencies. So, an example, if the word *"the"* happens to appear very often in a text, it will weight **less**, because it's a common word, thus, don't cause very impact when fiding patterns to differentiate the texts.

So the whole TF-IDF can be computed by

![](https://upload.wikimedia.org/math/e/8/1/e81492e44713270fd230d821ccebd100.png)

Scikit Learn gives us a great API to use the TF-IDF method, it's really simple. 


        from sklearn.feature_extraction.text import TfidfVectorizer

        vectorizer = TfidfVectorizer()
        vectorized_x = vectorizer.fit_transform(X)

Which will do the TF-IDF on the X data, and then vectorize this data, in other words, transform the whole thing into an array of inverse frequencies.
So, extracting those features, **this** will be our training and test data, that will feed the algorithm (along with the Y, which is the output, the class of each training data).

Re-thinking our Architecture, now we have:

&nbsp;

<div class="imgcenter">
<img src="/content/images/images/ml2.svg"/>
</div>
&nbsp;


Remembering that X is the set of features (e.g: vector with the TF-IDF of each data point) and Y is the output, which is, the labels/class (e.g: Spam or not-spam).

## Chosing the right Algorithms, Mathematical Models and Methods

With the data prepared, features selected and extracted, it's time to feed the algorithm with this data, this topic *per se* could go pages and pages, as learning algorithms is such a huge fields, with many publications and ideas to solve every kind of problem.

To not extend it very much, and as the purpose of this essay is to discuss the architecture, I'll use a few commons algorithms, such as Logistic Regression, Decision Trees, SVM and Neural Networks.

So, at this point, we'll treat the learning algorithm as a black-box algorithm, where it:

1. receive a training data X which is a vector of features, and training data Y, which is the label/class/output (we can binarize it, such as spam=1, ham=0)
2. return a model, where given an text X, can output its predicted class Y.

After generating this model, we will test it with our test dataset and check its performance, if it can predict correctly *(the test dataset has output values (Y) so we can check them)*, it is ready to predict new data *(data completely outside our initial dataset)*, so we say that the machine learned the task. 

Our architecure now:

&nbsp;

<div class="imgcenter">
<img src="/content/images/images/ml3.svg" height="800"/>
</div>
&nbsp;


## Wrapping everything up

With this architecture, we should be able to do most of the simple text classification tasks, as the main flow is: get data, clean data, identify and extract features, train your algorithm/mathematical model of choice, validate it and then, use the generated model to do the estimations.

Of course there are many improvements that can be made on this architecture and many, many, many lower level details, but you can see this architecture as a *"boilerplate code"* to get you started with the machine learning engineering task.


A few tips:

Learn the underlying mathematical models of the most commons learning algorithms, this will teach you the trade offs of each one, so you can apply the correct algorithms to the given dataset and scenario. For example, SVM can be good for unbalanced dataset, but why? You gotta know this. Neural Networks can be slow to be trained, but, if training time is not critical, it's okay to use Neural Networks.

Master the skills to clean data, if using Python, learn to use Pandas. This will be an extremely important skill.

Master the skills to understand data, this will be crucial to make you see what algorithm to use, you can't make something learn if you don't know about what you are teaching.

