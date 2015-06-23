---
layout: post
title: "Case Study: Python Performance on rotating one-dimensional vectors"  
categories: [general, setup, demo]
keywords: "machine learning, artificial intelligence, java, programming, engineering, tutorial"
tags: [machine learning, artificial intelligence, java, programming, engineering, tutorial]
fullview: true
---



I'm a big fan of a nice challenge, therefore, I like books like Programming Pearls, I like to dive into many kinds of solutions to the same problem and try to differentiate them by novelty, performance, elegance, etc... 

This time I was playing with a fun problem, from the column 2:

> "rotate a one dimensional array of *N* elements left by *I* positions". 

The author says it should consume little space and time, so, there are many solutions, obviously. The fun thing is that I was doing it in Python, so you can solve it in many ways, but you can decrease the performance a lot if you choose poorly. In C we could just swap pointers in a doubly linked list, which is what happens in the real implementation of CPython.

The idea that the Author exposed is pretty clever, which is based in reversing the vector only 3 times:

1. Reverse the vector, from the first position to the *I*-th position, where *I* is the number of position that is wanted to move to the left
2. Reverse the vector, from *I*-th + 1 position to *N*, where *N* is the size of this vector
3. Reverse the whole vector, again. 

In other words, we have the vector *AB* (Where *A* is the first part, *B* is the second), we reverse *A* so we have *Ar*, reverse *B* so we have *Br*, now we have *ArBr*, then we reverse the whole thing *(ArBr)r*, after that, we have the rotated vector. It's quite beautiful. A picture can help the visualization:

<div class="imgcenter">
<img src="/content/images/images/img1.jpeg" height="450"/>
</div>

here's an example with a simple vector:

<div class="imgcenter">
<img src="/content/images/images/img2.jpeg" height="350"/>
</div>

Now, searching for other approaches, I looked inside the code from CPython, and there's a nice comment: 

     "Conceptually, a rotate by one is equivalent to a pop on one side and an append on the other"

Quite elegant solution as well. So, an example:

<div class="imgcenter">
<img src="/content/images/images/img4.jpeg" height="350"/>
</div>

And the last example I found while searching through Pythonic ways to solve this problem (Even though may not be very fast)

<pre>
<code class="python hljs">
def rotate_pythonic_way(arr, i):
         return arr[i:] + arr[:i] 
</code>
</pre>
Which is super simple and elegant, what it's doing is the following:

<div class="imgcenter">
<img src="/content/images/images/img3.jpeg" height="350"/>
</div>

At first I thought this solution would be the slowest, but we'll get to that.

One thing that took my attention was the internal method to reverse a list that Python offers, there are two of them, one returns a reversed iterator and you can cast it into a list and the other reverse the exactly same list that is passed as parameter, it shouldn't take you long to realize which is faster. So I wrote a code with the many solutions to it and I profiled the functions, which gave us a interesting result when ran over a vector with 10 millions integers.

First, the original idea (*reverse the vector 3 times*), but using the reversed(array) method:

<pre>
<code class="python hljs">
def rotate_original_solution_with_reversed(arr, i):
    n = len(arr)
    array_A_reversed = list(reversed(arr[0:i]))
    array_B_reversed = list(reversed(arr[i:n]))
    ArBr = array_A_reversed + array_B_reversed
    rotated_array = list(reversed(ArBr))
    return rotated_array
</code>
</pre>


and the result:


        Original Idea using reversed(arr): 

        4 function calls in 0.611 seconds

        Ordered by: standard name

        ncalls tottime percall cumtime percall filename:lineno(function)
        1 0.440 0.440 0.440 0.440 2_1b.py:21(rotate_original_solution_with_reversed)
        1 0.171 0.171 0.611 0.611 string:1 module
        1 0.000 0.000 0.000 0.000 {len}



####Damn, 0.6s, that's too slow!

After this one, I changed the way I was reversing the vector, using the other internal method from Python:

<pre>
<code class="python hljs">
def rotate_original_solution_with_reverse(arr, i):
    n = len(arr)
    
    array_A = arr[0:i]
    array_B = arr[i:n]
    
    array_A.reverse()
    array_B.reverse()

    ArBr = array_A + array_B
    ArBr.reverse()
    return ArBr
</code>
</pre>

And the result:


        Original Idea using arr.reversed(): 
        7 function calls in 0.322 seconds

        Ordered by: standard name

        ncalls tottime percall cumtime percall filename:lineno(function)
        1 0.187 0.187 0.210 0.210 2_1b.py:30(rotate_original_solution_with_reverse)
        1 0.113 0.113 0.322 0.322 string:1 module
        1 0.000 0.000 0.000 0.000 {len}
        1 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler' objects}
        3 0.023 0.008 0.023 0.008 {method 'reverse' of 'list' objects}

####From 0.6s to 0.3s, that's a great improvement. lesson: choose your built-ins/Data Structures/Algorithms carefully.

And now, using the Pythonic Way, which I was thinking that would be the slowest:


And the result:


<pre>
<code class="bash hljs">
Pythonic Way: 

3 function calls in 0.295 seconds

Ordered by: standard name

ncalls tottime percall cumtime percall filename:lineno(function)
1 0.238 0.238 0.238 0.238 2_1b.py:45(rotate_pythonic_way)
1 0.056 0.056 0.295 0.295 string:1(module)
1 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler' objects}

</code>
</pre>

It was faster than the other methods! Well played, Python. 

And the last one, using the pop/append technique, repeated i times:


And the result:
<pre>
<code class="bash hljs">

Pop and append way: 

5 function calls in 0.013 seconds

Ordered by: standard name

ncalls tottime percall cumtime percall filename:lineno(function)
1 0.000 0.000 0.013 0.013 2_1b.py:49(rotate_pop_append)
1 0.000 0.000 0.013 0.013 string:1(module)
1 0.000 0.000 0.000 0.000 {method 'append' of 'list' objects}
1 0.000 0.000 0.000 0.000 {method 'disable' of '_lsprof.Profiler' objects}
1 0.013 0.013 0.013 0.013 {method 'pop' of 'list' objects}
</code>
</pre>


Ok. That was weird. 0.013 seconds?  13 milliseconds? Crazy, right? I thought the fact of repeating it many times would make it go way slower, but I guess I was wrong!

So, as you can see, an interesting problem can have many solutions, one more elegant, others faster. In the end you may pick the simplest and most intuitive solution, this may be the best for the situation. Some times you gotta go with the strangest and most non-intuitive solution (which reminds me the first time I saw the Quick Sort and all its non-intuitive way to think)

So, if you're willing to reverse a list with Python, go with List.reverse() method (unless you want the iterators to do something), and if you want to rotate a vector, go with pop/append, it looks faster.
