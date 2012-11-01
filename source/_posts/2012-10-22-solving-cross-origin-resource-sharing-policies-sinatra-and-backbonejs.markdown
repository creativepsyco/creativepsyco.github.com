---
layout: post
title: "Solving Cross Origin Resource Sharing policies Sinatra and BackboneJS"
date: 2012-10-22 22:57
comments: true
categories: BackboneJS
---

Admit it. There are so many times you wish that the localhost-ed version of the application that you are developing in BacboneJS just works. But it doesn't. And there is only one reason why it doesn't work. 

Cross Origin Resource Sharing
----------------------------
Yeah, the big culprit is CORS. And here is how you circumvent it. This snippet assumes you are using Sinatra for deploying your Rails API.

The Solution
------------
``` ruby
options '/*' do
  response["Access-Control-Allow-Headers"] = "origin, x-requested-with, content-type"
end
```


This snippet will add a header to the response that will allow cross-origin javascript coming specifically from localhost to be able to call the server and get a response.

