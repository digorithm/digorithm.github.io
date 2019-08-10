---
layout: post
title: Gradient Descent Algorithm in Java
categories: [machine-learning, java, algorithm]
keywords: "machine learning, artificial intelligence, java, programming, engineering, tutorial"
tags: [machine learning, artificial intelligence, java, programming, engineering, tutorial]
fullview: true
---

### How can we make a machine learn from data?

Then, how can we make the machine predicts things based on that learned data? Those are the question answered by one of the most classic Machine Learning Algorithms, the **Gradient Descent Algorithm**, from a Mathematical-Statistical side it’s called **Univariate Linear Regression**.

This is one of the tools of the Machine Learning toolbox, and what it tries to do is to model a relationship between a scalar dependent variable Y and a explanatory variable X.

### In Layman’s term…

Let’s suppose you have a few points distributed in a Graph, so you already know that in a point A you have a well defined X and Y, which means, if you input X, your output will be Y, and in a point B you have a well defined X’ and Y’ as well. But, thing is, if a point emerge between A and B, and you only have the X… what will be the Y _(the output)_?

<!--more-->

What this algorithm does is: **It tries to predict this Y value, based on the previous data**! Amazing, right?

At the end of the execution, you’ll have a full trend line that you can use to predict values! just like the image below.

![](/content/images/2015/06/linearRegression.png)

### The Theory Behind It

I’ll cover a few theories about this algorithm here, but, it won’t be complete, as this demand a great coverage of mathematical material that if I would write it all here, It would be a _long, long_, **very** _long_ post. So I’m assuming that you’re already familiar with Calculus _(Sums, Partial Derivatives)_, Statistics and Discrete Mathematics.

So, our goal here is to **fit the best straight line in our initial data**, right?

Thus, we need something to represent this straight line, which will be our hypothesis function:

<div id="math"> 
$ h_{\Theta}(x) = \Theta_{0} + \Theta_{1}x $ 
</div>

Where this \\( \Theta\_{0} \\) and \\( \\Theta\_{1} \\) are the parameters of the function, and **finding the best parameters for this function is what is going to give us the correct straight line to plot on our data**.

Here’s an example of what we're trying to do, which is, fit the best straight line in the data:

![](/content/images/2015/06/Linear-regression.svg)

So what we want to do is to find a \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) so our Hypothesis outputs can be very close to the real Y output.

Formally we want:

<div id="math">$ \underset{\Theta_{0}\Theta_{1}}{min}(h_{\Theta}(x) - y)^2 $</div>

So we want **minimize** \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) so the difference between the **Hypothesis** and the **real output** is minimal.

**But we want it for every point in our data**, which is \\(x(i)\\) _(which is the i-th x of our data)_, so we want the **sum of this average**, which is, formally:

<div id="math"> 
$ \underset{\Theta_{0}\Theta_{1}}{min}\,\frac{1}{2m} \sum_{i=1}^{m} (h_{0}(x^{(i)}) - y^{(i)})^2 $
</div>
<br/>

And we’re going to call this function **Cost Function**, with the following notation:

<div id="math"> 
$ J(\Theta_{0}, \Theta_{1}) = \,\frac{1}{2m} \sum_{i=1}^{m} (h_{0}(x^{(i)}) - y^{(i)})^2 $
</div>
<br/>

So, our goals is to **minimize this cost function**:

<div id="math"> 
$ \underset{\Theta_{0}\Theta_{1}}{min}\,\, J(\Theta_{0}, \Theta_{1}) $
</div>
<br/>

This cost function is also called [Square Error Function](http://en.wikipedia.org/wiki/Mean_squared_error).

### Now, the gradient descent algorithm

With our cost function built, we need to “keep” finding values for \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) so we can reach our ideal trend line. So, basically:

1\. We start with some \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\)

2\. keep changing \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) to reduce our cost function \\( J(\\Theta\_{0}, \\Theta\_{1}) \\), until we find the minimum.

That’s quite simple and intuitive, right? There’s a lot of intuitive explanation and more visual examples of what this algorithm is doing in the [machine learning course taught by Andrews Ng (From Stanford)](https://class.coursera.org/ml-007/).

What this algorithm will be doing is: partially derive our cost function for \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) simultaneously, so we can find the minimum value for them, with every iteration updating our \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) with their new value!

>_- Meh, talk is cheap show me the math!_

Formally, the algorithm is:

<div id="math">
$ repeat \, until \, convergence\,\{ \\ \,\,\,\,\,\,\,\,\,\,\,\, \Theta_{j} := \Theta_{j} - \alpha \frac{\partial }{\partial \Theta_{j}} \,\, J(\Theta_{0}, \Theta_{1}) \\ \} \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\, $
</div>
<br/>

_**Make sure that this will run for j = 0 and j = 1.**_

But, pay attention, this is the generic version, the \\( \\Theta\_{j} \\) represent both \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\).

