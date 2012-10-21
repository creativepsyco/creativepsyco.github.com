---
layout: post
title: "Node JS Script to run a web server for Backbone etc"
date: 2012-10-22 00:18
comments: true
categories: [NodeJS, Backbone] 
---

The Problem
-----------
Sometimes we find ourselves wanting to be able to host a webserver to test out some HTML or javascript functionality. For example when Cross Origin Resource Policy (CORS) does not allow running of scripts from outside the domain of the webserver. 

The Solution
------------
The solution is a simple piece of node js script living in the directory. Surprisingly it is way way small


``` javascript Sample NodeJS Script to run server
var connect = require('connect');
connect.createServer(
    connect.static(__dirname)
).listen(8080);
```

This would start listening for a request on port 8080 and then serve the files from the current directory.

In order to use you must put the code in a file called *server.js*

And Also must install `connect` module via 
{% codeblock lang:bash %}
  npm install connect
{% endcodeblock %}

--mohit
