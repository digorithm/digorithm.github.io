---
title: CRUD with Java, JSP and MySQL
keywords: "java, tutorial, programming, engineering, design, architecture, jsp, crud"
tags: [java, tutorial, programming, engineering, design, architecture, jsp, crud]
categories: [java, software-architecture, case-study]
fullview: true
permalink: post/crud-with-java-jsp
layout: post
---

<p>One of the most common kind of applications in today's tech/business world is the CRUD system, that is the idea of the user interacting with a database, so the user can Create, Retrieve, Update and Delete <em>(yep, that awful name came from this)</em> data from this database. Also, <strong>learning how to create a CRUD with a language is a great way to understand the basics of this language, the basics of software design, engineering and architecture.</strong> In layman's term, a CRUD page is just a page where you can insert a thing, edit that thing, search that thing and delete that thing, just that!</p>
&nbsp;
<!--more-->
<h3>Our goal</h3>
<p>Well, our scenario will be the simplest possible, so we can understand how to communicate with our database, so, it will be a simple contact book web app where we can do our CRUD with persons. This person will have <!--more--> a name, a phone number and a profession. We'll have a button to add a new person, a form to enter this new person's information, a page where will be loaded all persons from the database and buttons to edit and remove each person</p>

![alt](/content/images/2015/06/smallcrudscreen-1024x300.png)

&nbsp;
&nbsp;
<h3>Our Technology Stack</h3>
<p>We'll be using Java as our backend technology, Java ServerPages(JSP) as our view technology and MySQL as our relational database. Also, we'll be using the MVC architecture, which is the idea of separating the code by <em><strong>M</strong>odel</em>, <em><strong>V</strong>iew</em> and <em><strong>C</strong>ontroller</em>, that's a whole other topic to study, you can take a look <a href="http://blog.codinghorror.com/understanding-model-view-controller/">here</a>, <a href="http://code.tutsplus.com/tutorials/mvc-for-noobs--net-10488">here</a> and <a href="http://www.bennadel.com/blog/2379-a-better-understanding-of-mvc-model-view-controller-thanks-to-steven-neiland.htm">here</a>!</p>
<p>For those who doesn't know JSP technology, I highly advice to study just a little bit of it before read this article, as this will be our main view technology, it's a great abstraction over the <a href="http://www.ntu.edu.sg/home/ehchua/programming/java/JavaServlets.html">Servlets</a> technology. It simply helps to process Java code on our pages <em>without putting pure java syntax in the middle of our HTML</em>, which would be extremely ugly and hard to maintain.</p>
&nbsp;
<h3>The Data Access Object (DAO) pattern</h3>
<p>The DAO pattern is a great way to abstract the communication with our database, simply put, the ObjectDAO is a class that will implement the CRUD methods of the Object. This way, our entity class, which is a Java class with only its attributes and their respective getters and setters, won't have to deal directly with the database, you can read more about the DAO pattern <a href="http://www.oracle.com/technetwork/java/dataaccessobject-138824.html">here</a>.</p>

<h3>The JDBC</h3>
<p>This will be the driver that will abstract the connection with the database, it's pretty simple to use and it's the standard of the oracle, you can read more about it <a href="http://www.oracle.com/technetwork/java/overview-141217.html">here</a>.</p>

<p><strong>Finally, the final view of our scenario</strong></p>

![alt](/content/images/2015/06/SmallCRUD1.png)

<p>And, for any guidance, here's the <strong>project hierarchy</strong>:</p>

![alt](/content/images/2015/06/smallcrudhierarchy.png)


<h3>Creating the Database</h3>
<p>Small and simple step, just create the MySQL scheme that we'll use on this project, that is, just a table person, with name, profession and phone. Here's the code:</p>
<pre>
<code class="sql hljs">
CREATE TABLE IF NOT EXISTS `smallcrud`.`person` (
  `idperson` INT(11) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `phone` VARCHAR(45) NOT NULL,
  `profession` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`idperson`))
</code>
</pre>
&nbsp;
<h3>Creating the model</h3>
<p>We'll create the Person, which will be our Java Bean for Person.</p>

