---
title: "Neural Computation: An Intuitive Understanding of the Perceptron"  
categories: [machine-learning]
keywords: "machine learning, artificial intelligence, engineering, tutorial, sentiment analysis, nlp, python, scikit learn, sklearn"
tags: [machine learning, artificial intelligence, engineering, tutorial, text classification, NLP, Sentiment Analysis]
fullview: true
permalink: post/neural-computation-pt1
layout: post
---

Artificial Neurons is one of the most beautiful ways to simulate a biological behavior through computation, despite the fact that it's not very close to the level of details of a real neuron. But it captured the core of what a neuron is doing. 

And I find that trying to understand this mathematical method by first understanding the concept through a metaphor *(which, in this case, I think that the metaphor is our mathematical side, and not the biological one, the biological is just, you know, the real thing)* is really valuable to gain an intuition on this topic. 

<!--more-->

If you were about to make a decision, given many variables, how would you make this decision? It may not look obvious, due the fact that, on normal occasions, we're not thinking about our thinking process, but the truth is, we are collecting those variables that affect the future decision, and we're giving weights to them. Weights? how? Simple, some variables are more crucial than other while making decision, isn't it? the word *'crucial'*, in this case, means a huge weight on this variable, and it will strongly affect the final decision. 

Suppose a scenario: we're deciding whether we should go to beach or not. Let's put a few binary variables on table:

1. Are we in the mood to go to the beach?
2. Is it raining?
3. Our other friends are going?
4. Do we have money?

So, depending on those variables, it will be more likely that we will go to the beach... or not. Let's think about the variable *'is it raining?'*, if it's raining, we can say that it's a deal breaker, so we can conclude that this variable has more weights than the other. The fact is that we already trained those weights inside our brain, long time ago. 

That's what an Artificial Neuron try to do, the AN try to learn the weights of things so it can make decisions. So, drawing back to the mathematical and computational aspect of it, the perceptron is a single unit that will receive external inputs*(variables)*, it will have a weight for each input, it will do some computation, send this result to a decision function, and, finally, output the final decision. 

Those inputs are the variables we're talking before, and like our process to make a decision, the Perceptron will weight each input to make a decision. 

Now, how the Perceptron knows how the weights should be for each input? Well... it doesn't. At least, not from the beginning of the learning process, just like you and me, when trying to learn something new. 

That's why this case is a case of supervised learning, what the Perceptron will do is: start with a random small weights, take one previously trained example *(e.g: (1) yes to mood to beach, (2) not raining, (3) yes to friends going to beach and (4) yes to money, the output: 1 - yes, we shall go to the beach)* do some computation taking in consideration the random weights we defined before, send it to a decision function, output something *(1 - yes, 0 - no)* and check if this output is equal to the expected *(we're using a trained example, remember?)*, of course that at the first try, it won't be equal. So the Perceptron calculate the error rate and update its weights... after this, guess what? it repeat the process of trying to predict, but now, with the updated weights, after a few tries, the error rate tends to reduce and it starts predicting correctly. So it learned to predict a behavior, by training with previously trained examples.

So, a nice learning behavior would be something like this:

<div class="imgcenter">
	<img src="/content/images/images/perceptron_0.1.gif"/>
</div>

Which means, at every iteration *(we call it epochs)*, the error rate tend to decrease and, as consequence, the Perceptron starts to predict correctly!

As you may have noted, I omitted a few details of the process so you could see the whole picture of the process: we take an example, we practice on it, we see what we missed, we try again, we learn. That's the core process. 

Now, to the details:

### The computation

The first step that the Perceptron does it's a computation that I referred as *"some computation"*, this is a simple computation, it's just a simple Linear Combination of the feature vector *(the variables)* and the weight vector, which is just the sum of the products between feature/input and its respective weight:


$$
x_{1}w_{1}+x_{2}w_{2}+x_{3}w_{3}+ \cdots +x_{n}w_{n}
$$

&nbsp;

