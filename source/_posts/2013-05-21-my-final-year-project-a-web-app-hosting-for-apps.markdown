---
layout: post
title: "My Final Year Project - A web app hosting for apps"
date: 2013-05-21 21:53
comments: true
categories: ["FYP", "web-apps"]
---

For my final year project in the School of Computing NUS I had to do something very exciting related to working with web apps. If you are familiar with online continuous integration environments like [Travis][travis] and [Jenkins][jenkins] then cracking this issue is a hen's egg. The thing that I was required to do was compile and run research programs within a webapp. That is, feed input to a program written in C, C++ etc. and get the output, display it to the user. All of this included a data repository which housed patient data.
<!--more-->

###Abstract
Ever pervasive use of computational algorithms has changed the face of medical data analysis. Certain surgeries such as craniomaxillofacial (CMF) surgery require sophisticated planning. Hence computer-aided surgery planning through research algorithms becomes crucial to its success. Most analysis algorithms require themselves to be installed in a computer before they can be used. Some of the analysis algorithms also require technically intensive setups on local workstations, which medical clinicians don’t have much experience with. This significantly prevents mass distribution and adoption of new algorithms in the medical research industry.

To advance the state-of-the-art in computer-aided surgery planning and medical data analysis, we need to collect and analyze a large amount of patient data. The goal of this project is to develop a repository and processing center for the remote management and processing of patients’ medical data. The system will be a server-based system so that it can be extended to a multi-server system. It will have to manage text data, medical images and 3D models, but also the remote installation and batch execution of software algorithms to process the data, and reporting of processing results to the users. It should also support the access, retrieval and display of the data by authorized users. This ultimately will allow users to execute sophisticated algorithms on patient data without requiring them to go through extensive technical setups

[Download The Paper][report]

[travis]: http://travis-ci.org
[jenkins]: http://jenkins-ci.org
[report]: /docs/Final_Report.pdf