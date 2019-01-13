# What is an API?

Let's have a think about the HTTP request/response cycle. We should be reasonably familiar with the idea now.

![http](https://cloud.githubusercontent.com/assets/3531085/26764966/8f01838c-4969-11e7-890d-0d54dfe5bba4.gif)

A client makes a request which is received by the web-server, the HTTP request is handled, routed, the relevant data from the database is received and a response is sent back to the client.

So far, this response has been a HTTP status and then some `text/html` after a template has been rendered by the templating engine.

However, let's have a think about **content types**.

The problem about `text/html` is that it is designed for humans to understand.

**What if we wanted to request some data from a website across the internet and then do something programmatical with that data?**

We'd have to still make a HTTP request to get the data from our web-server the same - this is how the internet works! However, we might want the response to be in a format that is slightly easier for a computer to understand!

`text/html` is just one type of data that can be sent and received on the internet. Another common one is `application/json`.

## Definition of JSON

> **JSON** (JavaScript Object Notation) is a lightweight data-interchange format. It is easy for humans to read and write. It is easy for machines to parse and generate. It is based on a subset of the JavaScript Programming Language, Standard ECMA-262 3rd Edition - December 1999.

JSON is just one format that we can send and receive over the internet. There are some others like XML that you might see. However, JSON is the most common and the easiest to use.

## API (Application Programming Interface)

In computer science API [application programming interface](https://en.wikipedia.org/wiki/Application_programming_interface) is quite a broad term.

> **API** (application program interface) is a set of routines, protocols, and tools for building software applications. The API specifies how software components should interact and APIs are used when programming graphical user interface (GUI) components.

This term can be quite confusing as it is SO broad.

## Web API

However, when we think about APIs within the context of web application development we can simply think about it as a website that returns JSON instead of HTML so that a computer can use the data to do something instead of a human.

It allows a programme to interface with a website over the internet so that it can use that websites' data.

*Show this image on the board*

![api](https://cloud.githubusercontent.com/assets/3531085/26764965/8cdac33e-4969-11e7-87b1-8f1ef6b5ae42.png)

## Why do you need an API?

Nowadays, there are lots of different reasons why you would need an API [application programming interface](https://en.wikipedia.org/wiki/Application_programming_interface) for your website or tech project.

1. You might want to provide data to an IOS/Android app?
2. You might want to allow other developers to use your data?
3. You might want to split up your codebase into a number of smaller services so that it is easier to manage
4. You might want to use a front-end framework to serve your data like Angular, Backbone or Ember

These are just a few of the reasons. It's now very difficult to be a developer and not have to either work on or use an API.

## Building an API

There are a number of different frameworks that you could use when making an API for your project.

1. **Sinatra** (Stripped back codebase)
2. **Rails** (very quick to setup)
3. **Express** (Used a lot with the MEAN stack)

There are quite a few others! PHP is still used a lot as is Python.

## Further reading

- [What is REST?](https://restfulapi.net/)
- [REST API Concepts and Examples](https://www.youtube.com/watch?v=7YcW25PHnAA)