### The Decision Function
So, after the Linear Combination, we send the result of it to a decision function, which will, somehow, based on some threshold *(or without a threshold as we're going to see)*, give us the output, that will be checked with the correct output, in case if it's correct, fine, keep the weights like this, otherwise, it will calculate the error rate, adjust the weights and restart the process.

&nbsp;

#### Binary output with threshold 

This technique is very simple, after the linear combination, if the output is bigger than some threshold, it outputs 1 and we say that the neuron was activated, otherwise, output 0.

$$
\begin{equation}
    X=
    \begin{cases}
      1, & \text{if}\ Linear\,Combination>0 \\
      0, & \text{otherwise}
    \end{cases}
  \end{equation}
$$

&nbsp;

#### Using sigmoid function

Now, that a interesting one, it won't use a threshold anymore, we'll send the output of the Linear Combination to a sigmoid function

$$
S(\vec{w}\vec{x}) = \dfrac{1}{1+e^{-\vec{w}\vec{x}}}
$$

which will, then, smooth the output, making it in the range of 0 and 1. Which is the most used in many Machine Learning Algorithms.

<div class="imgcenter">
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Logistic-curve.svg/600px-Logistic-curve.svg.png"/>
</div>


### Adjusting the weights and learning

After the perceptron fails to predict correctly, it's time to adjust the weight, using some rule, the classic perceptron has 2 rules:

&nbsp;

#### Perceptron Learning Rule
This is very simple, when the perceptron has the incorrect output, it update its weights following this:

$$
w_{i} = w_{i} + \eta (t_{i} - o_{i})x_{i}
$$

Which is simply multiplying a learning rate *(usually 0.001)* by the difference between the correct output and the wrong output and then multiplying it by its original input, after this, we add this value to the previous weight and then we have the value of the new weight. 
It's fair simple, isn't it? The problem arises when the data isn't linearly separable, like this:


<div class="imgcenter">
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/DBSCAN-density-data.svg/2000px-DBSCAN-density-data.svg.png" width="400" />
</div>

&nbsp;

With this scenario, the result just won't converge. So we adopt another rule!

&nbsp;

#### Delta Rule

Now we must find a way to have a non-linear output, and what's the best for this if not the classic Gradient Descent algorithm? GD will simply search through hypothesis spaces and try to minimize the cost function, I wrote about this <a href="http://rodrigoaraujo.me/general/setup/demo/2015/01/09/Making-Your-Machine-Think-Learn-And-Predict--Gradient-Descent-Algorithm-in-Java.html">here</a>. 


<div class="imgcenter">
	<img src="http://www.yaldex.com/game-development/FILES/17fig06.gif"/>
</div>

So, it's just a technique to solve our previous problem, but still, the weights will be updated *(now, using the GD)* and then the process start over!


&nbsp;

### Code of a Perceptron

Now, here's a code of a Perceptron that will behave like a simple Boolean function. I didn't use the Delta Dule (Gradient Descent) to optimize the weights, as this problem (binary Boolean functions) is linearly separable.

{% highlight python %}

  import math
  from numpy import random, array
  from random import choice
  import matplotlib.pyplot as plt

  class Perceptron():
      
      """
      this is the thereshold to activate the unit 
      """
      def activation(self,x):
          return 1/(1 + math.exp(-x))

      def linear_combination(self, X):
          total = 0
          for i in xrange(len(self.weights)):
              total += self.weights[i] * X[i]
          return total
      
      def unit_output(self, X):
          return self.activation(self.linear_combination(X))

      def train(self, training_data, epochs):
          
          self.learning_rate = 0.5
          self.errors = []

          # initializing weights
          self.weights = random.rand(len(training_data[1][0]))
          
          for i in xrange(epochs):
              X, y = choice(training_data)
              # > calculate output with current weight
              output = self.unit_output(X)
              # > calculate error rate (t - o)
              error_rate = y - output
              self.errors.append(error_rate)
              print 'the X: ', X
              print 'output: ',output
              print 'correct target: ', y
              # > apply learning rule which will update weights
              self.weights += self.learning_rate * error_rate * X

      def predict(self,X):
          y = self.unit_output(X)
          return y
                  

          training_data = [
                  (array([0,0,1]), 0),
                  (array([0,1,1]), 0),
                  (array([1,0,1]), 0),
                  (array([1,1,1]), 1),
                  
          ]

  p = Perceptron()

  p.train(training_data, 2000)
  print p.predict([1,0,1])
  plt.plot(p.errors)
  plt.ylabel('errors')
  plt.xlabel('epochs')
  plt.show()

{% endhighlight %}

### Conclusion and next steps

With this we conclude what a simple single Perceptron is doing! Of course there are many improvements on it and we can connect many of these Artificial Neurons in many layers... that's what is called Artificial Neural Network, I'll be writing about it soon!