What the algorithm is saying is that we’ll be doing this procedure to \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) at the same time! So, we can put it in this way:

<div id="math">
$ repeat \, until \, convergence\,\{ \\ \Theta_{0} := \Theta_{0} - \alpha \frac{\partial }{\partial \Theta_{0}} \,\, J(\Theta_{0}, \Theta_{1}) \\ \Theta_{1} := \Theta_{1} - \alpha \frac{\partial }{\partial \Theta_{1}} \,\, J(\Theta_{0}, \Theta_{1}) \\ \} \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\ $
</div>
<br/>

Also, we can expand the **Cost Function** that is being derived, doing this, it will be exactly what I’ll be putting into code soon, so, our **final algorithm** is:

<div id="math">
$ repeat \, until \, convergence\,\{ \\ \Theta_{0} := \Theta_{0} - \alpha \frac{\partial }{\partial \Theta_{0}} \,\, \Big (\frac{1}{2m} \sum_{i=1}^{m} (h_{\Theta}(x^{(i)}) - y^{(i)})^2 \Big) \\ \Theta_{1} := \Theta_{1} - \alpha \frac{\partial }{\partial \Theta_{1}} \,\, \Big (\frac{1}{2m} \sum_{i=1}^{m} (h_{\Theta}(x^{(i)}) - y^{(i)})^2 \Big) \\ \}
\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\,\ $
</div>
<br/>

**Now it’s time to code all of it!**

First things first, we’ll be using [Google’s JMathPlot](http://code.google.com/p/jmathplot/) to plot graphs using Java and Swing to use its JFrame, we shall start with our class to represent our **Initial Data**, which will be the **Training Set**.
<pre>
<code class="java hljs">
    import javax.swing.JFrame;
    import org.math.plot.*;

    public class InitialData{
            public static double[] x = {2, 4, 6, 8};
            public static double[] y = {2, 5, 5, 8};

            public void plotData(){
                    Plot2DPanel plot = new Plot2DPanel();
                    plot.addScatterPlot("X-Y", this.x, this.y);
                    JFrame frame = new JFrame("Original X-Y Data");
                    frame.setContentPane(plot);
                    frame.setSize(600, 600);
                    frame.setVisible(true);
            }
    }
</code>
</pre>

<br/>
To try out the data plotting, create a main class and call _plotData()_ from **InitialData.java**, so we can have this:

![](/content/images/2015/06/example-plot.png)

Now we’re going to take our first steps on writing the **GradientDescent.java**, we must be very careful here. Let’s start with the main settings and parameters of it:

<pre>
<code class="java hljs">
    import javax.swing.JFrame;
    import org.math.plot.*;

    public class GradientDescent{
            private double theta0;
            private double theta1;

            private int trendline;

            // Algorithm settings
        double alpha = 0.01;  // learning rate
        double tol = 1e-11;   // tolerance to determine convergence
        int maxiter = 9000;   // maximum number of iterations in case convergence is not reached
        int dispiter = 100;   // interval for displaying results during iterations
        int iters = 0;

        //track of results
        double[] theta0plot = new double[maxiter+1];
        double[] theta1plot = new double[maxiter+1];
        double[] tplot = new double[maxiter+1];

        InitialData initial_data;

        Plot2DPanel plot;

        public GradientDescent(InitialData id){
                    //initial guesses
            this.theta0 = 0;
            this.theta1 = 0;
            this.initial_data = id;

            plot = new Plot2DPanel();
            plot.addScatterPlot("X-Y", initial_data.x, initial_data.y);
            JFrame frame = new JFrame("Final X-Y Data");
            frame.setContentPane(plot);
            frame.setSize(600, 600);
            frame.setVisible(true);
        }
    [...]
</code>
</pre>

Now let me explain a few details of this part:

**Alpha** is the **Learning Rate**, it’s a _dangerous variable_, it’s used to set the size of the step that the algorithm will take while trying to find the \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\), that’s the learning rate of the algorithm. If Alpha is **too low**, the algorithm can be very slow, although very precise, if Alpha is **higher**, it will be taking **larger steps**, which can be **faster**, or **dangerous**, causing the algorithm to **DIVERGE**, which, _trust me_, you don’t want this! _(Andrew Ng explain this part very well in its course)_

The variable **TrendLine** is what we’ll use to plot the straight line which is our main goal.

The **tol** variable is our safe move in case of a dangerous convergence, which means, in case of convergence, it will stop the execution.

The other variables and objects in this part are very intuitive to understand, it’s auto explainable! _(forgive if i’m wrong, just say something and I’ll put more detail on that)_.

About the Constructor, we’re saying that our initial guesses for \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) is 0\. The rest is just data plotting.

**next: our Hypothesis Function** _(that will be doing exactly as the model that I did show above)_
<pre>
<code class="java hljs">
    public double hypothesisFunction(double x){
            return this.theta1*x + theta0;
        }