<script src="https://gist.github.com/digorithm/6c1612d01d4c58c45282.js"></script>

<h3>Creating the connection class</h3>
<p>The connection class will be the responsible for the connection with the database, using the JDBC API, it's fair simple:</p>  

<script src="https://gist.github.com/digorithm/ed84ed8467c8e0813eca.js"></script>

<p><em><strong>!important: </strong>These Strings "jdbc:mysql://localhost:3306/simplecrud", "root", "sa" depends on the configuration of the database that you created, the first one is the URI of the database, the second "root" is the user of that database, and the last "sa" is the password of that database</em></p>

<p>So, what happens when we use this class? Every time we call its method "getConnection" it returns a Connection with the database, that will be useful to create Statements, execute SQL Queries and plenty <a href="http://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html">more</a>.</p>
&nbsp;
<h3>Creating the PersonDAO</h3>
<p>The PersonDAO will be the class that will manipulate the class Person and communicate with the database, sending persons to database, updating persons, retrieving persons from the DB, which means, the CRUD main methods</p>

<script src="https://gist.github.com/digorithm/4177708c2bf222920845.js"></script>

&nbsp;
<h3>Creating the Controller</h3>

<p>This is the most confusing part. All because of the confusion of <strong>not</strong> using JSP file to process the request, so what we will use? <strong>Servlets! </strong> A Servlet fits better in the Controller role, because they won't show anything to the user, it will only get the request data, verify which action the user is trying to make, do something and redirect the user to another page. If you're using JSP files as controllers, probably you're <a href="http://www.geekinterview.com/question_details/37537">doing it wrong</a>. So, basically, our PersonController will take the user request, see if the user is trying to edit, insert, update, remove or retrieve... and do the right thing, pay extremely attention to this code</p>

<script src="https://gist.github.com/digorithm/1f86192880faa70760a7.js"></script>

<h3>Creating the View: Index.jsp</h3>

<p>The index will be pretty simple and straight forward, it will just call our controller, passing the action by the get method, in this case, the ListPerson, the Controller will see that the action required is the ListPerson so it will do what it has to do, get all users from the database and list'em.</p>

<script src="https://gist.github.com/digorithm/bb8cc63c5efb47f59c74.js"></script>
<h3>Creating the View: ListPerson.jsp</h3>

The controller got the action coming from the index and tried to retrieve every person in the database and put all person objects in the session, if there is any Person in the DB, of course. Here is the code to do it, as you can see we’re already using the JSP tags to not use pure Java Scriplets  

<script src="https://gist.github.com/digorithm/bcb0acccceeb8a652b8f.js"></script>

<h3>Creating the View: Person.jsp</h3>
<p>This will be the page that the user will be able to insert or edit a Person in the database so it can be loaded on the ListUser.jsp. As the Controller says, if the request has an ID, the Person.jsp will be loaded with a Person, otherwise, it won't. (<em>expecting that the user fill the form to add a new Person to the database</em>)</p>

<a href="http://www.universocomputacao.com/wp-content/uploads/2014/07/smallcrudscreen2.png"><img class="aligncenter wp-image-74 size-medium" src="http://www.universocomputacao.com/wp-content/uploads/2014/07/smallcrudscreen2-300x263.png" alt="smallcrudscreen2" width="300" height="263" /></a>


<script src="https://gist.github.com/digorithm/c312976f8ddb79d71051.js"></script>

<h3>Last details and conclusion</h3>
<p>As you can see, at the first sight this implementation can be a little confusing, mainly because of the flow logic and the controller built with Servlet, understanding the flow means understanding it as a whole. Make sure you understand the MVC logic and the concepts behind JSP, Servlets and JDBC.</p>

<p>If you're trying to copy the whole code, make sure you import the right libs, including the JSTL, responsible for a few tags I've used in this code. Also i used the Twitter's Bootstrap to help me with the frontend UI.</p>

<p>You can download the code here on my <a href="https://github.com/digorithm/SmallCRUD">GitHub</a></p>

<p>If there's something wrong with your implementation, please let me know, drop me an email or post a comment here and I'll make sure it will work properly! :]</p>

