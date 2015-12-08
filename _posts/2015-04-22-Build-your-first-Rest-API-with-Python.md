---
layout: post
title: Building your first REST API with Python
categories: [general, setup, demo]
keywords: "api, python, programming, engineering, rest, flask"
tags: [api, python, programming, engineering, rest, flask]
fullview: true
permalink: post/build-your-first-rest-api-python
---

Are you total lost in this world full of jargons like: API, Rest API, microservices and stuffs? Come here, sit, grab a cup of coffee, and let's talk briefly about it.

Today a lot is said about APIs. Everything has an API, every programmer (newbie to expert) uses tons of API. Also, today we can see a lot of people talking about microservices and the idea of total separation of backend, frontend, web services or _whatever_. So we can _(and we do)_ hear a lot about REST/RESTful APIs. We have 3 current problems with it:

1\. Many new programmers don’t have a single clue of what a Rest API is.

2\. It’s probable that they’re using Rest APIs and don’t know about it.

3\. When they feel that they should learn more about it, there’s hardly any good and accessible material to learn it.

### So here’s a extremely simplistic approach to try to explain what is a Rest API

Starting with the concept of API, which stands for **A**pplication **P**rogramming **I**nterface, it’s just an interface which you, _dear programmer_, will be dealing with to extract whatever you _(or your program)_ want.

Suppose your program needs to create a connection with a given database, normally you do:

1\. You import the _library_ that will abstract the connection with the database

2\. You create an object to represent a Connection with the database

3\. You call functions that this object provide to you, so you can do whatever you want (and whatever they provide)

Here's a silly example:

    from database_library import Connection

    connection = Connection()
    connection.OpenConnection()
    … do whatever you want
    connection.CloseConnection()

When you called those function, you were dealing with the interface that the object provided to you, sure the object may be doing thousand of things at the moment you call its functions, but, it doesn’t matter to you, does it? You just want the _damn_ connection open and then closed.

So the library that you imported is giving you something you want, offering a service or a resource. That’s the sole purpose of an API. It’s a layer that you can use to get things from other(s) component(s).

So, it’s easy to deduct that, the better the engineer planned the API, the easier it will be to deal with and extract what you want, and the contrary is true.

But still, you're processing the core of this API in your own machine, which isn’t that great, here’s when the API evolves to whole web hosted services, so you can request this API something, and this API can give you something, through the WEB, via HTTP request. And that’s amazing.

And the community, recently, decided that the request and response that happen between applications and APIs, should be done with JSON, so this communication can become uniform and APIs can talk to other APIs effortlessly.

So, basically, what’s going on is:

![](/content/images/2015/06/json-rest3.png)


So... yes, you make your function calls, now, with just a simple URL + HTTP methods, wanna see a real example? We can just send a request to the Facebook’s API by accessing this URL: [http://graph.facebook.com/contatodigo](http://graph.facebook.com/contatodigo)

Which will request my profile’s data, and the Facebook’s API will return:

    {
       "id": "100001638888259",
       "first_name": "Rodrigo",
       "gender": "male",
       "last_name": "Ara\u00fajo",
       "link": "https://www.facebook.com/contatodigo",
       "locale": "pt_BR",
       "name": "Rodrigo Ara\u00fajo",
       "username": "contatodigo"
    }

As simple as that.

Now with this basic idea explained, we can do a simple Rest API using Python and Flask. (I’m assuming you’re already familiar with both technologies).

All we’re gonna do is, using the flask routing, create routes to the users so they can interact with the resources of our API. Let’s suppose our API will serve and receive only Books. So this is the resource we’re dealing with, users may use our API to get Books and insert new Books, so our only URIs will be:

1\. _/bookapi/v1.0/books_ with a GET method, which will just return the list of books

2\. _/bookapi/v1.0/books_ with a POST method, which will insert a book

So after the API is built, you or you program can get books or insert books by sending HTTP request to the API’s URIs, simple as that. I’ll be very straight forward, here’s the code:

    #!flask/bin/python
    from flask import Flask, jsonify, request

    app = Flask(__name__)

    books = [
        {
            'id': 1,
            'title': u'Game of Thrones',
            'description': u'Cool dragons', 
            'finished': False
        },
        {
            'id': 2,
            'title': u'50 shadows of grey',
            'description': u'It start as bullshit, end as a huge bullshit', 
            'finished': True
        }
    ]

    @app.route('/bookapi/v1.0/books/', methods=['GET'])
    def get_books():
        return jsonify({'books': books})

    @app.route('/bookapi/v1.0/books', methods=['POST'])
    def create_book():
        if not request.json or not 'title' in request.json:
            abort(404)

        book = {
                'id': books[-1]['id'] + 1,
                'title': request.json['title'],
                'description': request.json.get('description', ""),
                'finished': False
                }

        books.append(book)
        return jsonify({'book': book}), 201 

    if __name__ == '__main__':
        app.run(debug=True)

Note that we’re creating a simple in-memory _database_, which is a simple python’s dict. This could be a database. But for the sake of simplicity, I'm using just a dict.

When the requests come, it verifies the route that the user is asking for, which is: what resources is he/she wanting? and then, the code do whatever it must do (_remember the API idea of hiding the complexity, the user requesting just want the result_), and then it put everything in a JSON and returns it. That simple!

Of course, many improvements and extensions (_there are infinity possibilities_) can be made to this code, but, here’s a skeleton of the idea of an API, it’s just a start. One good practice it’s to _not_ return the ID of the resource, but return just its URI, which is surely a great idea. Another good practice is to ask authentication in every HTTP request, so your API won’t be exposed to everybody. There are, indeed, endless improvements, but in this code, you, that are completely beginner to the API’s concept, can now understand what’s going on underneath and start planning and building your own API using Flask.
