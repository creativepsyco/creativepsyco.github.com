---
layout: post
title: "New Technologies Galore"
date: 2012-11-02 01:12
comments: true
categories: [Redis, Backbone, ExpressJS, NodeJS]
---
*Most of the stuff is still under production, more details later*

Don't know why but these days I am hooked to Javascript. Perhaps that's coz I write way too much Javascript these days or maybe the world is really moving to the client side of things. Now I don't want to go into the details of the holy war of server side vs client side of things. However, I do find that client side stuff is very exciting not to mention that completely open. 

For client side development I am using a host of technologies to work my way around. These are BackboneJS, ExpressJS, NodeJS and Redis.

## Food For Thought
Now you would be wondering how come so many *JS appended libraries. Man! if I had known myself I would tell but really don't know what is up with the JS convention anyways.

## NodeJS
To me nodejs seems pretty darn powerful when it comes to serving requests. With it's event driven framework to go around I am sure it's the language of the server in the times to come. Although I would like to step back and think that Javascript is not really a `language` but more like a `scripting language` but I guess the perceptions will change over time. To start a simple node js server all you need is to install [Node][node] first. My favorite for doing this is [Homebrew][homebrew]

``` bash
$ brew install node
```

Here is a sample `server.js` file which acts as a Node web server.

``` js server.js
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```
To run this all you need to do is:

``` bash
$ node server.js
```

And voila goto `localhost:1337` and be greeted by a HTML page response. Cool isn't it?

## ExpressJS
Now we dive into a framework for creating web applications with Node. Why? Coz Frameworks rock and there is a clear well defined design strategy at work which frankly most of us would take a lot of time to devise. So why don't we use the community's advice and just do it!

[ExpressJS][express] makes the creation of web apps with Node such a breeze.

First you would need to install express. For this we would make use of the Node Package manager (NPM)

``` bash
$ npm install -g express
```

This would put a bunch of stuff in the directory of our node app. ExpressJS comes with a cool generator (aka rails style) and one can make use of it to create the next scaffold.

``` bash
$ express --sessions --css stylus --ejs myapp
```

So go ahead and start modifying code. Yay! Follow the guide at the [ExpressJS Docs][expressdoc]

## BackboneJS
Server side Javascript is fine but what about client side javascript. For this we have [BackboneJS][backbonejs], a client-side MVP (most people call it MVC or MV*) framework which organizes your code into awesome folders called `models`, `views` and `templates`. Find out more about it at the official website.

## Redis
And lastly for persistence, I am looking at [Redis][redis]. I am still looking at it. So i will update this once I have more information about it. 


[homebrew]: http://mxcl.github.com/homebrew/
[node]: http://nodejs.org/
[express]: http://expressjs.com/
[expressdoc]: http://expressjs.com/guide.html
[backbonejs]: http://backbonejs.org/
[redis]: http://try.redis-db.com/



