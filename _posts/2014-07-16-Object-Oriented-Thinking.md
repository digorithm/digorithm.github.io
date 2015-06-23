---
layout: post
title: Object Oriented Thinking
categories: [general, setup, demo]
keywords: "thoughts, object oriented, programming, engineering, architecture"
tags: [thoughts, object oriented, programming, engineering, architecture]
fullview: true
---

###What are the main goals of the object-oriented programming?###

_1\. Abstraction, yes, i had to put this in the first place, in my point of view, this is the biggest goal of the OO programming_

2\. Encapsulation, which is a form of abstraction!

3\. Reusability

4\. Modularity

5\. Readability

Let’s say that OO programming did **never** exist, so, i can say that any procedural programmer _(C, fortran, cobol, etc..)_ who has a little notion about how to build organized codes, would, naturally, evolve to an organization that would look like the actual concept of OO programming.

Don’t you think? It would be intuitive, the object-oriented thinking is a quite natural way to think, let me show you… We are used to separate _things_ (from now we’ll call thing as objects) by it’s _class_, just like the class “Car”, we know GM, wolksvagem, nissan, honda…but they all are cars, right? And we’re also used to say that something _inherited_ some particularities from his precedent… And that’s the idea behind the OO programming, grab the main concept of object and class that defines objects in **real world** and use it on programming!

![](/content/images/2015/06/dog.gif)

Just before the creation of Object-Oriented Programming, the system development in C was using the concept of breaking the big problem into small sub-problems, where each sub-result together would generate the solution of the big problem in question. It’s actually smart to think like this, the OOP languages just formalized this concept (and of course, create others features, but this was the start point). Look at this following problem: We want a data structure to represent the functionaries of determined business, this functionary should have a name, age, salary and a function to change his salary. Let’s deal with it in the smartest way, which is, creating separately some structure that will represent the functionaries, in C we would code something like this:

    typedef struct functionary{
      int age;
      char name[50];
      float salary;
    }stru_func, *ap_func;

    void riseSalary(ap_func pthis){ 
          int n;
          printf("Enter new salary: \n");
          scanf("%d", n);
          pthis->salary = n;
      }

As you can see, in this func.c, we have the structure to represent the functionaries and the function that we will use. (from now, we’ll call function as METHODS)

And here’s the **main code:**

    #include "func.c"
    int main(){
      stru_func new;
      printf("Enter the age, name and salary: \n");
      scanf("%d", &new.age);
      scanf("%s", &new.name);
      scanf("%f", &new.salary);
      printf ("%d \n", new.age);
      printf ("%s \n", new.name);
      printf ("%f \n", new.salary);
      riseSalary(&new);
      printf("%f", new.salary);
      return 0;
    }

Done, now we have a nice example of class and object…but, made with C! Where the class is this struct called “functionary” and the variables inside it we call “attributes” and the function “riseSalary” we call “method” (obs: look that the function/method is OUTSIDE the struct functionary), in fact we separate the module that represent the functionaries from the main module, and if we want, we can use this functionary module in other problems that need a struct to represent functionaries with those attributes/methods!

And…where’s the object? Right there in the main module, where we created a “variable” called “new” from the struct functionary, this is called “INSTANCE”, in this moment, we create a object from its class! Again, the class is the structure that represent the functionary and the object is the “variable” that we created with this structure!

All these concepts already happened before the creation of the pure Object-Oriented languages, but there was a need of formalize these techniques, creating more rigorous, useful and trustful standards.

The same problem that we solved in C, in java, would be something like this:

    public class functionary
    {
      public int age;
      public string name;
      public int salary;
      public void riseSalary(int n)
      {
          this.salary = n;
      }
    }

As you should have noticed, the method now is INSIDE the structure that represent the functionary (the class). And what the big advantage of this? the class is now more portable, we can reuse it easily!

in the main module we could just create as many object as we want just using: _“functionary func = new functionary();”_

**Conclusion**: Of course that the big evolution of procedural to Object-Oriented did not stop there! The concept of reuse, organize and divide the code and the problem created others many famous techniques that we can use on classes and objects, like the **polymorfism**, inheritance, abstract class and many others! But this evolution kept one main goal: **modularity with more abstraction.**