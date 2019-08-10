---
layout: post
title: Web Scaling - using Redis as Cache
categories: [python, software-architecture, software-scalability, caching]
keywords: "redis, web, scalability, programming, python, programming, engineering"
tags: [redis, web, scalability, programming, python, programming, engineering]
fullview: true
permalink: post/web-scaling-using-redis-as-cache
---
**Redis is such a great technology.** Unfortunately, there's still people who don't know Redis or don't know that Redis can be used as a Cache System to improve the speed of responses.

### Why Redis

Well, let's start this discussion remembering how a common Relational Database basically works: Suppose we're using a MySQL, every time your app sends a request to the MySQL client, the MySQL client gotta make a trip to the hard drive to get the data asked in the request, this can become a problem if the data asked in request is big... and if there are many requests at the same time, this can generate a huge latency, annoying users or worse.

<!--more-->

This is where Redis comes into play, Redis is a key-value database that will be running and storing data inside your memory, if you remember the basic of computers architecture:

![](/content/images/2015/06/memchart.jpg)

It's way faster to access data in memory (Physical RAM, main memory) than to access data in the Hard Drive, so it's easy to notice that if the data that the application wants to access is inside the main memory, it's way easier to reach to that data than if it was stored in the Hard Drive.

So, like I said, Redis will be storing its data inside the memory, but you may ask yourself: "but what if I turn off the machine?? isn't the ram memory volatile? " Yes, that's why Redis will be flushing the data to the hard drive from time to time, it's up to you to choose this time between flushes, it's all about Performance vs. Security.

So, What we'll be doing is just:

<img src="/content/images/2015/06/atv1.png" width="350" height="400"/>

_note that this print is taken from a talk I gave in my country, so it's in portuguese. aplicação = application, Não acha key = key don't found, retorna dados = return data_

**As you can see: So much win. We avoided redundant trips to the disk.**

Now that we understand the concept of what we'll be doing, the code becomes very easy to implement, here's a simple idea _(thougt it can be improved and extended in many ways, but it can demonstrate the idea we're working here)_

<pre>
<code class="python hljs">
    from flask import Flask
    from flask.ext.sqlalchemy import SQLAlchemy
    from sqlalchemy import create_engine
    import redis

    app = Flask(__name__)
    url = 'mysql+pymysql://username:password@ip/dbname'
    app.config['SQLALCHEMY_DATABASE_URI'] = url
    db = SQLAlchemy(app)
    cache = redis.StrictRedis(host='localhost', port=6379, db=0)

    #improvement: model could stay in a different file
    class User(db.Model):
        id = db.Column(db.Integer, primary_key=True)
        username = db.Column(db.String(80))
        email = db.Column(db.String(120))

        def __init__(self, username, email):
            self.username = username
            self.email = email

        def __repr__(self):
            return '' % self.username

    def createUsers():
            for x in xrange(0,100000):
                user = User('test', 'test')
                db.session.add(user)
            db.session.commit()

    def getUsers():
            users = cache.get('users')
            if not users:
                users = User.query.all()
                cache.set('users', users)

    # improvement: views/routing should stay in a different file
    @app.route('/')
    def hello_world():
        getUsers()
        return 'Hello Worldd!'

    if __name__ == '__main__':
        app.debug=True
        app.run(host='0.0.0.0')
</code>
</pre>

So, after you create the correct database, populate the database (there's a function in the code for that) and change username/password/dbname in the code, we'll run the app.py and go to localhost:port-you-exposed.

### _What will happen?_

1\. The first time you access it, it will take a few seconds to get the data from the MySQL

2\. The second time you access it, it will take just a few milliseconds to get the same data from redis

In my computer the result was:

first access: 45861 milliseconds

**second access: 5ms**

**I know, right? That's just blazing fast!**

And it can save a lot of computational resources and human time. Now, with this logic applied to one method (the method to get users), we can apply it to whichever method we want, or we can even create a generic decorator and annotate the methods that we want do the caching!!
