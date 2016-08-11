---
id: 54
title: Restaurant Recommendation
date: 2014-12-27T08:44:03+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=54
permalink: /2014/12/restaurant-recommendation/
categories:
  - Uncategorized
tags:
  - bootstrap
  - foundation
  - real-time comment
  - recommendation
  - scalability
---

In the last semester, I and my team have developed a restaurant recommendation system named <strong>iHotFood</strong> which supports <em>real-time comment system,</em> <em>restaurant recommendation </em>and <em>responsive UI</em>. Since I handled these three most important parts of the project, I wanted to share my own experiences I&#8217;ve collected after this 3-month project.


The architure is as follows. In the following part, the content will be arranged according to functional requirements the system should have.

{% include image.html url="/images/Screen-Shot-2015-01-27-at-10.23.53-AM.png" description="The iHotFood overall architecture" %}

1. The website must be cross-browser and reponsive

In my experience, the fastest way to achieve it is to use CSS framework. There was candidates in the market I had a look, including <strong>Bootstrap</strong>, <strong>Foundation</strong>. I picked Foundation for several reasons but I was satisfied with my choice after all.

Foundation is versatile in the way that it support super fast customization by using Sass. For example, I can change color of the top menu systematically and precisely with a little effort. In addition, the framework gains a sufficient user-guide which allows me make cross-reference quickly.

2. The website should have real-time comment system with scalability

When it comes to real-time stuffs, we usually think about scalability, i.e., how we handle the real-time system we build if the website scales up to thousands of users. That&#8217;s a tough question, especially if we take this requirement into consideration for this tiny project.

{% include image.html url="/images/Screen-Shot-2015-01-27-at-10.30.24-AM.png" description="The notification" %}


Nevertheless, there must be a solution. Instead of handling WebSocket myself, I found <strong>Pusher</strong> &#8211; a hosted API for building realtime system. The idea lies on the fundemental idea: if the user puts a comment in the system, the system will notify Pusher, and then Pusher notifies 10,000 other users. So, there would be no pressure at all for the server.

{% include image.html url="/images/hero_howitworks.png" description="Understanding Pusher (Source: Pusher.com)" %}

3. The website should be able to recommend restaurants to the user

I have a little experience on data mining stuff but it was still a challenge for me since I just had 1 week left to construct the recommendation engine and plug it into my existing system.

After a few hours of invesigation, I discovered an open source for recommendation <strong>PredictionIO</strong>. So far I am uncertain about its pros and cons but I can say it supports a clear user guide and additional tutorials from external sites as well if someone wants to have a fresh start on the recommendation system.

The installation went well for me since I used <strong>brew </strong>to install all dependencies effortlessly. Then, I began to build the model for further recommendation. The idea is as follows:

- If the user views a restaurant, I add 1 score to the system
- If the user rates a restaurant, I add the score corresponding the score he gives to the system
- Also, I need to take care of the geo location (lattitude and longitude) since it would be ridiculous if the system recommends the user a restaurant in Australia while he is currently in Belgium.

The model then was built based on these underlying ideas. I did not have time to test it statistically but it seemed to be enough for a quick demonstration.
