---
id: 41
title: Lamport clock vs Vector clock
date: 2014-05-03T01:10:52+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=41
permalink: /2014/05/lamport-clock-vs-vector-clock/
categories:
  - Uncategorized
tags:
  - algorithm
  - distributed
  - lamport clock
  - vector clock
---
<p style="text-align: justify;">
  First and foremost, I want to say I am pretty much tired mentally and physically yet do not how to boost up myself. But I am human, so just have some fun with knowledge and pray I will be better when I reach the end of the post. Pray for me!
</p>

<p style="text-align: justify;">
  My today topic is much different compared to other posts. Neither programming languages nor Web-related things. Today is about Distributed Computer Systems and I can assure that it is not difficult to grasp as it seems to be.
</p>

<p style="text-align: justify;">
  <strong>1. Timing in distributed system</strong>
</p>

<p style="text-align: justify;">
  One of the most fundamental concept in distributed system is <em>to manage the order of events</em> &#8211; knowing one event happening before the another. To the best of my knowledge, two algorithms were invented to solve such thing are <em>Lamport</em> and <em>Vector clock</em>.
</p>

<p style="text-align: justify;">
  <strong>1.1 Lamport clock</strong>
</p>

<p style="text-align: justify;">
  Lamport timestamp or Lamport clock is an algorithm named under its creator, Lessie Lamport. Lamport clock is used to determine<em> the partial ordering the events. </em>
</p>

<p style="text-align: justify;">
  Suppose we have 3 different processes <strong>P0</strong>, <strong>P1</strong>, <strong>P2</strong> in the system. By following Lamport rules, every processes <strong>P0</strong>, <strong>P1</strong>, <strong>P2 </strong>will maintain a single Lamport clock, says <strong>t</strong>. Whenever a event sends a message to the receiver, it will necessarily attach its timestamp along with its message.
</p>

{% include image.html url="/images/Screen-Shot-2014-05-03-at-11.56.49-AM-300x109.png" description="Figure 1" %}

<p style="text-align: justify;">
  The first rule is for every internal event inside the process, the clock <strong>t </strong>will be increased by <strong>1 </strong>as illustrated in the <em>Figure 1</em> with the process P0 and its events <em><strong>a, b, c, d</strong>&#8230; </em>
</p>

> t:= t + 1

<p style="text-align: justify;">
  The second rule is whenever the receiver receives the message, it will try to compare its current timestamp with the attached timestamp of the message, and after that the value will be increased by <strong>1. </strong>Mathematically, we say
</p>

> **t:= max(currentTimestamp, messageTimestamp) + 1** where
>
> _currentTimestamp_: the current timestamp of the receiver
>
> _messageTimestamp_: the attached timestamp of the message

<p style="text-align: justify;">
  In the <em>Figure 1, </em>event <em><strong>k</strong> </em>is the receipt of the sending message of event <strong><em>f </em></strong>of the process <strong>P0.</strong> Following the first rule, we can say that the timestamp of event <em><strong>k</strong> </em>should be <strong>2</strong>. However, because event <strong><em>k</em></strong> is the receiver, we have to consider the second rule in this case. In detail, the attached timestamp of <em><strong>f</strong> </em>is <strong>6</strong>, and the computation
</p>

<p style="text-align: justify;">
  <strong>t := max(2 = currentTimestamp, 6 = messageTimestamp) + 1</strong>
</p>

<p style="text-align: justify;">
  simply yields 7. Thus, we have timestamp of <strong><em>k</em> </strong>is 7. Easy enough?
</p>

<p style="text-align: justify;">
  <strong>1. 2 Vector clock</strong>
</p>

<p style="text-align: justify;">
  Vector clock works in a little different manner compared to Lamport clock. Origin from mathematics, vector demonstrates the displacement from point to point. For example, vector AB [x,y,z] from point A(0,0,0) to point B(3, 4, 5) in 3D space. So, you can image vector clock is such an array in which each process is a list item.
</p>

<p style="text-align: justify;">
  Formally, vector clock is an array of integer instead of Lamport clocks&#8217;s unique integer. Reuse the above example, we will have a vector clock as following.
</p>

{% include image.html url="/images/Screen-Shot-2014-05-03-at-2.12.06-PM-300x94.png" description="Figure 2" %}

<p style="text-align: justify;">
  I will now explain rules of the Vector clock algorithms step by step by using the example.
</p>

<p style="text-align: justify;">
  First of all, every processes will have single vector, says <strong>vp[N]</strong> where <strong>N</strong> is the number of processes. In our case, <strong>P0</strong>, <strong>P1</strong>, <strong>P2</strong> will have vectors <strong>(0, 0, 0)</strong> at the beginning. And whenever the event start sending message, it necessarily attach its vector clock to the message in the same way as we see in Lamport clock.
</p>

<p style="text-align: justify;">
  Secondly, for every internal event of the same process, the timestamp will be increased by <strong>1</strong>. Take a look at events <em>a, b, c, d&#8230; </em>of process <strong>P0</strong>, the according timestamps are (1, 0, 0), (2, 0, 0)&#8230; respectively.
</p>

> vp[p] := vp[p] + 1 where p is the current process.

<p style="text-align: justify;">
  Thirdly, whenever the receiver receives the message, it will compare the value as same as Lamport.
</p>

> vp[i] := max(currentVector[i], receivedVector[i])

<p style="text-align: justify;">
  Event <em><strong>k,</strong></em> for example, is supposed to have the vector clock (0, 0, 2). However, when it receives the message from the event <em><strong>f,</strong></em> the third rule executes, which makes the vector of <em><strong>k</strong></em> is (6, 1, 2).
</p>
