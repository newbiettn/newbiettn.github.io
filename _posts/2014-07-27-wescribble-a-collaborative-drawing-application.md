---
id: 134
title: 'weScribble &#8211; A collaborative drawing application'
date: 2014-07-27T12:11:01+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=134
permalink: /2014/07/wescribble-a-collaborative-drawing-application/
categories:
  - Uncategorized
tags:
  - ambienttalk
  - android
  - distributed computing
  - draw
  - faul-tolerent
  - p2p
  - vub
---
During the course &#8220;Distributed computing system&#8221;, I implemented a collaborative system for drawing in Android named weScribble Pro. The future application allows multiple users to collaborate with each other for sharing and editing shapes on a common canvas. Users can draw several shapes (e.g. rectangle, square, line), move them in the canvas, and change some properties ( e.g. color or size). On top of that, users can select one or more shapes and group them in a grouped shape. A group shape can also be moved or changed, i.e. all the composite shapes move or change accordingly.

Users can join a drawing sessions by contacting any user which is already part of a drawing session. When a user joins a drawing session, he can add, remove or move shapes on the canvas of that session which is virtually shared between the different participants. This means that any change on the canvas of a user is propagated to the rest of the participants of the session which updates their canvas accordingly.

The system is supposed to support non-functional requirements, including

  * Peer-to-peer design: There will no centralized server.
  * Fault-tolerant design: every computational unit in the network can fail at any point in time.

<iframe width="745" height="419" src="//www.youtube.com/embed/u8k8O7nxP7A" frameborder="0" allowfullscreen></iframe>