</code>
</pre>

Now, our two function to derive our \\( \\Theta \\), again, it will be doing exactly the same as the mathematical model, there’s no magic!

<pre>
<code class="java hljs">
    public double deriveTheta1(){
            double sum = 0;

            for (int j=0; j<initial_data.x.length; j++){
                    sum += (initial_data.y[j] - hypothesisFunction(initial_data.x[j])) * initial_data.x[j];
            }
            return -2 * sum / initial_data.x.length;
        }

        public double deriveTheta0(){
            double sum = 0;

            for (int j=0; j<initial_data.x.length; j++) {
                    sum += initial_data.y[j] - hypothesisFunction(initial_data.x[j]);
            }
            return -2 * sum / initial_data.x.length;

        }
</code>
</pre>

<br/>
Now, the **Gradient Descent Algorithm** _per se_, making use of the functions above:

<pre>
<code class="java hljs">
    public void execute(){
            do {

                    this.theta1 -= alpha * deriveTheta1();
                    this.theta0 -= alpha * deriveTheta0();

                    //used for plotting
                    tplot[iters] = iters;
                    theta0plot[iters] = theta0;
                    theta1plot[iters] = theta1;
                    iters++;

                    if (iters % dispiter == 0){
                            addTrendLine(plot, true);

                    }

                    if (iters > maxiter) break;
            } while (Math.abs(theta1) > tol || Math.abs(theta0) > tol);
            plot.addScatterPlot("X-Y", initial_data.x, initial_data.y);
            System.out.println("theta0 = " + this.theta0 + " and theta1 = " + this.theta1);
        }
</code>
</pre>

Note that it does _almost_ exactly the same as the mathematical model of the Gradient Descent demonstrated above, the difference is only a few details, such as the _if_ and _while_ to verify convergence or divergence _(which is, if it reached the iteration’s limit)_

Next we have the **addTrendLine** function, used to keep plotting our straight line as it will become more updated.

<pre>
<code class="java hljs">
    public void addTrendLine(Plot2DPanel plot, boolean removePrev){
            if (removePrev){
                    plot.removePlot(trendline);
            }
            double[] yEnd = new double[initial_data.x.length];
            for (int i=0; i<initial_data.x.length; i++)
                    yEnd[i] = hypothesisFunction(initial_data.x[i]);
            trendline = plot.addLinePlot("final", initial_data.x, yEnd);
        }
</code>
</pre>

And now we have an extra, it’s a function to store and plot the convergence history of both \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\), so we can see how it happened.

<pre>
<code class="java hljs">
    public void printConvergence(){

          double[] theta0plot2 = new double[iters];
          double[] theta1plot2 = new double[iters];
          double[] tplot2 = new double[iters];
          System.arraycopy(theta0plot, 0, theta0plot2, 0, iters);
          System.arraycopy(theta1plot, 0, theta1plot2, 0, iters);
          System.arraycopy(tplot, 0, tplot2, 0, iters);

          // Plot the convergence of data
          Plot2DPanel convPlot = new Plot2DPanel();

          // add a line plot to the PlotPanel
          convPlot.addLinePlot("theta0", tplot2, theta0plot2);
          convPlot.addLinePlot("theta1", tplot2, theta1plot2);

          // put the PlotPanel in a JFrame, as a JPanel
          JFrame frame2 = new JFrame("Convergence of parameters over time");
          frame2.setContentPane(convPlot);
          frame2.setSize(600, 600);
          frame2.setVisible(true);
        }
</code>
</pre>

Finally, we have our **Test Class**, that will execute everything:

<pre>
<code class="java hljs">
    import javax.swing.JFrame;

    import org.math.plot.*;

    public class TestGradDescent {
            public static void main(String[] args ){
                    InitialData id = new InitialData();
                    id.plotData();
                    GradientDescent gd = new GradientDescent(id);
                    gd.execute();
                    gd.printConvergence();

            }
    }
</code>
</pre>

Now, executing the code, the output will be, at first, the initial data:

![](/content/images/2015/06/example-plot-2.png)

**Executing the algorithm, it learns and generate its prediction based on its initial data:**

![](/content/images/2015/06/example-plot-line-e1420763780646.png)

### Magical, right?

And then, we can see the **convergence**:

![](/content/images/2015/06/example-convergence.png)

And you can see in the terminal the final values of \\( \\Theta\_{0} \\) and \\( \\Theta\_{1} \\) that minimized the Cost Function.

_If it wasn’t Science, probably would be black magic. heh._

### A few considerations

You can download the complete code [here](https://github.com/digorithm/ArtificialIntelligenceAlgorithms).

If you have any questions/suggestion, drop me an email so we can talk!

If you need further details of the mathematical model, I ultra advice to watch Andrew’s Ng Videos at Stanford@Coursera. **His skills to teach it is something unbelievable awesome**.

_Thanks for reading!_
