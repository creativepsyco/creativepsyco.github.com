---
layout: post
title: "Creating an API with .NET MVC4"
date: 2013-01-10 22:26
comments: true
categories: [ASP, .NET]
---

##Gist
*Recently while working on a vacation assigment I had to create an API in ASP .NET MVC. This was in order to create a booking system on top of the Google Calendar Event storage. Google provides 50000 calls per day free of chrage for the API so we thought this might work. But it also required some form of authentication to prevent the API facade from being misused.*

##OpenId
Well when you can't store the authentication result you need to think of some third party based authentication mechanism and OpenId is one those means. Most of the modern email and other service providers such as Dropbox etc provide openid schemes which can be leveraged to get basic userinfo and proceed to do something in web application.

##Nancy
Nancy is a NuGet package which mimics Sinatra. It allows creation of Http GET, POST, DELETE requests as mere functions with much ease. However it does not supports sessions and hence was found to be very hard to use in some of the places. For most operations, however, Nancy is a fantastic option.

##ASP .NET MVC

// Todo

##Authentication in MVC .NET

// Todo