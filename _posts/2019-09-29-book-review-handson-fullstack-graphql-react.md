---
layout: post
title:  "Book Review: Hands-On Full-Stack Web Development with GraphQL and React"
description: Review of a great book about building a full-stack webapp with GraphQL and React. This book is great for developers who want to broaden their skills in webapp development.
date:   2019-09-29 10:47:54 -0700
categories: blog
---

If you have been following the world of front-end development recently, you must have heard about [React](https://reactjs.org/), the widespread JavaScript library created by Facebook. You may have heard about [GraphQL](https://graphql.org), the querying language to build smarter APIs, also initially developed at Facebook, which is slowly replacing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) in the industry of web applications. I wanted to catch up on both recently and had the chance to do so using [this book (paid link)](https://amzn.to/2B7sNvo) by [Sebastian Grebe](https://twitter.com/sebastiangrebe).

<img src="https://images-na.ssl-images-amazon.com/images/I/51P15LOi1cL.jpg" height="256px" width="200px" alt="Book Cover" />


## Review
As the title suggests the book covers all aspects of a full stack JavaScript project and details the construction of a website that will remind you of a famous social network :). The progression is very gradual and demonstrates thoroughly the two main subjects: GraphQL and React, but also details other crucial topics when it comes to managing a web application: database migrations, containerization, code deployment, or hosting media files on the cloud.

This book is best suited for developers with a minimum of experience, preferably with JavaScript, who want to explore the many aspects of a web project: front-end app, back-end app, database, deployment, security, and scalability.

The book is very dense with information and details about a bunch of topics that a developer encounters while working on a web application. It makes it very interesting and extends your knowledge far beyond what you would learn from online tutorials. The main downside of the book is that all the React code is built using classes. At the time when this article is written, September 2019, it would be more interesting to see a version using Function Components and React Hooks. I think the book is still very valuable and the refactoring of the code to a more current React style could be a fun exercise.

If I had to choose, these are the topics that I found the most interesting:
* GraphQL communication between front-end and back-end using Apollo.
* Integrating GraphQL within React Components.
* Managing database migrations using code (and without manual scripts).
* Using AWS S3 to store media files.
* Using WebSocket to implement a real-time chat feature.
* How to design unit tests for a web application.


### A few tips if you acquire this book and study the example:
* Do not use the latest version of MySQL. The MySQL code provided in the book is compatible up to MySQL 5.6 (Avoid using MySQL 8.0 and above).
* Use git (or any version control system) to keep track of your progress while reading the book. There can be a lot of refactoring going on between chapters, so I recommend committing regularly. You will then be able to compare the codebase between different stages of the project, which is very valuable.
* Check the [github repo of the book](https://github.com/PacktPublishing/Hands-on-Full-Stack-Web-Development-with-GraphQL-and-React) to see the author's code if you get lost in the implementation.
* If you encounter bugs (you will!), one tip is to verify that you are using libraries correctly (for example: `findById()` is used in the book but is deprecated in Sequelize and needs to be replaced by `findByPk()`)

## In conclusion
I really enjoyed this book as a refresher on full stack development. In particular, I was not too familiar with JavaScripts based backend and I learned a lot. The main strength of this book resides in its coverage of all topics currently relevant in the industry including: components based architecture, authentication and security, testing, scalability, cloud storage, and performance. None of them are presented in too much detail but they are discussed sufficiently to get the gist. Finally the application implication used as an example through the book, a social network with a chat feature, is fun and keeps the reader motivated, as least in my case. 

#### Disclaimer
As an Amazon Associate I earn from qualifying purchases.
